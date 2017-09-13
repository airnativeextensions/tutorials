

This document contains notes on integration with AIR of various versions of the iOS platform. 
This is normally required during the phase where AIR support for a platform has not yet been released.


## iOS 11 

**12th September 2017**

Here we detail the usage of the iOS 11 SDK with AIR 27 beta. Using this SDK is required to use any iOS 11 features such as InAppBilling Promotions. 

>
> This is experimental and we provide no guarantee on the following process but it has worked for our needs
>



### iOS 11 SDK

#### Download

Firstly you will need to download the iOS 11 SDK and slightly modify it to suit AIR.

This can be done by downloading Xcode (GM seed at time of publishing) and installing as per the normal process.

Copy the libgcc_s.1 library from 10.3. This library seems to be removed from 11 and is used by AIR currently. 

```
cp iPhoneOS10.3.sdk/usr/lib/libgcc_s.1.tbd iPhoneOS11.0.sdk/usr/lib/.
```

We have logged a bug for this issue here: https://tracker.adobe.com/#/view/AIR-4198461 
Please vote to have the issue resolved.

We also have a direct download of the SDK with the above modifications available [here](http://resources.airnativeextensions.com/ios/iPhoneOS11.0.sdk.modified.zip)



### AIR SDK

Download the AIR 27 beta SDK

You will need to update the linker to use the latest linker from Xcode: 

```
cp /Applications/Xcode-beta.app/Contents/Developer/usr/bin/ld AIRSDK/lib/aot/bin/ld64/ld64
```

This is currently only possible for macOS builds



### Usage

Once you have performed the above you can use the iOS 11 SDK to package your applications. 
Ensure you are:

- Using the AIR 27 beta SDK
- Using the modified iOS 11 SDK via the platform sdk packaging option

















