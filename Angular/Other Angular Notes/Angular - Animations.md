## Angular - Animations

# Adding the animations module to Angular 4 project:
- You probably need to install the new animations package (running the command never hurts): npm install --save @angular/animations
- Add the BrowserAnimationsModule  to your imports[]  array in AppModule. This Module needs to be imported from @angular/platform-browser/animations'  => import { BrowserAnimationsModule } from '@angular/platform-browser/animations' (in the AppModule!)
- You then import trigger , state , style  etc from @angular/animations instead of @angular/core


# Setting animations up in the components

Angular animations kick in when something switches from one state to another. State is usually a property of the component. These states are set up in the `trigger()` function.
Transition takes a string which indicates which way the animation should go. For back and forth use the <=> arrow.
Animate is a method that at it's most basic takes a number which is the amount of milliseconds the transition should take place over.

```
import { trigger, state, style, transition, animate } from "@angular/animations";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  animations: [
    trigger('divState', [
      state('normal', style({
        'background-color': 'red',
        transform: 'translateX(0)'
      })),
      state('highlighted', style({
        backgroundColor: 'blue',
        transform: 'translateX(100px)'
      })),
      transition('normal => highlighted', animate(300)),
      transition('highlighted => normal', animate(600)),
    ])
  ]
})

export class AppComponent {
  list = ['Milk', 'Sugar', 'Bread'];
  state = 'normal'; // angular animation transfers from one state to another

	onAdd(item) {
		this.list.push(item);
	}

	onDelete(item) {
		this.list.splice(this.list.indexOf(item), 1);
  }

  onAnimate() {
    this.state == 'normal' ? this.state = 'highlighted' : this.state = 'normal';
  }
}

In the HTML:

<div [@divState]="state"></div> // the condition needs to bound to a property

```

## Adding a transition state

We can create a new div in the html and bind it to a new state in order to go through transition phases. If we called the state `shrunken`, we could then add a transition as such: `transition('shrunken <=> *', animate(500))`. This essentially means that to and from the shrunken state we will perform that animation.

## Advanced animations
We can pass a second argument to `animate()` which is the `style()` function. This may give us jagged transitions.
Instead we can pass in several other arguments in an array in order to dictatate how the transition takes place:

```
transition('shrunken <=> *', [
        style({
          'background-color': 'orange'
        }),
        animate(1000, style({
          borderRadius: '50px'
        })),
        animate(500)
       ]
      )
    ])
```

## The void state

```
transition('void => *', [
        style({
          opacity: 0,
          transform: 'translateX(-100px)'
        }),
        animate(300)
      ]),
      transition('* => void', [
        animate(300, style({
          transform: 'translateX(100px)',
          opacity: 0
        }))
      ]) // if the element didn't have a state to start with you should use the void state.
      // when using the void state we need to declare a style that the item has before the state is triggered
    ])

```

## Using keyframes for animations

The keyframes method in the animate function allows us to be more precise as to which styles are applied at which point.

```
trigger('list2', [
      state('in', style({
        opacity: 1,
        transform: 'translateX(0)'
      })),
      transition('void => *', [
        animate(1000, keyframes([ // Keyframes allow us to specify what happens at which point in the animation.
          style({
            transform: 'translateX(-100px)',
            opacity: 0,
            offset: 0 // offset allows us to specify when in the time frame for a style to be applied
          }),
          style({
            transform: 'translateX(-50px)',
            opacity: 0.5,
            offset: 0.3
          }),
          style({
            transform: 'translateX(-20px)',
            opacity: 1,
            offset: 0.8
          }),
          style({
            transform: 'translateX(0px)',
            opacity: 1,
            offset: 1
          })
        ]))
      ]),
      transition('* => void', [
        animate(300, style({
          transform: 'translateX(100px)',
          opacity: 0
        }))
      ])
    ])
```

## Grouping transitions

```
transition('* => void', [
  group([
    animate(300, style({
      transform: 'translateX(100px)',
      opacity: 0
    })),
    animate(100, style({
      color: 'red'
    }))
  ])
])
```
