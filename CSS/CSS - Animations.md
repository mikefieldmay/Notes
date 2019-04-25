# CSS - Animations

There are 2 ways to animate elements: 
- Transitions
- Animations

## Transitions

Transmitions are built in animations. They are added by adding one property and setting a property to be watched if that property changes.
```
.modal {
  position: fixed;
  // display: none;
  opacity: 0; // we remove display none as we still want the element to be part of the document flow, but we want it to be invisible
  transform: translateY(-3rem);
  transition: opacity 200ms, transform 500ms;
  z-index: 200;
  top: 20%;
  left: 30%;
  width: 40%;
  background: white;
  padding: 1rem;
  border: 1px solid #ccc;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.5);
}
```

`transition` takes up to 4 values. The first one is required and is the property that we want to watch. You can also define the duration at which you want the transition to take place. It transitions smoothly from the watched value to the new value. It can also take timing functions such as ease-in start slow and end fast over the duration or the opposite ease-out. The fourth property is a delay to when the animation begins.
 `transition: property timeFrame timingFunction delay`.

transition: WHAT DURATION DELAY TIMING-FUNCTION; 

Example:

transition: opacity 200ms 1s ease-out; 

Can be translated to: "Animate any changes in the opacity  property (for the element to which the transition  property is applied) over a duration of 200ms. Start fast and end slow, also make sure to wait 1s before you start".

Instead of this shorthand, you can also specify the four individual properties:

1) transition-property  (https://developer.mozilla.org/en-US/docs/Web/CSS/transition-property => transition-property: opacity; 

2) transition-duration  (https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) => transition-duration: 200ms; 

3) transition-timing-function  (https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function) => transition-timing-function: ease-out; 

Possible timing function values are: ease-out , ease-in , linear , cubic-bezier()  and a couple of others. See the above link as well as the next lecture for more details.

4) transition-delay  (https://developer.mozilla.org/en-US/docs/Web/CSS/transition-delay) => transition-delay: 1s; 

You can read the official MDN article on CSS transitions here: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions

## Timing functions

There are Transition timing functions. (This cheat sheet is usefull)[https://easings.net/en]. You can pass in specific named properties or also `cubic-beziers()` which can dictate the movement of the component. The chrome developer tools also allow you to play with the animations.

## Transitions and display

Display cannot be transitioned. If you change it, it won't trigger a transition within a css class. There are some hacky ways to get around it though. 

## Animations

Animations are css transition ++. We get way more control of the transition with css animations. We are able to define certain keyframes and get totale control throughpout the entire animation.
You define keyframes that can be used in a css animation by using `@keyframes`.
```
@keyframes wiggle { // define the keyframe wiggle
  from { // the starting state
    transform: rotateZ(0); // css property is defined here, but not a css class. You can use any css property here.
  }
  to { // the ending state
    
  }
}
```

To add it to an element: 
```
.main-nav__item-cta {
    animation: wiggle 200ms 3s 8 forwards;
}
```

`animation` takes multiple arguments: animation name, duration, delay, number of times and how it goes back to the starting state and the fill state which tells the animation what state to keep. The great thing about animations is that you can use more than 2 keyframes

The animation  property is used as see in the previous video:

animation: NAME DURATION DELAY TIMING-FUNCTION ITERATION DIRECTION FILL-MODE PLAY-STATE; 

Example:

animation: wiggle 200ms 1s ease-out 8 alternate forwards running; 

Can be translated to: "Play the wiggle keyframe set (animation) over a duration of 200ms. Between two keyframes start fast and end slow, also make sure to wait 1s before you start. Play 8 animations and alternate after each animation. Once you're done, keep the final value applied to the element. Oh, and you should be playing the animation - not pausing."

Instead of this shorthand, you can also specify the individual properties:

1) animation-name  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-name => animation-name: wiggle; 

2) animation-duration  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-duration) => animation-duration: 200ms; 

3) animation-timing-function  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timing-function) => animation-timing-function: ease-out; 

Possible timing function values are: ease-out , ease-in , linear , cubic-bezier()  and a couple of others. See the above link for more details.

4) animation-delay  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-delay) => animation-delay: 1s; 

5) animation-iteration-count  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-iteration-count) => animation-iteration-count: 8; 

6) animation-direction  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-direction) => animation-direction: alternate; 

7) animation-fill-mode  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-fill-mode) => animation-fill-mode: forwards; 

8) animation-play-state  (https://developer.mozilla.org/en-US/docs/Web/CSS/animation-play-state) => animation-play-state: running; 

You can read the official MDN article on CSS animations here: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations

## Adding Multiple keyframes

Using a percentage value allowas us to add multiple keyframes which give us greater control of our animation:
```
@keyframes wiggle {
  0% {
    transform: rotateZ(0);
  }
  50% {
    transform: rotateZ(-10deg);
  }
  100% {
    transform: rotateZ(10deg);
  }
}

animation: wiggle 400ms 3s 8 ease-out;
```
You can also use a timing function to dictate how you want a tansition to take place. 

## Animation event listeners

You can add event listeners to elements that have an animation property: 
```
ctaButton.addEventListener('animationstart', (event) => console.log('animation started', event))
ctaButton.addEventListener('animationended', (event) => console.log('animationended', event))
ctaButton.addEventListener('animationiteration', (event) => console.log('animation iteration', event))
```