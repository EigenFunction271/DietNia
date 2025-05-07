# ASEAN Calorie Scanner App - Application Flow

## Overview

This document outlines the application flow for the ASEAN Calorie Scanner App, detailing the user interactions, screens, and system behaviors. It serves as a guide for developers to understand how various components of the application should interact.

## 1. User Authentication and Onboarding Flow

```mermaid
flowchart TD
    A[App Launch] --> B{First Time User?}
    B -->|Yes| C[Welcome Screen]
    B -->|No| N[Login Screen]
    
    C --> D[Registration Screen]
    D --> E[Email/Password Entry]
    E --> F[Email Verification]
    F --> G[Profile Creation]
    
    N --> O[Email/Password Entry]
    O --> P{Valid Credentials?}
    P -->|Yes| L
    P -->|No| Q[Error Message] --> N
    
    G --> H[Basic Info Input]
    H --> I[Health Goals Selection]
    I --> J[Dietary Restrictions]
    J --> K[Onboarding Tutorial]
    K --> L[Dashboard]
```

### 1.1 Registration Flow Details

1. **Welcome Screen**
   - App introduction
   - Benefits overview
   - "Get Started" CTA button

2. **Registration**
   - Email input with validation
   - Secure password creation
   - Terms and privacy policy acceptance

3. **Email Verification**
   - Verification email sent
   - Verification code input screen
   - Resend option

4. **Profile Creation**
   - Personal info: Name, age, gender
   - Physical stats: Height, weight
   - Activity level selection with visual guides
   - Health goals: Weight loss, maintenance, muscle gain
   - Optional: Allergies and dietary restrictions

5. **Onboarding Tutorial**
   - Walkthrough of key features
   - Food scanning demonstration
   - Dashboard introduction
   - "Skip Tutorial" option

## 2. Main Application Flow

```mermaid
flowchart TD
    A[Dashboard] --> B{User Action?}
    
    B -->|Scan Food| C[Camera Screen]
    C --> D[Food Recognition Processing]
    D --> E{Food Identified?}
    E -->|Yes| F[Confirmation Screen]
    E -->|No| G[Manual Search]
    
    F --> H{Correct Food?}
    H -->|Yes| I[Portion Selection]
    H -->|No| G
    G --> I
    
    I --> J[Nutritional Info Display]
    J --> K[Add to Log]
    K --> A
    
    B -->|View Progress| L[Progress Screen]
    L --> A
    
    B -->|Update Profile| M[Profile Settings]
    M --> A
    
    B -->|View History| N[History Screen]
    N --> A
```

## 3. Food Scanning & Logging Flow

### 3.1 Food Scanning Process

```mermaid
flowchart TD
    A[Initiate Scan] --> B[Camera Screen]
    B --> C{Take Photo?}
    C -->|Yes| D[Image Capture]
    C -->|No| B
    D --> E[Processing Indicator]
    E --> F[AI Recognition]
    F --> G{Confidence Level}
    G -->|High| H[Show Results]
    G -->|Medium| I[Show Options]
    G -->|Low| J[Manual Search]
    
    H --> K{User Confirms?}
    K -->|Yes| L[Portion Selection]
    K -->|No| M[Correction Options]
    M --> L
    
    I --> N{User Selects?}
    N -->|Item| L
    N -->|None| J
    J --> O[Search Results]
    O --> L
    
    L --> P[Review Nutrition]
    P --> Q{Add to Log?}
    Q -->|Yes| R[Select Meal Type]
    Q -->|No| S[Discard]
    
    R --> T[Add Note/Rating]
    T --> U[Save Entry]
    U --> V[Dashboard Update]
    S --> V
```

### 3.2 Meal Composition Details

1. **Simple Meals**
   - Single item identification
   - Portion estimation
   - Direct logging

2. **Complex Meals**
   - Multiple items in single photo
   - Component separation
   - Individual portion adjustment
   - Composite nutritional calculation

3. **Custom & Recurring Meals**
   - Save meal combinations
   - Quick-add favorites
   - Meal templates

## 4. Dashboard & Tracking Flow

```mermaid
flowchart TD
    A[Dashboard Home] --> B{View Selection}
    
    B -->|Daily Overview| C[Daily Summary]
    C --> D[Meal Breakdown]
    D --> E[Macro Distribution]
    
    B -->|Weekly Analysis| F[7-Day View]
    F --> G[Trend Visualization]
    
    B -->|Nutrition Insights| H[Nutrition Quality Score]
    H --> I[Improvement Suggestions]
    
    B -->|Goal Progress| J[Goal Metrics]
    J --> K[Achievements]
```

### 4.1 Dashboard Components

1. **Daily Summary**
   - Calorie goal vs. consumed
   - Macro breakdown with visual indicators
   - Meal-by-meal breakdown
   - Water intake tracking

2. **Nutrition Quality**
   - Balance score
   - Color-coded indicators
   - Nutrient density metrics

3. **Goal Progress**
   - Progress toward weight/health goal
   - Consistency streaks
   - Achievements and milestones

## 5. Settings & Personalization Flow

```mermaid
flowchart TD
    A[Profile Screen] --> B{Setting Category}
    
    B -->|Personal Info| C[Edit Profile]
    C --> D[Update Personal Details]
    
    B -->|Goals & Targets| E[Edit Goals]
    E --> F[Recalculate Nutritional Targets]
    
    B -->|Preferences| G[App Settings]
    G --> H[Notification Preferences]
    G --> I[Language Settings]
    G --> J[Theme Options]
    
    B -->|Privacy| K[Privacy Settings]
    K --> L[Data Sharing Options]
    K --> M[Export Data]
```

### 5.1 Settings Details

1. **Profile Management**
   - Update personal information
   - Progress photos
   - Weight history

2. **Goal Adjustment**
   - Modify weight/health goals
   - Adjust activity levels
   - Update dietary restrictions

3. **App Preferences**
   - Notifications and reminders
   - Language selection for ASEAN regions
   - Measurement units (metric/imperial)
   - Theme settings

4. **Privacy Controls**
   - Data sharing preferences
   - Export personal data
   - Delete account option

## 6. Key User Interactions

### 6.1 Food Logging

**Scan Flow:**
1. User taps "+" icon on dashboard
2. Selects "Scan Food"
3. Camera view appears with framing guides
4. User takes photo of food
5. Loading indicator appears during processing
6. Recognition results displayed
7. User confirms or corrects identification
8. Selects portion size
9. Reviews nutritional information
10. Assigns to meal type (breakfast/lunch/dinner/snack)
11. Optional: adds notes or tags
12. Saves entry
13. Dashboard updates with new data

**Manual Search Flow:**
1. User taps "+" icon on dashboard
2. Selects "Search Food"
3. Enters food name in search field
4. Browses results with ASEAN food emphasis
5. Selects correct food item
6. Follows same steps as scan flow from step 8

### 6.2 Progress Review

**Daily Review:**
1. User opens app to dashboard
2. Views today's summary
3. Taps on specific meal for details
4. Reviews macro distribution
5. Checks remaining calorie/macro budget

**Weekly Review:**
1. User navigates to Progress tab
2. Views 7-day overview
3. Examines trend charts
4. Reviews consistency metrics
5. Checks goal progress

### 6.3 Social & Community

**Recipe Sharing:**
1. User finds favorite meal in history
2. Taps "Share" option
3. Adds recipe details and preparation notes
4. Sets visibility (public/friends)
5. Posts to community feed

**Achievement Sharing:**
1. User earns milestone/achievement
2. Notification with share option appears
3. User customizes share message
4. Selects sharing platform
5. Posts achievement externally

## 7. System Flows

### 7.1 Data Synchronization

```mermaid
sequenceDiagram
    participant U as User App
    participant C as Cloud Database
    participant ML as ML Service
    
    U->>U: User logs food offline
    U->>U: Store in local database
    U->>+C: Connect to network
    U->>C: Sync new entries
    C->>U: Update remote data
    U->>ML: Send food images for training
    ML->>C: Update recognition models
    C->>-U: Sync model improvements
```

### 7.2 Food Recognition Process

```mermaid
sequenceDiagram
    participant U as User App
    participant ML as ML Service
    participant DB as Food Database
    
    U->>U: Capture food image
    U->>+ML: Send image for processing
    ML->>ML: Pre-processing
    ML->>ML: Apply recognition model
    ML->>DB: Query food candidates
    DB->>ML: Return potential matches
    ML->>ML: Confidence scoring
    ML->>-U: Return results with confidence levels
    U->>U: Display to user for confirmation
```

### 7.3 Personalized Recommendations

```mermaid
sequenceDiagram
    participant U as User App
    participant A as Analytics Engine
    participant R as Recommendation Engine
    
    U->>+A: Request insights
    A->>A: Analyze user patterns
    A->>A: Compare to goals
    A->>R: Send pattern data
    R->>R: Generate recommendations
    R->>-U: Return personalized suggestions
    U->>U: Display recommendations
```

## 8. Error Handling Flows

### 8.1 Recognition Failures

```mermaid
flowchart TD
    A[Image Capture] --> B[Recognition Attempt]
    B --> C{Recognition Result}
    C -->|Success| D[Show Result]
    C -->|Low Confidence| E[Show Top 3 Options]
    C -->|Failure| F[Manual Search Prompt]
    
    E --> G{User Action}
    G -->|Select Option| H[Continue Flow]
    G -->|None Correct| F
    
    F --> I[Search Interface]
    I --> J[User Selects Food]
    J --> K[Food Logging Continues]
    
    C -->|Server Error| L[Offline Storage]
    L --> M[Retry Later]
    M --> N[Background Sync]
```

### 8.2 Connectivity Issues

```mermaid
flowchart TD
    A[User Action] --> B{Network Available?}
    B -->|Yes| C[Normal Operation]
    B -->|No| D[Offline Mode]
    
    D --> E[Local Database Storage]
    E --> F[Visual Indicator]
    F --> G{Network Restored?}
    G -->|Yes| H[Background Sync]
    G -->|No| I[Retry Later]
    
    H --> J[Conflict Resolution]
    J --> K[Update Local Database]
```

## 9. Special Flows for ASEAN Context

### 9.1 Regional Food Variants

```mermaid
flowchart TD
    A[Food Recognition] --> B{Multiple Regional Variants?}
    B -->|Yes| C[Show Variant Options]
    B -->|No| D[Standard Recognition]
    
    C --> E[Display Country Icons]
    E --> F{User Selects Variant}
    F --> G[Apply Specific Nutritional Data]
    
    D --> G
    G --> H[Continue Logging]
```

### 9.2 Language Switching

```mermaid
flowchart TD
    A[User Changes Language] --> B[Update UI Language]
    B --> C[Translate Food Names]
    C --> D[Adjust Measurement Units]
    D --> E[Reload Dashboard]
```

## 10. Key User Journeys

### 10.1 First-Time User

Day 1:
1. Downloads app
2. Completes registration and profile setup
3. Takes tutorial tour
4. Scans first meal with guidance
5. Views nutritional breakdown
6. Sets reminder for next meal

Day 2-7:
1. Responds to meal logging reminders
2. Builds consistent logging habit
3. Reviews daily summaries
4. Gets first weekly report
5. Adjusts goals based on initial experience

### 10.2 Health-Focused Regular User

Weekly Routine:
1. Logs all meals consistently
2. Checks nutrition balance daily
3. Reviews weekly trends each Sunday
4. Adjusts meal plans for upcoming week
5. Shares progress with support network

### 10.3 Diabetic User

Daily Routine:
1. Logs pre-meal blood glucose
2. Scans meal to check carbohydrate content
3. Makes food choices based on recommendations
4. Tracks post-meal glucose response
5. Reviews carbohydrate trends

## 11. Post-MVP Enhancement Flows

### 11.1 Social Community Features

```mermaid
flowchart TD
    A[Community Tab] --> B{User Action}
    
    B -->|View Feed| C[Community Posts]
    C --> D[Filter by Region/Cuisine]
    
    B -->|Share Recipe| E[Create Recipe Post]
    E --> F[Add Photos/Description]
    F --> G[Include Nutritional Data]
    G --> H[Publish to Community]
    
    B -->|Join Challenge| I[Challenge Details]
    I --> J[Opt-in Participation]
    J --> K[Track Challenge Progress]
```

### 11.2 Advanced Analytics

```mermaid
flowchart TD
    A[Insights Tab] --> B{Analysis Type}
    
    B -->|Pattern Analysis| C[Eating Patterns]
    C --> D[Time Distribution]
    D --> E[Macro Timing]
    
    B -->|Nutrient Deep Dive| F[Micronutrient Analysis]
    F --> G[Deficiency Alerts]
    
    B -->|Goal Simulation| H[What-If Scenarios]
    H --> I[Projected Outcomes]
```

## 12. Data Integration Flows

### 12.1 Wearable Integration

```mermaid
sequenceDiagram
    participant U as User App
    participant W as Wearable Device
    participant C as Cloud Service
    
    U->>U: Enable device connection
    U->>W: Authentication request
    W->>U: Connection established
    W->>C: Sync activity data
    C->>U: Update calorie adjustments
    U->>U: Recalculate daily goals
```

### 12.2 Health App Integration

```mermaid
sequenceDiagram
    participant SC as Scanner App
    participant H as Health Platform
    
    SC->>H: Request permissions
    H->>SC: Grant data access
    SC->>H: Write nutrition data
    H->>SC: Read activity data
    SC->>SC: Holistic health analysis
```

## 13. Cross-Platform Consistency

This application flow is designed to work consistently across:
- iOS and Android mobile platforms
- Tablet optimized layouts
- Web companion (future)

The user flow prioritizes:
- Minimal steps for common actions
- Consistent navigation patterns