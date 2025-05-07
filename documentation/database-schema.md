# Database Schema Design: ASEAN Calorie Scanner App

## 1. Overview

This document outlines the database schema for the ASEAN Calorie Scanner App. We'll be using MongoDB as our primary database, with Mongoose as the ODM (Object Document Mapper) to interact with it from our Node.js backend.

## 2. Collections

### 2.1 Users Collection

```javascript
const UserSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true,
    trim: true,
    lowercase: true,
    validate: {
      validator: function(v) {
        return /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v);
      },
      message: "Please enter a valid email"
    }
  },
  password: {
    type: String,
    required: true,
    minlength: 8
  },
  profile: {
    firstName: { type: String, trim: true },
    lastName: { type: String, trim: true },
    gender: { 
      type: String, 
      enum: ['male', 'female', 'other', 'prefer_not_to_say'] 
    },
    dateOfBirth: Date,
    height: { 
      value: Number,
      unit: { type: String, enum: ['cm', 'ft'], default: 'cm' }
    },
    weight: { 
      value: Number,
      unit: { type: String, enum: ['kg', 'lb'], default: 'kg' }
    },
    activityLevel: { 
      type: String, 
      enum: ['sedentary', 'lightly_active', 'moderately_active', 'very_active', 'extremely_active'],
      default: 'moderately_active'
    },
    dietaryPreferences: [{
      type: String,
      enum: ['vegetarian', 'vegan', 'pescatarian', 'gluten_free', 'dairy_free', 'halal', 'kosher', 'none']
    }],
    allergies: [String],
    profileCompleted: { type: Boolean, default: false }
  },
  healthGoals: {
    primaryGoal: { 
      type: String, 
      enum: ['weight_loss', 'weight_maintenance', 'weight_gain', 'muscle_gain', 'overall_health'],
      default: 'overall_health'
    },
    targetWeight: {
      value: Number,
      unit: { type: String, enum: ['kg', 'lb'], default: 'kg' }
    },
    weeklyGoal: { type: Number, default: 0 }, // Weight change per week (can be negative)
    calorieTarget: { type: Number, default: 2000 },
    macroTargets: {
      protein: { type: Number, default: 30 }, // percentage
      carbs: { type: Number, default: 40 },   // percentage
      fat: { type: Number, default: 30 }      // percentage
    },
    waterIntakeTarget: { type: Number, default: 2000 } // ml
  },
  settings: {
    language: { type: String, default: 'en' },
    notificationsEnabled: { type: Boolean, default: true },
    mealReminders: [{ 
      time: String, // HH:MM format
      days: [Number], // 0-6 (Sunday-Saturday)
      enabled: Boolean
    }],
    measurementSystem: { 
      type: String, 
      enum: ['metric', 'imperial'], 
      default: 'metric' 
    },
    privacySettings: {
      shareProgress: { type: Boolean, default: false },
      publicProfile: { type: Boolean, default: false }
    }
  },
  deviceTokens: [String], // For push notifications
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now },
  lastLoginAt: Date,
  isActive: { type: Boolean, default: true },
  verificationStatus: {
    email: { type: Boolean, default: false },
    mobile: { type: Boolean, default: false }
  },
  resetPasswordToken: String,
  resetPasswordExpires: Date
}, { timestamps: true });

// Virtual for BMI calculation
UserSchema.virtual('bmi').get(function() {
  if (!this.profile.weight || !this.profile.height) return null;
  
  const weightKg = this.profile.weight.unit === 'lb' 
    ? this.profile.weight.value * 0.453592
    : this.profile.weight.value;
    
  const heightM = this.profile.height.unit === 'ft'
    ? this.profile.height.value * 0.3048
    : this.profile.height.value / 100;
    
  return weightKg / (heightM * heightM);
});

// Virtual for BMR calculation (Mifflin-St Jeor Equation)
UserSchema.virtual('bmr').get(function() {
  if (!this.profile.weight || !this.profile.height || !this.profile.dateOfBirth || !this.profile.gender) return null;
  
  const weightKg = this.profile.weight.unit === 'lb' 
    ? this.profile.weight.value * 0.453592
    : this.profile.weight.value;
    
  const heightCm = this.profile.height.unit === 'ft'
    ? this.profile.height.value * 30.48
    : this.profile.height.value;
    
  const age = Math.floor((new Date() - new Date(this.profile.dateOfBirth)) / 31557600000); // Age in years
  
  if (this.profile.gender === 'male') {
    return (10 * weightKg) + (6.25 * heightCm) - (5 * age) + 5;
  } else {
    return (10 * weightKg) + (6.25 * heightCm) - (5 * age) - 161;
  }
});

// Pre-save hook to hash password
UserSchema.pre('save', async function(next) {
  if (!this.isModified('password')) return next();
  
  try {
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
    next();
  } catch (error) {
    next(error);
  }
});
```

### 2.2 Food Items Collection

```javascript
const FoodItemSchema = new mongoose.Schema({
  name: {
    en: { type: String, required: true, index: true },
    local: { type: String, index: true }
  },
  category: {
    type: String,
    enum: [
      'rice_dishes', 'noodle_dishes', 'soups', 'curries', 'salads', 
      'snacks', 'desserts', 'beverages', 'proteins', 'vegetables', 
      'fruits', 'condiments', 'packaged_foods', 'other'
    ],
    required: true,
    index: true
  },
  region: {
    country: { 
      type: String, 
      enum: ['thailand', 'vietnam', 'malaysia', 'indonesia', 'singapore', 
             'philippines', 'myanmar', 'cambodia', 'laos', 'brunei', 'other'],
      required: true,
      index: true
    },
    specificRegion: String
  },
  nutrition: {
    servingSize: {
      value: { type: Number, required: true },
      unit: { 
        type: String, 
        enum: ['g', 'ml', 'oz', 'cup', 'tablespoon', 'teaspoon', 'piece'],
        default: 'g',
        required: true
      },
      description: String // E.g., "1 cup cooked"
    },
    calories: { type: Number, required: true },
    macronutrients: {
      protein: { type: Number, required: true }, // grams
      carbohydrates: {
        total: { type: Number, required: true }, // grams
        fiber: Number, // grams
        sugar: Number  // grams
      },
      fat: {
        total: { type: Number, required: true }, // grams
        saturated: Number, // grams
        unsaturated: Number, // grams
        trans: Number  // grams
      }
    },
    micronutrients: {
      vitamins: {
        a: Number, // IU
        c: Number, // mg
        d: Number, // IU
        e: Number, // mg
        k: Number, // mcg
        b1: Number, // mg (Thiamine)
        b2: Number, // mg (Riboflavin)
        b3: Number, // mg (Niacin)
        b5: Number, // mg (Pantothenic Acid)
        b6: Number, // mg
        b7: Number, // mcg (Biotin)
        b9: Number, // mcg (Folate)
        b12: Number // mcg
      },
      minerals: {
        calcium: Number, // mg
        iron: Number, // mg
        magnesium: Number, // mg
        phosphorus: Number, // mg
        potassium: Number, // mg
        sodium: Number, // mg
        zinc: Number, // mg
        copper: Number, // mg
        manganese: Number, // mg
        selenium: Number // mcg
      }
    },
    cholesterol: Number, // mg
    sodium: Number // mg
  },
  ingredients: [{
    name: String,
    amount: Number,
    unit: String,
    optional: { type: Boolean, default: false }
  }],
  preparation: {
    cookingMethod: [String], // E.g., ["fried", "steamed"]
    isProcessed: { type: Boolean, default: false }
  },
  imageUrls: [String],
  recognitionData: {
    features: [Number], // Vector for ML model
    visualSignature: String, // Hash or signature for quick comparison
    commonVariations: [String], // Different appearances or presentations
    confusedWith: [{ // Similar-looking foods that may be confused
      foodId: { type: mongoose.Schema.Types.ObjectId, ref: 'FoodItem' },
      similarityScore: Number // 0-1 score
    }]
  },
  metadata: {
    source: String, // Where the nutritional data came from
    verificationLevel: {
      type: String,
      enum: ['unverified', 'community_verified', 'expert_verified', 'lab_tested'],
      default: 'unverified'
    },
    popularity: { type: Number, default: 0 }, // For sorting and recommendations
    addedBy: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    updatedBy: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    lastVerifiedAt: Date
  },
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
}, { timestamps: true });

// Text index for search functionality
FoodItemSchema.index({
  'name.en': 'text',
  'name.local': 'text'
});
```

### 2.3 User Meals Collection

```javascript
const UserMealSchema = new mongoose.Schema({
  userId: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true,
    index: true
  },
  date: {
    type: Date,
    required: true,
    index: true
  },
  mealType: {
    type: String,
    enum: ['breakfast', 'lunch', 'dinner', 'snack'],
    required: true
  },
  foods: [{
    foodItem: {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'FoodItem'
    },
    customFoodName: String, // For unrecognized or custom foods
    portionSize: {
      value: { type: Number, required: true },
      unit: { type: String, required: true }
    },
    nutrition: {
      calories: Number,
      protein: Number,
      carbohydrates: Number,
      fat: Number
    },
    addedManually: { type: Boolean, default: false },
    notes: String
  }],
  totalNutrition: {
    calories: { type: Number, default: 0 },
    macronutrients: {
      protein: { type: Number, default: 0 },
      carbohydrates: { type: Number, default: 0 },
      fat: { type: Number, default: 0 }
    },
    micronutrients: {
      vitamins: {
        a: { type: Number, default: 0 },
        c: { type: Number, default: 0 },
        d: { type: Number, default: 0 },
        // Other vitamins...
      },
      minerals: {
        calcium: { type: Number, default: 0 },
        iron: { type: Number, default: 0 },
        // Other minerals...
      }
    }
  },
  imageUrl: String, // URL to the meal photo
  location: {
    type: { type: String, enum: ['Point'], default: 'Point' },
    coordinates: { type: [Number], default: [0, 0] }, // [longitude, latitude]
    name: String, // E.g., "Home", "Office", "Restaurant Name"
  },
  tags: [String], // User-defined tags
  notes: String,
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now }
}, { timestamps: true });

// Index for date queries and geospatial queries
UserMealSchema.index({ userId: 1, date: -1 });
UserMealSchema.index({ location: '2dsphere' });

// Pre-save hook to calculate total nutrition
UserMealSchema.pre('save', function(next) {
  if (this.foods && this.foods.length > 0) {
    let calories = 0;
    let protein = 0;
    let carbs = 0;
    let fat = 0;
    
    this.foods.forEach(food => {
      calories += food.nutrition.calories || 0;
      protein += food.nutrition.protein