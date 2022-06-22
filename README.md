# android-permission-findings

Runtime Permission is introduced in Android 6 Marshmallow (API 23). So if it's below API 23, we don't have to ask for permission in-app, just declare in manifest.

Therefore, we need to add `Build.VERSION.SDK_INT >= Build.VERSION_CODES.M` checks each time we ask for permissions.


| Android 10 (API 29) or below FIRST TIME| Android 10 (API 29) or below SUBSEQUENT TIME | 
|:---:|:---:|
|<img width="436" alt="image" src="https://user-images.githubusercontent.com/35066207/174816982-45393c56-83e8-4421-bc15-51fb3868cc02.png">|<img width="440" alt="image" src="https://user-images.githubusercontent.com/35066207/174808010-2fb36e37-81c4-4c70-b444-86404be56ab9.png">

|Android 11 (API 30) or above |
|:---:|
 |<img width="391" alt="image" src="https://user-images.githubusercontent.com/35066207/174808326-09188586-1161-41bb-8598-f840112934b5.png">|

Starting in Android 11, if the user [taps Deny for a specific permission more than once](https://developer.android.com/about/versions/11/privacy/permissions#dialog-visibility) during your app's lifetime of installation on a device, the user doesn't see the system permissions dialog if your app requests that permission again. The user's action implies "don't ask again." 

Before Android 11, users would see the system permissions dialog each time your app requested a permission, unless the user had previously selected a "don't ask again" checkbox or option.

```
shouldShowRationale <= API 29 (We assume user denies everytime)
1st time - false
2nd time - true
3rd time - false
4th time - false
```

```
shouldShowRationale >= API 30 (We assume user denies everytime)
1st time - false
2nd time - true
3rd time - N/A (OS popup will not be shown)
4th time - N/A (OS popup will not be shown)
```

`shouldShowRationale()` will still return false before you call `ActivityResultLauncher::launch()`. The only time it returns true is when you calling dialog the second time and user has denied during the first time.

[Android 11 location permission popup, users when clicks on "only this time" option, why OS not asking the permission again](https://stackoverflow.com/questions/66288530/android-11-location-permission-popup-users-when-clicks-on-only-this-time-opti)
