## Telegram Ar messenger for Android

This in unoffical telegram application, [Telegram](https://telegram.org) is a messaging app with a focus on speed and security. It’s superfast, simple and free.
This repo contains the official source code for [Telegram App for Android](https://play.google.com/store/apps/details?id=org.telegram.messenger).

## Building for Windows

To build this application on windows, there are some steps you have to change in the original telegram gradle file before we start, if you are using linux or macOs skip this step

1- After downloading and install the android ndk, add `LOCAL_SHORT_COMMANDS: true` to `Android.mk` and `APP_SHORT_COMMANDS := true` to `Application.mk`  because of limitations on the number of characters a Windows command can handle.

2- run `ndk-build` in the root of your project ( not in jna directory).

3-After building you will face this error on your machine 

> Execution failed for task ':TMessagesProj:assembleAfatDebugTgVoipDex'.
> A problem occurred starting process 'command 'D:\androidsdk/build-tools/29.0.3/dx''

go to `build.gradle` and change the dxUtilPath like this

    // Add .bat suffix
    def dxUtilPath = "${sdkDirectory.path}/build-tools/${buildToolsVersion}/dx.bat"
    // Change the run command to call batch via CMD
    commandLine(["cmd", "/c", dxUtilPath, "--dex", "--output=${tgVoipDexFile}"] + classes)

you may also want to add the java path to your `dx.bat` file, you just need to change your `java_exe` path.

4-If you encounter this error 
 
> ':TMessagesProj:assembleAfatDebugTgVoipDex.> Process command 'E:\Program Files\Java\jdk1.8.0_152\jre\bin\javac' '

go to `gradle.build` and find `findJavaHome()` methond change the original method to this, make sure that the `javaPath` point to your java directory on windows.

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
