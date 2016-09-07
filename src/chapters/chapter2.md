## The basics

For our first hands-on workshop, you’ll learn how to build and manage NativeScript apps, as well as how to make simple changes and debug your code.



### Prerequisites
In order to complete all of todays' workshops you need the NativeScript CLI installed on your machine.

[Here are the instructions on how to do it](https://docs.nativescript.org/start/quick-setup)


### How the CLI works
The CLI allows you to perform various tasks from creating a new project, building an app to importing plugins.
All the CLI commands start with a prefix `tns`

<h4 class="exercise-start">
    <b>Exercise</b>: Create your first project
</h4>

Open your terminal (Mac) or command line console (Windows) and navigate to the folder where you want to create your first project. Now run the following command:

```
tns create WorkshopOne --ng
```

`WorkshopOne` is the name of the project, while `--ng` tells the CLI to create an ng2 type of project.

When complete navigate into the created project folder:

```
cd WorkshopOne
```

<div class="exercise-end"></div>


At this point we have a hello world type of project, with a bit of simple logic.
Now we need add the platforms we want to build this app for.

<h4 class="exercise-start">
    <b>Exercise</b>: Adding mobile platforms
</h4>

To add Android to your project run:

```
tns platform add android
```

For iOS run:

```
tns platform add ios
```

<div class="exercise-end"></div>


<h4 class="exercise-start">

Now we will learn how to build and run your apps.

    <b>Exercise</b>: Running the app
</h4>

To build an Android app and run it on a connected device or Genymotion instance call: 

```
tns run android
```

However if you want to run the app in an emulator you should call

```
tns run android --emulator
```

The same applies to building and running iOS apps. Just swap `android` for `ios`

```
tns platform add ios --emulator
```

As a side note. If you called `tns run` and then realised that you want to make a change first, you can cancel the build by pressing `CTRL+C`

<div class="exercise-end"></div>


Sometimes waiting for the build process can be a bit painful, especially when you need to rebuild the app every time you make a tiny change to the code.
This is why the CLI has the `livesync` command, which allows you to build the app in such way, so that whenever you make a change in your code the app will get refreshed really quickly.
To make it more insteresting, if you make a change to your `TypeScript` code, the CLI will parse it to `JavaScript` first and then refresh the app.

<h4 class="exercise-start">

Streamlining the build process.

    <b>Exercise</b>: Livesync
</h4>

Build your app using the `livesync` command and then open `app.css`, make some changes to the styling and save the file.
```
tns livesync android --watch
```

or 

```
tns livesync ios --emulator --watch
```

<div class="exercise-end"></div>


<h4 class="exercise-start">

    <b>Exercise</b>: Debugging
</h4>
Build your app in the debug mode using the following command.
This should launch debugging tools for you. Once that is ready find app.component.js add a breakpoint to `onTap()` then tap on the button and try to explore what you can do with it.

```
tns debug android
```

```
tns debug ios --emulator
```

Here is an interesting thing. Launching a debugger is a very slow process, but yet again `livesync` comes in to save the day.
You can run the debugger in the livesync mode by adding `--watch` at the end of the command, like this:

```
tns debug android --watch
```

<div class="exercise-end"></div>


Now that we know how to build the apps, let's add an npm module to the project to help us with getting additional functionality.
If you go to [`npmjs` and search for `nativescript`](https://www.npmjs.com/search?q=nativescript) you will get a long list of NativeScript specific npm modules that might come in handy when building an app.

<h4 class="exercise-start">
Here we will learn how to add an npm module.

    <b>Exercise</b>: Installing a NativeScript plugin from npm
</h4>

The usual way to add an npm module would be to call `npm i module-name`, however when it comes to installing NativeScript specific plugins it is better to call `tns plugin add module-name`. That is because `tns plugin add` will do all that `npm install` does plus it configures any native dependencies that the plugin may need to use. This might include installing additional native iOS and/or components.


Let's add the [appinfo plugin](https://www.npmjs.com/package/nativescript-appinfo), which will allow us to get info like the `version name`, `build number` and `app ID`.

```
tns plugin add nativescript-appinfo
```

Once you run this command you should be able to see the `nativescript-appinfo` module inside the `node_modules` folder and also the dependency to the module should be added to the `package.json` at the root of the project (not the one in the `app` folder)


Now let's try to use the `nativescript-appinfo` module.
Open `app.component.ts` and add the following line to the top of the file (or anywhere before `@Component` starts):

```
var appinfo = require("nativescript-appinfo");
```

This will give us access to the appinfo module in this area of code.

Now we can call the `appinfo` instance to get some app information. Add the following piece of code inside the `onTap()` function:

```
appinfo.getAppId()
    .then(function(id) {
        console.log("Your app's id is: " + id);
    });
```

Now build the app and see what happens when you tap the button.

As a bonus exercise: run this app in the `debug mode` and a break point at `getAppId()` and try to step in to getAppId function to see what happens.

<div class="exercise-end"></div>

### NativeScript folder structure

### Managing `platforms` and `node_modules`

### Making your first change

### Working in Visual Studio Code

### Setting up a debugging workflow
