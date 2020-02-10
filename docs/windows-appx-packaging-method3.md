
# Windows Store Packaging: Method 3

**Using an Installer for your AIR application**

This method involves running the `DesktopAppConverter` using an installer application. 

You may already have an installer for your AIR application and wish to use this to create your Windows Store application (`appx`) or you can generate an installer using one of the many tools available.

You will have to install the certificate after each build to install the newly generated appx.


## Method

The output from your AIR application will be a directory, containing your `swf`, the `exe`, 
along with the AIR runtime, any ANEs, and all the other files packaged with your application.

You will then need to create an installer from this directory using one of the many windows 
installer tools, eg:

- [Wix](http://wixtoolset.org/)
    - [WixEdit](http://wixedit.sourceforge.net)
- [Inno Setup](http://www.jrsoftware.org/isinfo.php)


Tutorials:

- [Generating a Windows installer](http://www.adobe.com/devnet/air/articles/customize-setup-for-AIR-app-with-captive-runtime.html)



You now convert your AIR application installer to a Windows Store application (`appx`). It is important that you only have files related to your project in the directory with your installer (i.e. `C:\dac\installer` below) as anything in this directory will be copied to the virtual file system to run the conversion. 

To run the conversion, use the following command, replacing the [] items to match your setup:

```
DesktopAppConverter.exe 
    -Installer [PATH\TO\YOUR\AIR\INSTALLER] 
    -InstallerArguments "[INSTALLEROPTIONS]" 
    -Destination [PATH\TO\OUTPUT] 
    -PackageName "[Package/Identity/Name]" 
    -Publisher "CN=[Package/Identity/Publisher]" 
    -PackagePublisherDisplayName "[Package/Properties/PublisherDisplayName]" 
    -PackageDisplayName "[DISPLAYNAME]" 
    -AppDisplayName "[DISPLAYNAME]" 
    -Version [VERSION] 
    -MakeAppx 
    -Sign 
    -Verbose
```

- `-Installer` points to the installer that you want to use.
- `-InstallerArguments` is where you must specify the parameter that triggers the unattended setup procedure because DAC can only work with silent installers.
- `-Destination` is where the DAC should produce the output of the command.
- `-MakeAppx` creates an .appx file that is the installation format of a Windows Store application.
- `PackageDisplayName`: Display package name for the application
- `AppDisplayName`: Display name for the application
- `-Sign` generates a certificate that you can install on your computer to test the generated package. When the final package is submitted to the Store, it is automatically signed by a trusted certificate during the process, so there is no need to do it later when you are ready to publish your application.
- `-Verbose` gives you better information while the conversion is happening.


Your app has a unique identity, assigned by the Store. In order to submit your application you will need to add some parameters setting information about your application from the developer dashboard. Identity details that can be found in the **App identity** section of your application in the developer dashboard. This information is required to correctly package your application for store submission however can be dummy values for local testing:

- `PackageName`: Package/Identity/Name
- `Publisher`: Package/Identity/Publisher
- `PackagePublisherDisplayName`: Package/Properties/PublisherDisplayName	


For example with our msi generated using Wix:

```
DesktopAppConverter.exe 
    -Installer C:\dac\installer\TestDistriqt.msi
    -InstallerArguments "/quiet" 
    -Destination C:\dac\final
    -PackageName "distriqt.airnativeextensions" 
    -Publisher "CN=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" 
    -PackagePublisherDisplayName "distriqt" 
    -PackageDisplayName "airnativeextensions" 
    -AppDisplayName "airnativeextensions" 
    -Version 1.0.0.0 
    -MakeAppx 
    -Sign 
    -Verbose
 ```

 


## Testing the conversion

You can now install the `appx` application on your local machine. In order to do this you will need to install the certificate that was used to sign the application. This will called `auto-generated.cer` and be located in the output path alongside your `appx` application file. 


To install the certificate:

    - Double click cer file
    - Select "Install Certificate..."
    - Select "Local Machine"
    - Select "Place all certificates in the following store"
        - "Browse"
        - "Trusted Root Certification Authorities"

You can now install the application by double clicking the `appx` to launch the installer. This will install your application locally and have access to the UWP platform.

