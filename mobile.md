Mobile Application Pentesting Checklist
## Preparation
### Tools:
- Jadx
- Apktool
- APKid
- Ghidra
- Android Debug Bridge
	- Useful commands:
		- `adb pull <file-from-device> <local-device-location>`
		- `adb shell pm list packages | grep <app-name>`
		- `adb pull /data/data/com.example.apk`
- Oversecured app scanner

### Emulators:
- Genymotion
- Android Studio virtual device
- Physical device (rooted/non-rooted)

## Recon & Analysis
#### Common App Frameworks | Languages Used
- Native Applications
	- Android
		- Java, Kotlin
		- Android NDK: C, C++
	- iOS
		- Objective-C, Swift
- Hybrid Applications
	- Xamarin: C#
		- decompile application libraries using dnspy or other c# decompilers
	- React Native, Cordova: Javascript
		- check for `index.android.bundle` to view the source code

#### Static Analysis
- [ ] Check for sensitive strings (API keys, hardcoded credentials)
		- [ ] res/strings.xml
- [ ] Source code review
	- [ ] Apps which handle sensitive information should have integrity checks, check if device is rooted
	- [ ] vulnerable/weak hash and cryptographic algorithms
	- [ ] Check for anti-debugging/anti-hooking techniques
- [ ] Review AndroidManifest.xml to view which components are defined
	- [ ] Dangerous permissions
	- [ ] Android api level, sdk version used
- [ ] Reverse engineer native libraries. 
	- System.LoadLibrary(“name-of-library”)
- [ ] Is the application obfuscated?
- [ ] Dynamic code loading through .dex files
- [ ] Does the app contain a network security policy?
- [ ] Does the app create any new files when being installed?
- [ ] Firebase integration, check strings.xml for firebase url
	- [ ] Is `app-firebase-url.io/.json` accessible?

#### Dynamic Analysis
- [ ] Bypass SSL pinning to intercept requests sent by the application
- [ ] Monitor logs/clipboard for data leakage, debug statements
- [ ] Hook, call arbitrary methods using frida
- [ ] Connect to device shell
	- [ ] `adb shell` / `adb root`
	- [ ] View shared preferences, sqlite databases
- [ ] Patch an application
	- Decompile the apk using apktool: `apktool d app.apk`
	- Edit the .smali code that you want to patch
	- Recompile with: `apktool b app/ -o app-recompiled.apk`
	- Sign the apk with your custom keystore: `jarsigner -sigalg SHA1withRSA -digestalg SHA1 -keystore mycustom.keystore -storepass mystorepass app-recompiled.apk mykeyaliasname`
	- Zipalign: `zipalign 4 app-recompiled.apk app-final.apk`

## Vulnerability Checklist: Android
- [ ] https://oversecured.com/vulnerabilities
- [ ] android:debuggable=”true”
- [ ] android:allowBackups=”true”
	- [ ] `adb backup -f backup.ab <app.apk> ; dd if=backup.ab bs=24 skip=1 | python3 -c "import zlib,sys;sys.stdout.buffer.write(zlib.decompress(sys.stdin.buffer.read()))" > backup.tar ; tar -xvf backup.tar`
- Exported Components:
- [ ] Activities: if an activity with sensitive information is exported you could bypass the authentication mechanisms to access it.
	- `adb shell am start -n com.example.demo/com.example.ExportedActivity --es extra_key “extra_value”`
- [ ] Services
- [ ] Broadcast Receivers
- [ ] Content Providers
- [ ] Insecure deep links
	- [ ] Check android manifest for activities that have intent filters which accept data/scheme fields
	- `adb shell am start -a android.intent.action.VIEW -d "scheme://hostname/path?param=value" [your.package.name]`
- [ ] Remote Code Execution | Arbitrary Code Execution
- [ ] Interception of implicit intents
- [ ] Insecure use of http
- [ ] Insecure Data Storage
	- [ ] Stored files stored in world-readable | world-writable locations
- [ ] Lack of integrity/tampering checks for dynamically loaded code, native libraries
	- If there is an integrity check, it might be vulnerable to race condition
	- If no integrity checks, try man-in-the-disk attacks
- [ ] Access to arbitrary files, leads to theft
- [ ] Insecure broadcasts (which may have sensitive information) can be intercepted by system-wide broadcast receivers
- [ ] Vulnerable Webviews
	- setJavascriptEnabled: may lead to javascript code exec if parsing user-provided input
- [ ] Zip Directory Traversal
- [ ] Exposed notification controller
- [ ] Easily bypassable host checks
- [ ] Exposed ports
- [ ] Weak 2FA
	- [ ] Bruteforceable pins
## Vulnerability Checklist: iOS
TODO: add stuff

## API security
- [ ] Insecure data object reference
- [ ] Improper authorization checks/bypasses
- [ ] Race conditions
- [ ] Information exposure
- [ ] Lack of ratelimit on authentication endpoints
## Android Bug Reports 
- https://github.com/B3nac/Android-Reports-and-Resources
