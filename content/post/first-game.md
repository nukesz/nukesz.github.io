+++
title = "First Game"
date = 2018-10-25T15:46:05+02:00
draft = false

authors = []
tags = ["libgdx", "intellij", "kotlin", "gradle"]
categories = []

[header]
image = ""
caption = ""
preview = true

+++

# Create the first game

After reading and following along the [design environment post](../design-environment) you should have a fully working environment and we can start creating our first game.
Using the gdx setup tool from libgdx it's pretty straightforward to use. I recommend to read the official [documentation](https://github.com/libgdx/libgdx/wiki/Project-Setup-Gradle) how to run and configure the tool. However there are some trick and tips what you need to be aware of and do after the game has been created. Lets download the [jar](https://libgdx.badlogicgames.com/nightlies/dist/gdx-setup.jar) and run it.

```sh
java -jar gdx-setup.jar
```

I usually keep this jar inside of some "dev" folder and I don't need to download it each time (it's usually good practice to have a separated folder for these tools, especially because libgdx provides many).
Running the command use should see something like this:
![logo](https://camo.githubusercontent.com/e4c5170e04b8583b42b81375067dfc3cc70d5a5c/687474703a2f2f692e696d6775722e636f6d2f6e49356c514b542e6a7067)

On the UI fill the following things:

- Name: _my-first-game_
- Package: _com.yourname.game_
- Game class: _MyFirstGame_
- Destination: _~/games/my-first-game_
- Android SDK: _~/dev/android-sdk_

Select Desktop and Android sub projects. Unfortunately we cannot use the html project with kotlin. For the web libgdx uses GWT which uses the java source code and not the bytecode itself. There are other projects like [TeaVM](http://www.teavm.org/) and maybe in the near future we can publish our games on the web, but right now it's not possible out of the box. Our main target is android, so we can live and continue without the html support.

On the advanced options tick `Use Kotlin` checkbox and click Generate. Wait until the you see the "BUILD SUCCESSFUL" message and then the generated project can be imported into IntelliJ or eclipse. File / Open, and select your new gradle project. I usually check "use auto-import" and click "OK". If everything went well you should see your synced projects on the left and under gradle projects on the right. Running the game can be done from either from IntelliJ or the command line.

## Using IDE

Open the DesktopLauncher class -> Right click -> Run ... You will get an error saying "Couldn't load file: badlogic.jpg", to solve it go to the run configurations and change the working directory to point to the android/assets folder. Try to run it again and you should see the Bad Logic Games logo.

## Using command line

Using kotlin and libgdx from IntelliJ can generate unknown errors. "FileNotFound", "ClassNotFound", etc errors. Sometimes it's hard to know what's the problem so I prefer to run the games from command line. Just open a terminal, navigate to the game folder and run:

```sh
./gradlew desktop:run
```

Of course, the same can be done from IntelliJ itself thanks to the gradle integration.

### What's next

There a few things that you can and should do after this.

- Update dependencies. The setup still uses old libraries what you can upgrade (like kotlin, android plugin, ashley etc). Usually IntelliJ gives you a warning about a version mismatch or if a new version is available.
- Convert java code to kotlin. The "Use Kotlin" still generates java code, so you should convert all those classes into kotlin.
- IntelliJ has a nice [plugin](https://plugins.jetbrains.com/plugin/8509-libgdx-plugin) which I highly recommend to install and use. It gives you nice integration with the library.

### A little help

Of course using the official setup tool is recommended, but there are other setup tools available. One popular is [gdx-setup](https://github.com/czyzby/gdx-setup) by czyzby. Have a look, probably it can help you simplify the creation of games.
