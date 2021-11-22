>
> This content is deprecated. See the latest version of this tutorial [here](https://docs.airnativeextensions.com/docs/tutorials/device-logs)
> 

# Access the Device Logs

Getting access to the device logs often yields additional information when attempting to isolate an issue with an ANE. We may sometimes ask for you to provide these logs in order for us to debug an issue or isolate a problem.

## Android

On Android it is relatively simple to get the device logs. The debugger command line utility (`adb`/`adb.exe`) can be used to output the logs along with installing your application and is packaged as part of AIR at `AIRSDK/lib/android/bin`. If you have installed the Android SDK you can also use the version from there.

> Note: It is important you have set your device into debug mode.

### macOS (OSX)

To access the logs:

- open a Terminal
- run `adb logcat`

You can get more relevant information by using grep to filter the logs. For example, we often use the following:

```
adb logcat | grep -E --color=always 'distriqt|AndroidRuntime|System.err'
```


### Windows

To access the logs:

- open a Command Prompt window
- run `AIRSDK\lib\android\bin\adb.exe logcat`



## iOS

On iOS, there are 2 types of logs, "Device Logs" and the "Console". The device logs are crash reports and generally aren't very useful to us unless we can resymbolcate your application. However the console is quite helpful and contains debugging information output from the ANE's.

### macOS (OSX)

iOS has changed its logging method in iOS 10 so there are 2 ways to retrieve the logs depending on the version of iOS you have on your device.

#### iOS < 10

To get access to the console:

- Open Xcode
- Navigate to Window / Devices
    - click on your device in the left panel
    - ignore the "Device Logs" button (this is the crash reports mentioned)
    - click the small triangle in the bottom left of the right panel to expand the 'console log'
    - copy and paste this log when an issue occurs


#### iOS 10 +

To access the console open the **Console** application and select the device in the left hand panel.

You can use the filter to search the logs for any relevant information. 


#### libimobiledevice

Using these tools you can do a range of iOS device related tasks, such as installing an application and accessing the device logs.

Firstly install the tools using homebrew:

```
brew install --HEAD usbmuxd
brew install --HEAD libimobiledevice  
brew install --HEAD ideviceinstaller  
```

Then in a terminal type:

```
idevicesyslog
```

[Reference](http://www.libimobiledevice.org/)



### Windows

It is possible to get the logs on a Windows machine. There are several methods that we have found to be reliable, using iTunes and the other using a program called iTools.

If you know of a better way please let us know and we will add it to this answer.


#### iOSLogInfo Tool

Using this tool from Blackberry appears to be the best option for Windows developers. 

Requires the [iOSLogInfo tool](https://www.blackberry.com/blackberrytraining/web/KB_Resources/KB36986_iOSLogInfo_4.3.4.zip) and iTunes installed.

Steps:

- Download and save the iOSLoginfo zip download by clicking [here](https://www.blackberry.com/blackberrytraining/web/KB_Resources/KB36986_iOSLogInfo_4.3.4.zip).
- Extract the zip file contents.
- Ensure iTunes for Windows is installed.
- Connect your iOS device to a PC.
- From a command prompt navigate to the location you extracted the above contents and run one of the following to start a live log collection session:

```
sdsiosloginfo.exe -d
```

OR to write the contents to a log file run:
 
```
sdsiosloginfo.exe -d > c:\ios_log.log
```

[Reference](http://support.blackberry.com/kb/articleDetail?articleNumber=000036986)



#### iTools

It is possible to get the device logs on a Windows machine using a program called iTools. You will need to get a copy of this program, be aware that there are spyware versions of this program so don't just download any version of it.

We have had users have success with the version from this Chinese website (an english version is available): http://pro.itools.cn/.

We in no way endorse or accept any responsibility for using this program, use at your own risk.



#### iTunes

The crash logs are transferred to iTunes everytime you sync your device. These aren't as helpful as the device logs but can yield information about an issue in some circumstances. To start plugin your device, launch iTunes and start the synchronisation process.

The logs are copied to your file system which you can locate using the following:

- Open Explorer
- Enter %appdata%
- Navigate to : `Roaming\Apple computer\Logs\CrashReporter\Mobile Device\DEVICENAME`

You should see a list of log files starting with the name of the application, locate your application name and open the log


#### libimobiledevice

These tools should work on Windows as well however we haven't tested this option:

[Reference](http://www.libimobiledevice.org/)


## Disclaimer

Any links to third-party software available on this website are provided “as is” without warranty of any kind, either expressed or implied and such software is to be used at your own risk.

