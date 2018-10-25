+++
title = "First Game"
date = 2018-10-25T15:46:05+02:00
draft = false

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

# Create the first game

After reading and following along the [design environment post](../design-environment) you should have a fully working environment and we can start creating our first game.   
Using the gdx setup tool from libgdx it's pretty straigthforward to use. I recommend to read the official [documentation](https://github.com/libgdx/libgdx/wiki/Project-Setup-Gradle) how to run and configure the tool. However there are some trick and tips what you need to be aware of and do after the game has been created. Lets download the [jar](https://libgdx.badlogicgames.com/nightlies/dist/gdx-setup.jar) and run it. 
```
$ java -jar gdx-setup.jar
```
I usually keep this jar inside of some "dev" folder and I don't need to download it each time (it's usually good practice to have a separated folder for these tools, especially because libgdx provides many).
Running the command use should see something like this:
![logo](https://camo.githubusercontent.com/e4c5170e04b8583b42b81375067dfc3cc70d5a5c/687474703a2f2f692e696d6775722e636f6d2f6e49356c514b542e6a7067)

On the UI fill the following things:  

- Name: 
- Package:  
- Game class:
- Destination:
- Android SDK:

Select Desktop, Android projects. Unfortunately we cannot use the html project with kotlin. For the web libgdx uses GWT which uses java source code and not the bytecode itself. There are other projects like teavm and maybe in the near future we can  publish our game on web, but right now its not possible out of the box. Our main target is android, so we can live and continue without the html support. 
On the advanced options tick Use kotlin checkbox. 
The generated project as a gradle which we can import into intellij or eclipse. File / open / from existing source / ok. If everything went well we should see our synced projects on the left and under gradle projects on the right. 
Running the game can be done from intellij or from the command line. Right click run .. workslace.. from the command line gradlew (use wrapper to make sure your using the correvct gradle cersion) desktop:run. In either case yoh should see the followíng @kep.  There a few things that you can and should do after this. 1. Use the latest kotlin version. Gradle android plugin ashley. Add google jcenter dependenyies. Convert java code to kotlin. Intellij has a nice plugin which I highly recommend. It has features like... it can be installed by clicking... disable gradle configure on demand. You still should be able to run the game as we did before. Whenever you create a new game you should repeat the same steps. But fortunatelly some parts can be aoutomated. Using the project generator you can create a shell with the prefield valus anď only the post installations have to be done. There is also another project that try to help to setup a new project,in the next blog post I create a game using that. 