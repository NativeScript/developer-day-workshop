## The user interface

Now that you’ve gotten a feel for how NativeScript apps work, let’s shift our focus to the user interface. In our second hands-on workshop, you’ll learn about using UI components, styling them with CSS, and animating them with NativeScript’s robust animation library. In this workshop, we'll create an adorable Kitten Adoption app with a card interface.

### Adding UI components

<h4 class="exercise-start">
    <b>Exercise</b>: Create a new app
</h4>

Let's create a new NativeScript app for this workshop:

```
tns create WorkshopTwo --ng
```

Next, change directories into your new project:

```
cd WorkshopTwo
```

<div class="exercise-end"></div>

Open this new project up in Visual Studio Code (you can type `code .`) and add our first UI component.

First, let's take a look at the files we will need to work with. Notice in the `/app` folder of your project there is a file called `app.component.ts`. This is where you put the functions that are called by the front-end file, `app.component.html`.

Open <code>app.component.html</code> and delete everything there.

Start your app's livesyncing process on iOS by typing into the command line:

```
tns livesync ios --watch
```

For Android, we recommend using Genymotion; open Genymotion and start an emulator running. type:

```
tns livesync android --watch
```

Now, when you make changes to your code, your app will refresh in the emulator.

<h4 class="exercise-start">
    <b>Exercise</b>: Add your first UI element
</h4>

Native apps need native-looking controls. We will create a native ActionBar using XML markup. In `app.component.html`, delete all the markup and replace it with this line:

```
<ActionBar title="Adopt A Kitten!"></ActionBar>
```

<div class="exercise-end"></div>

Before we go further, we need some data that we can layout on the screen. Let's grab ten kitten images from [placekitten.com](http://placekitten.com) to have them ready to place on our app screen.

<h4 class="exercise-start">
    <b>Exercise</b>: Get some data ready
</h4>

We won't go into the details now of binding your data to UI as you'll cover this in the next workshop.

Open `app.component.ts` and delete everything there. Replace it with the code below:

```
import {Component} from "@angular/core";

        @Component({
            selector: "my-app",
            templateUrl: "app.component.html"
        })
        export class AppComponent {
          public kittens: Array<any>;
          public url: string;
          public counter:number = 200;

            constructor() {
                this.kittens = [];
                this.url = 'https://placekitten.com/200/';
                                    
                for (var i = 0; i < 10; i++) {
                    this.counter++;
                    this.kittens.push(this.url+this.counter);
                }
            }   
        }       
```

We'll use this data in our layout in a minute.

<div class="exercise-end"></div>


### Types of layouts

There are five basic layouts offered by NativeScript that correspond to standard native ways of laying elements out onto a page: StackLayout, GridLayout, WrapLayout, DockLayout, and AbsoluteLayout. Let's take a look at these.

### Create a custom layout

Now it's time to have some fun with creating a little functional layout for our kitten adoption app. We're going to create a card layout by combining StackLayout and WrapLayout.

<h4 class="exercise-start">
    <b>Exercise</b>: Place the kitten images in a scrolling StackLayout
</h4>

What happens if you just put an array of kitten images within a StackLayout?

In `app.component.html`, add the following under the ActionBar:

```
<StackLayout>
    <Image src="https://placekitten.com/300/300"></Image>
    <Image src="https://placekitten.com/300/300"></Image>
    <Image src="https://placekitten.com/300/300"></Image>
    <Image src="https://placekitten.com/300/300"></Image>
    <Image src="https://placekitten.com/300/300"></Image>
</StackLayout>
```
The images are stacked, but they don't scroll.

Nest this StackLayout into a ScrollView:

```
<ScrollView>
  <StackLayout>
      <Image src="https://placekitten.com/300/300"></Image>
      <Image src="https://placekitten.com/300/300"></Image>
      <Image src="https://placekitten.com/300/300"></Image>
      <Image src="https://placekitten.com/300/300"></Image>
      <Image src="https://placekitten.com/300/300"></Image>
  </StackLayout> 
</ScrollView>
```

<div class="exercise-end"></div>

It's all fine to add image manually to an app, but any data-driven app will need to have date loaded dynamically. Let's get our StackLayout to loop, so we can start creating cards.

<h4 class="exercise-start">
    <b>Exercise</b>: Create the kitten cards
</h4>

In `app.component.html`, replace the current ScrollView with a layout that is generated dynamically:

```
<ScrollView>
    <StackLayout *ngFor="let kitten of kittens">
       <Image [src]="kitten"></Image>
    </StackLayout>     
</ScrollView>
```

<div class="exercise-end"></div>

Notice the actual images (there should be 10) aren't laid out in a loop. We need to nest our layout to do this.

<h4 class="exercise-start">
    <b>Exercise</b>: Dynamically layout the cards
</h4>

In `app.component.html`, replace the current ScrollView with a WrapLayout that will allow the StackLayout to loop.

```
<ScrollView>
    <WrapLayout>
        <StackLayout *ngFor="let kitten of kittens">
           <Image [src]="kitten"></Image>
        </StackLayout>
    </WrapLayout>     
</ScrollView>
```

This allows the layout to loop, but the cards don't look very good. Let's control the width of the cards by making their width a percentage and align them horizontally:

```
<StackLayout width="40%" *ngFor="let kitten of kittens" horizontalAlignment="center">
```

Align the cards to the center by controlling the alignment of the WrapLayout:

```
<WrapLayout horizontalAlignment="center">
```

Now we have a wrapping layout of cards dynamically loaded onto the screen!

The final thing we want to do to these cards is to add a caption underneath the image. Add a caption of your own under the image:

```
<Label text="p'tit minou" horizontalAlignment="center"></Label>
```

<div class="exercise-end"></div>

<blockquote>
    <p><strong>CHALLENGE!</strong>: Can you find a nicer way to layout these cards? Experiment with GridLayouts and AbsoluteLayouts. Show us your results!</p>
</blockquote>

### Styling apps with CSS

The cards are laid out properly, but they don't look very good. Time to add some styles!

<h4 class="exercise-start">
    <b>Exercise</b>: Add some color
</h4>

Choose a pallette of color from [Coolors.co](http://coolors.co) or another pallette generator of your choice.

Attach a style sheet to your app by editing the `@Component` block in `app.component.ts`:

```
@Component({
    selector: "my-app",
    templateUrl: "app.component.html",
    styleUrls: ["app.css"]
})
```

Delete everything in app.css. Add some color to the ActionBar and the Page background by adding a few styles to this file:

```
Page {
    background-color: #EBEBD3;
}

ActionBar {
    background-color: #083D77;
    color: #EBEBD3;
}
```

Style the card by adding a css class to the StackLayout in `app.component.html`:

```
<StackLayout width="40%" *ngFor="let kitten of kittens" horizontalAlignment="center" class="card">
```

Finally, add styles to the card itself by adding this style to `app.css`:

```
.card {
    background-color: #DA4167;
    margin: 10;
    border-radius: 5;
    color: #EBEBD3;
}
```
<div class="exercise-end"></div>

<blockquote>
    <p><strong>CHALLENGE!</strong>: Can you find a nicer way to layout these cards? Experiment with GridLayouts and AbsoluteLayouts. Show us your results!</p>
</blockquote>

Congratulations! With a bit of code, you've succesfully created a nice card interface!

### Creating robust animations

The more mobile app development you do, the more you realize that clean animations are not just a 'nice-to-have', they are a 'must-have'. Fortunately, NativeScript makes animating components really easy!