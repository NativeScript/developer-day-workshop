## Beyond the basics

You now know how to start NativeScript apps, and how to scaffold out a basic user interface. Now let’s dig deeper. In this workshop you’ll learn how to create Angular services, how to deal with data, how to setup routing, and how to access native code. Let’s start by building an Angular service.

### Creating Angular services

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

@Component({
  selector: "my-app",
  templateUrl: "app.component.html",
})
export class AppComponent {
  groceries = [];
  ngOnInit() {
    this.groceries.push({ name: "Bananas" });
    this.groceries.push({ name: "Apples" });
    this.groceries.push({ name: "Grapes" });
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

```

<div class="exercise-end"></div>



Reference:

- [Angular services](https://angular.io/docs/ts/latest/tutorial/toh-pt4.html)
- [Angular dependency injection](https://angular.io/docs/ts/latest/guide/dependency-injection.html)
- [Using NativeScript ListViews](https://docs.nativescript.org/angular/ui/list-view.html)

### Dealing with data

### Setting up routing and navigation

### Accessing native iOS and Android APIs

<h4 class="exercise-start">
    <b>Challenge</b>: Change the placeholder text color
</h4>

NativeScript currently does not support styling a text field’s hint color through CSS, but iOS and Android both do. Pick either iOS or Android, and try to change the text field’s hint color to red.

<div class="solution-start"></div>

Add the following code to your `foo.ts` file:

``` TypeScript
let foo = "bar";
```

<div class="solution-end"></div>

<div class="exercise-end"></div>
