---
published: true
---
Hey there! I think this is stupidity and waste of resources to install _whole Android Studio_.
So here I will explain how to in minutes **generate installable apk without Android Studio**.

> I use GaliumOS that based on **Ubuntu 16.04**

## Installing Java JDK
Go to [oracle official website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and install 8th version of Java Development Kit for you OS.  
If you are using **Fedora**/**Ubuntu** you can easily type `sudo apt-get install default-jdk` in a Terminal.

> To ensure you have properly installed jdk use `java -version`.   
If you still got nothing then you should set enviroment variables to do it https://docs.oracle.com/javase/tutorial/essential/environment/paths.html 

## Installing Android SDK tools
For this go to in the end of [official page](https://developer.android.com/studio/) (Command line tools only) and download right version of **sdk** for your OS.
Then just unzip it whenever you want and set environment variables to use android tools from any part of your system. To do it follow this steps (ubuntu/fedora):
1. Type `nano ~/.bashrc'
2. Add  following lines to the end of the file:  
`export ANDROID_HOME="/usr/lib/android-sdk/"`  
`export PATH="${PATH}:${ANDROID_HOME}tools/:${ANDROID_HOME}platform-tools/"`  
_Don't forget to edit your paths!_
3. Reopen terminal window
4. Go to Android HOME. Type `cd /usr/lib/android-sdk/` and got to _tools_ `cd toos`
5. Type `./sdkmanager "build-tools;19.1.0"`, to **install build tools**
6. Type `./sdkmanager --licenses`, to automatically **accept all SDK liscenses**

> Test it - type `android`

## Generating android build
Follow steps and be carefull to use your path not mine =)
1. Go to you ionic project `cd ~/Documents/myApp`  
Probably you will need to connect as super user -> `sudo -i`
2. Type `ionic cordova build android --prod --release`, this will **generate unsigned apk**  
wait untill it's successfully completing (it should generate unsigned .apk)  
relative path to it `platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk`
3. Type `keytool -genkey -v -keystore my-release-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000`, this will **generate keystore file**
4. Type `jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.jks platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk my-alias`, this will **sign unsigned apk**.
5. Type `cd ~/Android/build-tools/19.1.0` 
6. Type `./zipalign -v 4 /home/chrx/Documents/myApp/platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk ~/HelloWorld.apk`

## Conclusion
That's it! You have HelloWorld.apk in your hove directory.

[Source](https://verblike.com/post/how-to-build-application-without-android-studio/)
