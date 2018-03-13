# Android Splash Screen

Android doesn't have a built in method of displaying an image at launch like iOS does however there are some techniques we can use to achieve a similar goal.

![](images/android-splashscreen.png)



## Modifying the AIR SDK

The simplest method to give your AIR application a splash screen on Android is to change the theme of the main AIR activity to contain an image and a background colour.

This method generally requires no modification to your application code, however you will have to modify your version of the AIR SDK.

To get started open up the AIR SDK `styles.xml` file located at `AIRSDK/lib/android/lib/resources/app_entry/res/values/styles.xml`

This file should contain the following:

```xml
<resources>
    <style name="Theme.NoShadow" parent="android:style/Theme.NoTitleBar">
        <item name="android:windowContentOverlay">@null</item>
    </style>
</resources>
```

To start, remove `android:windowContentOverlay` and add a `android:windowBackground` as below:

```xml
<resources>
    <style name="Theme.NoShadow" parent="android:style/Theme.NoTitleBar">
        <item name="android:windowBackground">@drawable/splash_background</item>
    </style>
</resources>
```



This will set a `splash_background` resource as the background. Lets create this resource by adding a `splash_background.xml` file at the following location: `AIRSDK/lib/android/lib/resources/app_entry/res/drawable/splash_background.xml`

This is where we will create the background for your application which will appear as the splasg screen.



### Centered Application Icon

A simple splash screen is to use your application icon and center it in the view with a background colour. 

Add the following content to the `splash_background.xml` file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@color/splash_background_color" />
    <item>
        <bitmap
            android:gravity="center"
            android:src="@mipmap/icon" />
    </item>
</layer-list>
```

We will define the background colour in the `colors.xml` located at `AIRSDK/lib/android/lib/resources/app_entry/res/values/colors.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="splash_background_color">#ff0000</color>
</resources>
```

Change this background colour to suit your application.


Once you've made these changes to the AIR SDK any Android application that is built with this AIR SDK will display your splash screen.



### Image

Alternatively if you want to use a custom image as the splash screen you can add an image file to the resources to use as the background. 

For example lets add a `splash_image.png` to the drawable directory (alongside `splash_background.xml`), you then need to modify `splash_background.xml` to use this image as below:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <bitmap 
            android:gravity="center"
            android:src="@drawable/splash_image" />
    </item>
</layer-list>
```




### Reverting

To revert your changes you simply revert the `styles.xml` to the original contents above.


