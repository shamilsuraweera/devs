# Android Emulator — COMPLETE CLI GUIDE (Linux / Ubuntu)

Single, full, copy-pasteable README.
CLI only. NOT Flutter-specific.
Works for native Android, Flutter, React Native, CI, APK debugging.

==================================================
1. SYSTEM ASSUMPTIONS
==================================================

- OS: Ubuntu 20.04+ (tested on 22.04 / 24.04)
- CPU virtualization enabled (AMD-V / Intel VT-x)
- Android SDK installed
- Android Emulator installed
- Platform Tools (adb) installed
- At least one AVD already created

Default SDK path used everywhere:

$HOME/Android/Sdk

==================================================
2. ENVIRONMENT VARIABLES (MANDATORY)
==================================================

Run EVERY new terminal session:

export ANDROID_SDK_ROOT="$HOME/Android/Sdk"
export ANDROID_HOME="$HOME/Android/Sdk"
export PATH="$ANDROID_SDK_ROOT/emulator:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$PATH"

Verify tools:

emulator -version
adb version
sdkmanager --version

If any command fails → PATH or SDK install is broken.

==================================================
3. LIST AVAILABLE EMULATORS (AVDs)
==================================================

emulator -list-avds

Example output:

pixel_light
pixel_api_33
test_phone

Use names EXACTLY as listed.

==================================================
4. START EMULATOR (FAST BOOT / SNAPSHOT)
==================================================

Uses saved snapshot (fastest startup):

emulator -avd pixel_light

==================================================
5. START EMULATOR (COLD BOOT)
==================================================

Ignores snapshots.

Use when:
- UI glitches
- Theme / splash changes
- Random startup crashes

emulator -avd pixel_light -no-snapshot-load

==================================================
6. START EMULATOR (LOW RESOURCE / STABLE MODE)
==================================================

Use for:
- Low-RAM laptops
- Laggy UI
- GPU problems

emulator -avd pixel_light \
-gpu swiftshader_indirect \
-memory 1024 \
-cores 1 \
-no-snapshot \
-no-boot-anim \
-no-audio

==================================================
7. START EMULATOR (FACTORY RESET / WIPE DATA)
==================================================

WARNING: Deletes ALL emulator data.

Use when:
- Emulator corrupted
- APK install keeps failing
- System UI broken

emulator -avd pixel_light -wipe-data

==================================================
8. CHECK EMULATOR CONNECTION
==================================================

adb devices

Expected:

emulator-5554	device

If offline → wait or restart emulator.

==================================================
9. KILL EMULATOR (GRACEFUL SHUTDOWN)
==================================================

adb emu kill

==================================================
10. RESTART ADB (FIX DETECTION ISSUES)
==================================================

adb kill-server
adb start-server
adb devices

==================================================
11. INSTALL APK MANUALLY
==================================================

adb install app-debug.apk

Reinstall / overwrite:

adb install -r app-debug.apk

==================================================
12. UNINSTALL APP
==================================================

adb uninstall com.example.app

==================================================
13. VIEW LOGS (LOGCAT)
==================================================

All logs:

adb logcat

Only errors:

adb logcat *:E

Clear logs:

adb logcat -c

==================================================
14. PERFORMANCE & STABILITY TIPS
==================================================

- Prefer x86_64 system images
- Disable animations in emulator:
  Developer Options → Animator duration scale → OFF
- Always use -no-boot-anim
- Never run multiple emulators on low RAM
- SwiftShader fixes most Linux GPU issues

==================================================
15. COMMON PROBLEMS & FIXES
==================================================

Emulator won’t start:
- Enable virtualization in BIOS
- Cold boot
- Wipe data

Black screen / invisible UI:
emulator -avd pixel_light -gpu swiftshader_indirect

Device not detected:
adb kill-server
adb start-server
adb devices

Emulator freezes:
- Reduce RAM / cores
- Kill emulator
- Cold boot

==================================================
16. TYPICAL DAILY CLI WORKFLOW
==================================================

export ANDROID_SDK_ROOT="$HOME/Android/Sdk"
export PATH="$ANDROID_SDK_ROOT/emulator:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$PATH"

emulator -avd pixel_light -no-snapshot-load
adb devices

==================================================
17. IMPORTANT NOTES
==================================================

- Emulator is NOT Flutter-specific
- Used by:
  - Android Studio
  - Flutter
  - React Native
  - APK testing
  - CI pipelines
- Flutter only wraps adb + emulator

==================================================
18. QUICK COMMAND REFERENCE
==================================================

emulator -list-avds
emulator -avd pixel_light
emulator -avd pixel_light -no-snapshot-load
emulator -avd pixel_light -wipe-data
adb devices
adb emu kill
adb logcat
