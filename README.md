# Cordova Plugin of Kii Cloud

This plugin uses Kii Cloud JavaScript SDK. For the detail, please confirm  the [Guide](http://documentation.kii.com/en/guides/javascript/) documentation.

## Add plugin in to Cordova project

### Configure Cordova environment

Please check the official site for the [Guild](https://cordova.apache.org/#getstarted)

### Add kiicloud-plugin

```
$ cordova plugin add https://github.com/KiiCorp/monaca-plugin-dev#cordova
```

## Usage

After added this plugin, you can use Kii Cloud features in your project.

This plugin can enable your project to handle GCM notification of Android and APNS of iOS with Kii Cloud. For the detail of using push notification in Kii Cloud, please confirm the [Guild](http://documentation.kii.com/en/starts/cloudsdk/managing-push-notification/).

### Initialize Kii Cloud

```
var kii
document.addEventListener('deviceready', function () {
  // The top level apis of Kii Cloud are under kii scope
  // ex. To use KiiUser or KiiSite, you should use kii.KiiUser or kii.KiiSite
  kii = window.kii.create();

  // Replace APP_ID, APP_KEY, and APP_SITE with correct ones.
  kii.Kii.initializeWithSite(APP_ID, APP_KEY, APP_SITE);

  // Setup GCM push for Android App
  // 1st parameter: SENDER_ID of GCM.
  // 2nd parameter: name of callback function when received GCM notification.
  // 3rd parameter: success callback, fail callback, and settings for the notification in background.

  // replace the sender_id with appropriate value
  window.kiiPush.initAndroid("sender_id", "pushReceived", {
    user: {
      ledColor: "#FFFF00FF",
      notificatonText: "user"
    },
    direct: {
      showInNotificationArea: true,
      ledColor: "#FFFFFFFF",
      notificatonTitle: "$.title",
      notificatonText: "$.msg"
    },
    success: function () {
      console.log('init done');
    },
    failure: function (msg) {
      console.log('error ' + msg);
    }
  });
});

// Register device(installation)
// 1st parameter: the object after called window.kii.create()
// 2nd parameter: name of callback function when received GCM message, success callback and failure callback
window.kiiPush.register(kii, {
  received: "pushReceived",
  success: function (token) {
    console.log('token=' + token);
  },
  failure: function (msg) {
    console.log('error ' + msg);
  }
});

function pushReceived(data) {
  // received JSON formated message,
}
```

## Sample

There is a [Sample project](https://github.com/KiiPlatform/cordova-plugin-sample).
