# Cordova Plugin of Kii Cloud

This is a Cordova 4.1 plugin of Kii Cloud. Can be imported to your [Monaca](https://monaca.io/) projects.

This plugin uses Kii Cloud JavaScript SDK. For the detail, please confirm  the [Guide](http://documentation.kii.com/en/guides/javascript/) documentation.

## Import to Monaca project

After created a new project. You need to import this plugin.

1. Clone source code of this repository.

  **Note**: The Kii Cloud JavaScript SDK is not updated automatically in this repository. To use the latest version of SDK, please download from [Kii Cloud](https://developer.kii.com/v2/downloads). Then replace the library file `www/kiisdk.js` with the downloaded JS file.

2. Compress `plugin.xml`, `src` folder and `www` folder, which are located under root folder, to a zip file.
  ```
  $ zip -r monaca.zip plugin.xml src www
  ```
3. Import the plugin to Monaca.
  * Click `File` -> `Manage Cordova Plugins`
  * On the Cordova Plugins page, click `Import Cordova Plugin` button, then select the plugin zip file(`monaca.zip`) generated in step 2.

  **Note**: If you would like to update the plugin, remove the old plugin, then import new one.

## Usage

After imported this plugin in Monaca, you can use Kii Cloud features in your project.

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
  window.kiiPush.initAndroid("125448119133", "pushReceived", {
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

## Limitation of Push

Push for iOS app only works with Release Build.

## Sample

There is a [Monca sample project](https://github.com/KiiPlatform/monaca-plugin-sample).
