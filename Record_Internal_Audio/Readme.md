# Record Internal Audio


 - Download *LineageOS 15.1* source code

 - Modify framework and application code

 - Compile Test

## Key Directorys

 - **A**: `frameworks/av/services/audiopolicy/`

 - **B**: `packages/apps/Recorder/`
 


## Add two lines in Engine.cpp 


```diff
--- a/services/audiopolicy/enginedefault/src/Engine.cpp
+++ b/services/audiopolicy/enginedefault/src/Engine.cpp
@@ -482,6 +482,8 @@ audio_devices_t Engine::getDeviceForStrategyInt(routing_strategy strategy,
             if (availableOutputDevices.getDevice(AUDIO_DEVICE_OUT_REMOTE_SUBMIX,
                                                  String8("0")) != 0) {
                 device2 = availableOutputDevices.types() & AUDIO_DEVICE_OUT_REMOTE_SUBMIX;
+                device2 |= (availableOutputDevices.types() & AUDIO_DEVICE_OUT_WIRED_HEADPHONE);
+                device2 |= (availableOutputDevices.types() & AUDIO_DEVICE_OUT_SPEAKER);
             }
         }
         if (isInCall() && (strategy == STRATEGY_MEDIA)) {
```

The change will make the audio output to `WIRED_HEADPHONE`(if headphone is plug in) or `SPEAKER` when output to `REMOTE_SUBMIX`.


## Change code in *Recorder* application

Add a line to *AndroidManifest.xml*.

```diff
--- a/app/src/main/AndroidManifest.xml
+++ b/app/src/main/AndroidManifest.xml
@@ -15,7 +15,8 @@
 -->
 <manifest xmlns:android="http://schemas.android.com/apk/res/android"
     xmlns:tools="http://schemas.android.com/tools"
-    package="org.lineageos.recorder">
+    package="org.lineageos.recorder"
+    android:sharedUserId="android.uid.system">
 
     <uses-permission android:name="android.permission.CAPTURE_SECURE_VIDEO_OUTPUT" />
     <uses-permission android:name="android.permission.CAPTURE_VIDEO_OUTPUT" />
```

Then change a line in `RecordingDevice.java`, this will change audio source from **microphone** to **remote submix**:


```diff
--- a/app/src/main/java/org/lineageos/recorder/screen/RecordingDevice.java
+++ b/app/src/main/java/org/lineageos/recorder/screen/RecordingDevice.java
@@ -160,7 +160,7 @@ class RecordingDevice extends EncoderDevice {
                 bufferSize = ((minBufferSize / 1024) + 1) * 1024 * 2;
             }
             Log.i(LOGTAG, "AudioRecorder init");
-            record = new AudioRecord(MediaRecorder.AudioSource.MIC, 44100,
+            record = new AudioRecord(MediaRecorder.AudioSource.REMOTE_SUBMIX, 44100,
                     AudioFormat.CHANNEL_IN_MONO, AudioFormat.ENCODING_PCM_16BIT, bufferSize);
         }
```



|AudioSource|Description|
|:-|:-|
|MIC|Microphone audio source |
|EMOTE_SUBMIX|Audio source for a submix of audio streams to be presented remotely. An application can use this audio source to capture a mix of audio streams that should be transmitted to a remote receiver such as a Wifi display. While recording is active, these audio streams are redirected to the remote submix instead of being played on the device speaker or headset. Certain streams are excluded from the remote submix, including `STREAM_RING`, `STREAM_ALARM`, and `STREAM_NOTIFICATION`. These streams will continue to be presented locally as usual. Capturing the remote submix audio requires the `CAPTURE_AUDIO_OUTPUT` permission. This permission is reserved for use by system components and is not available to third-party applications. Requires the `CAPTURE_AUDIO_OUTPUT` permission.|





# Reference

 - [**Google**: Audio Terminology](https://source.android.com/devices/audio/terminology)

 - [**Google**: MediaRecorder.AudioSource](https://developer.android.com/reference/android/media/MediaRecorder.AudioSource)
