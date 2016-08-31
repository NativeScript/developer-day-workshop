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

Open your terminal (on Mac) or command line console (on Windows) and navigate to a folder where you want to create your first project. Now run the following command:

```
tns create WorkshopOne --ng
```

`WorkshopOne` is the name of the project, while `--ng` tells the CLI to create an ng2 type of project.

Now navigate into the created project folder:

```
cd WorkshopOne
```

<div class="exercise-end"></div>

Add this point we have a hello world type of project, with a bit of simple logic.
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
    <b>Exercise</b>: Exercise Title
</h4>
<div class="exercise-end"></div>

<h4 class="exercise-start">
    <b>Exercise</b>: Exercise Title
</h4>
<div class="exercise-end"></div>



--tns help <command>
A list of templates can be found [here](http://nativescript.rocks/templates.php)

### NativeScript folder structure

### Managing `platforms` and `node_modules`

### Making your first change

### Working in Visual Studio Code

### Setting up a debugging workflow
