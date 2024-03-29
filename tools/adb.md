Android Debug Bridge (ADB) is a development tool that facilitates communication between an Android device and a personal computer.

How view devices?
adb devices

How extract apk?
For this you need have installed the application in your device and know package name
adb shell pm path package_name

adb pull <remote> [<locaDestination>]
This command pulls the file remote to local. If local isn’t specified, it will pull to the current folder.