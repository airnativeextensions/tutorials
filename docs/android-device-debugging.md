>
> This content is deprecated. See the latest version of this tutorial [here](https://docs.airnativeextensions.com/docs/tutorials/android-device-debugging)
> 

# Device Debugging

## Android

In order to debug an application on an Android device you need to enable "Developer Mode" and then enable USB debugging.

Enabling developer mode opens up an additional settings menu that allows you to enable different development options.

To enable "Developer mode":

- Go to the settings menu 
- Open "System" and then "About phone"
- Tap **Build number** several times and you should see a dialog that says "you're X taps away from being a developer".
- Continue tapping the build number until the developer mode is enabled. 

To enable USB debuging:

- Go to the settings menu
- Open "System" and then the new "Developer options" menu
- Make sure "USB debugging" is enabled.


You will now be able to connect a usb cable to your device and use the `adb` tool to install applications (APKs) and access the device logs. 








