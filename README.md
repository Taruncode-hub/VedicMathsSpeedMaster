# Vedic Maths Speed Master - Complete Android App

## Production-Ready MAUI App for Play Store

A gamified, subscription-based Android application for teaching Vedic Mathematics to students (Grades 1-10) with real-time leaderboards, competitive challenges, and parent dashboards.

### Features

#### Authentication & User Management
- Email & Password Registration/Login
- Google Sign-In
- Forgot Password Recovery
- User Roles: Student & Parent
- Remember Login (Secure Token)

#### Learning Modes
- **Practice Mode**: 200+ questions per grade/topic with instant feedback
- **Test Mode**: 20-question full test with detailed results
- **Daily Quiz**: Same questions for all students per grade, reset daily at midnight
- **Challenge Mode**: Competitive 15-question matches with real-time scoring

#### Gamification
- XP Points & Coins System
- Speed Rating Levels (Beginner → Legend)
- Achievement Badges
- Daily Streak Tracking
- Rank Progression

#### Leaderboards
- Daily Leaderboard (per grade, top 50)
- Global Leaderboard (Weekly/Monthly/All-time)
- Speed Rating Rankings
- Real-time Updates

#### Premium Features
- All Grades Unlocked (Free: Grades 1-3 only)
- Unlimited Practice Questions
- Full Solution Views
- Challenge Mode Access
- Parent Dashboard
- Ad-Free Experience

#### Parent Dashboard (Premium)
- Child Progress Tracking
- Weak Topic Analysis
- Weekly Performance Reports
- Time Spent Monitoring
- PDF Report Generation

### Technology Stack

- **Framework**: .NET MAUI (Android)
- **Backend**: Firebase (Auth, Firestore, Realtime DB)
- **Billing**: Google Play In-App Subscriptions
- **Architecture**: MVVM + Services Layer
- **UI**: XAML with Material Design
- **Language**: C# 12

### Project Structure

```
VedicMathsSpeedMaster/
├── App.xaml / App.xaml.cs
├── AppShell.xaml / AppShell.xaml.cs
├── MauiProgram.cs
├── Models/
│   ├── User.cs
│   ├── Question.cs
│   ├── Challenge.cs
│   └── LeaderboardEntry.cs
├── Services/
│   ├── AuthService.cs
│   ├── UserService.cs
│   ├── QuestionService.cs
│   ├── ChallengeService.cs
│   ├── LeaderboardService.cs
│   ├── SubscriptionService.cs
│   └── RatingService.cs
├── ViewModels/
│   ├── LoginViewModel.cs
│   ├── HomeViewModel.cs
│   ├── PracticeViewModel.cs
│   ├── TestViewModel.cs
│   ├── ChallengeViewModel.cs
│   ├── LeaderboardViewModel.cs
│   └── ProfileViewModel.cs
├── Views/
│   ├── SplashPage.xaml
│   ├── LoginPage.xaml
│   ├── HomePage.xaml
│   ├── PracticePage.xaml
│   ├── TestPage.xaml
│   ├── ChallengePage.xaml
│   └── LeaderboardPage.xaml
├── Resources/
│   ├── Colors.xaml
│   └── Styles.xaml
└── Platforms/Android/
    ├── AndroidManifest.xml
    ├── google-services.json
    └── MainActivity.cs
```

### Setup Instructions

#### Prerequisites
1. Visual Studio 2022 Community Edition
2. .NET 8.0+ SDK
3. Android SDK (API 33+)
4. Firebase Project
5. Google Play Console Account

#### Installation

1. Clone the repository
2. Open in Visual Studio 2022
3. Install NuGet packages
4. Configure Firebase credentials
5. Update AndroidManifest.xml
6. Build and Run on Emulator

### Firebase Configuration

1. Create project at firebase.google.com
2. Add Android app with package name: `com.yourbrand.vedicmaths`
3. Download google-services.json
4. Place in Platforms/Android/
5. Enable:
   - Authentication (Email & Google)
   - Firestore Database
   - Cloud Functions
   - Realtime Database

### Firestore Collections Schema

```
users/{uid}
questions/practice_questions/{id}
questions/test_questions/{id}
quiz/daily_quiz/{date}
leaderboards/daily/{grade}/{date}
leaderboards/global/{period}
challenges/{id}
subscriptions/{uid}
```

### Subscription Plans

- **Free**: Grades 1-3, Limited daily practice
- **Monthly**: All grades, unlimited access
- **6 Months**: 15% discount
- **Yearly**: 25% discount

### Release Configuration

#### Generate Signed AAB
1. Create keystore file
2. Configuration → Release
3. Build Archive → Save .aab
4. Upload to Play Console

#### Play Console Requirements
- Privacy Policy URL
- Content Rating Questionnaire
- Screenshots (8+ required)
- App Icon (512x512)
- Feature Graphic (1024x500)
- Minimum SDK: 26
- Target SDK: 35+

### Performance Metrics

- Support 10,000+ concurrent users
- Optimized Firestore queries with pagination
- Batch question loading
- Real-time challenge synchronization
- Sub-second question load time

### Security

- Firebase Authentication with JWT
- Firestore Security Rules
- Secure token storage
- HTTPS only API calls
- Subscription verification via Firebase Functions

### License

MIT License - See LICENSE file

### Support & Documentation

- Firebase Docs: https://firebase.google.com/docs
- .NET MAUI: https://learn.microsoft.com/en-us/dotnet/maui
- Play Console: https://play.google.com/console

### Contributors

Created for production Android deployment on Google Play Store.

---

**App Status**: Production-Ready for Play Store Deployment
**Last Updated**: February 2026
**Vedic Mathematics Focus**: All major sutras and tricks for Grades 1-10
