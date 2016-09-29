# Push Notifications in iOS 10 [Swift 3]

<p align="justify">The new framework called "UserNotifications"	is introduced with iOS 10 SDK. The <a href="https://developer.apple.com/reference/usernotifications" target="_blank">UserNotifications framework</a> (UserNotifications.framework) supports the delivery and handling of local and remote notifications.

So, Let see what we have to change to get the push notifications in iOS 10.
</p>

##Steps for implement code to handle push notifications in iOS 10

###Import UserNotifications.framework in your AppDelegate file

```swift
import UserNotifications
```
Also add UNUserNotificationCenterDelegate.

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
}
```
###Register for push notification

<p align="justify">Before registration check the version of iOS and then based on versions do the code. For iOS 7 code was different, fro iOS 8 & 9 code was different and again for iOS 10 code is different.</p>

<p align="justify"><em>As per my opinion you have to set the deployment target to iOS 8 or iOS 9 and later. For this you can check the <a href="https://developer.apple.com/support/app-store/" target="_blank">adoption ratio of iOS</a> in the devices.</em></p>

<strong>Add code in your did finish launching</strong>

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    // Override point for customization after application launch.
    registerForRemoteNotification()        
    return true
}
 
func registerForRemoteNotification() {
    if #available(iOS 10.0, *) {
        let center  = UNUserNotificationCenter.current()
        center.delegate = self
        center.requestAuthorization(options: [.sound, .alert, .badge]) { (granted, error) in
            if error == nil{
                UIApplication.shared.registerForRemoteNotifications()
            }
        }
    }
    else {
        UIApplication.shared.registerUserNotificationSettings(UIUserNotificationSettings(types: [.sound, .alert, .badge], categories: nil))
        UIApplication.shared.registerForRemoteNotifications()
    }
}
```

###Handling delegate methods for UserNotifications

<p align="justify"><strong><em>You will be surprise that notification displayed when application in foreground too in iOS 10. As we know that in old versions we display alert or something else which will be look like notification comes in foreground.</em></strong></p>

There are two delegate methods need to be handled :

```swift
//Called when a notification is delivered to a foreground app.
@available(iOS 10.0, *)
func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
    print("User Info = ",notification.request.content.userInfo)
    completionHandler([.alert, .badge, .sound])
}
 
//Called to let your app know which action was selected by the user for a given notification.    
@available(iOS 10.0, *)
func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
    print("User Info = ",response.notification.request.content.userInfo)
    completionHandler()
}
```

###Add Push Notifications Entitlements

<p align="justify">Go to your project target's <strong>Capabilities</strong> tab and add Push Notifications Entitlements.</p>

<img src="http://ashishkakkad.com/wp-content/uploads/2016/09/Push-Notifications-Entitlements.png" alt="Push Notifications Entitlements" />

<p align="justify">If it's available in your certificates then it will enable directly else configure your profile with the certificates and you can enable this capability by that.</p>

<b>Blogpost on my website : <a href="http://ashishkakkad.com/2016/09/push-notifications-in-ios-10-swift/" target="_blank">Push Notifications in iOS 10 [Swift]</a></b>
