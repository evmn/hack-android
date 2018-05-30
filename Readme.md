# Hack Android



- [Record Internal Audio](https://github.com/evmn/hack-android/tree/master/Record_Internal_Audio)

 
## Stop captive portal detection

This command work on my LineageOS 15.1:

```
adb shell settings put global captive_portal_mode 0
adb shell reboot
```

Some use another command, but that doesn't work on my phone:

```
adb shell settings put global captive_portal_detection_enabled 0
```


## Todo

 - Find the reason why when I attemp record video with front camera, there is a toast say "Could not start media recorder. Can't start video recording.".
