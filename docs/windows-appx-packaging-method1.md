
# Windows Store Packaging: Method 1

This method gives the most versatility in editing the final package you create, giving you the ability to make changes to the AppxManifest. Additionally it simplifies testing as you only need to install a single test certificate on your machine.

This method involves the following steps:

- [Convert your AIR app to Windows package format (APPX/MSIX)](#converting-air-to-appx-package)
    - [Make any changes to content](#update-content)
- [Package into APPX](#package-into-appx)
- [Sign](#sign)
    - [Create a certificate](#create-a-certificate)
    - [Sign APPX/MSIX](#sign-appx)


## Converting AIR to APPX package

This is the first step of the packaging process across all the methods and is the most important thing the `DesktopAppConverter` does. It involves taking the application and converting it into the correct structure for a Windows package.

The result we are looking for here is a directory called `PackagedFiles` that contains the eventual content of your `appx` package.

```
DesktopAppConverter.exe 
    -Installer [PATH\TO\YOUR\AIR\OUTPUT] 
    -AppExecutable YourApplication`.exe 
    -Destination [PATH\TO\OUTPUT] 
    -PackageName "[Package/Identity/Name]" 
    -PackageDisplayName "[DISPLAYNAME]" 
    -Publisher "CN=[Package/Identity/Publisher]"
    -PackagePublisherDisplayName "[Package/Properties/PublisherDisplayName]" 
    -AppDisplayName "[DISPLAYNAME]" 
    -Version [VERSION] 
```

This is very similar to the command used in method 2, except we are removing the `-MakeAppx` and `-Sign` options.




## Update Content

At this point you can modify the content of the package before packaging it into the `appx` package.

Most commonly you may need to add items into the `AppxManifest.xml`.

We normally copy the first version of this file to another location and edit it as we require, then copy this file back into the `PackagedFiles` directory in our build script.



## Package into APPX

Now that you have your package structure to convert this into an `appx` file we use the `MakeAppx` tool.

*This is the equivalent of the `-MakeAppx` option in the `DesktopAppConverter` tool*

https://docs.microsoft.com/en-us/windows/msix/package/create-app-package-with-makeappx-tool


This tool creates application packages.

```
"C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\makeappx.exe" pack ^
/d [PATH\TO\PackageFiles\] ^
/p YourApplication.appx 
```

eg: 

```
"C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\makeappx.exe" pack ^
/d C:\build\distriqt.airnativeextensions\PackageFiles\ ^
/p distriqt.airnativeextensions.appx 
```





## Sign

You will need a `pfx` certificate file in order to sign your `appx` file. 



### Create a certificate

You will need to create a certificate 

For testing we suggest creating a self-signed certificate which you can use for testing your application locally.

https://docs.microsoft.com/en-us/windows/msix/package/create-certificate-package-signing


For example. to create a test certificate:

```
New-SelfSignedCertificate -Type Custom `
-Subject "CN=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" `
-KeyUsage DigitalSignature `
-FriendlyName "distriqt Pty Ltd" `
-CertStoreLocation "Cert:\CurrentUser\My" `
-TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

>
> Note: It is important that the CN (common name) matches the one used in the conversion of your AIR app to APPX. 
>


This will output a thumbprint which you use to export the certificate:

```
$password = ConvertTo-SecureString -String YOUR_CERT_PASSWORD -Force -AsPlainText
Export-PfxCertificate -cert "Cert:\CurrentUser\My\YOUR_CERT_THUMBPRINT" -FilePath certificate.pfx -Password $password
```


For store release you will need to use the certificate setup for store distribution.


At the end of this process you should have a `pfx` certifcate file which you will use to sign your app.


If you are using a test certificate, at this point you need to install the certificate onto your machine. 

To install the certificate:

    - Double click the `pfx` file
    - Select "Local Machine" for the store location
    - Check the certificate path
    - Enter your certificates password
    - Select "Place all certificates in the following store"
        - "Browse"
        - "Trusted Root Certification Authorities"

If you don't install the certificate you will get an error about the signature when attempting to install your appx.


### Sign APPX

In order to sign the `appx` file generated earlier you will use the `SignTool`.

*This is the equivalent of the `-Sign` option in the `DesktopAppConverter` tool*

https://docs.microsoft.com/en-us/windows/msix/package/sign-app-package-using-signtool


For example:

```
SignTool sign /fd SHA256 /a /f certificate.pfx /p <Your Password> <File path>.appx
```

You will now have a signed `appx` application package that you can use for test installation or store release (depending on the certificate used). 






## Example BAT


The following is just some pseudo commands demonstrating how you could automate the above process into a batch file. 


```bat
@ECHO OFF
SET BASEDIR=C:\path\to\project
SET AIR_OUTDIR=%BASEDIR%\out\air\build
SET UWP_OUTDIR=%BASEDIR%\out\uwp\build


ECHO ===========================================================
ECHO Converting to UWP application

MKDIR %UWP_BUILDDIR%

DesktopAppConverter.exe -Installer %AIR_BUILDDIR%\ ^
-AppExecutable AirApplication.exe ^
-InstallerArguments "/quiet" ^
-Destination %UWP_OUTDIR% ^
-PackageName "distriqt.airnativeextensions" ^
-PackagePublisherDisplayName "distriqt" ^
-Publisher "CN=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" ^
-Version 1.0.0.0 


ECHO ===========================================================
ECHO Updating Manifest

copy AppxManifest.xml %UWP_OUTDIR%\distriqt.airnativeextensions\PackageFiles\AppxManifest.xml


ECHO ===========================================================
ECHO Creating Appx

del distriqt.airnativeextensions.appx

"C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\makeappx.exe" pack ^
/d %UWP_OUTDIR%\distriqt.airnativeextensions\PackageFiles\ ^
/p distriqt.airnativeextensions.appx 


ECHO ===========================================================
ECHO Signing Appx

"C:\Program Files (x86)\Windows Kits\10\bin\10.0.18362.0\x64\signtool.exe" sign /fd SHA256 ^
/a /f distriqt_selfsigned.pfx /p "p4ssword" ^
distriqt.airnativeextensions.appx
```


>
> Note: it is possible to automate the removal and install of the appx package for testing as well. 
>
> Have a look at the `Remove-AppxPackage` and `Add-AppxPackage` powershell commands.
> 


