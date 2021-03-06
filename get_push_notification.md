# Get Push Notification [Optional]

MCS has [Trigger & Action][mcs-tutorial-notif] function. The user can set the trigger and action for a data channel when its value passes the limit of the defined range. The user will get email notification or the GCM notification based on the trigger and action setting. 

## Setup

Please be sure you have 

1. Create your own [Google Cloud Messaging][gcm] project to obtain **GCM_SENDER_ID** and **GCM_API_KEY**.
2. Follow the step of [MCS Android SDK - Setup with Push Installation][sdk-github-push]

## Check it on MCS Website

If the setup is correct, then the mobile device is registered to MCS Server when you get `OnSuccess()` response of `McsPushInstallation.getInstance().registerInBackground()`.

```java
McsPushInstallation.getInstance().registerInBackground(
    "YOUR_GCM_SENDER_ID", "YOUR_GCM_API_KEY",
    new McsResponse.SuccessListener<JSONObject>() {
        @Override public void onSuccess(JSONObject response) {
            // If you correctly setup with push installation, 
            // the mobile device is registered to MCS Server at this point.
            McsLog.d("This mobile's id: " + McsMobile.getMobileId());
        }
    },
    new McsResponse.ErrorListener<JSONObject>() {
        @Override public void onError(Exception error) {
            // something went wrong
        }
    }
);
```

Or, you can see it in the [User Profile page][mcs-profile].

![img-mobiles][img-mobiles]

Please refer to [Push Notifications- MCS Android SDK API Reference][sdk-api-notif] for detailed mechanism behind `McsPushInstallation.getInstance().registerInBackground()`.

## Trigger and Action

Note that you have to manually **switch on the notification for each registered mobile**. The mobile notification is **default as false.**

Please be sure you have already turn on the notification in **both**

1. **User Profile page**
2. **Test device page**

to receive push notification. Check [Setting trigger and action for different mobiles][mcs-tutorial-notif] section for detailed information.

Also, only true devices could trigger actions. True devices make request with header that contains `deviceKey`. Manipulation on MCS website or Mobile app will NOT trigger any action. Check [Upload data points][mcs-api] section for detailed information.


[mcs-api]: https://mcs.mediatek.com/resources/latest/api_references/
[mcs-tutorial-notif]: https://mcs.mediatek.com/resources/latest/tutorial/setting_notification
[mcs-profile]: https://mcs.mediatek.com/v2console/console/profile

[sdk-github-push]: https://github.com/Mediatek-Cloud/MCS-Android-SDK#b-setup-with-push-installation
[sdk-api-notif]: https://mtk-mcs.gitbooks.io/mcs-android-sdk-api-reference/content/push_notifications.html

[gcm]: https://developers.google.com/cloud-messaging/

[img-mobiles]: https://img.mediatek.com/1500/mtk.linkit/mcs-resources/en/2.12.5/Trigger/img_trigger_06.png