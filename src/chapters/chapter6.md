## Extensibility and plugins

In this workshop you will learn how to create your own UI components and also how to create plugins which will use native iOS and Android functionality.


### Your first UI Custom UI Component

Start by creating a new ng project.

```
tns create test-ng-rc7 --template https://github.com/NativeScript/template-hello-world-ng
```

We will start be creating a `hello world` type component first, then add it to the `app.component`.

<h4 class="exercise-start">
    <b>Exercise</b>: Set up the first component
</h4>

First create a new file called `fun-label.ts` in the `app` and add the following code. 
``` TypeScript
import {Component} from "@angular/core";

@Component({
    selector: "FunLabel",
    template: `
    <Label text="FunLabel">
    </Label>
    `
})
export class FunLabel {

}
```

This component is a simple @Component that contains NativeScript label saying "FunLabel".

Now let's add `FunLabel` to app.component.html. Add the following line below the label
``` html
<FunLabel> </FunLabel>
```

But that is not enough to make it work just yet.
The final step is to add the `FunLabel` declaration to the `@NgModule` in `main.ts`
First add this import line to the rest of the imports.
``` TypeScript
import {FunLabel} from "./fun-label";
```

and now add `FunLabel` to declarations
``` TypeScript
declarations: [AppComponent, FunLabel],
```

<div class="solution-start"></div>
``` TypeScript
import { platformNativeScriptDynamic, NativeScriptModule } from "nativescript-angular/platform";
import { NgModule } from "@angular/core";
import { AppComponent } from "./app.component";
import {FunLabel} from "./fun-label";

@NgModule({
    declarations: [AppComponent, FunLabel],
    bootstrap: [AppComponent],
    imports: [NativeScriptModule],
})
class AppComponentModule {}

platformNativeScriptDynamic().bootstrapModule(AppComponentModule);
```
<div class="solution-end"></div>

Now build the app and you should see the nice and shiny `FunLabel`.

<div class="exercise-end"></div>


The next step is to add binding property to our FunLabel.
<h4 class="exercise-start">
    <b>Exercise</b>: Property Binding
</h4>

Open `fun-label.ts`
We will call the new property `line` and bind it to the `text` of the underlying `<Label>'.

This can be accomplished in three steps.

1) Add Input to the imports
``` TypeScript
import {Component, Input} from "@angular/core
```

2) Change the `text="FunLabel"` to
``` TypeScript
    <Label 
        [text]="line">
    </Label>
```

3) Add @Input property ins
 (you can read more about @Inputs properties in the [Angular docs](https://angular.io/docs/ts/latest/guide/template-syntax.html#!#inputs-outputs)) 
``` TypeScript
export class FunLabel {
    @Input() public line: string = "";
}
```

<div class="solution-start"></div>
This is how the `fun-label.ts` should look now.
``` TypeScript
import {Component, Input} from "@angular/core";

@Component({
    selector: "FunLabel",
    template: `
    <Label 
        [text]="line">
    </Label>
    `
})
export class FunLabel {
    @Input() public line: string = "";
}
```
<div class="solution-end"></div>

To test it out, update the `FunLabel` in app.component.html to the below code and run the app.
``` html
    <FunLabel 
        [line]="message"
        class="message">
    </FunLabel>
```

<div class="exercise-end"></div>


Now let's make the FunLabel a bit fun and add some animations.


<h4 class="exercise-start">
    <b>Exercise</b>: Add animations
</h4>

To perform animations we need to get hold of the `Label` object first.

We will do it in few steps:

1) Add the required imports for ViewChild, ElementRef and View.

``` TypeScript
import {Component, Input, ViewChild, ElementRef} from "@angular/core";
import {View} from "ui/core/view";
```

2) Add variable `#myLabel` definion to the label and call animate on tap
``` TypeScript
    <Label #myLabel
        [text]="line"
        (tap)="animate()">
    </Label>
```

3) Add the label reference to the call
``` TypeScript
export class FunLabel {
    @Input() public line: string = "";
    @ViewChild("myLabel") labelRef: ElementRef;
}
```

4) Next we can create a getter that will return the NativeScript label object
``` TypeScript
export class FunLabel {
    @Input() public line: string = "";
    @ViewChild("myLabel") labelRef: ElementRef;

    private get view() : View {
        return <View>this.labelRef.nativeElement;
    }    
}
```

5) Finally we need the function to animate.
Add the following code to the `FunLabel` class
``` TypeScript
    private animate() {
        this.zoom();
    }

    public zoom() {
        this.view.animate({ scale: { x: 0.5, y: 0.5 }  })
            .then(() => 
                this.view.animate({ scale: { x: 2, y: 2 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 1, y: 1 }, duration: 2000  })
            )
    }
```

<div class="solution-start"></div>
This is how `fun-label.ts` should look at this point.
``` TypeScript
import {Component, Input, ViewChild, ElementRef} from "@angular/core";
import {View} from "ui/core/view";

@Component({
    selector: "FunLabel",
    template: `
    <Label #myLabel
        [text]="line"
        (tap)="animate()">
    </Label>
    `
})
export class FunLabel {
    @Input() public line: string = "";
    @ViewChild("myLabel") labelRef: ElementRef;

    private get view() : View {
        return <View>this.labelRef.nativeElement;
    }

    private animate() {
        this.zoom();
    }

    public zoom() {
        this.view.animate({ scale: { x: 0.5, y: 0.5 }  })
            .then(() => 
                this.view.animate({ scale: { x: 2, y: 2 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 1, y: 1 }, duration: 2000  })
            )
    }
}
```
<div class="solution-end"></div>

<div class="exercise-end"></div>


descrition of the exercise
<h4 class="exercise-start">
    <b>Bonus Exercise</b>: Add animationType property
</h4>

Using the below animation functions, add `animationType` @Input property.
Now the `animate` function should play one of the below functions.

``` TypeScript
    public zoom() {
        this.view.animate({ scale: { x: 0.5, y: 0.5 }  })
            .then(() => 
                this.view.animate({ scale: { x: 2, y: 2 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 1, y: 1 }, duration: 2000  })
            )
    }

    public spin() {
        this.view.animate({ rotate: 1080, duration: 750 })
            .then(() => 
                this.view.animate({ rotate: 0, duration: 750 } )
            )
    }

    public move() {
        this.view.animate({ translate: { x: 100, y: 0 }  })
            .then(() => 
                this.view.animate({ translate: { x: 100, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 0 }  })
            )
    }
    public fade() {
        this.view.animate({ opacity: 0  })
            .then(() => 
                this.view.animate({ opacity: 1  })
            )
    }
```

This is how the property should be set in `app.component.html`

``` html
    <FunLabel 
        [line]="message"
        class="message"
        animationType="fade">
    </FunLabel>
```

<div class="solution-start"></div>
Here is the full solution:
``` TypeScript
import {Component, Input, ViewChild, ElementRef} from "@angular/core";
import {View} from "ui/core/view";

@Component({
    selector: "FunLabel",
    template: `
    <Label #myLabel
        [text]="line"
        (tap)="animate()">
    </Label>
    `
})
export class FunLabel {
    @Input() public line: string = "";
    @ViewChild("myLabel") labelRef: ElementRef;
    @Input() public animationType: string = "fade";

    private get view() : View {
        return <View>this.labelRef.nativeElement;
    }

    private animate() {
        switch(this.animationType.toLowerCase()) {
            case "spin":
                this.spin();
                break;
            case "zoom":
                this.zoom();
                break;
            case "move":
                this.move();
                break;
            case "fade":
                this.fade();
                break;
            default:
                this.spin();
        }
    }

    public zoom() {
        this.view.animate({ scale: { x: 0.5, y: 0.5 }  })
            .then(() => 
                this.view.animate({ scale: { x: 2, y: 2 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 1, y: 1 }, duration: 2000  })
            )
    }

    public spin() {
        this.view.animate({ rotate: 1080, duration: 750 })
            .then(() => 
                this.view.animate({ rotate: 0, duration: 750 } )
            )
    }

    public move() {
        this.view.animate({ translate: { x: 100, y: 0 }  })
            .then(() => 
                this.view.animate({ translate: { x: 100, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 0 }  })
            )
    }
    public fade() {
        this.view.animate({ opacity: 0  })
            .then(() => 
                this.view.animate({ opacity: 1  })
            )
    }
}
```
<div class="solution-end"></div>

<div class="exercise-end"></div>


The final part of creating the `FunLabel` is to raise an event every time the animation is finished.
<h4 class="exercise-start">
    <b>Exercise</b>: Adding event handlers
</h4>

1) Let's start with the usual of adding new imports `Output` and `EventEmitter`

``` TypeScript
import {Component, Input, ViewChild, ElementRef, Output, EventEmitter} from "@angular/core";
```

2) Now we need to add `@Output` event property.
When defining an Output property, you can give it a `prop-name` (this is how it will referenced in the parent component) and `return-type` (this is the type that is expected to be returned)
@Output public `prop-name`: EventEmitter<`return-type`> = new EventEmitter<`return-type`>();

Add the following `@Output` property below all the `@Input` properties. Btw. the order is not really important.
``` TypeScript
@Output() public animateFinished: EventEmitter<number> = new EventEmitter<number>();
```

3) In order to raise the event we just need to call `eventname.next(param)`.
For this exercise we will add a `counter` property and return a new value each time an animation is complete.
Then we will wrap it nicely in `emitAnimateFinished()` function 
``` TypeScript
    private counter: number = 0;
    private emitAnimateFinished() {
        this.counter++;
        this.animateFinished.next(this.counter);
    }
```

4) Now the only thing that we have left is to call `this.emitAnimateFinished()` each time the `animation()` function is complete.
Here is how to do it for `spin()`
And your task is to do the same for the rest of them (zoom, move and fade).
``` TypeScript

    public spin() {
        this.view.animate({ rotate: 1080, duration: 750 })
            .then(() => 
                this.view.animate({ rotate: 0, duration: 750 } )
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }
```

<div class="solution-start"></div>
The `FunLabel` class should look like this:
``` TypeScript
import {Component, Input, ViewChild, ElementRef, Output, EventEmitter} from "@angular/core";
import {View} from "ui/core/view";

@Component({
    selector: "FunLabel",
    template: `
    <Label #myLabel
        [text]="line"
        (tap)="animate()">
    </Label>
    `
})
export class FunLabel {
    @Input() public line: string = "";
    @ViewChild("myLabel") labelRef: ElementRef;
    @Input() public animationType: string = "fade";
    @Output() public animateFinished: EventEmitter<number> = new EventEmitter<number>();

    private get view() : View {
        return <View>this.labelRef.nativeElement;
    }

    private counter: number = 0;
    private emitAnimateFinished() {
        this.counter++;
        this.animateFinished.next(this.counter);
    }

    private animate() {
        switch(this.animationType.toLowerCase()) {
            case "spin":
                this.spin();
                break;
            case "zoom":
                this.zoom();
                break;
            case "move":
                this.move();
                break;
            case "fade":
                this.fade();
                break;
            default:
                this.spin();
        }
    }

    

    public spin() {
        this.view.animate({ rotate: 1080, duration: 750 })
            .then(() => 
                this.view.animate({ rotate: 0, duration: 750 } )
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }

    public move() {
        this.view.animate({ translate: { x: 100, y: 0 }  })
            .then(() => 
                this.view.animate({ translate: { x: 100, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }

    public zoom() {
        this.view.animate({ scale: { x: 0.5, y: 0.5 }  })
            .then(() => 
                this.view.animate({ scale: { x: 2, y: 2 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 1, y: 1 }, duration: 2000  })
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }
    public fade() {
        this.view.animate({ opacity: 0  })
            .then(() => 
                this.view.animate({ opacity: 1  })
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }
}
```
<div class="solution-end"></div>

To put the new event property to test we need to:

a) add an `onFinished(val)` function to `AppComponent` in `app.component.ts`

``` TypeScript
    public onFinished(val: boolean) {
        alert("Finished Animation:" + val);
    }
```

b) add `animateFinished` handler to `FunLabel` in  `app.compnent.html`
Notice the `$event` attribute passed in as the parameter. This is how you pass the value returned by the component.
``` html
    <FunLabel 
        [line]="message"
        class="message"
        animationType="fade"
        (animateFinished)="onFinished($event)">
    </FunLabel>
```

Now run the app. Each time an animation is complete you should get a nice popup with a counter.
<div class="exercise-end"></div>


NativeScript UI components can respond to other events like `Double Tap`, `Long Press`, `Swipe`, `Pan`, `Pinch`, `Rotation`, `Touch`

<h4 class="exercise-start">
    <b>Bonus Exercise</b>: Animate on swipe
</h4>
Read in the documenation how [Swipe](https://docs.nativescript.org/ui/gestures#swipe) works and implement different animation for swipe left, right, up and down.

<div class="solution-start"></div>
Here is the full solution. Have fun.
``` TypeScript
import {Component, Input, ViewChild, ElementRef, Output, EventEmitter} from "@angular/core";
import {View} from "ui/core/view";
import {SwipeGestureEventData} from "ui/gestures";

@Component({
    selector: "FunLabel",
    template: `
    <Label #myLabel
        [text]="line"
        (tap)="animate()"
        (swipe)="onSwipe($event)">
    </Label>
    `
})
export class FunLabel {
    @Input() public line: string = "";
    @ViewChild("myLabel") labelRef: ElementRef;
    @Input() public animationType: string = "fade";
    @Output() public animateFinished: EventEmitter<number> = new EventEmitter<number>();

    private get view() : View {
        return <View>this.labelRef.nativeElement;
    }

    private counter: number = 0;
    private emitAnimateFinished() {
        this.counter++;
        this.animateFinished.next(this.counter);
    }

    private animate() {
        switch(this.animationType.toLowerCase()) {
            case "spin":
                this.spin();
                break;
            case "zoom":
                this.zoom();
                break;
            case "move":
                this.move();
                break;
            case "fade":
                this.fade();
                break;
            default:
                this.spin();
        }
    }

    

    public spin() {
        this.view.animate({ rotate: 1080, duration: 750 })
            .then(() => 
                this.view.animate({ rotate: 0, duration: 750 } )
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }

    public move() {
        this.view.animate({ translate: { x: 100, y: 0 }  })
            .then(() => 
                this.view.animate({ translate: { x: 100, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 100 }  })
            )
            .then(() => 
                this.view.animate({ translate: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }

    public zoom() {
        this.view.animate({ scale: { x: 0.5, y: 0.5 }  })
            .then(() => 
                this.view.animate({ scale: { x: 2, y: 2 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 0, y: 0 }  })
            )
            .then(() => 
                this.view.animate({ scale: { x: 1, y: 1 }, duration: 2000  })
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }
    public fade() {
        this.view.animate({ opacity: 0  })
            .then(() => 
                this.view.animate({ opacity: 1  })
            )
            .then(() => 
                this.emitAnimateFinished()
            )
    }

    private onSwipe(args: SwipeGestureEventData) {
        switch(args.direction) {
            case 1: //Right
                this.view.animate({ translate: { x: 100, y: 0 }, rotate: 90  })
                .then(() => 
                    this.view.animate({ translate: { x: 0, y: 0 }, rotate: 0  })
                )
                break;
            case 2: //Left
                this.view.animate({ translate: { x: -100, y: 0 }, rotate: -90  })
                .then(() => 
                    this.view.animate({ translate: { x: 0, y: 0 }, rotate: 0  })
                )
                break;
            case 4: //Up
                this.view.animate({ translate: { x: 0, y: -100 }, scale: { x: 1, y: 3}  })
                .then(() => 
                    this.view.animate({ translate: { x: 0, y: 0 }, scale: { x: 1, y: 1}   })
                )
                break;
            case 8: //Down
                this.view.animate({ translate: { x: 0, y: 100 }, scale: { x: 1, y: -1}   })
                .then(() => 
                    this.view.animate({ translate: { x: 0, y: 0 }, scale: { x: 1, y: 1}   })
                )
                break;
        }
    }

}
```
<div class="solution-end"></div>

<div class="exercise-end"></div>


### Creating a two way binding component



### Creating a Native plugin
