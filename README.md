# TripSense — Android Studio Starter Project

A buildable Android Studio (Kotlin + Jetpack Compose) starter project for TripSense. It includes a Home dashboard with bottom navigation, an SOS flow, and Map, Alerts, and Profile screens — all with mock data — plus **all 48 features** from the product spec listed in a catalog (`model/Feature.kt`) so the rest can be wired up step by step.

## How to build the APK

This sandbox environment doesn't have network access to download the Android SDK/Gradle, so I couldn't compile a `.apk` directly here. You can build one on your own computer like this:

1. Install **Android Studio** (Hedgehog or newer) — https://developer.android.com/studio
2. Open this folder (`TripSense/`) via `File → Open` in Android Studio
3. On first open, Android Studio will automatically download the Gradle wrapper jar and required SDK components (needs an internet connection)
4. Click **Run ▶** in the toolbar — it will run directly on an emulator or a connected phone
5. To get a signed APK: `Build → Generate Signed Bundle / APK → APK` — create a new keystore and sign it
6. Or from the terminal (once the project is open in Android Studio): `./gradlew assembleDebug` — output will be at `app/build/outputs/apk/debug/app-debug.apk`

## Setting up your API key (one time)

1. Copy `local.properties.example` to a new file named `local.properties` (same folder)
2. Get a free key: go to https://console.cloud.google.com/ → create a project → enable **Places API** and **Maps SDK for Android** → Credentials → Create Credentials → API Key
3. Paste it into `local.properties`: `MAPS_API_KEY=your_actual_key`
4. `local.properties` is already git-ignored, so your key is never committed if you push this to GitHub

Once this is set, the **Nearby Hospitals** feature (tap it from the Home grid) will show real hospitals around your current location, pulled live from Google Places — with working "Directions" and rating/open-now info.

## What already works (with mock data unless noted)

- Home dashboard — weather card, SOS button, grid of all features
- **Nearby Hospitals — fully working** once `MAPS_API_KEY` is set (`HospitalScreen.kt` + `PlacesRepository.kt`)
- SOS screen — UI flow ready (see the TODO comment in `SosScreen.kt` — that's where a real `FusedLocationProviderClient` + `SmsManager` integration goes)
- Map, Alerts, Profile screens — UI ready with mock data

## What's left — needs real integrations

Each of these needs a real API key/service, so they're only listed in the catalog for now (`model/Feature.kt`) without a UI built yet:

| Feature | What's needed |
|---|---|
| Weather / UV Index | OpenWeatherMap or similar API |
| GPS/Maps/Navigation | Google Maps SDK + API key |
| Ride Sharing, Fare Estimator | Uber/Pathao partner API |
| Train/Flight schedule | Bangladesh Railway and airline data sources |
| AI Landmark/Food/Snake/Plant Scanner | CameraX + a vision AI model/API (e.g. the Claude API or a custom model) |
| Voice Command | Android SpeechRecognizer |
| SOS SMS/Call | SmsManager, CALL_PHONE permission (already declared) |
| Offline mode | Room database + WorkManager sync |
| Push notifications (Disaster Alert) | Firebase Cloud Messaging |
| Traveller ID / Account | A backend (server-generated random UUID — never derive it from name/email/phone) |

## Project structure

```
app/src/main/java/com/tripsense/app/
  MainActivity.kt
  navigation/NavGraph.kt        — bottom-nav routes
  model/Feature.kt              — catalog of all 48 features
  ui/screens/                   — HomeScreen, SosScreen, MapScreen, AlertsScreen, ProfileScreen
  ui/components/FeatureTile.kt  — each feature icon in the grid
  ui/theme/                     — colors and typography
```

To add a new feature: it already has an entry in `model/Feature.kt` → build a new Composable in `ui/screens/` → add a route in `navigation/NavGraph.kt` → navigate to it from `onFeatureClick` in `HomeScreen.kt`.
