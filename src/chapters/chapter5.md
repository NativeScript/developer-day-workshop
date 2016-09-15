## User interface deep dive

Now that you have the basics down, as well as the basics of Angular, let’s look at some more advanced user interface concepts you can use to build your applications.

### Working with tabs views

Tab views are one of the most common mobile navigation patterns in mobile apps. Let’s look at how to implement one in NativeScript.

<h4 class="exercise-start">
    <b>Exercise</b>: Start up a new app
</h4>

Go ahead and start up a new NativeScript app for this workshop:

```
tns create WorkshopFour --ng
```

Next, change directories into your new project:

```
cd WorkshopFour
```

After that, open your new project in Visual Studio Code, and replace the contents of `app.component.ts` with the code below:

``` TypeScript
import { Component } from "@angular/core";

@Component({
  selector: "my-app",
  template: `
<ActionBar title="Account Management"></ActionBar>
<TabView>
  <StackLayout *tabItem="{title: 'Account'}">
    <Label text="This is account tab"></Label>
  </StackLayout>
  <StackLayout *tabItem="{title: 'Settings'}">
    <Label text="This is Label in Tab 2"></Label>
  </StackLayout>
</TabView>
`
})
export class AppComponent {}
```

<div class="exercise-end"></div>

This is what a very basic tab view looks like in an app built with NativeScript and Angular 2. Next, let’s add a side drawer to this same app so you can see each in action.

<h4 class="exercise-start">
    <b>Exercise</b>: Start up a new app
</h4>

In NativeScript, drawer navigation is implemented as part of [UI for NativeSrcipt](http://www.telerik.com/nativescript-ui). UI for NativeScript contains a variety of high-quality components for using in NativeScript applications. The two components we’ll use today are the SideDrawer and ListView controls, both of which are free to use.

> **NOTE**: UI for NativeScript also features premium chart, calendar, and data form components, which we will not be covering today.

To install UI for NativeScript, run the following command in your workshop four project:

```
tns plugin add nativescript-telerik-ui
```

> **TIP**: You might need to hit `Ctrl` + `C` to kill the previous file watcher.

Next, rerun your app using the `tns run` command.

```
tns run ios --watch
```

Or

```
tns run android --watch
```




<div class="exercise-end"></div>

To earn a gift card:
  - Allow the user to submit the form with the four fields on the screenshots. Don’t worry how the data is formatted when you send it the backend—just get the thing to submit.

Bonus points:
  - Improve on our very basic design.
  - Disable the form when you submit.
  - Show an activity indicator while submitting.
  - Show an alert after the form successfully submits.
  - Make all fields mandatory.