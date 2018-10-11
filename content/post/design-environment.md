+++
title = "Design Environment for Game Development"
date = 2018-10-04T16:02:29+02:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = []

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["libgdx", "android"]
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = ""
preview = true

+++

# Getting started

In this article I would like to show what is required to have an up and ready environment for
game development with the **libgdx** framework. There is a getting started page on the libgdx's website, but I wanted to gather all the required steps in one place. I will focus on what is needed for a 2D Game Developer who is targeting **Android** mainly. I'm using Ubuntu 18.04 right now, but probably fellow windows, macOS users can follow along.  

We're going to install and setup the following tools:

* java
* gradle
* android
* IntelliJ (skip if you're using other IDE)

---
## JAVA

```
$ sudo add-apt-repository ppa:webupd8team/java -y
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
```
Set Oracle Java 8 as default
```
$ sudo apt-get install oracle-java8-set-default
```
Verify the installation was successful
```
$ java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```

---
## GRADLE

Usually projects use [gradle wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html) as it helps to reduce the cost to setup the build environment for the project. For example if you're using docker (as you should! :) it helps to base you're image on some existing java image only. You don't have to worry about finding an image which has gradle also, just use your wrapper and you're good to go[build].  
But... of course to create the wrapper you need to have gradle installed. Probably you can get away to copy-paste from another project. But I think it's just convenient to have it in your terminal as you need.  
 I've copied the part from the [installation](https://docs.gradle.org/current/userguide/installation.html) page:
```
$ mkdir /opt/gradle
$ unzip -d /opt/gradle gradle-4.10.2-bin.zip
$ ls /opt/gradle/gradle-4.10.2
```

Verify installation
```
$ gradle -v
```

Add it to the path. I recommend you to add this line to end of your ~/.bashrc so it will be loaded each time you open a new terminal.  
```
export PATH=$PATH:/opt/gradle/gradle-4.10.2/bin
```
---
## ANDROID  

The market we aim is going to be the Android Marketplace, Google Play. In order to build, run and
test games, we need to have Android SDK on our machine, so let's download it.
I prefer to use the command line tools only, so just go to [android developer] (https://developer.android.com/studio/) page, scroll to the "Command line tools" only section and
download the linux zip.
```
$ mkdir ~/dev/android-sdk
$ unzip sdk-tools-linux-*.zip
$ mv tools/ ~/dev/android-sdk/tools
```
We can repeat the same steps as we did for gradle to create the environment variables and add the binaries to the path.
```
export ANDROID_HOME=~/dev/android-sdk
export ANDROID_TOOLS=$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_TOOLS/bin
```
Verify the installation by running the `sdkmanager`.
```
$ sdkmanager
Warning: File /home/nukesz/.android/repositories.cfg could not be loaded.            
[=======================================] 100% Computing updates...
```
You can easily fix the warning by create an empty cfg file.
```
$ touch ~/.android/repositories.cfg
```
We have more thing to do and that is to install build-tools and platforms for android. I tend to pick a stable version and make sure all the games I develop are using that version. In my opinion it's easier to maintain one version (and emulators to it) than getting weird errors because of different versions. So let's install Android 28.
## Install Build Tools
```
$ yes | sdkmanager "build-tools;28.0.1"
```
## Install platforms
```
$ yes | sdkmanager "platforms;android-28"
```
After these two commands, you should have a following structure in your  *ANDROID_HOME*:
```
$ ls $ANDROID_HOME
build-tools  licenses  platforms  sdk-tools
```

---
## INTELLIJ

I don't want to waste my pen to write down what you can find on the awesome [jetbrains](https://www.jetbrains.com/) page, but I just want to highlight that instead of downloading all the cool products from jetbrains one-by-one, just download the [toolbox](https://www.jetbrains.com/toolbox/app/). With that app you can download/upgrade/delete apps, and never have to worry about upgrading manually, linking bins etc.    

---
## SUMMARY

With all these, you now have a fully functional environment and ready to jump into the game development business! In the next post we're going to create our first game using libgdx.
