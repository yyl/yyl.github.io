---
layout: post
title:  "Build AOSP keyboard"
date:   2014-06-16 11:20:00
categories: Android
---

In previous post I wrote about how I compiled the stock keyboard on Android in Eclipse. Unfortunately I came across numerous problems trying to get the keyboard installed on a few different test devices after that. Also, I realized there was not any comprehensive tutorial on how to build the stock keyboard for Android. Therefore, I start a new one here trying to cover at least all of problems I have met and steps I took. Hopefully it would be helpful to others.

This blog would be written in a sequential style of steps to build the keyboard application from scratch.

### Get source code

The app we are trying to build, **AOSP keyboard**, is the default keyboard application in Android OS. It is open sourced under the Android Open Source Project (AOSP). Google has put the source code on a git repository for anyone to download and play.

The precise link for the keyboard application is [here](https://android.googlesource.com/platform/packages/inputmethods/LatinIME/). The app in the git repository is called _LatinIME_. You need to have _Git_ to download the source code. You could choose from different branches, which map to different versions of Android, or just the master branch.

The downloaded folder, LatinIME, has a structure like this:

    Android.mk
    CleanSpec.mk
    dictionaries/
    java/
    native/
    tests/
    tools/

The source code of the keyboard app locates at `java/` folder. If you are using Eclipse IDE to code Android like I do, you could open up Eclipse and choose "import Android project from existing code", and then choose this folder. Eclipse will automatically load them into a proper Android project.

Now you probably will see some errors in the imported project immediately. Please refer to my previous blog about how to solve some common issues to get rid of those errors.

### Get native library

Apparently the keyboard application uses some native C codes in addition to normal Java code, which are all in the `native/` folder. One needs to have a Java native interface (jni) to interact with such native codes. In short, sometimes people write C codes and run in Android to ensure performance. Anyway, we do not need to write C code, the source code already has that written for us; but since the app has such codes, we have to include them in the project. Without them in the project, the terminal will say something like `cannot find native method` when you build and run the application in Eclipse.

It turns out all those codes could be compiled and built into one single file `libjni_latinime.so`. Once we have the file, we could add it to our project.

However, at least for me, getting the file is not as easy as it seems. All I have from `native/` are source code, which I have to compile and build the file I want.

I go on and download the entire source code of Android from [here](https://android.googlesource.com/platform/manifest/), which is the same git repository where holds the keyboard codes. The entire code tree is huge, 30G for the master branch, 22G for the branch I downloaded (android-4.3_r1). One could find detailed steps of downloading Android source [here](https://source.android.com/source/building.html). One note that, the web site says to downloading the entire tree one needs 10G disk space, which is clearly not my case. It might be referring to old versions such as ICS. I tried downloading different branches and most of takes up 20-30G disk space.

Once you have the code tree, go ahead and build it. The link I gave for downloading and building the source is pretty comprehensive so I will skip this. Just for the record, it takes me another 19G to build the entire Android 4.3_r1 choosing the `mini_x86-eng` as a the BUILD-TYPE. `mini` stands for minimal build, as I actually do not need the entire Android system, so I just want to build as little as possible. BUILD-TYPE is just the target you try to build your Android system to. In addition to `mini`, you also have options like `full`, `core`, etc. A `full` build brings you the fully featured Android system, but it needs more than 19G space on disk.

In all, a tested building environment for Android source tree is: 60G+ disk space (50G if you go `mini`), 6G RAM, Ubuntu 12.04 **64bit** OS. You also need Oracle Java 6 SDK unless you are building the master branch.

After the build of Android source tree, move to `packages/inputmethods/LatinIME/native/jni`, where the native code for keyboard is in. Then do `mma`. This is a command provided by Android build, which you have to source from `build/envsetup.sh`. It meas build the current directory and all its dependencies. Once the process is done, you could find `libjni_latinime.so` under `out/target/product/generic/system/lib` by default. You could change the output directory of course. I am not sure if you could build the library without building the entire source tree. It at least requires `LatinIME/` and the build tools.

Now put the file under `libs/armeabi-v7a/` subfolder of the root dir of your project. `armeabi-v7a` refers to the specific platform you are building, it could also be others, such as `x86` and `armeabi`, depending on on which device you want to install the app.

That is all for native library.

### Resolve package conflict

The keyboard app used to be a part of the Android OS until around Android 4.3. This means if you install the app to a Android version under 4.3, it will conflict with the stock Android keyboard as they have the same package name. It will probably result in errors like "app not installed" or `INSTALL_FAILED_VERSION_DOWNGRADE`.

My solution is to use the refactor function of Eclipse to change all package names of the project into something else. To note that not only each package inside of the project has to be changed, but also [the project itself](http://stackoverflow.com/questions/6500042/refactoring-package-name-breaks-entire-app).

In addition, since native codes are linked to the project through jni, you have to change them too. In `native/` folder, change every place resembling `com.android.inputmethod.latin` to your own package name. The exact code will be in a different format from the package name style as it is written in C code.

After change, you have to also rebuild the native library again. If you build and run the project without changing the native library file, you will probably meet `UnsatifiedLinkError`.

### Miscs and conclusion

Sometimes I meet weird problem of installing the app. The IDE says `INSTALL_FAILED_UID_CHANGED`. If this comes up, remember to uninstall the keyboard app first (also clean the data before uninstall), and then reboot. This will usually do the trick.

Ok, looks like a big mess. I will refine it later. Hope it helps someone who has been stuck with nowhere to go like me.
