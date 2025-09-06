<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Performant Animations Using KUTE.js: Part 1, Getting Started</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p><a href="https://thednp.github.io/kute.js/">KUTE.js</a> is a JavaScript-based animation engine which focuses on performance and memory 
efficiency while animating different elements on a webpage. I have already written a series 
on using <a href="https://code.tutsplus.com/series/javascript-based-animations-using-animejs--cms-1193">
Anime.js to create JavaScript-based animations</a>. This time we will learn about KUTE.js 
and how we can use it to animate CSS properties, SVG, and text elements, among other things.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Installation</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Before we dive into some examples, let's install the library first. KUTE.js has a core 
engine, and then there are plugins for animating the value of different CSS properties, 
SVG attributes, or text. You can directly link to the library from popular CDNs like 
<a href="https://cdnjs.cloudflare.com/ajax/libs/kute.js/1.6.2/kute.min.js">cdnjs</a> 
and <a href="https://cdn.jsdelivr.net/kute.js/1.6.2/kute.min.js">jsDelivr</a>.</p>

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/kute.js/1.6.2/kute.min.js"></script>
<script src="https://cdn.jsdelivr.net/kute.js/1.6.2/kute.min.js"></script>
```

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Tween Objects</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>When creating your animation using KUTE.js, you need to define tween objects. These tween 
objects provide all the animation-related information for a given element or elements. This 
includes the element itself, the properties that you want to animate, the duration of the 
animation, and other attributes like the repeat count, delay, or offset.</p>

<p>You can use the .to() method or the .fromTo() method in order to animate a set of CSS 
properties from one value to another. The .to() method animates the properties from 
their default value or their computed/current value to a final provided value. In the 
case of the .fromTo() method, you have to provide both the starting and ending animation values.</p>

<p>The .to() method is useful when you don't know the current or default value for the 
property that you want to animate. One major disadvantage of this method is that the 
library has to compute the current value of all the properties by itself. This results 
in a delay of a few milliseconds after you call .start() to start the animation.</p>

<p>The .fromTo() method allows you to specify the starting and ending animation values 
yourself. This can marginally improve the performance of the animations. You can now 
also specify the units for starting and ending values yourself and avoid any surprises 
during the course of the animation. One disadvantage of using .fromTo() is that you 
won't be able to stack multiple transform properties on chained tweens. In such cases, 
you will have to use the .to() method.</p>

<p>Remember that both .fromTo() and .to() are meant to be used when you are animating 
individual elements. If you want to animate multiple elements at once, you will have 
to use either .allTo() or .allFromTo(). These methods work just like their single 
element counterparts and inherit all their attributes. They also get an extra offset 
attribute that determines the delay between the start of the animation for different 
elements. This offset is defined in milliseconds.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p><a href="https://codepen.io/Shokeen/pen/ZXjrpW">Here is an example that animates 
the opacity of three different boxes in sequence</a>.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The following JavaScript is used to create the above animation sequence:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- html -->
<pre>
&lt;div&gt;
  &lt;div class="box a"&gt;&lt;/div&gt;
  &lt;div class="box b"&gt;&lt;/div&gt;
  &lt;div class="box c"&gt;&lt;/div&gt;
&lt;/div&gt;
&lt;button class="start"&gt;Start Animation&lt;/button&gt;
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- css -->
<pre>
body {
  margin: 20px;
  font-family: 'Lato';
  font-weight: 300;
  text-align: center;
}

.box {
  width: 100px;
  height: 100px;
  display: inline-block;
  border: 2px solid black;
  margin: 10px;
  opacity: 0.1;
}

.a {
  background: orange;
}

.b {
  background: red;
}

.c {
  background: yellow;
}

button {
  background: wheat;
  border: 1px solid black;
  font-family: 'Lato';
  border-radius: 5px;
  padding: 8px;
  margin: 20px 0;
  outline: none;
  cursor: pointer;
}
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- js -->
<pre>
var theBoxes = document.querySelectorAll(".box");
var startButton = document.querySelector(".start");
var animateOpacity = KUTE.allFromTo(
  theBoxes,
  { opacity: 1 },
  { opacity: 0.1 },
  { offset: 700 }
);
startButton.addEventListener(
  "click",
  function() {
    animateOpacity.start();
  },
  false
);
</pre>

<p>All the boxes above have a box class which has been used to select them all using the querySelectorAll() 
method. The allFromTo() method in KUTE.js is used to animate the opacity of these boxes from 1 to 0.1 
with an offset of 700 milliseconds. As you can see, the tween object does not start the animation by 
itself. You have to call the start() method in order to start the animation.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Controlling the Animation Playback</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In the previous section, we used the start() method in order to start our animations. The KUTE.js 
library also provides a few other methods that can be used to control the animation playback.</p>

<p>For example, you can stop any animation that is currently in progress with the help of the stop() 
method. Keep in mind that you can use this method to stop the animation of only those tween 
objects that have been stored in a variable. The animation for any tween object that was created 
on the fly cannot be stopped with this method.</p>

<p>You also have the option to just pause an animation with the help of the pause() method. This is 
helpful when you want to resume the animation again at a later time. You can either use resume() 
or play() to resume any animation that was paused.</p>

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p><a href="https://codepen.io/Shokeen/pen/rGrZOR">The following example is an updated version 
of the previous demo with all the four methods added to it</a>.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io -->

```
<div>
  <div class="box a"></div>
  <div class="box b"></div>
  <div class="box c"></div>
</div>
<button class="start">Start Animation</button>
<button class="stop">Stop Animation</button>
<button class="pause">Pause Animation</button>
<button class="resume">Resume Animation</button>
```

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->

```css
body {
  margin: 20px;
  font-family: 'Lato';
  font-weight: 300;
  text-align: center;
}

.box {
  width: 100px;
  height: 100px;
  display: inline-block;
  border: 2px solid black;
  margin: 20px 10px;
  opacity: 0.1;
}

.a {
  background: orange;
}

.b {
  background: red;
}

.c {
  background: yellow;
}

button {
  background: wheat;
  border: 1px solid black;
  font-family: 'Lato';
  border-radius: 5px;
  padding: 8px;
  margin: 5px;
  outline: none;
  cursor: pointer;
}
```

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->

```js
var theBoxes = document.querySelectorAll(".box");
var startButton = document.querySelector(".start");
var stopButton = document.querySelector(".stop");
var pauseButton = document.querySelector(".pause");
var resumeButton = document.querySelector(".resume");

var animateOpacity = KUTE.allFromTo(
  theBoxes,
  { opacity: 1 },
  { opacity: 0.1 },
  { offset: 700, duration: 2000 }
);

startButton.addEventListener(
  "click",
  function() {
    animateOpacity.start();
  },
  false
);

stopButton.addEventListener(
  "click",
  function() {
    animateOpacity.stop();
  },
  false
);

pauseButton.addEventListener(
  "click",
  function() {
    animateOpacity.pause();
  },
  false
);

resumeButton.addEventListener(
  "click",
  function() {
    animateOpacity.resume();
  },
  false
);
```

<p>I have changed the animation duration to 2,000 milliseconds. This gives us enough time to press 
different buttons and see how they affect the animation playback.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Chaining Tweens Together</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>You can use the chain() method to chain different tweens together. Once different tweens have 
been chained, they call the start() method on other tweens after their own animation has finished.</p>

<p>This way, you get to play different animations in a sequence. You can chain different tweens with 
each other in order to play them in a loop. The following example should make it clear:</p>

```js
var animateOpacity = KUTE.allFromTo(
  theBoxes,
  { opacity: 1 },
  { opacity: 0.1 },
  { offset: 100, duration: 800 }
);
var animateRotation = KUTE.allFromTo(
  theBoxes,
  { rotate: 0 },
  { rotate: 360 },
  { offset: 250, duration: 800 }
);
opacityButton.addEventListener(
  "click",
  function() {
    animateOpacity.start();
  },
  false
);
rotateButton.addEventListener(
  "click",
  function() {
    animateRotation.start();
  },
  false
);
chainButton.addEventListener(
  "click",
  function() {
    animateOpacity.chain(animateRotation);
    animateOpacity.start();
  },
  false
);
loopButton.addEventListener(
  "click",
  function() {
    animateOpacity.chain(animateRotation);
    animateRotation.chain(animateOpacity);
    animateOpacity.start();
  },
  false
);
```

<p>We already had one tween to animate the opacity. We have now added another one that animates the 
rotation of our boxes. The first two buttons animate the opacity and the rotation one at a time. 
The third button triggers the chaining of animateOpacity with animateRotation.</p>

<p>The chaining itself doesn't start the animation, so we also use the start() method to start the 
opacity animation. The last button is used to chain both the tweens with each other. This time, 
the animations keep playing indefinitely once they have been started. Here is a CodePen demo 
that shows all the above code in action:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p><a href="https://codepen.io/Shokeen/pen/QqBZEe">Codepen.io</a></p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>To fully understand how chaining works, you will have to press the buttons in a specific 
sequence. Click on the Animate Opacity button first and you will see that the opacity 
animation is played only once and then nothing else happens. Now, press the Animate Rotation 
button and you will see that the boxes rotate once and then nothing else happens.</p>

<p>After that, press the Chain Animations button and you will see that the opacity animation 
plays first and, once it completes its iteration, the rotation animation starts playing 
all by itself. This happened because the rotation animation is now chained to the opacity 
animation.</p>

<p>Now, press the Animate Opacity button again and you will see that both opacity and rotation 
are animated in sequence. This is because they had already been chained after we clicked on 
Chain Animations.</p>

<p>At this point, pressing the Animate Rotation button will only animate the rotation. The 
reason for this behavior is that we have only chained the rotation animation to the opacity 
animation. This means that the boxes will be rotated every time the opacity is animated, 
but a rotation animation doesn't mean that the opacity will be animated as well.</p>

<p>Finally, you can click on the Play in a Loop button. This will chain both the animations 
with each other, and once that happens, the animations will keep playing in an indefinite 
loop. This is because the end of one animation triggers the start of the other animation.</p>

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Performant Animations Using KUTE.js: Part 2, Animating CSS Properties</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This tutorial will teach you how to animate different kinds of CSS properties using KUTE.js, 
using either the core engine or the CSS plugin.</p>

<p>This post is part of a series called Performant Animations Using KUTE.js.</p>

<p>KUTE.js is a JavaScript-based animation engine which focuses on performance and memory efficiency 
while animating different elements on a webpage. I have already written a series on using Anime.js 
to create JavaScript-based animations. This time we will learn about KUTE.js and how we can use it 
to animate CSS properties, SVG, and text elements, among other things.</p>

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Installation</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Before we dive into some examples, let's install the library first. KUTE.js has a core engine, 
and then there are plugins for animating the value of different CSS properties, SVG attributes, 
or text. You can directly link to the library from popular CDNs like cdnjs and jsDelivr.</p>

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/kute.js/1.6.2/kute.min.js"></script>
<script src="https://cdn.jsdelivr.net/kute.js/1.6.2/kute.min.js"></script>
```

<p>You can also install KUTE.js using either NPM or Bower with the help of the following commands:</p>

```
npm install --save kute.js
bower install --save kute.js
```

<p>Once you have included the library in your projects, you can start creating your own 
animation sequences.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Tween Objects</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>When creating your animation using KUTE.js, you need to define tween objects. These tween 
objects provide all the animation-related information for a given element or elements. This 
includes the element itself, the properties that you want to animate, the duration of the 
animation, and other attributes like the repeat count, delay, or offset.</p>

<p>You can use the .to() method or the .fromTo() method in order to animate a set of CSS 
properties from one value to another. The .to() method animates the properties from their 
default value or their computed/current value to a final provided value. In the case of the 
.fromTo() method, you have to provide both the starting and ending animation values.</p>

<p>The .to() method is useful when you don't know the current or default value for the property 
that you want to animate. One major disadvantage of this method is that the library has to compute 
the current value of all the properties by itself. This results in a delay of a few milliseconds 
after you call .start() to start the animation.</p>

<p>The .fromTo() method allows you to specify the starting and ending animation values yourself. 
This can marginally improve the performance of the animations. You can now also specify the units 
for starting and ending values yourself and avoid any surprises during the course of the animation. 
One disadvantage of using .fromTo() is that you won't be able to stack multiple transform properties 
on chained tweens. In such cases, you will have to use the .to() method.</p>

<p>Remember that both .fromTo() and .to() are meant to be used when you are animating individual 
elements. If you want to animate multiple elements at once, you will have to use either .allTo() 
or .allFromTo(). These methods work just like their single element counterparts and inherit all 
their attributes. They also get an extra offset attribute that determines the delay between the 
start of the animation for different elements. This offset is defined in milliseconds.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p><a href="https://codepen.io/Shokeen/pen/ZXjrpW">Here is an example that animates the opacity of 
three different boxes in sequence</a>.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The following JavaScript is used to create the above animation sequence:</p>

```
var theBoxes = document.querySelectorAll(".box");
var startButton = document.querySelector(".start");
var animateOpacity = KUTE.allFromTo(
  theBoxes,
  { opacity: 1 },
  { opacity: 0.1 },
  { offset: 700 }
);
startButton.addEventListener(
  "click",
  function() {
    animateOpacity.start();
  },
  false
);
```

<p>All the boxes above have a box class which has been used to select them all using the 
querySelectorAll() method. The allFromTo() method in KUTE.js is used to animate the opacity 
of these boxes from 1 to 0.1 with an offset of 700 milliseconds. As you can see, the tween 
object does not start the animation by itself. You have to call the start() method in order 
to start the animation.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Controlling the Animation Playback</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In the previous section, we used the start() method in order to start our animations. The 
KUTE.js library also provides a few other methods that can be used to control the animation 
playback.</p>

<p>For example, you can stop any animation that is currently in progress with the help of the 
stop() method. Keep in mind that you can use this method to stop the animation of only those 
tween objects that have been stored in a variable. The animation for any tween object that was 
created on the fly cannot be stopped with this method.</p>

<p>You also have the option to just pause an animation with the help of the pause() method. This 
is helpful when you want to resume the animation again at a later time. You can either use 
resume() or play() to resume any animation that was paused.</p>

<p>The following example is an updated version of the previous demo with all the four methods 
added to it.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Here is the JavaScript code needed to add the start, stop, play, and pause functionality.</p>

```
var theBoxes = document.querySelectorAll(".box");
var startButton = document.querySelector(".start");
var stopButton = document.querySelector(".stop");
var pauseButton = document.querySelector(".pause");
var resumeButton = document.querySelector(".resume");
var animateOpacity = KUTE.allFromTo(
  theBoxes,
  { opacity: 1 },
  { opacity: 0.1 },
  { offset: 700, duration: 2000 }
);
startButton.addEventListener(
  "click",
  function() {
    animateOpacity.start();
  },
  false
);
stopButton.addEventListener(
  "click",
  function() {
    animateOpacity.stop();
  },
  false
);
pauseButton.addEventListener(
  "click",
  function() {
    animateOpacity.pause();
  },
  false
);
resumeButton.addEventListener(
  "click",
  function() {
    animateOpacity.resume();
  },
  false
);
```

<p>I have changed the animation duration to 2,000 milliseconds. This gives us enough time to press 
different buttons and see how they affect the animation playback.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Chaining Tweens Together</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>You can use the chain() method to chain different tweens together. Once different tweens have 
been chained, they call the start() method on other tweens after their own animation has finished.</p>

<p>This way, you get to play different animations in a sequence. You can chain different tweens 
with each other in order to play them in a loop. The following example should make it clear:</p>

```js
var animateOpacity = KUTE.allFromTo(
  theBoxes,
  { opacity: 1 },
  { opacity: 0.1 },
  { offset: 100, duration: 800 }
);
var animateRotation = KUTE.allFromTo(
  theBoxes,
  { rotate: 0 },
  { rotate: 360 },
  { offset: 250, duration: 800 }
);
opacityButton.addEventListener(
  "click",
  function() {
    animateOpacity.start();
  },
  false
);
rotateButton.addEventListener(
  "click",
  function() {
    animateRotation.start();
  },
  false
);
chainButton.addEventListener(
  "click",
  function() {
    animateOpacity.chain(animateRotation);
    animateOpacity.start();
  },
  false
);
loopButton.addEventListener(
  "click",
  function() {
    animateOpacity.chain(animateRotation);
    animateRotation.chain(animateOpacity);
    animateOpacity.start();
  },
  false
);
```

<p>We already had one tween to animate the opacity. We have now added another one that animates 
the rotation of our boxes. To fully understand how chaining works, you will have to press the 
buttons in a specific sequence. Click on the Animate Opacity button first and you will see that 
the opacity animation is played only once and then nothing else happens. Now, press the Animate 
Rotation button and you will see that the boxes rotate once and then nothing else happens.</p>

<p>After that, press the Chain Animations button and you will see that the opacity animation 
plays first and, once it completes its iteration, the rotation animation starts playing all by 
itself. This happened because the rotation animation is now chained to the opacity animation.</p>

<p>Now, press the Animate Opacity button again and you will see that both opacity and rotation 
are animated in sequence. This is because they had already been chained after we clicked on 
Chain Animations.</p>

<p>At this point, pressing the Animate Rotation button will only animate the rotation. The 
reason for this behavior is that we have only chained the rotation animation to the opacity 
animation. This means that the boxes will be rotated every time the opacity is animated, but 
a rotation animation doesn't mean that the opacity will be animated as well.</p>

<p>Finally, you can click on the Play in a Loop button. This will chain both the animations 
with each other, and once that happens, the animations will keep playing in an indefinite 
loop. This is because the end of one animation triggers the start of the other animation. 
The first two buttons animate the opacity and the rotation one at a time. The third button 
triggers the chaining of animateOpacity with animateRotation.</p>

<p>The chaining itself doesn't start the animation, so we also use the start() method to 
start the opacity animation. The last button is used to chain both the tweens with each 
other. This time, the animations keep playing indefinitely once they have been started. 
Here is a CodePen demo that shows all the above code in action:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Performant Animations Using KUTE.js: Part 2, Animating CSS Properties</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This tutorial will teach you how to animate different kinds of CSS properties using 
KUTE.js, using either the core engine or the CSS plugin.</p>

<p>The first tutorial of the series focused on providing a beginner-friendly introduction 
to the KUTE.js library. In that tutorial, we only animated the opacity and rotateZ property 
for our elements. In this tutorial, you will learn how to animate the rest of the CSS 
properties using KUTE.js.</p>

<p>Some of these properties will require you to load the CSS plugin, while others can be 
animated using the core engine itself. Both these scenarios will be discussed separately 
in the tutorial.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Box Model Properties</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The core KUTE.js engine can animate only the most common box model properties: width, 
height, top, and left. You will have to use the CSS plugin to animate almost all other 
box model properties. Here is an example that animates the top position, width and height 
of our boxes from the previous tutorial:</p>

```
var animateTop = KUTE.allFromTo(
  theBoxes,
  { top: 0 },
  { top: 100 },
  { offset: 100 }
);
var animateA = KUTE.fromTo(
  boxA,
  { height: 100 },
  { height: 175 }
);
var animateB = KUTE.fromTo(
  boxB,
  { width: 100 },
  { width: 200 }
);
```

<p>You might have noticed that I used allFromTo() to animate the top property of all the boxes. 
However, I used fromTo() to animate individual boxes. You should remember that the boxes stay 
in their final state once the animation completes.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/ZXjgaZ  -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>With the help of the CSS plugin, you will be able to animate margin, padding, and borderWidth 
as well. Once you have included the plugin in your project, the rest of the process is exactly 
the same.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Animating Transform Properties</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>You can animate almost all the transform properties mentioned in the spec with the help of the 
core engine itself. There is no need to load the CSS plugin in this case.</p>

<p>You can animate the element translation in 2D space using translate. Similarly, you can use 
translateX, translateY, and translateZ in order to animate the elements along the respective 
axes. For the translateZ property to have any effect, you will also have to set a value for 
the parentPerspective property. Here is an example:</p>

```
var animateAll = KUTE.allFromTo(
  theBoxes,
  { translateY: 0 },
  { translateY: 100 },
  { offset: 1000 }
);
var animateA = KUTE.fromTo(
  boxA,
  { translateZ: 0 },
  { translateZ: 50 },
  { parentPerspective: 100, parentPerspectiveOrigin: "0% 0%" }
);
var animateB = KUTE.fromTo(
  boxB,
  { translateX: 0 },
  { translateX: -200 }
);
startButton.addEventListener(
  "click",
  function() {
    animateAll.start();
    animateA.start();
    animateB.start();
  },
  false
);
```

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>If you click the Start Animation button and observe the animation closely, you will see that 
the red box translates -200 in the X direction first. After that, it moves to its original 
position and starts translating in the Y direction. The reason for the box to animate translateX 
first is that we have added a delay for the translateY animation using the offset property.</p>

<p>Just like translation, you can also perform rotations along a specific axis using the rotate, 
rotateX, rotateY, and rotateZ properties. Since rotateX and rotateY are 3D rotations, you will 
have to use the perspective property for the rotation animation to work as expected. The 
following example shows how using the perspective property affects the overall animation 
for these two properties.</p>

```
var animateAll = KUTE.allFromTo(
  theBoxes,
  { rotate: 0 },
  { rotate: 360 },
  { offset: 1000 }
);
var animateA = KUTE.fromTo(
  boxA,
  { rotateY: 0 },
  { rotateY: 180 },
  { perspective: 100 }
);
var animateB = KUTE.fromTo(
  boxB,
  { rotateY: 0 },
  { rotateY: -180 }
);
startButton.addEventListener(
  "click",
  function() {
    animateAll.start();
    animateA.start();
    animateB.start();
  },
  false
);
```

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/YrOzPB -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In the above example, box A and box B start their rotation along the Y axis at the same time, 
but the resulting animation is very different because of the perspective property. You might have 
noticed that the orange box is not performing the rotation around its center that was applied to 
it using animateAll. This is because all animations have a duration of 500 milliseconds by default, 
and we are applying both animateAll and animateA on the orange box at the same time.</p>

<p>Since animateA is applied after animateAll, its animation takes precedence over animateAll. You 
will see that the common rotation using animateAll is still being applied on the orange box once 
you increase the animation duration. In other words, you cannot animate different transform 
properties using multiple tween objects at the same time. All the transform properties that you 
want to animate should be specified inside a single tween object. The following example should 
make it clear:</p>

```
// This will not work as expected 
var translateAll = KUTE.allFromTo(
  theBoxes,
  { translateY: 0 },
  { translateY: 100 },
  { offset: 1000 }
);
var rotateAll = KUTE.allFromTo(
  theBoxes,
  { rotate: 0 },
  { rotate: 360 },
  { offset: 1000 }
);
startButton.addEventListener(
  "click",
  function() {
    translateAll.start();
    rotateAll.start();
  },
  false
);
// This will work as expected 
var rtAll = KUTE.allFromTo(
  theBoxes,
  { translateY: 0, rotate: 0 },
  { translateY: 100, rotate: 360 },
  { offset: 1000 }
);
startButton.addEventListener(
  "click",
  function() {
    rtAll.start();
  },
  false
);
```

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io https://codepen.io/Shokeen/pen/MEqWOr -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Using the CSS Plugin</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>As I mentioned earlier, not all CSS properties can be animated using only the core KUTE.js 
engine. You need to use an extra CSS plugin to animate properties like padding, margin, 
background position of images, and other border-related properties. So, before you try any of 
the examples in this section, you should include the plugin in your project.</p>

```
<script src="https://cdn.jsdelivr.net/kute.js/1.6.2/kute-css.min.js"></script>
```

<p>Once you have included the plugin, you will be able to animate the border-radius property 
using borderRadius. You can also animate all the corner border-radius values individually 
using borderTopLeftRadius, borderTopRightRadius, borderBottomLeftRadius, and borderBottomRightRadius.</p>

<p>Here is an example that shows the animation in action.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/yzxyPG -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>If you click the Start Animation button, you will notice that the top left border-radius for the 
red and yellow box is animated after a delay. This is because of the offset property. The rest 
of the radii are animated as soon as we click on the button. The above example was created using 
the following code:</p>

```
var animateAll = KUTE.allFromTo(
  theBoxes,
  { borderTopLeftRadius:'0%' },
  { borderTopLeftRadius:'100%' },
  { offset: 1000 }
);
var animateA = KUTE.fromTo(
  boxA,
  { borderTopRightRadius:'0%' },
  { borderTopRightRadius:'100%' }
);
var animateB = KUTE.fromTo(
  boxB,
  { borderBottomLeftRadius:'0%' },
  { borderBottomLeftRadius:'100%' }
);
var animateC = KUTE.fromTo(
  boxC,
  { borderBottomRightRadius:'0%' },
  { borderBottomRightRadius:'100%' }
);
startButton.addEventListener(
  "click",
  function() {
    animateAll.start();
    animateA.start();
    animateB.start();
    animateC.start();
  },
  false
);
```

<p>We have not chained the tween objects together, so all the animations start at once this time. You 
can also animate the color of different borders in a similar manner using borderColor, borderTopColor, 
borderLeftColor, borderBottomColor, and borderRightColor.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Final Thoughts</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In this tutorial, we learned about different CSS properties that can be animated with and without the 
use of the KUTE.js CSS plugin. If you have any questions, please let me know in the comments.</p>

<p>The next tutorial will cover different animations that can be created using the KUTE.js SVG plugin.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Performant Animations Using KUTE.js: Part 3, Animating SVG</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This tutorial will teach you how to use the KUTE.js SVG plugin to morph SVG elements and animate the 
drawing of different shapes using strokes.</p>

<p>The previous tutorial of the series showed you how to animate different CSS properties of any element 
using KUTE.js. However, the core engine does not allow you to animate properties that are specific 
to SVG elements. Similarly, you can't animate the SVG morphing of different path shapes or the drawing 
of different SVG elements using strokes. You will have to use the KUTE.js SVG plugin to achieve any 
of these tasks.</p>

<p>Before we begin, keep in mind that you will have to include both the KUTE.js core engine and the SVG 
plugin for the examples in this tutorial to work.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Morphing SVG Shapes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Morphing one SVG shape into another is a very common feature that you will come across. The KUTE.js 
SVG plugin gives us everything that we need to create our own morphing animations with ease.</p>

<p>There are three ways to morph SVG shapes using this library:</p>
<ol type="1">
  <li>You can use the fromTo() method to specify both the initial and the final SVG path for 
    your element.</li>
  <li>You can also use the to() method and avoid specifying the initial path. In this case, 
    the start value for the morphing will be determined based on the value of the d attribute 
	of the selected element that you want to morph.</li>
  <li>One more option that you have is to pass the final path as a string directly to the 
    tween. This way, you can avoid having two different paths in your SVG.</li>
</ol>

```
KUTE.fromTo('#shape-a', {path: '#shape-a' }, { path: '#shape-b' });
KUTE.to('#shape-a', { path: '#shape-b' });
KUTE.fromTo('#shape-a', {path: '#shape-a' }, { path: 'The path of #shape-b as a valid string.' });
KUTE.to('#shape-a', { path: 'The path of #shape-b as a valid string.' });
```

<p>During initialization, the library samples some points based on the paths that we provided. 
These points are then stored in two different arrays. Finally, these arrays are used for the 
interpolation. There are a number of options that you can configure to control the morphing 
behavior for different paths.</p>
<ul>
  <li>morphPrecision: As you might have guessed, this option allows you to specify the precision 
    or accuracy of the morphing. It is specified as a number, and a lower value means higher 
	precision. Keep in mind that higher precision will result in more accuracy, but it will also 
	be detrimental to the performance. This option does not apply when you are dealing with 
	polygonal shapes or paths where the d attribute consists only of h, l, and v. In such cases, 
	the original polygon paths are used instead of sampling new ones.</li>
  <li>reverseFirstPath: You can set the value of this option to true in order to reverse the 
    drawing path for your first shape. Its default value is false.</li>
  <li>reverseSecondPath: You can set the value of this option to true in order to reverse the 
    drawing path for your second shape. Its default value is also false.</li>
  <li>morphIndex: Sometimes, the points on a path might have to cover a lot of distance during 
    morphing. You can control this behavior using the morphIndex parameter. When specified, this 
	parameter allows you to rotate the final path in such a way that all the points travel the 
	least distance possible.</li>
</ul>

<p>Let's use what we have learned so far to morph a battery icon into a bookmark icon. You 
should note that I have used lowercase l in order to specify the path in relative terms. This 
is the required markup:</p>

```
<path id="battery-a" 
      d="M50,10 l150,0 l0,25 l20,0 l0,50 l-20,0 l0,25 l-150,0 l0,-100z"/>
<path id="bookmark-a"
      d="M70,10 l0,125 l40,-40 l40,40 l0,-125 l0,0 l0,0 l0,0 l0,0z"/>
```

<p>The following JavaScript creates the tween object and starts the animation on button click:</p>

```
var morphA = KUTE.to(
    '#battery-a', 
    { path: '#bookmark-a' },
    { duration: 5000 }
);
startButton.addEventListener(
  "click",
  function() {
    morphA.start();
  },
  false
);
```

<p>Here is a demo that shows the above code in action. I have also added an extra element where 
the morph animation sets reverseFirstPath to true. This will help you understand the overall 
impact of different configuration options on the morphing. The animation duration has been set 
to 5 seconds so that you can closely observe both the animations and spot the differences.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/VMRLXY -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In the previous example, the main path did not have any subpaths. This made the morphing very 
straightforward. However, this might not always be the case.</p>

<p>Let's add an extra subpath to our bookmark as well as the battery icon. If you morph the icons 
now, you will see that only the first subpath animates. The second subpath just disappears at the 
beginning of the animation and reappears at the end. The only way to animate all the subpaths in 
such cases is by changing the subpaths into individual paths. Here is an example:</p>

```
<!-- Before -->
<path id="battery-a"
      d="M50,10 l150,0 l0,25 l20,0 l0,50 l-20,0 l0,25 l-150,0 l0,-100z 
M70,30 l60,65 l-10,-65 l60,65z"/>
<path id="bookmark-a"
      d="M70,10 l0,125 l40,-40 l40,40 l0,-125 l0,0 l0,0 l0,0 l0,0z 
M80,80 l30,-45 l30,45 l0,0z"/>
 
<!-- After -->
<path id="battery-b1"
      d="M250,10 l150,0 l0,25 l20,0 l0,50 l-20,0 l0,25 l-150,0 l0,-100z"/>
<path id="battery-b2" 
      d="M270,30 l60,65 l-10,-65 l60,65z"/>
<path id="bookmark-b1"
      d="M270,10 l0,125 l40,-40 l40,40 l0,-125 l0,0 l0,0 l0,0 l0,0z"/>
<path id="bookmark-b2"
      d="M280,80 l30,-45 l30,45 l0,0z"/>
```

<!-- image: codepen.io https://codepen.io/Shokeen/pen/eGPjyR -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Animating SVG Strokes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Another popular SVG-related animation effect includes starting from nothing and then drawing a 
predefined shape using SVG strokes. This can be used to animate the drawing of logos or other 
objects. In this section, you will learn how to use KUTE.js to create a stroking animation for 
the Font Awesome bicycle icon.</p>

<p>There are three ways to animate SVG strokes in KUTE.js. You can animate from 0% to 100% by setting 
the fromTo values as 0% 0% and 0% 100%. You can also draw a part of the SVG shape by setting the 
values to something like 0% 5% and 95% 100%. Finally, you can set the ending value to 0% 0% in 
order to create an erasing effect instead of a drawing effect.</p>

<p>Here is the JavaScript code that I have used to animate our bicycle:</p>

```
var wholeAnimation = KUTE.fromTo(
  "#icon",
  { draw: "0% 0%" },
  { draw: "0% 100%" },
  { duration: 10000}
);
var partialAnimation = KUTE.fromTo(
  "#icon",
  { draw: "0% 5%" },
  { draw: "95% 100%" },
  { duration: 10000}
);
var eraseAnimation = KUTE.fromTo(
  "#icon",
  { draw: "0% 100%" },
  { draw: "0% 0%" },
  { duration: 5000}
);
```

<p>As you can see in the example below, you don't need to worry about multiple subpaths inside a 
path. KUTE.js animates all of these subpaths individually without any issues. The animation 
duration is used to determine the time for the animation of the longest path. The stroke duration 
for the rest of the subpaths is then determined based on their length.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/mBvxvj -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Animating SVG Transforms</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>We have already learned how to animate CSS transform values in the second tutorial of the 
series. The KUTE.js SVG plugin also allows you to use the svgTransform attribute in order to 
rotate, translate, scale, or skew different SVG elements on a webpage.</p>

<p>The rotate attribute accepts a single value that determines the angle of rotation. By default, 
the rotation happens around the center point of the element, but you can specify a new center of 
rotation using the transformOrigin attribute.</p>

<p>The translate attribute accepts the values in the format translate: [x, y] or translate: x. 
When provided with a single value, the value of y is assumed to be zero.</p>

<p>When skewing elements, you will have to use skewX and skewY. There is no support for 
skew[x, y] in SVG. Similarly, the scale attribute also accepts only one value. The same 
value is used to scale the elements in both x and y directions.</p>

<p>Here is a code snippet that applies all these transformations on a rectangle and a circle.</p>

```
var rotation = KUTE.allTo(
  "rect, circle",
  { svgTransform: { rotate: 360 } },
  { repeat: 1, yoyo: true }
);
var scaling = KUTE.allTo(
  "rect, circle",
  { svgTransform: { scale: 1.5 } },
  { repeat: 1, yoyo: true }
);
var translation = KUTE.allTo(
  "rect, circle",
  { svgTransform: { translate: [100, -50] } },
  { repeat: 1, yoyo: true }
);
var skewing = KUTE.allTo(
  "rect, circle",
  { svgTransform: { skewX: 25 } },
  { repeat: 1, yoyo: true }
);
```

<p>I have set the yoyo parameter to true so that after playing the animation in reverse, the 
transform properties are set to their initial value. This way, we can replay the animations 
again and again by clicking on the buttons.</p>

<p>If you press the Rotate button in the demo, you will notice that it does not seem to have any 
effect on the circle. To observe the rotation of circle, you will have to apply a skew transform 
on it in order to change its shape and then click on rotate immediately.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/xXeZLr  -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Final Thoughts</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>We began this tutorial by covering the basics of SVG morphing and stroke animations. You learned 
how to properly morph complex paths that have subpaths and how we can create an erasing stroke effect 
instead of a drawing one by choosing the right values for the draw attribute. After that, we discussed 
how we can use the svgTransform attribute in order to animate different transforms.</p>

<p>In various tutorials, we've seen just how powerful JavaScript has become. It’s not without its learning 
curves, and there are plenty of frameworks and libraries to keep you busy, as well. If you’re looking 
for additional resources to study or to use in your work, check out what we have available on Envato 
Market.</p>

<p>The tutorial was meant to introduce you to all the features of the KUTE.js SVG plugin and help you 
get started quickly. You can learn more about the SVG plugin by reading the documentation.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2 id="part4">Performant Animations Using KUTE.js: Part 4, Animating Text</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This tutorial will teach you how to animate the values of different kinds of attributes using 
the attribute plugin in KUTE.js.</p>

<p>So far in this series, you have learned how to animate the CSS properties of different 
elements, how to create different SVG-related animations, and how to animate the text content 
of different elements on a webpage. There is one more way in which you can animate the elements 
on a webpage using KUTE.js, and that is by changing the values of different attributes. This 
requires you to include the attributes plugin in your project.</p>

<p>In this tutorial, you will learn how to use the attributes plugin to animate the value of 
different kinds of attributes in KUTE.js. We will also discuss different easing functions that 
you can use to control the pace of different animations.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
Easing Functions
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Objects in real life very rarely move linearly. They are either accelerating or decelerating. 
Even the acceleration and deceleration occur at different magnitudes. Up to this point, all 
our animations have progressed linearly. This doesn't feel natural at all. In this section, 
you will learn about all the easing functions that KUTE.js provides for controlling the pace 
of different animations.</p>
<p>The core easing functions in the library are included in the core engine out of the box. Let's 
say you want to apply the QuadraticInOut easing to an animation. This can be achieved in two ways:</p>

```
easing: KUTE.Easing.easingQuadraticInOut
// OR 
easing: 'easingQuadraticInOut'
```

<p>Each of the easing functions has a unique curve that determines how the elements will 
accelerate during the animation. A sinusoidal curve implies linear acceleration. Keep in 
mind that this is different from the linear easing function. The linear function implies 
a linear speed of animation, while a sinusoidal curve implies a linear speed of acceleration 
for the animation. In other words, the speed of the animation will increase or decrease 
linearly. Similarly, quadratic implies acceleration with a power of two, cubic implies a 
power of three, quartic implies a power of four, and quintic implies a power of five. 
There are also circular and exponential easing functions.</p>

<p>You can append In, Out, or InOut to any of the easing functions. The value In implies that 
the animation will start very slowly and keep accelerating until the end. The value Out implies 
that the animation will start at the maximum speed and then decelerate slowly until it comes to 
a halt at the end. The value InOut means that the animation will speed up at the beginning and 
slow down at the end.</p>

<p>You can also use bounce and elastic easing functions in your animations and append In, 
Out, or InOut to any of them. In the following demo, I have applied all these easing functions 
on different circles so that you can see how they affect the pace of the animation.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/KXELoX -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>It is possible that none of the core easing functions provide the animation pace that 
you are looking for. In such cases, you can include the Cubic Bezier functions in your 
project from the experiments branch and start using those easing functions.</p>

<p>Similarly, KUTE.js also provides some physics-based easing functions imported from the 
Dynamics.js library. You can read more about all these easing functions and how to properly 
use them on the easing function page of the library.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Animating Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
Attributes in SVG can accept numbers as well as strings as their value. The strings can be 
color values or numbers suffixed with a unit like px, em, or %. The names of the attributes 
themselves can also consist of two words joined by a hyphen. Keeping these differences in 
mind, KUTE.js provides us different methods that can be used to specify the values of 
different attributes.

```
var tween = KUTE.to('selector', {attr: {'r': 100}});
var tween = KUTE.to('selector', {attr: {'r': '10%'}});
var tween = KUTE.to('selector', {attr: {'stroke-width': 10}});
var tween = KUTE.to('selector', {attr: {strokeWidth: 10}});
```

<p>As you can see, suffixed values need to be enclosed within quotes. Similarly, attributes 
which contain a hyphen in their name need to be enclosed inside quotes or specified in 
camelCase form.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Unitless Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>A lot of attributes accept unitless values. For example, the stroke-width of a path 
could be unitless. Similarly, you don't have to specify a unit for the r, cx, and cy 
attributes of a circle element. You can animate all these attributes from one value to 
another using the attributes plugin.</p>

<p>Now that you know how to use different easing functions, you will be able to animate 
different attributes at different paces. Here is an example:</p>

```
var radiusAnimation = KUTE.allTo(
  "circle",
  {
    attr: { r: 75 }
  },
  {
    repeat: 1,
    yoyo: true,
    offset: 1000,
    easing: 'easingCubicIn'
  }
);
var centerxAnimationA = KUTE.to(
  "#circle-a",
  {
    attr: { cx: 500 }
  },
  {
    repeat: 1,
    yoyo: true,
    easing: 'easingCubicInOut',
  }
);
var centerxAnimationB = KUTE.to(
  "#circle-b",
  {
    attr: { cx: 100 }
  },
  {
    repeat: 1,
    yoyo: true,
    easing: 'easingCubicInOut'
  }
);
var centeryAnimation = KUTE.allTo(
  "circle",
  {
    attr: { cy: 300 }
  },
  {
    repeat: 1,
    yoyo: true,
    offset: 1000,
    easing: 'easingCubicOut'
  }
);
```

<p>The first tween animates the radius of both circles at once using the allTo() method we 
discussed in the first tutorial. If set to true, the yoyo attribute plays the animation in 
the reverse direction.</p>

<p>The cx attribute of both the circles is animated individually. However, they are both 
triggered by the same button click. Finally, the cy attribute of both the circles is animated 
at once with an offset of 1000 milliseconds.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/XeGYyy -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Color Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Starting from version 1.5.7, the attribute plugin in KUTE.js also allows you to animate the 
fill, stroke, and stopColor attributes. You can use valid color names or hex values for the 
colors. You can also provide the color values in RGB or HSL format.</p>

<p>One important thing that you have to keep in mind is that the animations will only seem 
to work if you are not setting the value of these properties in CSS. In the following demo, 
the fill color wouldn't have animated at all if I had added the following CSS in our demo.</p>

```
rect {
    fill: brown;
}
```

<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- images: codepen.io  https://codepen.io/Shokeen/pen/aLxorx  -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The demo I created is very basic, but you can make it more interesting by applying transforms 
and using more colors.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Suffixed Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>A lot of SVG attributes like r and stroke-width can work with and without suffixes. For 
example, you can set the value of r to be a number like 10 or in terms of em units like 10em. 
There are some attributes like offset attribute for color stops that always require you to 
add a suffix. While specifying a value for suffixed attributes in KUTE.js, always make sure 
that you enclose the value within quotes.</p>

<p>In the following example, I have animated the offset value of the first stop in a gradient 
and the color of the second stop. Since offset requires a suffix, I have enclosed the value 
inside quotes.</p>

```
var offsetAnimation = KUTE.allTo(
  ".stop1",
  {
    attr: { offset: '90%'}
  },
  {
    repeat: 1,
    offset: 1000,
    yoyo: true,
    easing: 'easingCubicIn'
  }
);
var colorAnimation = KUTE.allTo(
  ".stop2",
  {
    attr: { stopColor: 'black'}
  },
  {
    repeat: 1,
    offset: 1000,
    yoyo: true,
    easing: 'easingCubicIn'
  }
);
var scaleAnimation = KUTE.allTo(
  "circle",
  {
    svgTransform: { scale: 2}
  },
  {
    repeat: 1,
    offset: 1000,
    yoyo: true,
    easing: 'easingCubicIn'
  }
);
```

<p>There are three different gradients in the demo, and each of these gradients has two color 
stops with the class names stop1 and stop2. I have also applied a scale transform using the 
svgTransform attribute, which we discussed in the 
<a href="https://code.tutsplus.com/performant-animations-using-kutejs-part-3-animating-svg--cms-29727t">
third tutorial</a> of the series.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/LzvYVY  -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Final Thoughts</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In this tutorial, you learned about different easing functions available in KUTE.js and 
how you can use them to control the pace of your own animations. You also learned how to 
animate different kinds of attributes.</p>

<p>I have tried to cover all the important aspects of KUTE.js in this series. This should be 
enough to help you use KUTE.js confidently in your own projects. You can also read the 
documentation in order to learn more about the library.</p>

<p>I would also recommend that you go through the source code and see how the library 
actually works. If you have any questions or tips related to this tutorial, feel free 
to share them in the comments.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2 id="part5">Performant Animations Using KUTE.js: Part 5, Easing Functions and Attributes</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This tutorial will teach you how to animate the values of different kinds of attributes 
using the attribute plugin in KUTE.js.</p>

<p>So far in this series, you have learned how to animate the CSS properties of different 
elements, how to create different SVG-related animations, and how to animate the text 
content of different elements on a webpage. There is one more way in which you can animate 
the elements on a webpage using KUTE.js, and that is by changing the values of different 
attributes. This requires you to include the attributes plugin in your project.</p>

<p>In this tutorial, you will learn how to use the attributes plugin to animate the value of 
different kinds of attributes in KUTE.js. We will also discuss different easing functions 
that you can use to control the pace of different animations.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Easing Functions</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Objects in real life very rarely move linearly. They are either accelerating or decelerating. 
Even the acceleration and deceleration occur at different magnitudes. Up to this point, all our 
animations have progressed linearly. This doesn't feel natural at all. In this section, you will 
learn about all the easing functions that KUTE.js provides for controlling the pace of different 
animations.</p>
<p>The core easing functions in the library are included in the core engine out of the box. Let's 
say you want to apply the QuadraticInOut easing to an animation. This can be achieved in two ways:</p>

```
easing: KUTE.Easing.easingQuadraticInOut
// OR 
easing: 'easingQuadraticInOut'
```

<p>Each of the easing functions has a unique curve that determines how the elements will accelerate 
during the animation. A sinusoidal curve implies linear acceleration. Keep in mind that this is 
different from the linear easing function. The linear function implies a linear speed of animation, 
while a sinusoidal curve implies a linear speed of acceleration for the animation. In other words, 
the speed of the animation will increase or decrease linearly. Similarly, quadratic implies 
acceleration with a power of two, cubic implies a power of three, quartic implies a power of four, 
and quintic implies a power of five. There are also circular and exponential easing functions.</p>

<p>You can append In, Out, or InOut to any of the easing functions. The value In implies that the 
animation will start very slowly and keep accelerating until the end. The value Out implies that 
the animation will start at the maximum speed and then decelerate slowly until it comes to a halt 
at the end. The value InOut means that the animation will speed up at the beginning and slow down 
at the end.</p>

<p>You can also use bounce and elastic easing functions in your animations and append In, Out, or 
InOut to any of them. In the following demo, I have applied all these easing functions on different 
circles so that you can see how they affect the pace of the animation.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/KXELoX -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>It is possible that none of the core easing functions provide the animation pace that you 
are looking for. In such cases, you can include the Cubic Bezier functions in your project 
from the experiments branch and start using those easing functions.</p>

<p>Similarly, KUTE.js also provides some physics-based easing functions imported from the 
Dynamics.js library. You can read more about all these easing functions and how to properly 
use them on the easing function page of the library.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Animating Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Attributes in SVG can accept numbers as well as strings as their value. The strings can be 
color values or numbers suffixed with a unit like px, em, or %. The names of the attributes 
themselves can also consist of two words joined by a hyphen. Keeping these differences in mind, 
KUTE.js provides us different methods that can be used to specify the values of different attributes.</p>

```
var tween = KUTE.to('selector', {attr: {'r': 100}});
var tween = KUTE.to('selector', {attr: {'r': '10%'}});
var tween = KUTE.to('selector', {attr: {'stroke-width': 10}});
var tween = KUTE.to('selector', {attr: {strokeWidth: 10}});
```

<p>As you can see, suffixed values need to be enclosed within quotes. Similarly, attributes which 
contain a hyphen in their name need to be enclosed inside quotes or specified in camelCase form.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Unitless Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>A lot of attributes accept unitless values. For example, the stroke-width of a path could be 
unitless. Similarly, you don't have to specify a unit for the r, cx, and cy attributes of a circle 
element. You can animate all these attributes from one value to another using the attributes plugin.</p>

<p>Now that you know how to use different easing functions, you will be able to animate different 
attributes at different paces. Here is an example:</p>

```
var radiusAnimation = KUTE.allTo(
  "circle",
  {
    attr: { r: 75 }
  },
  {
    repeat: 1,
    yoyo: true,
    offset: 1000,
    easing: 'easingCubicIn'
  }
);
var centerxAnimationA = KUTE.to(
  "#circle-a",
  {
    attr: { cx: 500 }
  },
  {
    repeat: 1,
    yoyo: true,
    easing: 'easingCubicInOut',
  }
);
var centerxAnimationB = KUTE.to(
  "#circle-b",
  {
    attr: { cx: 100 }
  },
  {
    repeat: 1,
    yoyo: true,
    easing: 'easingCubicInOut'
  }
);
var centeryAnimation = KUTE.allTo(
  "circle",
  {
    attr: { cy: 300 }
  },
  {
    repeat: 1,
    yoyo: true,
    offset: 1000,
    easing: 'easingCubicOut'
  }
);
```

<p>The first tween animates the radius of both circles at once using the allTo() method we 
discussed in the first tutorial. If set to true, the yoyo attribute plays the animation in 
the reverse direction.</p>

<p>The cx attribute of both the circles is animated individually. However, they are both 
triggered by the same button click. Finally, the cy attribute of both the circles is animated 
at once with an offset of 1000 milliseconds.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/XeGYyy  -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Color Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Starting from version 1.5.7, the attribute plugin in KUTE.js also allows you to animate 
the fill, stroke, and stopColor attributes. You can use valid color names or hex values for 
the colors. You can also provide the color values in RGB or HSL format.</p>

<p>One important thing that you have to keep in mind is that the animations will only seem 
to work if you are not setting the value of these properties in CSS. In the following demo, 
the fill color wouldn't have animated at all if I had added the following CSS in our demo.</p>

```
rect {
    fill: brown;
}
```

<!-- image: codepen.io  https://codepen.io/Shokeen/pen/aLxorx  -->

<p>The demo I created is very basic, but you can make it more interesting by applying transforms 
and using more colors.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Suffixed Attributes</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>A lot of SVG attributes like r and stroke-width can work with and without suffixes. For example, 
you can set the value of r to be a number like 10 or in terms of em units like 10em. There are 
some attributes like offset attribute for color stops that always require you to add a suffix. 
While specifying a value for suffixed attributes in KUTE.js, always make sure that you enclose 
the value within quotes.</p>

<p>In the following example, I have animated the offset value of the first stop in a gradient and 
the color of the second stop. Since offset requires a suffix, I have enclosed the value inside quotes.</p>

```
var offsetAnimation = KUTE.allTo(
  ".stop1",
  {
    attr: { offset: '90%'}
  },
  {
    repeat: 1,
    offset: 1000,
    yoyo: true,
    easing: 'easingCubicIn'
  }
);
var colorAnimation = KUTE.allTo(
  ".stop2",
  {
    attr: { stopColor: 'black'}
  },
  {
    repeat: 1,
    offset: 1000,
    yoyo: true,
    easing: 'easingCubicIn'
  }
);
var scaleAnimation = KUTE.allTo(
  "circle",
  {
    svgTransform: { scale: 2}
  },
  {
    repeat: 1,
    offset: 1000,
    yoyo: true,
    easing: 'easingCubicIn'
  }
);
```

<p>There are three different gradients in the demo, and each of these gradients has two color stops 
with the class names stop1 and stop2. I have also applied a scale transform using the svgTransform 
attribute, which we discussed in the third tutorial of the series.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!-- image: codepen.io  https://codepen.io/Shokeen/pen/LzvYVY  -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Final Thoughts</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In this tutorial, you learned about different easing functions available in KUTE.js and 
how you can use them to control the pace of your own animations. You also learned how to 
animate different kinds of attributes.</p>

<p>I have tried to cover all the important aspects of KUTE.js in this series. This should be 
enough to help you use KUTE.js confidently in your own projects. You can also read the 
documentation in order to learn more about the library.</p>

<p>I would also recommend that you go through the source code and see how the library 
actually works. If you have any questions or tips related to this tutorial, feel free 
to share them in the comments.</p>

