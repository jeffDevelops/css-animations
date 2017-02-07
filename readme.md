# CSS Transitions & Animations

## Objectives

- Understand keyframe animation
- Use CSS Transitions to animate elements
- Use CSS Animations to create detailed animations
- Describe the importance of prefixing CSS properties
- Compare & contrast using CSS and JS for animations

## Intro (5 minutes)

So far, we've used JavaScript and jQuery to make elements move around on our page. While easy to do, using JavaScript to animate will slow our page load time in noticiable ways. It can also be messy to mix cosmetic code in with the core of your frontend functionality.

Luckily, CSS Animations allows us to handle animations where they belong - in the stylesheet! CSS Animations have a much lower impact on page loads, and are a great way to add a layer of polish to your design. Good design inpsires trust, so adding little touches to your work is a great way to impress users (and potential employers!).

## Concept (10 minutes)

CSS uses the same basic concepts found in hand-drawn animation - Keyframes:

![Keyframes!](http://www.utdallas.edu/atec/midori/Handouts/history_files/key_n_inbetween.jpg)

In the above image, the the left and right-most drawings are **Keyframes** - they define the starting or ending point of a smooth transition. The drawings between the left and right-most image are **Inbetweens** - they don't have to be drawn on a storyboard, because the animator can assume what they will look like without a visual reference.

In web-based animations, Keframes work the same way - the represent the begining or ending state of the element being animated. However, our Inbetweens will be generated by code, instead of being filled in by hand later.

Let's take a look at the starter code in this repo. You'll find some beautifully designed CSS attached to the index file, greeting you with a nice message. But there's a second design hiding in our file, that we can't yet see! We have to add the class `alt` first.
Let's use a jquery toggleClass click function to add/remove the class on click:

```javascript
	$( "#greeting" ).click(function() {
	  $( "#greeting" ).toggleClass( "alt" );
	});	
```
Now clicking on the greetings div will toggle between the two CSS styles. But, there's no animation - its more like a flipbook, right?

That's because these two class states represent our keyframes. What we have right now is kind of like watching a storyboard instead of animated movie - we get the idea, but its not much fun to watch.

So now that we have Keyframes, how do we generate our inbetweens? With CSS, there's a few different ways.

## Transitions (10 minutes)

**Transitions** let us tell the browser how to change a property over time. Let's add a new property to the #greeting CSS:

```css
	transition: all 2s ease;
```

- **Transition** is the name of the CSS property. It tells the element to try to animate certain values if it can.
- **All** means, 'try to animate ALL property values'. This could also be replaced with specific properties, like width or color.
- **2s** is the amount of time you'd like the animation to take - in this case, 2 seconds.
- **Ease** is the "Speed Curve" of animation - basically, a tweak to the math of how long the animation takes to produce more realistic animation effect. Ease, for instance, starts slow, speeds up around the middle, then slows down at the end -  giving the impression of "weight" in the animation. This part of the value is optional - if left out, it defaults to "ease". 

Now let's reload and check out our animation - way smoother! Feel free to play with the transition values until you find a speed and transition you like.

You'll notice that some properties animated their transition, while others didn't - for instance, our background color transitioned beautifully through the color spectrum, but our font-family simply toggled into the new font. That's because font-family can't be animated - its way to complex for a browser to figure out on its own. However, while there are some values like font-family that can't be animated, you'll find that most cosmetic properties can be.


## Keyframe Animations (10 minutes)

But why stop there? We can go further!

**Animations** are similar to transitions, in that they let us have properties
change over time, but they give us more control over how those changes happen.
For example, we have more control over how the animation repeats, change between
multiple values at once, etc.

To keyframe animate a CSS element, we need two components - the animation structure itself, and then a call to the animation with specific instructions.
Let's write a keyframe animation that will make our greeting bounce:

```css
	@keyframes bounce {
		0% {
			position: relative;
			top: 0px;
		}
		50% {
			position: relative;
			top: -120px;
		}
		100% {
			position: initial;
			top: 0px;
		}
	}
```
Here, we're able to see our keyframe sturcture even more clearly - they're broken up by percentages, 100% being the complete duration of the animation (we'll set the specific timing in our animation call). We can add as many keyframes as we want, and as few as two (0 and 100%). At each point, we can specify a new set of css properties. 

While we could go nuts and add all sorts of property changes in here, I would encourage you to think of Keyframe Animations the same way you would JS prototypal functions - they work best when they are clear, simple, and extensible. Our 'bounce' animation serves a single function - to bounce an element. By keeping it style-neutral, we allow ourselves to use the animation on lots of different elements.

Now that we have a basic animation set up, let's call it on our `#greetings` element:

```css
	#greeting.animated {
		animation-name: bounce;
		animation-duration: .5s; /* or: Xms */
		animation-iteration-count: 10;
		animation-direction: normal; /* or: alternate, reverse */
		animation-timing-function: ease-out; /* or: ease, ease-in, ease-in-out, linear, cubic-bezier(x1, y1, x2, y2) */
		animation-fill-mode: forwards; /* or: backwards, both, none */
		animation-delay: 0s; /* or: Xms */
	}
``` 
The above call runs the animation as soon as the class `animated` is added to our element (if we wanted a delay, we could adjust animation-delay). We start by calling the animation by its name - bounce. We tell it how long to take in animation-duration. In fact, there's so many animation options, I would encourage you to look them up and use whatever properties make the most sense for you:

[http://www.w3schools.com/cssref/css3_pr_animation-direction.asp](http://www.w3schools.com/cssref/css3_pr_animation-direction.asp)

As a final step, let's add our new `animated` class to our click function so we can test it out:

```javascript
	$( "#greeting" ).toggleClass( "animated" );
```

Wow! That's . . . interesting. It's bouncing alright, but it doesn't look very natural. Maybe there's some more CSS properties that can help . . .

## Transforms (10 minutes)

**Transforms** allow you to rotate, skew, and pivot your HTML elements in 3D space! While you'll still be rendering your result on a 2D canvas - your computer screen - you can still move your object around like it were a physical object. This is both exciting and terrifying, as the CSS tranform property has [almost 2 dozen unique value types](http://www.w3schools.com/cssref/css3_pr_transform.asp).

My suggestion? Keep it simple at first, then see how deep the rabbit hole goes. Let's start with a basic roate:


```css
	#greeting.rotate {
		-webkit-transform: rotateY(180deg);
		-moz-transform: rotateY(180deg);   
		transform: rotateY(180deg);
	}
```

Now add the `rotate` class to our click function and give it a go! You'll see our greeting flip around, showing you its reverse side. This is still live text, and a live HTML element - so you can imagine the possibilities hidden in the tranform property. 

> Sidenote: notice the `-webkit-` and `-moz-` prefixes on the above CSS? Those are called vendor prefixes. Some of the animations we're doing are so cutting edge, that they haven't been formally adopted by all browsers. In cases like this, you'll have to call the same value multiple times with vendor-specific prefixes to make sure that Chrome(webkit), Firefox(moz), IE(ms), and Opera(O) all display the animation correctly. Always leave a non-prefixed call in as well - as these properties are formally adopted, the need for the vendor prefix will dissapear, as will its support. 
> Not sure if you need a prefix? Go to [Can I Use](http://caniuse.com/) and search for the CSS property you want to use - you'll receieve a detailed breakdown of what browsers support it.


## Lab - Experiment time! (30 minutes)

Obviously, this is just the tip of the iceberg. CSS can be used to create and animate [almost anything](https://pattle.github.io/simpsons-in-css/). Let's dig in and see what we can create!

1. Grab a picture from the internet, and make an animation for it. 
1. Fire the animation with a click function.
1. Below your image animation, write a block of text that tells the story of your animation. It should animate in some way too.

### BONUS!

Codepen is a great place to see what you can do with CSS animations. Fork some of the examples, and try playing with them!

[15 Inspriring Examples of CSS Animation]https://webdesign.tutsplus.com/articles/15-inspiring-examples-of-css-animation-on-codepen--cms-23937

## Questions / Recap  (10 minutes)

CSS Animations are super cross-browser compatible, simple to implement, and take much fewer resources than JavaScript animations. And users love them! When implementing your next project, consider how you can add in simple animations and transforms to improve your user experience.