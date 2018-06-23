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

## Check Application's Signature

Download `Signal 4.21.6` from `https://signal.org/android/apk/`, unzip it, use the following command to check its signature:


```
$keytool -printcert -file META-INF/CERT.RSA
Owner: CN=Whisper Systems, OU=Research and Development, O=Whisper Systems, L=Pittsburgh, ST=PA, C=US
Issuer: CN=Whisper Systems, OU=Research and Development, O=Whisper Systems, L=Pittsburgh, ST=PA, C=US
Serial number: 4bfbebba
Valid from: Tue May 25 23:24:42 CST 2010 until: Tue May 16 23:24:42 CST 2045
Certificate fingerprints:
	 MD5:  D9:0D:B3:64:E3:2F:A3:A7:BD:A4:C2:90:FB:65:E3:10
	 SHA1: 45:98:9D:C9:AD:87:28:C2:AA:9A:82:FA:55:50:3E:34:A8:87:93:74
	 SHA256: 29:F3:4E:5F:27:F2:11:B4:24:BC:5B:F9:D6:71:62:C0:EA:FB:A2:DA:35:AF:35:C1:64:16:FC:44:62:76:BA:26
Signature algorithm name: SHA1withRSA
Subject Public Key Algorithm: 1024-bit RSA key
Version: 3
```


## Todo

 - Find the reason why when I attemp record video with front camera, there is a toast say "Could not start media recorder. Can't start video recording.".
