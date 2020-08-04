## Telegram Ar messenger for Android

This in unoffical telegram application[Telegram](https://telegram.org) is a messaging app with a focus on speed and security. It’s superfast, simple and free.
This repo contains the official source code for [Telegram App for Android](https://play.google.com/store/apps/details?id=org.telegram.messenger).

## Building for Windows

To build this application on windows, there are some steps you have to change in the original telegram gradle file before we start, if you are using linux or macOs skip this step

1- After downloading and install the android ndk, add 'LOCAL_SHORT_COMMANDS: true' to Android.mk and 'APP_SHORT_COMMANDS := true' to Application.mk  because of limitations on the number of characters a Windows command can handle.

2- run ndk-build in the root of your project ( not in jna directory).

3-After building you will face this error on your machine 

Execution failed for task ':TMessagesProj:assembleAfatDebugTgVoipDex'.
> A problem occurred starting process 'command 'D:\androidsdk/build-tools/29.0.3/dx''

go to build.gradle and change the dxUtilPath like this

    // Add .bat suffix
    def dxUtilPath = "${sdkDirectory.path}/build-tools/${buildToolsVersion}/dx.bat"
    // Change the run command to call batch via CMD
    commandLine(["cmd", "/c", dxUtilPath, "--dex", "--output=${tgVoipDexFile}"] + classes)

you may also want to add the java path to your dx.bat file, you just need to change your java_exe path.

4-If you encounter this error 
 
 ':TMessagesProj:assembleAfatDebugTgVoipDex.> Process command 'E:\Program Files\Java\jdk1.8.0_152\jre\bin\javac' '

go to gradle.build and find findJavaHome() methond change the original method to this, make sure that the javaPath point to your java directory on windows.

    String javaPath = "C:\\Program Files\\Java\\jdk1.8.0_231"//After modification Code
    if (javaPath != null) {
        File javaBase = new File(javaPath)
        if (javaBase.exists()) {
            if (javaBase.getName().equalsIgnoreCase("jre") && new File(javaBase.getParentFile(), "bin/java").exists()) {
                return javaBase.getParentFile()
            } else {
                return javaBase
            }
        } else {
            return null
        }
    } else {
        return null
    }


## Creating your Telegram Application

We welcome all developers to use our API and source code to create applications on our platform.
There are several things we require from **all developers** for the moment.

1. [**Obtain your own api_id**](https://core.telegram.org/api/obtaining_api_id) for your application.
2. Please **do not** use the name Telegram for your app — or make sure your users understand that it is unofficial.
3. Kindly **do not** use our standard logo (white paper plane in a blue circle) as your app's logo.
3. Please study our [**security guidelines**](https://core.telegram.org/mtproto/security_guidelines) and take good care of your users' data and privacy.
4. Please remember to publish **your** code too in order to comply with the licences.

### API, Protocol documentation

Telegram API manuals: https://core.telegram.org/api

MTproto protocol manuals: https://core.telegram.org/mtproto

### Compilation Guide

**Note**: In order to support [reproducible builds](https://core.telegram.org/reproducible-builds), this repo contains dummy release.keystore,  google-services.json and filled variables inside BuildVars.java. Before publishing your own APKs please make sure to replace all these files with your own.

You will require Android Studio 3.4, Android NDK rev. 20 and Android SDK 8.1

1. Download the Telegram source code from https://github.com/DrKLO/Telegram ( git clone https://github.com/DrKLO/Telegram.git )
2. Copy your release.keystore into TMessagesProj/config
3. Fill out RELEASE_KEY_PASSWORD, RELEASE_KEY_ALIAS, RELEASE_STORE_PASSWORD in gradle.properties to access your  release.keystore
4.  Go to https://console.firebase.google.com/, create two android apps with application IDs org.telegram.messenger and org.telegram.messenger.beta, turn on firebase messaging and download google-services.json, which should be copied to the same folder as TMessagesProj.
5. Open the project in the Studio (note that it should be opened, NOT imported).
6. Fill out values in TMessagesProj/src/main/java/org/telegram/messenger/BuildVars.java – there’s a link for each of the variables showing where and which data to obtain.
7. You are ready to compile Telegram.

### Localization

We moved all translations to https://translations.telegram.org/en/android/. Please use it.
