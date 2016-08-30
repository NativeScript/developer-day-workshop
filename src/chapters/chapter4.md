## Beyond the basics

You now know how to start NativeScript apps, and how to scaffold out a basic user interface. Now let’s dig deeper. In this workshop you’ll learn how to create Angular services, how to deal with data, how to setup routing, and how to access native code. Let’s start by building an Angular service.

### Learning Angular 2 data binding, events, and servicess

In this workshop you’ll be building a master/detail view for a simple grocery-management application. Let’s dive right in.

<h4 class="exercise-start">
    <b>Exercise</b>: Start up a new app
</h4>

Go ahead and start up a new NativeScript app for this workshop:

```
tns create WorkshopThree --ng
```

Next, change directories into your new project:

```
cd WorkshopThree
```

<div class="exercise-end"></div>

Open this new project up in Visual Studio Code and let’s start working with Angular services.

<h4 class="exercise-start">
    <b>Exercise</b>: Your first service
</h4>

Replace the contents of `app.component.ts` with the code below, which creates a simple grocery list:

``` TypeScript
import { Component, OnInit } from "@angular/core";
import { GroceryService } from "./grocery.service";
import { Grocery } from "./grocery";

@Component({
  selector: "my-app",
  templateUrl: "app.component.html",
  providers: [GroceryService]
})
export class AppComponent {
  groceries: Array<Grocery>;

  constructor(private groceryService: GroceryService) {}
  ngOnInit() {
    this.groceries = this.groceryService.get();
  }
}

```

And then replace the contents of `app.component.html` with the code below, which shows the groceries on the screen:

``` XML
<GridLayout>
  <ListView [items]="groceries">
    <template let-item="item">
      <Label [text]="item.name"></Label>
    </template>
  </ListView>
</GridLayout>
```

Next, create a file in the `app` folder named `grocery.service.ts`, and paste in the following code:

``` TypeScript
import { Injectable } from "@angular/core";
import { Grocery } from "./grocery";

@Injectable()
export class GroceryService {
  get() {
    let groceries = [];
    groceries.push(new Grocery("Bananas"));
    groceries.push(new Grocery("Apples"));
    groceries.push(new Grocery("Grapes"));
    return groceries;
  }
}
```

Finally, create another file in the `app` folder named `grocery.ts`, and paste in the following code:

``` TypeScript
export class Grocery {
  name: string;
  constructor(name) {
    this.name = name;
  }
}
```

<div class="exercise-end"></div>

At this point you should see a hardcoded list of data. Let’s refactor this app so that the data isn’t hardcoded.

<h4 class="exercise-start">
    <b>Exercise</b>: Allow users to add groceries
</h4>

Update you `app.css` with the code below, which will add a bit of spacing to the app you’re building.

``` CSS
TextField, Button, Label {
    padding: 15;
}
```

Next, change your `app.component.html` file to use the code below, which sets up a UI that allows users to add data to the list.

``` XML
<GridLayout>
  <GridLayout rows="auto, *">

    <GridLayout row="0" columns="*, auto">
      <TextField col="0" hint="Enter a grocery"></TextField>
      <Button col="1" text="Add"></Button>
    </GridLayout>

    <ListView row="1" [items]="groceries">
      <template let-item="item">
        <Label [text]="item.name"></Label>
      </template>
    </ListView>
  </GridLayout>
</GridLayout>
```

With that setup out of the way, let’s try a quick challenge.

<div class="exercise-end"></div>

<h4 class="exercise-start">
    <b>Challenge</b>: Adding a tap handler
</h4>

NativeScript button elements have a `tap` event that you can subscribe to using `<Button (tap)="functionName()">`. Angular 2 lets you implement two-way data binding between a text field and a component property using the `[(ngModel)]` styntax, for instance `<TextField [(ngModel)]="grocery">`.

Your challenge is to combine the two, so that when the user taps the “Add” button they see the value they typed in the text field.

<div class="solution-start"></div>

Use the following code for your `app.component.html`:

``` XML
<GridLayout>
  <GridLayout rows="auto, *">

    <GridLayout row="0" columns="*, auto">
      <TextField col="0" hint="Enter a grocery" [(ngModel)]="grocery"></TextField>
      <Button col="1" text="Add" (tap)="add()"></Button>
    </GridLayout>

    <ListView row="1" [items]="groceries">
      <template let-item="item">
        <Label [text]="item.name"></Label>
      </template>
    </ListView>
  </GridLayout>
</GridLayout>
```

And the following code for your `app.component.ts`:

``` TypeScript
import { Component, OnInit } from "@angular/core";
import { GroceryService } from "./grocery.service";
import { Grocery } from "./grocery";

@Component({
  selector: "my-app",
  templateUrl: "app.component.html",
  providers: [GroceryService]
})
export class AppComponent {
  grocery: String;
  groceries: Array<Grocery>;

  constructor(private groceryService: GroceryService) {}

  ngOnInit() {
    this.groceries = this.groceryService.get();
  }

  add() {
    alert("You typed " + this.grocery);
  }
}
```

<div class="solution-end"></div>

<div class="exercise-end"></div>

Reference:

- [Angular services](https://angular.io/docs/ts/latest/tutorial/toh-pt4.html)
- [Angular dependency injection](https://angular.io/docs/ts/latest/guide/dependency-injection.html)
- [Using NativeScript ListViews](https://docs.nativescript.org/angular/ui/list-view.html)

### Dealing with data

In this next section of the workshop you’ll enhance the small grocery list to allow users to add items to the list, and also to persist grocery lists to their devices.

<h4 class="exercise-start">
    <b>Exercise</b>: Allow users to add data
</h4>

First, replace the contents of `grocery.service.ts` with the code below. The changes include removing the hardcoded list, as well as a new `add()` method.

``` TypeScript
import { Injectable } from "@angular/core";
import { Grocery } from "./grocery";

@Injectable()
export class GroceryService {
  private groceries: Array<Grocery>;

  constructor() {
    this.groceries = [];
  }

  get() {
    return this.groceries;
  }

  add(name) {
    this.groceries.unshift(new Grocery(name));
  }
}
```

Next, replace the contents of `app.component.ts` with the code below:

``` TypeScript
import { Component, OnInit } from "@angular/core";
import { GroceryService } from "./grocery.service";
import { Grocery } from "./grocery";

@Component({
  selector: "my-app",
  templateUrl: "app.component.html",
  providers: [GroceryService]
})
export class AppComponent {
  grocery: String;
  groceries: Array<Grocery>;

  constructor(private groceryService: GroceryService) {}

  ngOnInit() {
    this.groceries = this.groceryService.get();
  }

  add() {
    this.groceryService.add(this.grocery);
    this.grocery = "";
  }
}
```

<div class="exercise-end"></div>

You now have a very basic list, but there’s a big problem: this list isn’t persisted. Let’s look at how to change that.

<h4 class="exercise-start">
    <b>Exercise</b>: Persisting data
</h4>

Start by adding the import below to the top of your `grocery.service.ts` file:

``` TypeScript
import { getString, setString } from "application-settings";
```

Next, in the same file, replace the existing `add()` function with the code below, which saves the grocery list using the application settings module you just imported.

``` TypeScript
add(name) {
  this.groceries.unshift(new Grocery(name));
  let names = this.groceries.map((item: Grocery) => {
    return item.name;
  });
  setString("groceries", names.toString());
}
```

Your data is now being saved, but it’s up to you to determine how load this data when the application loads. A challenge!

<div class="exercise-end"></div>

<h4 class="exercise-start">
    <b>Challenge</b>: Loading saved data
</h4>

Your challenge is to change `grocery.service.ts` to automatically load any data previously stored by the user. As a tip, you’ll want to use the `getString()` method you imported in the previous exercise.

> **NOTE**: A convenient way to restart the app is to add or remove a new line from the TypeScript file you’re currently working on.

<div class="solution-start"></div>

Replace the constructor in `grocery.service.ts` with the code below:

``` TypeScript
constructor() {
  this.groceries = [];
  let savedGroceriesString = getString("groceries"); 
  if (savedGroceriesString) {
    savedGroceriesString.split(",").forEach((grocery) => {
      this.groceries.push(new Grocery(grocery));
    });
  }
}
```

<div class="solution-end"></div>
<div class="exercise-end"></div>

There are many ways to store data in NativeScript apps, in addition to using the application settings module, you may wish to use one of the options listed below.

Reference:

- [NativeScript Firebase plugin](https://github.com/EddyVerbruggen/nativescript-plugin-firebase)
- [NativeScript Couchbase plugin](https://github.com/couchbaselabs/nativescript-couchbase)
- [NativeScript SQLLite plugin](https://github.com/NathanaelA/nativescript-sqlite)
- [Accessing RESTful HTTP APIs](http://docs.nativescript.org/angular/tutorial/ng-chapter-3#34-services)

### Setting up routing and navigation

### Accessing native iOS and Android APIs


