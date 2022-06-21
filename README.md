# android-permission-findings

| Android 10 (API 29) or below | Android 11 (API 30) or above |
|:---:|:---:|
|<img width="440" alt="image" src="https://user-images.githubusercontent.com/35066207/174808010-2fb36e37-81c4-4c70-b444-86404be56ab9.png">|<img width="391" alt="image" src="https://user-images.githubusercontent.com/35066207/174808326-09188586-1161-41bb-8598-f840112934b5.png">|

Starting in Android 11, if the user [taps Deny for a specific permission more than once](https://developer.android.com/about/versions/11/privacy/permissions#dialog-visibility) during your app's lifetime of installation on a device, the user doesn't see the system permissions dialog if your app requests that permission again. The user's action implies "don't ask again." 

Before Android 11, users would see the system permissions dialog each time your app requested a permission, unless the user had previously selected a "don't ask again" checkbox or option.

```
shouldShowRationale >= API 30 (We assume user denies everytime)
1st time - false
2nd time - true
3rd time - false
4th time - false
```
`shouldShowRationale()` will still return false before you call `ActivityResultLauncher::launch()`. The only time it returns true is when you calling dialog the second time and user has denied during the first time.
