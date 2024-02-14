//
//  AppDelegate.swift
//  ParentApp
//
//  Created by thamizharasan t on 08/09/23.
//

import UIKit
import AWSS3
import Firebase
import FirebaseMessaging
import UserNotifications
@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        FirebaseApp.configure()
        Messaging.messaging().delegate = self
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) { success, _ in
            guard success else{
                return
            }
            print("Success in APNS registry")
        }
        application.registerForRemoteNotifications()
//        registerForPushNotifications()
        initializeS3()
        return true
    }
    func initializeS3() {
        let credentialsProvider = AWSCognitoCredentialsProvider(regionType: .APSouth1, identityPoolId: S3Constant.PoolID)
        let configuration = AWSServiceConfiguration(region: .APSouth1, credentialsProvider: credentialsProvider)
        AWSServiceManager.default().defaultServiceConfiguration = configuration
      }
    // MARK: UISceneSession Lifecycle
//    private func registerForPushNotifications() {
//        UNUserNotificationCenter.current().delegate = self
//        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
//            (granted, error) in
//            print("Permission granted: \(granted)")
//            // 1. Check if permission granted
//            guard granted else { return }
//            // 2. Attempt registration for remote notifications on the main thread
//            DispatchQueue.main.async {
//                UNUserNotificationCenter.current().delegate = self
//                UIApplication.shared.registerForRemoteNotifications()
//            }
//        }
//    }
    


}

//MARK: - UNUserNotificationCenterDelegate
extension AppDelegate: MessagingDelegate, UNUserNotificationCenterDelegate {
    func messaging(_ messaging: Messaging, didReceiveRegistrationToken fcmToken: String?) {
        messaging.token { token, _ in
            guard let token = token else{
                return
            }
            UserDefaults.standard.set(token, forKey: "Devicetoken")
            print("Token:\(token)")
        }
    }
    func application(_ application: UIApplication,
                     didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
      Messaging.messaging().apnsToken = deviceToken
    }

  // Receive displayed notifications for iOS 10 devices.
  func userNotificationCenter(_ center: UNUserNotificationCenter,
                              willPresent notification: UNNotification) async
    -> UNNotificationPresentationOptions {
    let userInfo = notification.request.content.userInfo
    print(userInfo)
        return [[.alert, .sound,.badge]]
  }

  func userNotificationCenter(_ center: UNUserNotificationCenter,
                              didReceive response: UNNotificationResponse) async {
    let userInfo = response.notification.request.content.userInfo

    print(userInfo)
  }
}

//extension AppDelegate: UNUserNotificationCenterDelegate {
//
//    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
//        // 1. Convert device token to string
//        let tokenParts = deviceToken.map { data -> String in
//            return String(format: "%02.2hhx", data)
//        }
//        let token = tokenParts.joined()
//        Messaging.messaging().apnsToken = deviceToken
//        print("Device Token: \(token)")
//    }
//
//    func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
//        print("Register remote notification error \(error.localizedDescription)")
//    }
//
//    func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
//        let userInfo = notification.request.content.userInfo
//        print(userInfo)
//        completionHandler([])
//    }
//
//    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void){
//        print(response.notification.request.content.userInfo)
//        completionHandler();
//    }
//
//}
