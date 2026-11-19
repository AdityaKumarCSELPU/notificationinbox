# Firebase Notification Inbox (Kotlin + XML)

A native Android sample implementing a locally persisted notification inbox using Firebase Cloud Messaging, Room, RecyclerView, and Coil.

## Features
- Receives FCM data messages and posts Android system notifications.
- Uses notification channel ID `assignment`, channel name `Assignment`.
- Saves inbox items in Room; IDs are primary keys and duplicate inserts are ignored.
- Inbox list includes title, description, banner, timestamp, and unread state.
- Details screen marks an item read.
- Handles missing/invalid image URLs with a placeholder drawable.
- Requests notification permission on Android 13 (API 33) and above.


## Requirements
- Android Studio with Android SDK 36 installed
- A Firebase project with Firebase Cloud Messaging enabled

- Add your Firebase Android app using package `com.example.notificationinbox`
- Download `google-services.json` from Firebase Console and place it in the `app/` directory. Do not commit a file containing credentials or project secrets if your organization requires it to be private.


## Build
1. Open the `NotificationInbox` folder in Android Studio.
2. Add `app/google-services.json` from your Firebase project.
3. Sync Gradle.
4. Run on a device/emulator (Android 13+ asks for notification permission).


## FCM message format
Send a **data-only** message so `FirebaseMessagingService.onMessageReceived()` can save it to Room and display a system notification while the app is in the background. Example data payload:

```json
{
  "message": {
    "token": "YOUR_DEVICE_FCM_TOKEN",
    "data": {
      "id": "notification_101",
      "title": "New Notification",
      "description": "This is a sample notification for the assignment.",
      "bannerImg": "https://example.com/banner.jpg",
      "timestamp": "1791873000000"
    }
  }
}
```

Send it through the Firebase Cloud Messaging HTTP v1 API using a server-side environment. Never embed service-account credentials in the Android app or publish them in a public repository.


## Notes / limitations
- This sample currently relies on data-only FCM payloads for inbox persistence. FCM notification-only messages may be displayed automatically by Android/FCM in the background and may not invoke `onMessageReceived()` for local inbox storage.
- `bannerImg` is loaded on demand by Coil; image errors show a placeholder.
- The notification click opens the inbox. Opening an inbox item opens details and marks it read.
- This repository is a source starter; build it in Android Studio and add Firebase configuration before running.
