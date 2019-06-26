# Make Art with JavaScript

## Example

![sample](https://i.imgur.com/4BybKWa.png)

## Getting Started

We'll be working out of this template on Codepen: https://codepen.io/ipnicholson/pen/OeRZOL

Click the green "Create Pen from this Template" button to start. You may have to sign in to Codepen (Use your GitHub account 😀)

![](https://i.imgur.com/HPXvGoA.png)

## p5.js: Where the Magic Happens
We are using [p5.js](https://p5js.org/), a JavaScript drawing library. A library is code that someone else (typically many people) have written for others to use. It's pre-loaded into the Codepen template for you

p5 uses two main functions, setup() and draw():
- setup runs once when the page first loads
- draw runs on a loop and happens many times

## Let's Write some Code
In that setup function, add this line:
`line(15, 25, 60, 60);`

It should look like this:

```js
function setup() {
  line(15, 25, 60, 60);
}
```

### How did that work?
Here is the Application Programming Interface (API) for line:
https://p5js.org/reference/#/p5/line

`line(x1, y1, x2, y2)`
Draw a line from `(x1,y1)` to `(x2, y2)`


### Now make a box!

Let's consult the documentation!

https://p5js.org/reference/

Square -> Rectangle -> Rect

We found the right function in the p5 API, rect(). Let's use it:
`rect(60, 60, 80, 80)`

```js
function setup() {
  line(15, 25, 60, 60);
  rect(60, 60, 80, 80);
}
```

#### Uh oh... why do i just have a stick figures legs?

The problem is our canvas is too small!

Lets make it bigger:

`createCanvas(800, 800);`

```js
function setup() {
  createCanvas(800, 800);
  line(15, 25, 60, 60);
  rect(60, 60, 80, 80);
}
```

And so we don't run into this problem again, let's make our background a
different color

`background(50, 100, 50);`

```js
function setup() {
  createCanvas(800, 800);
  background(50, 100, 50);
  line(15, 25, 60, 60);
  rect(60, 60, 80, 80);
}
```

### Ok, how bout a circle?
Back to the documentation!

An ellipse can be a circle!
https://p5js.org/reference/#/p5/ellipse

`ellipse(60, 60, 80, 80);`

```js
function setup() {
  createCanvas(800, 800);
  background(50, 100, 50);
  line(15, 25, 60, 60);
  rect(60, 60, 80, 80);
  ellipse(60, 60, 80, 80);
}
```

It's curious that we gave the same coordinates to both rect and ellipse but they
rendered in different spots...or did they?

### Remove the fill

To see more clearly what is going on, let's remove that fill:

https://p5js.org/reference/#/p5/noFill

`noFill();`

```js
function setup() {
  nofill();
  createCanvas(800, 800);
  background(50, 100, 50);
  line(15, 25, 60, 60);
  rect(60, 60, 80, 80);
  ellipse(60, 60, 80, 80);
}
```

Ah! The _center_ of the circle is the _top left_ corner of the square...

This is a good thing to know, different object have different origins!


## Let's Make an Interactive Thing!
Ok this is cool and all but, honestly, I can do this in ms paint... why is art
in JS better?

### The Draw Function
This is hooked up to something called the `animation frame`, which is
responsible for keeping your app running at 60 frames per second.

Lets get a circle to go through the middle of the screen!

### Drawing a circle
In our `draw` function, let's add a circle:
- with a radius of `20`
- vertically centered
- all the way to the left

`ellipse(0, height/2, 20);`

```js
function draw() {
  ellipse(0, height/2, 20);
}
```

That's great!

But where did that `height` come from? Magic?

No! The API!
https://p5js.org/reference/#/p5/height

`height` is part of the environment provided to us by `p5`.

That's great but the bigger question here is this:
- How do we get that to move to the right?
- What do we need to change?

### Moving the circle to the right
This is a big step for us, so pay close attention!

#### Using a global variable, introducing "State"
At the top of our file, let's add a variable:

```js
var x = 0;
```
We will use this to keep track of our position, what "state" it is in.

Now let's use that in our draw function:

```js
function draw() {
  ellipse(x, height/2, 20);
}
```

...Fantastic?

Why didn't that work?

#### Incrementing 'x'

We never made `x` bigger, we should do that, but how!!?!?!
```js
function draw() {
  ellipse(x, height/2, 20);
  console.log(x);
}
```

The `draw` function is being called by `p5`, we should use this to make our x
increase!


```js
function draw() {
  ellipse(x, height/2, 20);
  x = x + 5;
  console.log(x);
}
```

HOLY.
CATS.

That worked...THAT WORKED!!!!!

But it is working too well...let's make it wrap!

### Making our circle wrap
When the circle hits the end of the screen, let's make it go down a line and
move from the left to right again.

Let's decode this request to something that is easier to understand...

When our `x` variable is larger than the `width` of our `canvas`, increase the `y`
position of the `ellipse` and reset our `x` to `0`;

This tells us two things:
1) We need more state to describe our system, `y` being that state
2) We need to have some sort of `conditional logic`

#### Adding y
At the top of our file, let's add `y`
```js
var x = 0;
var y = 0;
```

And let's make our `ellipse` use it:

```js
function draw() {
  ellipse(x, y, 20);
  x = x + 5;
  console.log(x);
  console.log(y);
}
```

And lets see what that changed?

Now our circle runs across the top of the screen!

Let's get it to update `y` when the position of the circle is at the right of
the canvas

#### Adding an 'if' statement
A big part of programming is something called conditional logic, and here is a
perfect place to use it.

Let's think about it in English:
If we get to the right of the screen, go to a new line.

Right?

Let's make it more programmery

```js
if ( weAreAtTheRightSideOfTheScreen ) {
  y = y + 50;
}
```

but what is `weAreAtTheRightSideOfTheScreen`?

How can we figure that out?

#### Comparing values
We can tell we are at the full width by using a comparison operator
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators

These are really useful and are the building blocks of _all_ programming
languages.

What do we compare?
That x is greater than 800!

or in code:
`x > 800;`

Let's use this!

```js
if ( x > 800) {
  y = y + 50;
}
```

Let's make it read more like English:

```js
var weAreAtTheRightSideOfTheScreen = x > 800;
if ( weAreAtTheRightSideOfTheScreen ) {
  y = y + 50;
}
```

And since we are using `800` twice, let's pull that into a variable as well;

```js
var x = 0;
var y = 0;
var myCanvasWidth = 800;
var myCanvasHeight= 800;
```

Woo! Ok, so what did we do?

Our code should look like this now:
```js
var x = 0;
var y = 0;
var myCanvasWidth = 800;
var myCanvasHeight = 800;

function setup() {
  noFill();
  createCanvas(myCanvasWidth, myCanvasHeight);
  background(50, 100, 50);
  line(15, 25, 60, 60);
  rect(60, 60, 80, 80);
  ellipse(60, 60, 80, 80);
}

function draw() {
  ellipse(x, y, 20, 20);
  x = x + 5;

  var weAreAtTheRightSideOfTheScreen = x > myCanvasWidth;
  if (weAreAtTheRightSideOfTheScreen) {
    y = y + 100;
  }
  console.log(x);
  console.log(y);
}
```

Why didn't that work?

Let's look at our requirement again:
When our `x` variable is larger than the `width` of our `canvas`, increase the `y`
position of the `ellipse` and reset our `x` to `0`;

> Also, check out the conosle, that should also give you a hint as to what is
> going on :)

Doh! We never reset `x` to `0`!
`x = 0;`

```js
function draw() {
  ellipse(x, y, 20, 20);
  x = x + 5;

  var weAreAtTheRightSideOfTheScreen = x > myCanvasWidth;
  if (weAreAtTheRightSideOfTheScreen) {
    y = y + 100;
    x = 0;
  }
  console.log(x);
  console.log(y);
}
```

How cool is this?!?!?!?
We got ourselves a bonafide movin n groovin web thingy!

### Stopping the loop
An important part of programming (and life) is knowing when to stop

In this case, it is when we get to the bottom of the page.
Let's write our requirement!

When we get to the bottom of the page, stop calling the draw function.

...hummm....

The first part of that seems fine, we can deal with that:
```js
if ( y > myCanvasHeight ){
  // do something
}
```
but that second part seems like it is outside of our control...or is it...?

#### To the docs!
Whenever something seems out of your control, look at the docs and see what you
can find!

Since we are really smart and lucky, we guess that it starts with `no` just like
`noFill` did:
https://p5js.org/reference/#/p5/noLoop

`noLoop();`

```js
if ( y > myCanvasHeight ){
  noLoop();
}
```


### Cleanup the Setup
Remove the line, rect and ellipse from the setup() function
Your code should look like this:

```js
var x = 0;
var y = 0;
var myCanvasWidth = 800;
var myCanvasHeight = 800;

function setup() {
  noFill();
  createCanvas(myCanvasWidth, myCanvasHeight);
  background(50, 100, 50);
}

function draw() {
  ellipse(x, y, 20, 20);
  x = x + 5;

  var weAreAtTheRightSideOfTheScreen = x > myCanvasWidth;
  if (weAreAtTheRightSideOfTheScreen) {
    y = y + 100;
    x = 0;
  }

  if ( y > myCanvasHeight ) {
    noLoop();
  }
}
```

It's crazy how simple our code is for what it is doing!

But it's gonna get EVEN better!!!
Let's add some user interaction!

## Adding Some Interaction
The interaction I want to add is basically making a storm stopper:
[](https://c2.staticflickr.com/6/5222/5869691167_0130cf97f3_b.jpg)

Try and click the circle as it is moving!

As we always should, look at the documentation and see if p5 has anything that
can help us with this!

It sure does!
https://p5js.org/reference/#/p5/mousePressed

Let's give that function some directions!

### Implementing the mousePressed function
This mousePressed function is provided to us by p5.
We can make it whatever we would like, but it the commands in this function only
happen when the mouse is pressed.

Let's do something simple, like display an ellipse when the mouse is pressed:

```js
function mousePressed() {
  ellipse(myCanvasWidth/2, myCanvasHeight/2, 40, 40);
}
```

### Drawing the circle under your mouse
Just like with `height` we can get the `X` and `Y` position of our mouse at any
time through p5.

Let's use those built in `Environment` variables to our advantage and draw our
ellipse where our mouse is!

`ellipse(mouseX, mouseY, 40, 40);`

```js
function mousePressed() {
  ellipse(mouseX, mouseY, 40, 40);
}
```

That's great but I only want one circle at a time, how do I clear our the
screen?

### Clearing out the Screen
A very important function is the `clear` function:
https://p5js.org/reference/#/p5/clear

It allows us to clear out the canvas;
`clear();`

```js
function mousePressed() {
  clear();
  ellipse(mouseX, mouseY, 40, 40);
}
```

Well that works, kind of...

Where did the background go?!?!

### Adding the background again
We can make a function that fills our background for us!

```js
function makeBackground() {
  background(50, 100, 50);
}
```

and we can call it in `mousePressed` and `setUp` to keep the background green!

```js
function mousePressed() {
  clear();
  makeBackground();
  ellipse(mouseX, mouseY, 40, 40);
}
```

### Coloring our new circle
Let's add some color, a nice blue, to our new circle using the `fill()` function
https://p5js.org/reference/#/p5/fill

```js
function mousePressed() {
  clear();
  makeBackground();
  fill(0, 0, 200);
  ellipse(mouseX, mouseY, 40, 40);
}
```

### Detecting if we clicked the circle snake thing
Here is our requirement:
Is the `mouseX` and `mouseY` coordinate of my mouse within `20` pixels of the origin of the latest known `x` and `y` coordinates?

It is much easier to think about this problem in terms of our two directions, X
and Y:
- is `mouseX` within 40 of `x`
- is `mouseY` within 40 of `y`

In code this might look like this:
```js
var bottomBoundaryX = x - 20;
var topBoundaryX = x + 20;
var isNearX = ( mouseX > bottomBoundaryX ) && ( mouseX < topBoundaryX );

var bottomBoundaryY = y - 20;
var topBoundaryY = y + 20;
var isNearY = ( mouseY > bottomBoundaryY ) && ( mouseY < topBoundaryY );
```

### Determining if we should show the "success" message
Our requirement:
If it is near the x origin and near the y origin print out the text

```js
if(isNearX && isNearY) {
  textSize(32);
  fill(200, 0, 0);
  text("success! ", 20, 200);
}
```

We need to establish a text size and color before we call `text`

And that is it!

Here is our super duper mvp!

# Finished Game!
```js
var x = 0;
var y = 0;
var myCanvasWidth = 800;
var myCanvasHeight = 800;

function setup() {
  noFill();
  createCanvas(myCanvasWidth, myCanvasHeight);
  makeBackground();
}

function makeBackground() {
  background(50, 100, 50);
}

function draw() {
  noFill();
  ellipse(x, y, 20, 20);
  x = x + 5;

  var weAreAtTheRightSideOfTheScreen = x > myCanvasWidth;
  if (weAreAtTheRightSideOfTheScreen) {
    y = y + 100;
    x = 0;
  }

  if ( y > myCanvasHeight ) {
    noLoop();
  }
}


function mousePressed() {
  clear();
  makeBackground();
  fill(0, 0, 200);
  ellipse(mouseX, mouseY, 40, 40);
  textSize(32);
  fill(0, 102, 153);

  var bottomBoundaryX = x - 20;
  var topBoundaryX = x + 20;
  var isNearX = ( mouseX > bottomBoundaryX ) && ( mouseX < topBoundaryX );

  var bottomBoundaryY = y - 20;
  var topBoundaryY = y + 20;
  var isNearY = ( mouseY > bottomBoundaryY ) && ( mouseY < topBoundaryY );

  if (isNearX && isNearY) {
    textSize(32);
    fill(200, 0, 0);
    text("success! ", 20, 200);
  }
}
```

# Additional Features
- Make the snake go in a circle
  - oscillate the radius
  - increase / decrease the speed
- keep a score
- Make the snake go in a random direction
- track longest combo
- increase the difficulty
