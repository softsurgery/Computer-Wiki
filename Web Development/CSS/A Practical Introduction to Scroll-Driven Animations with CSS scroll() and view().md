With just CSS you can add scroll animations that toggle shadows on navbars, reveal images, add scrollytelling, link up carousel elements and much more.

Let’s make a [CSS scroll animation](https://drafts.csswg.org/scroll-animations-1/)! No frameworks, no JavaScript. Connect user interaction with real time scroll interaction feedback; helping transition color, position, visibility, and more. 

At the time of writing this, it is Chromium only:

**Browser Support:** 

- Chrome
- Firefox
- Internet Explorer
- Safari
- Opera

I feel scroll animations are perfect for progressive enhancement, but you may consider using a [polyfill](https://github.com/flackr/scroll-timeline) if it is crucial for them to function across all browsers today.

## A humble spinning animation

Let’s begin with something familiar—an infinitely spinning element. It has keyframes that rotate the star five times over five seconds, continuously:

```css
@keyframes spin {
  to {
    transform: rotateY(5turn);
  }
}

@media (prefers-reduced-motion: no-preference) {
  div {
    animation: spin 5s ease infinite;
  }
}
```

[Try the demo](https://codepen.io/argyleink/pen/BaboeRb/13baeebe69e01667b8e5402cab808654)

## A scroll driven animation (SDA)

Let’s **convert** that animation **to a scroll-driven** animation, if the browser supports it. 

Add **one line of CSS** that instructs the animation to be triggered by scrolling, using the [`scroll()`](https://drafts.csswg.org/scroll-animations-1/#scroll-notation) function:

```css
@media (prefers-reduced-motion: no-preference) {
  @supports (animation-timeline: scroll()) {
    div {
      animation: spin linear both;
      animation-timeline: scroll();
    } 
  }
}
```

[Try the demo](https://codepen.io/argyleink/pen/abMvrdB/7425e042efa174aaf02c7f27501204a5)

### What just happened?!

To the keyframes we defined, they still run from 0-100%. But now, **0% is the scroll start** position and **100% is the scroll end** position. You changed the [animation-timeline](https://drafts.csswg.org/css-animations-2/#propdef-animation-timeline).

Notice the addition of `both`, an [animation-fill-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-fill-mode) value that allows the animation to play forwards and backwards.

I did add some supporting styles. I applied `position: fixed` to the image, allowing us to observe its spin as it remains in the viewport. Additionally, I modified the animation easing to linear because, in my experience, scroll-linked animations tend to feel better when linearly connected to my scrolling gesture.

## A scroll port intersection driven animation

Next, change [`scroll()`](https://drafts.csswg.org/scroll-animations-1/#scroll-notation) to [`view()`](https://drafts.csswg.org/scroll-animations-1/#view-notation) and **add one more line** of CSS that specifies the `animation-range`:

```css
@media (prefers-reduced-motion: no-preference) {
  @supports (animation-timeline: scroll()) {
    div {
      animation: spin linear both;
      animation-timeline: view();
      animation-range: contain;
    }
  }
}
```

[Try the demo](https://codepen.io/argyleink/pen/wvOKbyL/2d672362df9ac37cf6920b5b6bc3a243)

### What just happened?!

To the keyframes we defined, they still run from 0-100% 🤓 But now, 0% is when the element is entering the scroll area and 100% is when it’s about to go out of that scroll area.

This [`view()`](https://drafts.csswg.org/scroll-animations-1/#view-notation) function powers the animation as it crosses a scrollport.

The default value for `animation-range` is `cover`, which stinks in this star spinning demo because it makes it hard to distinguish between the `scroll()` demo; it would be spinning the whole time you see it, as it would spin edge to edge. So I’ve also added `animation-range` and set it to `contain` so that the animation is edge to edge within the scrollport. It’s a little goofy looking that it’s so stiff as it enters and exits, but I hope it’s at least super clear what’s happening.

Bramus has built a super rad [tool](https://scroll-driven-animations.style/tools/view-timeline/ranges/) that helps visualize the options you can pass into `animation-range`, definitely worth checking out if you intend to make some CSS scroll animations.

### More elements!

We saw one element animate itself across the viewport, but what if multiple elements get assigned the same animation and `view()` attachment, but enter and exit the scrollport at different times? Let’s see!

In this next demo, each image scales up as it enters the scrollport, with individually running animations based on their unique scrollport intersection.

Instead of a keyword for the `animation-range`, this time I show that you can use lengths. In this case, I wanted cute product images to scale up once they were 25vh in and complete the scaling by 75vh of their scrolling area:

[Try the demo](https://codepen.io/argyleink/pen/RwvOmvY)

The CSS for **this effect is just** **this**:

```css
@keyframes scale-a-lil {
  from {
    scale: .5;
  }
}
  
@media (prefers-reduced-motion: no-preference) {
  figure img {
    animation: scale-a-lil linear both;
    animation-timeline: view();
    animation-range: 25vh 75vh;
  }
}
```

The keyframes set the out state for when scaled down. The animation says scale the images to regular size given the element intersects with the scrollport at the provided `animation-range`.

## More examples!

With those building blocks of scroll attachment, view attachment and animation ranges, there’s **a lot** you can do. Let’s have a look at some more examples.

### Fade in the primary nav on scroll

[This demo](https://codepen.io/argyleink/pen/KKrGNEO) has a very clean and minimal initial load where navigation is hidden, but then a little bit of scroll brings it in. It also shows how to animated the navbar shadow on scroll:

### Theme scroll

[This next demo](https://codepen.io/argyleink/pen/BaGVyyJ) animates an angle `@property` CSS variable, which is then used in a cylindrical color space. As the user scrolls, it makes a rainbow effect because the effect changes `0turn` to `1turn`; encompassing all the hues in the color space:

### Pull to refresh with scroll snap

In [this demo](https://codepen.io/argyleink/pen/ExOWjMe), the pull-to-refresh icon rotates and the prompt text fades in as the user scrolls, effectively conveying the result of their gesture:

## A more advanced example

Up until now, examples have been with the whole page, and vertical scroll only.

In this section I will show you how to:

- Hookup **horizontal** `scroll()` animation
- Link animation with **scroll-snap points**
- **Share scroll()** or **view()** progress with other elements with `timeline-scope`

[Try the demo](https://codepen.io/argyleink/pen/vYQQEmo)

### Setting up snapping horizontal `scroll()`

It’s time to pass some parameters to `scroll()`. [It is a function](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timeline/scroll) after all 🤓

Attach the animation to horizontal scroll by specifying the axis in the function, such as `scroll(x)`. Ensure that the axis matches the setup of your overflow. If you’re utilizing logical properties, you would use `scroll(inline)`.

```css
html {
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  animation-timeline: scroll(x);

  body > section {
    scroll-snap-align: start;
  }
}
```

Now, any animation added to the markup will be controlled by the horizontal scroll of the page.

### Animating carousel cards in and out

This is one of the best parts of the code to play with in the demo. It’s the animation as the cards go in and out of the horizontal scroll view:

The keyframes run from 0%-20% as elements “enter stage right”, then hold a natural state until 80% of the scroll area, then “exit stage left” finishing the 80%-100% part of the keyframes. My animation keyframes are like, “be shy as you enter the stage, then try to stick around as you exit the stage.” 😂

```css
@keyframes fancy-in {
  /* card entry */
  0% {
    transform: translateX(25vw);
  }
  /* card on stage */
  20%, 80% {
    opacity: 1;
    transform: none;
  }
  /* card exit */
  100% {
    transform: translateX(90%) scale(.98);
    opacity: 0;
  }
}
```

You should definitely mess with this code. Change the timing percentages or change the transforms.

Next, connect these dynamic keyframes to the scroll intersection on the x-axis for each `.card` element. Ensure that the animation occurs only if motion is acceptable for the visitor.

```css
@media (prefers-reduced-motion: no-preference) {
  @supports (animation-timeline: scroll()) {
    animation: fancy-in linear both;
    animation-timeline: view(x);
  }
}
```

If you want to go off rails and play with what you’ve learned, try making the keyframes just 0%-100% and change the animation-range to get the effect. Or, [add the animation ranges into the keyframes](https://developer.chrome.com/docs/css-ui/scroll-driven-animations#attaching_to_multiple_view_timeline_ranges_with_one_set_of_keyframes:~:text=it%20is%20also%20possible%20to%20create%20one%20set%20of%20keyframes%20that%20already%20contains%20the%20range%20information). There are options here, and room for you to find a preference as an author and crafter.

### Crossfading the next and previous buttons with `scroll()`

It’s a nice, elegant touch to visually fade the next button when at the end of a carousel, and also fade out a previous button if at the start. We can use CSS to do this! Here are the keyframes, they make this look like it might be simple:

```css
@keyframes toggle-control {
  50% { opacity: 0 }
}
```

The next section is advanced and extremely important when you want to redirect an intersection of one element to power a different element’s animation.

#### Define observable snap sections

Rather than attempting to fade out the buttons with an estimated animation range for snap sections’ size, each button can observe a specific section and its intersection with the scrollport. This allows the buttons to be perfectly animated as the first and last sections go in and out of view. It’s awesome.

#### Intrinsically sized snap area animation timelines

Use `view-timeline` to expose the `view()` progress of a specific element. Provide a custom name and axis. Soon we’ll make these names more public to other elements, they can reference a sections view progress to power their own animation.

```css
#section-1 { view-timeline: --section-1 x }
#section-2 { view-timeline: --section-2 x }
#section-3 { view-timeline: --section-3 x }
#section-4 { view-timeline: --section-4 x }
#section-5 { view-timeline: --section-5 x }
#section-6 { view-timeline: --section-6 x }
#section-7 { view-timeline: --section-7 x }
```

Next, we have to talk a little bit about the HTML of the carousel. The buttons and the section elements are distant siblings in the DOM tree. In order for the buttons to be able to observe the timelines of distant relatives scrollport intersection, it’s essential to raise the scope of each section’s timeline into a shared parent element. This is done with the [timeline-scope](https://developer.mozilla.org/en-US/docs/Web/CSS/timeline-scope) property.

Each section’s [view-timeline-name](https://developer.mozilla.org/en-US/docs/Web/CSS/view-timeline-name) is now available to all elements in `<body>`.

```css
body {
  timeline-scope: 
    --section-1, 
    --section-2, 
    --section-3, 
    --section-4, 
    --section-5, 
    --section-6, 
    --section-7
  ;
}
```

I can now tell the previous (left arrow) button to fade out when the first section is showing, and tell the next (right arrow) button to fade out when the last section is showing.

```css
.controls {
  & > button {
    /* if supported, enable the visibility toggling animation */
    @supports (animation-timeline: scroll()) {
      animation: toggle-control linear both;
    }

    /* fade out the previous button when section 1 is in view */
    &.previous {
      animation-timeline: --section-1;
    }

    /* fade out the next button when at the last section */
    &.next {
      animation-timeline: --section-7;
    }
  }
}
```

The work to expose each sections timeline pays off in the next section, where more buttons want to know which section is snapped.

#### Synchronizing the pagination dots with snapped carousel sections

First, we need some keyframes that represent being in and out of view. Since this is a `view()` linked animation, 0% is out of view to the left, 100% is out of view to the right, and 50% is when the specified `--section` by a pagination dot is snapped.

My keyframes chose scale and color to indicate a selected state.

```css
@keyframes dot-selected { 
  0%, 100% {
    scale: .75;
  }
  50% {
    scale: 1;
    background: var(--text-2);
  }
}
```

My pagination consists of a series of links that target section IDs, which is advantageous because the browser handles scrolling elements into view, and they are focusable. The initial step is to apply our recently created animation to each dot.

```css
.pagination > a {
  @supports (animation-timeline: scroll()) {
    animation: dot-selected linear both;  
  }
}
```

Link each dot’s animation progress to a relevant section’s view timeline.

```css
.pagination > a {
  @supports (animation-timeline: scroll()) {
    animation: dot-selected linear both;  
  }

  &:nth-child(1) { animation-timeline: --section-1 }
  &:nth-child(2) { animation-timeline: --section-2 }
  &:nth-child(3) { animation-timeline: --section-3 }
  &:nth-child(4) { animation-timeline: --section-4 }
  &:nth-child(5) { animation-timeline: --section-5 }
  &:nth-child(6) { animation-timeline: --section-6 }
  &:nth-child(7) { animation-timeline: --section-7 }
}
```

Now pagination dots restore to normal size and are a brighter color when their linked section’s `view()` timeline is intersecting with the scroll area.

### Animating the theme with `scroll()`

This part is extra credit and just super fun.

The entire scroll area of the carousel is linked to a full turn of an angle named `--hue`. As you scroll, the progress is converted into an angle between 0 and 1 turns. That angle is used as a hue variable inside an OKLCH color palette.

#### Scroll linked hue angles

The type safe casting of [@property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property) is what unlocks this.

**Browser Support:** 

- Chrome
- Firefox
- Internet Explorer
- Safari
- Opera

Here’s how to define a type safe custom property in CSS:

```css
@property --hue {
  syntax: '<angle>';
  initial-value: 0turn;
  inherits: false;
}
```

Use this typed property in some keyframes. The browser now knows to maintain the type when interpolating 0turn and 1turn, which also means it knows how to interpolate the value over time… or over scroll progress.

```css
@keyframes hue-cycle { 
  to {
    --hue: 1turn;
  }
}
```

Link the progress of our carousel scroll to the keyframes with just a few lines.

```css
:root {
  animation: hue-cycle linear both;
  animation-timeline: scroll(x);
}
```

Last, use the `--hue` variable inside a color palette. I chose `oklch` but you could also use `hsl` or any hue angle accepting color space. My little theme has a couple of text colors, a couple of surface colors, a link color and a focus color.

As the hue rotates on scroll, all of these color variables update.

```css
:root {
  /* dynamic color props, hue powered by scroll */
  --surface-1: oklch(40% 50% var(--hue));
  --surface-2: oklch(50% 40% var(--hue));
  --text-1: oklch(98% 10% var(--hue));
  --text-2: oklch(95% 20% var(--hue));
  --link: oklch(99% 10% var(--hue));
  --focus: oklch(80% 90% var(--hue));

  /* fallback for if @property isnt supported */
  --hue: 275;
}
```

Add a neat gradient trick by leveraging `background-attachment` to make a nice vignette radial gradient that changes color but doesn’t move on scroll. I also use [relative color syntax](https://developer.chrome.com/blog/css-relative-color-syntax) for a quick one off variant from the palette, it’s 25% lighter than surface 2, resulting in that nice sunburst effect.

```css
:root {
  background: radial-gradient(closest-corner circle, 
    oklch(from var(--surface-2) calc(l * 1.25) c h),
    var(--surface-1)
  ) fixed no-repeat;
}
```

## Field notes

Here are key takeaways of `scroll()` and `view()`.

### CSS `scroll()` recap

**Features:**

- Links animation progress to any element with overflow auto, clip or scroll
- Can use any `scroll-timeline` named element, not just a nearest parent scroller, as long as `timeline-scope` is used
- Range can be customized to hone the animation timing

**Great for:**

- [Read progress](https://scroll-driven-animations.style/demos/progress-bar/css/)
- Scroll hints
- [Sticky shadows](https://scroll-driven-animations.style/demos/shrinking-header-shadow/css/)
- Carousels ([cover flow](https://scroll-driven-animations.style/demos/cover-flow/css/))
- [Reversals](https://scroll-driven-animations.style/demos/reverse-scroll/css/)

### CSS `view()` recap

**Features:**

- An elements intersection with an element with overflow auto, clip or scroll
- Can be any scroller
- Can use any `view-timeline` named element, as long as `timeline-scope` is used
- Range can be customized
- Ranges are per element
- Intersection is [not limited to scroll gestures](https://nerdy.dev/add-a-rad-gradient-progress-fill-to-a-range-input-with-no-JS)
- Animation can be [customized based on entry and exit](https://scroll-driven-animations.style/demos/contact-list/css/)

**Great for:**

- Introducing elements into a scroll area
- Carousels
- [Header](https://scroll-driven-animations.style/demos/cover-to-fixed-header/css/) and footer tricks
- [Reveals](https://scroll-driven-animations.style/demos/image-reveal/css/)
- [Sticky tricks](https://scroll-driven-animations.style/demos/stacking-cards/css/)

### Not covered in this article

There’s syntax for combining keyframes and `animation-range` into one 🤯

```css
@keyframes animate-in-and-out {
  entry 0%  {
    opacity: 0; transform: translateY(100%);
  }
  entry 100%  {
    opacity: 1; transform: translateY(0);
  }
  exit 0% {
    opacity: 1; transform: translateY(0);
  }
  exit 100% {
    opacity: 0; transform: translateY(-100%);
  }
}

#list-view li {
  animation: animate-in-and-out linear both;
  animation-timeline: view();
}
```

[Read all about it here from Bramus](https://developer.chrome.com/docs/css-ui/scroll-driven-animations#:~:text=it%20is%20also%20possible%20to%20create%20one%20set%20of%20keyframes%20that%20already%20contains%20the%20range%20information.), it’s slick.

### Respecting User Motion

A good default user experience is a legible and static experience.

With the following media query, you are adding animations for users who have no indication via their device that they wish for reduced motion. I call this `[motionOK](https://open-props.style/#media-queries)` when using [custom media queries](https://drafts.csswg.org/mediaqueries-5/#at-ruledef-custom-media).

```css
@media (prefers-reduced-motion: no-preference) {
  /* OK to add scroll motion * /
}
```

### Checking support

```css
@supports (animation-timeline: scroll()) {
 
}
```

### More reading

- [https://scroll-driven-animations.style/](https://scroll-driven-animations.style/) (all the tools, examples, and copy/paste code you need to know)
- [https://developer.chrome.com/docs/css-ui/scroll-driven-animations](https://developer.chrome.com/docs/css-ui/scroll-driven-animations) (everything you need to know)
- [https://www.youtube.com/watch?v=oDcb3fvtETs](https://www.youtube.com/watch?v=oDcb3fvtETs) 

### Tools

- [Chrome extension debugger pane](https://chromewebstore.google.com/detail/scroll-driven-animations/ojihehfngalmpghicjgbfdmloiifhoce?pli=1)
- [scroll-timeline progress visualizer](https://scroll-driven-animations.style/tools/scroll-timeline/progress/)
- [view-timeline progress visualizer](https://scroll-driven-animations.style/tools/view-timeline/progress/) 
- [view-timeline ranges progress visualizer](https://scroll-driven-animations.style/tools/view-timeline/ranges/)