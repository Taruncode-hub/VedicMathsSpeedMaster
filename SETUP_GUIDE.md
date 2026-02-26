# VEDIC MATHS SPEED MASTER - COMPLETE SETUP GUIDE

## Step-by-Step Project Setup

### Prerequisites
- Windows 10/11 (64-bit)
- Visual Studio 2022 Community (Latest)
- .NET 8.0 SDK
- Android SDK (API 33+)
- Firebase Account
- Google Play Console Account
- Google Cloud Account

---

## PHASE 1: ENVIRONMENT SETUP

### Step 1: Install Visual Studio 2022

1. Download from https://visualstudio.microsoft.com/vs/community/
2. Run installer
3. Select these workloads:
   - **ASP.NET and web development**
   - **.NET MAUI** (Critical!)
   - **Mobile development with .NET**
   - **Android SDK (API 33+)**
4. Continue installation (~30 minutes)
5. Restart computer when prompted

### Step 2: Verify .NET Installation

```bash
dotnet --version  # Should show 8.0 or higher
```

### Step 3: Create Project

1. Open Visual Studio 2022
2. File → New → Project
3. Search: ".NET MAUI App"
4. Click Next
5. **Project name**: `VedicMathsSpeedMaster`
6. **Location**: `C:\Projects\`
7. **Framework**: .NET 8.0
8. Click Create

---

## PHASE 2: FIREBASE CONFIGURATION

### Step 1: Create Firebase Project

1. Go to https://console.firebase.google.com
2. Click "Add project"
3. **Project name**: "VedicMathsApp"
4. Enable Google Analytics (optional)
5. Click Create

### Step 2: Add Android App to Firebase

1. In Firebase Console, click "Add app" → Android
2. **Package name**: `com.yourbrand.vedicmaths` (MUST match AndroidManifest.xml)
3. **App nickname**: "Vedic Maths App"
4. Click "Register app"
5. Download `google-services.json`
6. Place in: `VedicMathsSpeedMaster/Platforms/Android/google-services.json`

### Step 3: Get Firebase API Key

1. Firebase Console → Project Settings → Service Accounts
2. Click "Generate new private key"
3. Copy the API Key from: Project Settings → General → Web API Key
4. Update in `MauiProgram.cs`:

```csharp
var firebaseConfig = new FirebaseAuthConfig
{
    ApiKey = "YOUR_API_KEY_HERE",
    AuthDomain = "your-project-id.firebaseapp.com"
};
```

### Step 4: Enable Firebase Services

1. **Authentication**:
   - Firebase Console → Authentication → Sign-in method
   - Enable: Email/Password
   - Enable: Google
   - Add OAuth credentials (see Google Play Console section)

2. **Firestore Database**:
   - Firestore Database → Create Database
   - Location: `us-central1`
   - Start in test mode (change to production rules later)

3. **Realtime Database**:
   - Realtime Database → Create Database
   - Location: `us-central1`
   - Start in test mode

4. **Cloud Functions** (for daily quiz reset):
   - Cloud Functions → Create Function
   - Trigger: Pub/Sub
   - Topic: `daily-quiz-reset`
   - Runtime: Node.js 18

---

## PHASE 3: GOOGLE PLAY CONSOLE SETUP

### Step 1: Create Developer Account

1. Go to https://play.google.com/console
2. Sign in with Google account
3. Pay $25 registration fee
4. Complete developer profile

### Step 2: Create App

1. Click "Create app"
2. **App name**: "Vedic Maths Speed Master"
3. **Default language**: English
4. App type: App
5. Category: Education
6. Content rating: ESRB rating - "Intended for families"
7. Click Create

### Step 3: Configure App

1. **App info**:
   - Short description (max 80 chars): "Master Vedic Maths 10x faster with gamified learning"
   - Full description: 4000 char limit (highlight features)
   - Screenshots: 8 minimum (1080x1920 PNG)
   - Icon: 512x512 PNG (blue/orange theme)
   - Feature Graphic: 1024x500 PNG

2. **Content rating questionnaire**:
   - Mark appropriate age groups
   - Answer all privacy questions
   - Mark "Made for families" if applicable

3. **Target audience**: Ages 6-12+ / Education

### Step 4: Generate Signing Key

1. In Visual Studio → Project Properties → Android Options
2. Signing Tab:
   - **Keystore name**: `VedicMaths.keystore`
   - **Keystore password**: `YourSecurePassword123!`
   - **Key alias**: `vedicmaths`
   - **Key password**: `YourSecurePassword123!`
3. Save keystore backup safely!

### Step 5: Get SHA Keys for Firebase

```bash
# After creating keystore in VS, get SHA-1:
cd C:\path\to\keystore
keytool -list -v -keystore VedicMaths.keystore
```

Add **SHA-1** and **SHA-256** to Firebase Console:
- Project Settings → Your apps → Android → Add fingerprint

---

## PHASE 4: FIRESTORE DATABASE SETUP

### Create Collections & Seed Initial Data

```
Firestore Structure:

users/
  {uid}/
    - name: string
    - grade: int (1-10)
    - xp: int
    - subscriptionType: string
    - accuracy: double
    - streak: int
    - etc...

practice_questions/
  {questionId}/
    - grade: int
    - topicId: string
    - difficulty: string
    - questionText: string
    - correctAnswer: string
    - steps: array
    - etc...

test_questions/ (similar structure)

daily_quiz/
  {date}/
    - grade: int
    - questions: array of questionIds
    - resetAt: timestamp

leaderboard_daily/
  {grade}/{date}/
    - userId: string
    - score: int
    - accuracy: double
    - totalTime: int

leaderboard_global/
  weekly/
    {uid}/
      - xp: int
      - speedRating: string
  monthly/ (similar)
  alltime/ (similar)

challenges/
  {challengeId}/
    - player1Id: string
    - player2Id: string
    - status: string (pending/active/finished)
    - questionIds: array
    - scores: object
    - times: object

subscriptions/
  {uid}/
    - type: string (Free/Monthly/SixMonths/Yearly)
    - purchaseToken: string
    - expiryDate: timestamp
```

### Firestore Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read/write their own documents
    match /users/{uid} {
      allow read, write: if request.auth.uid == uid;
    }
    
    // Questions are publicly readable
    match /practice_questions/{document=**} {
      allow read: if true;
    }
    
    match /test_questions/{document=**} {
      allow read: if true;
    }
    
    // Leaderboards are publicly readable
    match /leaderboard_daily/{document=**} {
      allow read: if true;
    }
    
    match /leaderboard_global/{document=**} {
      allow read: if true;
    }
    
    // Results are user-specific
    match /results/{document=**} {
      allow read, write: if request.auth.uid == resource.data.userId;
    }
  }
}
```

---

## PHASE 5: ADD NUGET PACKAGES

1. Tools → NuGet Package Manager → Package Manager Console
2. Run these commands:

```powershell
Install-Package FirebaseAuthentication.net
Install-Package Plugin.Firebase.Firestore
Install-Package Plugin.Firebase.CloudMessaging
Install-Package CommunityToolkit.Mvvm
Install-Package Newtonsoft.Json
Install-Package Xamarin.GooglePlay.BillingClient
```

---

## PHASE 6: BUILD AND TEST

### Build for Android Emulator

1. Visual Studio → Build → Build Solution
2. Tools → Android → Android Emulator Manager
3. Create emulator (API 33+)
4. Select Android project
5. Debug → Run on Device → Select Emulator
6. Wait for app to launch (~2 mins first time)

### Test on Physical Device

1. Enable Developer Mode: Settings → About Phone → Build Number (tap 7 times)
2. Enable USB Debugging: Developer Options → USB Debugging
3. Connect via USB
4. Visual Studio will detect device
5. Debug → Run on Device → Select device

---

## PHASE 7: GENERATE RELEASE BUILD (AAB)

### Step 1: Configure Release Build

1. Build Configuration: **Release** (top dropdown)
2. Platform: **Any CPU**

### Step 2: Archive Project

1. Build → Archive
2. Select configuration: Release|AnyCPU
3. Click Archive
4. Wait for completion (~2 mins)

### Step 3: Save AAB File

1. After archiving, Distribution → Store → Next
2. Sign and distribute
3. Browse to save location
4. AAB file saved to: `bin/Release/net8.0-android/*.aab`

### Step 4: Upload to Play Console

1. Play Console → VedicMaths app → Internal Testing
2. Releases tab → Create new release
3. Upload AAB file
4. Add release notes
5. Review and rollout to Internal Testing first
6. After testing, promote to Production

---

## PHASE 8: PLAY STORE LISTING

### Required Assets

**Screenshots** (8+ minimum, 1080x1920):
1. Home Dashboard
2. Practice Question
3. Test Result
4. Daily Leaderboard
5. Challenge Mode
6. Gamification/Ranks
7. Subscription Plans
8. Parent Dashboard

**Copy**:

**Short Description** (max 80):
"Master Vedic Maths 10x faster with gamified learning challenges"

**Full Description** (~4000 chars):
"Vedic Maths Speed Master is a revolutionary app designed for students in grades 1-10 to learn ancient Vedic mathematical techniques that enable faster mental calculations. Features include:

✓ 200+ practice questions per topic
✓ Daily quiz challenges
✓ Real-time leaderboards
✓ Competitive challenges with other students
✓ XP points and achievement badges
✓ Parent dashboard for progress tracking
✓ Step-by-step solution explanations

Subscription plans available for unlimited access."

---

## TROUBLESHOOTING

### Build Error: Android SDK not found
- Tools → Android → Android SDK Manager
- Install API 33+
- Update ANDROID_HOME environment variable

### Firebase Authentication fails
- Check API key in MauiProgram.cs
- Verify SHA-1 key in Firebase Console
- Enable Email/Password auth in Firebase

### Firestore queries return empty
- Check Firestore security rules
- Verify collection names match exactly
- Use Firebase Console to manually add test data

### Play Store upload fails
- Verify app signing is configured
- Check version code incremented
- Ensure AAB is valid (not APK)

---

## DEPLOYMENT CHECKLIST

- [ ] Firebase project created and configured
- [ ] Android app added to Firebase with SHA keys
- [ ] Google Play Console account active
- [ ] google-services.json added to project
- [ ] All NuGet packages installed
- [ ] AndroidManifest.xml updated with permissions
- [ ] Firestore collections created with seed data
- [ ] Signing key generated and backed up
- [ ] Release build (AAB) generated
- [ ] AAB uploaded to Play Console
- [ ] Store listing completed with all assets
- [ ] Content rating questionnaire completed
- [ ] Privacy policy URL added
- [ ] Internal testing passed
- [ ] Released to production

---

## NEXT STEPS

After deployment:
1. Monitor crash logs in Play Console
2. Respond to user reviews
3. Track analytics in Firebase
4. Plan next feature release
5. A/B test subscription pricing
6. Gather user feedback for improvements

---

**Need Help?**
- Firebase Docs: https://firebase.google.com/docs
- MAUI Docs: https://learn.microsoft.com/dotnet/maui
- Play Console Help: https://support.google.com/googleplay/android-developer
- Community: Stack Overflow tag "android-maui"
