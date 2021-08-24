# Notification
A notification is a message that Android displays outside your app's UI to provide the user with reminders, communication from other people, or other timely information from your app. Users can tap the notification to open your app or take an action directly from the notification.

## [Usage](https://material.io/design/platform-guidance/android-notifications.html#usage)
Notifications are intended to inform users about events in your app. These two types of notifications are the most effective:
- Communication from other users;
- Well-timed and informative task reminders.

**Notification Anatomy**:
- Header area;
- Content area;
- Action area.

**How notifications may be noticed**:
- Showing a status bar icon;
- Appearing on the lock screen;
- Playing a sound or vibrating;
- Peeking onto the current screen;
- Blinking the device's LED.

## Notification drawer
The notification drawer in Android typically shows notifications in reverse-chronological order, with adjustments influenced by:
- The app's stated notification priority or importance;
- Whether the notification recently alerted the user with a sound or vibration;
- Any people attached to the notification and whether they are starred contacts;
- Whether the notification represents an important ongoing activity, such as a phone call in progress or music playing.

Starting in Android O, the Android system may alter the appearance of some notifications at the top and bottom of the list by adding emphasis or deemphasis, to help the user scan content.

## Grouping
Your app can present multiple notifications according to hierarchy:
- A parent notification displays a summary of its child notifications;
- If the parent notification is expanded by the user, all child notifications are revealed;
- A child notification may be expanded to reveal its entire content.

Child notifications are presented without duplicate header information. For example, if a child notification has the same app icon as its parent, then the child’s header doesn’t include an icon.

Child notifications should be understandable if they appear solo, as the system may show them outside of the group when they arrive.

## [Types of notifications](https://material.io/design/platform-guidance/android-notifications.html#types-of-notifications)

Notifications are considered either transactional or non-transactional.

### Transactional
Transactional notifications provide content that a user must receive at a specific time in order to do one of the following:
- Enable human-to-human interaction;
- Function better in daily life;
- Control or resolve transient device states.

If none of the above situations describe your notification, then it is non-transactional.

### Non-transactional opt-out and opt-in
Non-transactional notifications should be optional, as they may not appeal to all users. You can make them optional in one of two ways:

- Opt-out: Users receive opt-out notifications by default, but they may stop receiving them by turning off a setting;
- Opt-in: Users only receive opt-in notifications by turning on a setting in your app.

## [Settings](https://material.io/design/platform-guidance/android-notifications.html#settings)

### Channels in Android O

When you upgrade your app to Android O, you'll be required to define channels for your notifications – one for each type of notification you want to send.

Users control app notifications in Android O with channels. If a user doesn't want a certain notification from your app, they can block that channel rather than all notifications.

### Channel importance levels

For each channel you define, you'll assign it an importance level. Starting in Android O, importance levels control the behavior of each channel (taking the place of priority levels).

Importance levels have the following restrictions:
- The importance level you assign will be the channel’s default. Users can change a channel’s importance level in Android Settings;
- Once you choose an importance level, you're limited in how you can change it: you may only lower the importance, and only if the user hasn't explicitly changed it.

Channel importance should be chosen with consideration for the user's time and attention. When an unimportant notification is disguised as urgent, it can produce unnecessary alarm.

| Importance  | Behavior  |  Usage  | Examples  |
|---|---|---|---|
| HIGH | Makes a sound and appears on screen | Time-critical information that the user must know, or act on, immediately | Text messages, alarms, phone calls |
| DEFAULT | Makes a sound | Information that should be seen at the user’s earliest convenience, but not interrupt what they're doing | Traffic alerts, task reminders |
| LOW | No sound | Notification channels that don't meet the requirements of other importance levels | New content the user has subscribed to, social network invitations |
| MIN | No sound or visual interruption | Non-essential information that can wait or isn’t specifically relevant to the user | Nearby places of interest, weather, promotional content |

## [Create a basic notification](https://developer.android.com/training/notify-user/build-notification#SimpleNotification)

A notification in its most basic and compact form (also known as collapsed form) displays an icon, a title, and a small amount of content text. In this section, you'll learn how to create a notification that the user can click on to launch an activity in your app.

![](./res/notification.png "Notification")

### [Set the notification content](https://developer.android.com/training/notify-user/build-notification#builder)

To get started, you need to set the notification's content and channel using a `NotificationCompat.Builder` object. The following example shows how to create a notification with the following:
- A small icon, set by `setSmallIcon()`. This is the only user-visible content that's required;
- A title, set by `setContentTitle()`;
- The body text, set by `setContentText()`;
- The notification priority, set by `setPriority()`. The priority determines how intrusive the notification should be on Android 7.1 and lower.

Example:
```
var builder = NotificationCompat.Builder(this, CHANNEL_ID)
        .setSmallIcon(R.drawable.notification_icon)
        .setContentTitle(textTitle)
        .setContentText(textContent)
        .setPriority(NotificationCompat.PRIORITY_DEFAULT)
```

Notice that the `NotificationCompat.Builder` constructor requires that you provide a channel ID. This is required for compatibility with Android 8.0 (API level 26) and higher, but is ignored by older versions.

## [Update a notification](https://developer.android.com/training/notify-user/build-notification#Updating)

To update this notification after you've issued it, call `NotificationManagerCompat.notify()` again, passing it a notification with the same ID you used previously. If the previous notification has been dismissed, a new notification is created instead.

You can optionally call `setOnlyAlertOnce()` so your notification interupts the user (with sound, vibration, or visual clues) only the first time the notification appears and not for later updates.

## [Remove a notification](https://developer.android.com/training/notify-user/build-notification#Removing)

Notifications remain visible until one of the following happens:
- The user dismisses the notification;
- The user clicks the notification, and you called `setAutoCancel()` when you created the notification;
- You call `cancel()` for a specific notification ID. This method also deletes ongoing notifications;
- You call `cancelAll()`, which removes all of the notifications you previously issued;
- If you set a timeout when creating a notification using `setTimeoutAfter()`, the system cancels the notification after the specified duration elapses. If required, you can cancel a notification before the specified timeout duration elapses.

## When not to use a notification
Notifications should not be the primary communication channel with your users, as frequent interruptions may cause irritation. The following cases do not warrant notification:
- Cross-promotion, or advertising another product within a notification, which is strictly prohibited by the Play Store;
- An app that a user has never opened;
- Messages that encourage the user to return to an app, but provide no direct value, such as "Haven't seen you in a while";
- Requests to rate an app;
- Operations that don't require user involvement, like syncing information;
- Error states from which the app may recover without user interaction.

# Links
[Android notifications](https://material.io/design/platform-guidance/android-notifications.html)

[Create a Notification](https://developer.android.com/training/notify-user/build-notification)

# Further reading
[Notification samples](https://github.com/android/user-interface-samples/tree/main/Notifications)
