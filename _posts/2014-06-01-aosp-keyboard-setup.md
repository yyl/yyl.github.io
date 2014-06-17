---
layout: post
title:  "Setup AOSP keyboard"
date:   2014-06-01 16:50:00
categories: Android
---

Recently I need to build a keyboard that looks and functions as close as the stock Android keyboard, but with additional background functions.

The solution is to build upon the open-sourced Android framework (AOSP). Google has been open sourced Android source code under the name of AOSP, as a git repo. Theoretically, people could build and run the entire functioning Android OS from the framework. It also includes the code for stock Android keyboard. It is said that the codes are not updated: it does not include the gesture typing function in the latest stock Android keyboard. To me it does not matter, because gesture typing is
not a major part in my project.

Here I will explain how to setup the stock keyboard app and make it work on your phone or simulator. The AOSP full git repo could be found [here](https://android.googlesource.com). It is displayed as a list of different modules and packages. What we need is a package called `platform/packages/inputmethods/LatinIME`. It is enssentially a standalone app.

Since it is an app, we could download just that app and import it into Eclipse as an existing project directly.

Once imported, there are several errors. First the keyboard requires Android support library, which you have to add to the `/libs` folder under the project folder, and add it to build path.

Next, I noticed some files have trouble importing a class called `InputMethodSettingsFragment`, which the AOSP keyboard codes do not have. It seems to be a class supporting setting function for the keyboard. The sample soft keyboard code in Android SDK includes a package called `com.android.inputmethodcommon` that has this class. Therefore I duplicate the package in the `/src` folder of our AOSP keyboard project.

The next error in the codes comes from a string `JELLY_BEAN_MR2` that contains Android build version code; it seems the project cannot find it from my SDK. The version code is for version 4.3. I think maybe because I do not have the Jelly beans (4.3) SDK, but not sure. Anyway, I changed the referenced code into `JELLY_BEAN_MR1`(4.2), which my SDK has, and the error goes away.

Now theres no errors in the code which means we are ready to build. However once I built it a new problem came up and crashed the app:

    FATAL EXCEPTION: InitializeBinaryDictionary
    android.content.res.Resources$NotFoundException: File res/raw/main_en.dict from drawable resource ID #0x7f070003

Seems to be some problems with the resource files. I found one temporary solution [here](http://stackoverflow.com/questions/19373833/android-latinime-build): changing the extension of the mentioned dict file from `.dict` to `.jet`. It did work but it did not provide much explanation for the error. I will keep an eye on that. Note you have to convert each dictionary you want to use. I only use English so I changed only `main_en.dict`.

After such error correcting, I was able to build and run my own AOSP keyboard on my phone. Like I said some of the solutions I provided here are just quick fix without digging into the cause of the problem. But it should be enough to get you started.
