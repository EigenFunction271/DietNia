# Product Requirements Document: ASEAN Calorie Scanner App

## 1. Product Overview

### 1.1 Product Vision
Create a mobile application that enables users to easily track their nutrition by scanning ASEAN food items, providing accurate nutritional information with minimal effort, and guiding users toward their health goals through personalized recommendations.

### 1.2 Target Users
- Health-conscious individuals in ASEAN countries
- Fitness enthusiasts tracking macronutrients
- People with dietary restrictions needing nutritional information
- Individuals on weight management programs
- Users frustrated with global apps that don't recognize local cuisine

### 1.3 User Personas

**Persona 1: Fitness-Focused Farah**
- 28-year-old fitness enthusiast
- Tracks macros carefully for muscle building
- Eats a mix of home-cooked and restaurant meals
- Lives in Malaysia
- Tech-savvy and uses multiple fitness apps

**Persona 2: Health-Conscious Heng**
- 45-year-old professional with type 2 diabetes
- Needs to monitor carbohydrate intake carefully
- Mostly eats traditional dishes
- Lives in Singapore
- Moderate tech proficiency

**Persona 3: Busy Student Binh**
- 22-year-old university student
- Wants to avoid weight gain while eating campus food
- Limited budget and time for meal planning
- Lives in Vietnam
- High tech proficiency but low nutrition knowledge

## 2. Feature Requirements

### 2.1 Core Features (MVP)

#### User Authentication and Profile
- **User registration with email and password**
  - Email verification
  - Secure password requirements
- **User profile creation**
  - Biometric data input (age, height, weight, gender)
  - Activity level selection (sedentary, lightly active, active, very active)
  - Health goals selection (weight loss, maintenance, weight gain)
  - Optional: allergies and dietary restrictions

#### Food Scanner
- **Camera integration for food photos**
  - Real-time food recognition
  - Multiple-item detection in single photo
- **Food identification**
  - Recognize common ASEAN dishes and ingredients
  - Display food name and serving size estimation
- **Manual search option**
  - Search database when scanning is inconvenient
  - Recent and favorite foods quick access

#### Nutrition Dashboard
- **Macro breakdown**
  - Proteins, carbohydrates, fats with visual indicators
  - Progress bars toward daily targets
- **Calorie summary**
  - Daily goal based on profile and activity
  - Consumed calories and remaining balance
- **Meal breakdown**
  - Separate tracking for breakfast, lunch, dinner, snacks
  - Time-stamped entries

#### Basic Progress Tracking
- **Daily nutrition summary**
  - Complete vs. incomplete macro targets
  - Calorie goal achievement
- **Weekly overview**
  - Simple trend visualization
  - Consistency indicators

### 2.2 Secondary Features (Post-MVP)

#### Enhanced Food Recognition
- **Multi-angle food scanning**
  - Improved portion size estimation
  - Component separation for mixed dishes
- **Customization of recognized foods**
  - Adjust ingredients and portions
  - Save custom versions of dishes

#### Advanced Nutrition Insights
- **Micronutrient tracking**
  - Vitamins and minerals
  - Hydration tracking
- **Nutrition quality scoring**
  - Balance of nutrients
  - Suggested improvements

#### Social Features
- **Community recipe sharing**
  - User-verified nutrition information
  - Regional variations of dishes
- **Progress sharing**
  - Optional social media integration
  - Achievements and milestones

#### Smart Recommendations
- **Meal suggestions**
  - Complementary foods to balance nutrients
  - Personalized based on preferences and goals
- **Behavioral insights**
  - Eating pattern analysis
  - Habit formation guidance

## 3. User Journey Maps

### 3.1 New User Onboarding
1. Download app and open
2. Register account (email/password)
3. Complete profile with biometric data
4. Set health goals
5. Tutorial on food scanning technique
6. First food scan with guided assistance
7. View nutritional breakdown
8. Overview of dashboard features

### 3.2 Daily Usage Flow
1. Open app before/during/after meal
2. Select meal type (breakfast/lunch/dinner/snack)
3. Scan food using camera
4. Review recognized items and portions
5. Confirm or adjust recognition results
6. View updated nutritional progress
7. Receive feedback on macro balance

## 4. Non-Functional Requirements

### 4.1 Performance
- Food recognition response time < 3 seconds
- App launch time < 2 seconds
- Smooth scrolling and transitions (60fps)
- Offline functionality for core features

### 4.2 Security
- Encrypted user data storage
- Secure authentication
- Privacy-focused image processing
- GDPR/PDPA compliance for ASEAN regions

### 4.3 Usability
- Intuitive, minimal interface
- Accessible to users with disabilities
- Support for multiple languages common in ASEAN
- Effective on various screen sizes and resolutions

### 4.4 Reliability
- App crash rate < 0.5%
- Data backup and synchronization
- Error recovery mechanisms
- Graceful degradation with poor connectivity

## 5. Success Metrics

### 5.1 User Engagement
- Daily active users (DAU)
- Average sessions per day
- Session duration
- Feature usage distribution
- Retention rate at 1, 7, 30 days

### 5.2 Technical Performance
- Recognition accuracy rate
- Average response time
- Error rate in food identification
- User correction frequency

### 5.3 Business Metrics
- User growth rate
- Premium conversion rate (if applicable)
- Cost per acquisition
- Lifetime value

## 6. Constraints and Assumptions

### 6.1 Constraints
- Limited initial database of ASEAN foods
- Variable lighting conditions affecting scan quality
- Internet connectivity variations across ASEAN regions
- Device camera quality differences

### 6.2 Assumptions
- Users are willing to take photos of food before eating
- Users will provide accurate biometric information
- Food presentation is reasonably distinct enough for recognition
- Users have basic nutrition knowledge or interest in learning

## 7. Timeline and Priorities

### 7.1 Development Phases
- **Phase 1 (MVP)**: Authentication, basic scanning, core nutritional tracking
- **Phase 2**: Enhanced recognition, expanded food database, improved UI
- **Phase 3**: Advanced analytics, social features, recommendations

### 7.2 Feature Priority Matrix

| Feature | Importance | Complexity | Priority |
|---------|------------|------------|----------|
| User Auth | High | Medium | P0 |
| Food Scanner | High | High | P0 |
| Basic Nutrition Dashboard | High | Medium | P0 |
| Profile & Goals | High | Low | P0 |
| Daily Progress | Medium | Medium | P1 |
| Enhanced Recognition | Medium | High | P1 |
| Micronutrient Tracking | Medium | Medium | P1 |
| Recommendations | Medium | High | P2 |
| Social Features | Low | Medium | P2 |

## 8. Open Questions and Risks

### 8.1 Open Questions
- How to handle homemade dishes with no standard recipe?
- Will users need barcode scanning for packaged foods?
- How to address regional variations of similar dishes?
- What level of nutritional education should be incorporated?

### 8.2 Risks
- Recognition accuracy for similar-looking dishes
- User frustration with incorrect identifications
- Data privacy concerns with food images
- Competition from global apps expanding ASEAN coverage
- Maintaining an up-to-date food database