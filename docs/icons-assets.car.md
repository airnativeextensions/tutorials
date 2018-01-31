
# Icons and the Assets.car file

From iOS 11 Apple now requires a new process of adding icons to your application, you can no longer simply package them as you have done with previous versions of iOS and AIR. Instead you need to create an "Assets.car" file and package in the root directory of your application.

>
> Note: The Assets.car file is need for iOS 11+ when you are using AIR SDK v28+
>


## Method 1: Online tool

This is the simplest way if you are a Windows developer. 

Simply go to the following url:

[http://applicationloader.net/appuploader/icontool.php](http://applicationloader.net/appuploader/icontool.php)

and upload a 1024x1024 image of your icon. You will get a zip download containing the `Assets.car` file and all the icon sizes needed to embed in your iOS application. 




## Method 2: Command Line

>
> Note: You will need a macOS machine with Xcode 9+ for this method to generate the Assets.car file
>

This is the method we prefer as it's simpler to update and create than having to drag files into Xcode. 

It uses the same directory structure (`Assets.xcassets`) as in your Xcode application however uses the command line to convert this into the `Assets.car`, so you can simply replace the files in the directory and run the script to create your `Assets.car`. 

- Download the following zip and extract it to a working directory somewhere on your machine.
  - [assets-car-build.zip](resources/ios/assets-car-build.zip)
  - You should find a script and a directory called `Assets.xcassets` which contains another directory called `AppIcon.appiconset`.

- Replace all the images in the `AppIcon.appiconset` directory with your own icons. You must replace them with the correctly sized images. You can do this manually or we suggest you use an online tool: [appicon.build](https://www.appicon.build/). This tool will generate all the icons at the correct sizes from a single 1024x1024 image.

- Run the `createAssetsCar` script. This will create a `build` directory and run the following command to build the `Assets.car` file:

```
xcrun actool Assets.xcassets --compile build --platform iphoneos --minimum-deployment-target 8.0 --app-icon AppIcon --output-partial-info-plist build/partial.plist
```



## Method 3: Using Xcode

>
> Note: You will need a macOS machine with Xcode 9+ for this method to generate the Assets.car file
>

Firstly you will need to open Xcode and create a new application

- Start a new project and select the "Single View App" under the iOS section (or tvOS section if you are creating this file for a tvOS application).

![](images/ios-assets-car-xcode-1.png)

- Fill Product Name, Organization Name and Organization Identifier (no specific names required).

![](images/ios-assets-car-xcode-2.png)

- Save the project 
- In the left hand panel select the `Assets.xcassets` file

![](images/ios-assets-car-xcode-3.png)

- Select the `AppIcon`

![](images/ios-assets-car-xcode-4.png)


- Add all the required versions of the AppIcon

![](images/ios-assets-car-xcode-5.png)


- Build the project ( Product -> Build).
- Right-click on your ‘.app’ -> Show in finder.
- Right click on your ‘.app’ -> Show package contents.
- Now copy `Assets.car` and package with AIR application.






## Packaging Assets.car

The `Assets.car` file must be placed at the root of your application, this means alongside your `swf` and other application content. 

You do this by ensuring that it is in the root of your applications source and selected as a single file in your application package.



## Supporting previous versions of iOS

You must also make sure that you include the icons in your application using the icon tags in the application descriptor xml. 
This ensures that older versions of iOS still have the correct icons packaged and that other platforms still have the appropriate app icons.



## Launch images

Recently Apple changed the supported names of the files for the default / launch images. Make sure you have correctly added the default images according to the Adobe docs:

[http://blogs.adobe.com/airodynamics/2015/03/09/launch-images-on-ios-with-adobe-air/](http://blogs.adobe.com/airodynamics/2015/03/09/launch-images-on-ios-with-adobe-air/)


