---
layout: page
title: "Flutter Animations FAQs"
permalink: /animations/animations_faqs/
---

* TOC Placeholder
{:toc}

<img src="/animations/images/icon_animation2_loop2.gif" onload="loadImage()" style="width:75%">
<script>
function loadImage()
</script>

This page includes frequently asked questions about Flutter animations.  
Let us know what questions you have about animations!

### How do animations fit into the reactive/stateful model?

Animations are driven by a change in the state of an `AnimationController` (a ticker really). An animated widget's visuals "react to" this changing state and all of the animation's that are chained to it.

### Are they a bridge between states?

Sometimes that's a simple way to explain what's going on yes. For example if an `AnimatedContainer` is rebuilt with a new `backgroundColor`, it automatically (implicitly) animates from the old `backgroundColor` to the new one.  That's an animated state change.

###  Do they exist outside of state? because the things they are tied to usually have state.

Not really. Only stateful widgets can respond to animations with `setState()`, to rebuild or *react*.

###  Why do you need an AnimationController and an Animation object if AnimationController is subclassed from Animation?

You don't need both. The `AnimationController` is usually used to drive other Animations (chaining). You could build animated widgets that just listened to an `AnimationController` and computed `tween` values based on the AnimationController's value. All of the `Tween`, `Curve`, and `Animation` classes exist to make doing that simpler and easier.
