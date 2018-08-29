---
layout: page
title: "Flutter Transition Animations"
permalink: /animations/transition_widgets/
---

* TOC Placeholder
{:toc}

Transition animations implement the abstract class `AnimatedWidget`. The `AnimatedWidget` class allows you to separate the widget code from the animation code in the `setState()` call. The `AnimatedWidget` class doesn’t need to maintain a `State` object to hold the animation. Use the  `AnimatedWidget` helper class (instead of `addListener()` and `setState()`) to create a reusable transition animation widget.  

< additional info on transition widgets >

## Using a transition animation widget

<div>
<table class="table" width="100%">
  <col width="35%">
  <col width="65%">
	<tbody>
    <tr>
      <td><img src="/animations/images/transitions.png" alt="Transition animation widgets"></td>
      <td> < Revise for a transition widget example >   
      1. Create your <code>StatefulWidget</code>.<br>
      2. Create the <code>State</code> class to hold the animation object and the _transition_  animation widget.<br>
      3. Specify the <code>BuildContext</code> to return the _transition_ animation widget, such as <code>animationexample</code>.<br>
      4. Add the animation object, the duration of the animation, and any parameters relating to the _transition_ animation. For example, you would set the < example > for a <code>animationexample</code> widget.
      </td>
    </tr>
   </tbody>
  </table>
</div>


To create your own reusable transition animations, create a widget that extends `AnimatedWidget`.

For more complex or customized animations, you can build [Explicit animations](/animations/explicit_widgets/) using the  `AnimationController` class.  

## Transition animation examples  
The `RotationTransition` and `SlideTransition` are two transition animation widgets included in the widget package.  

### RotationTransition example
<!-- The `AnimatedOpacity` implicit widget automatically transitions a child's opacity over a given duration whenever the specified opacity changes. -->
<table cellpadding="10">
  <tr>
    <td style="width:20%">
    <a href="" onMouseOver="document.MyImage.src='/animations/images/rotation_transition.gif';" onMouseOut="document.MyImage.src='/animations/images/rotation_transition.png';">
    <img src="/animations/images/rotation_transition.png" name="MyImage">
    </a></td>
    <td>
    The <code>RotationTransition</code> widget automatically rotates a widget. <br>Mouseover the image to view the animation.
    </td>
  </tr>
</table>

<!-- <a href="" onMouseOver="document.MyImage1.src='images/AnimatedOpacity_blue.gif';" onMouseOut="document.MyImage1.src='images/AnimatedOpacity_blue.png';">
<img src="images/AnimatedOpacity_blue.png" name="MyImage1" height="180" width="160" name="MyImage1">
</a>
Mouseover the image to view the animation using the <code>AnimatedOpacity</code> implicit widget. -->

<!-- <div>
<img src="images/AnimatedOpacity_blue.png" name="img" height="180" width="160">

 <script>
 imgOn2 = new Image;
 imgOff2 = new Image;
 imgOn2.src = "images/AnimatedOpacity_blue.gif";
 imgOff2.src = "images/AnimatedOpacity_blue.png";
 </script>

 <a href="#C4" onClick="document.img.src=imgOn2.src;">▶︎</a>
 <a href="#C4" onClick="document.img.src=imgOff2.src;">◼︎</a>

 This example shows an animation using the <code>AnimatedOpacity</code> implicit widget.
</div> -->

 <!-- <br>
 The `AnimatedOpacity` implicit widget automatically transitions a child's opacity over a given duration whenever the given opacity changes. Notice that the listeners, tickers, and other animation elements are *implied* so you don't need to add them. The `AnimatedOpacity` class extends `ImplicitlyAnimatedWidget` and those elements are already included in the `ImplicitlyAnimatedWidget` class. -->

The code for the `RotationTransition` example is shown below.   
<!-- skip -->
{% prettify dart %}
```Dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    new MaterialApp(
      home: new HomePage(),
    ),
  );
}

class HomePage extends StatefulWidget {
  @override
  HomePageState createState() => new HomePageState();
}

class HomePageState extends State<HomePage> {
  bool selected = false;

  @override
  Widget build(BuildContext context) {
    return new RotationTransitionExample(
      selected: selected,
      onTap: (bool value) {
        setState(() {
          selected = value;
        });
      },
    );
  }
}

class RotationTransitionExample extends StatefulWidget {
  const RotationTransitionExample({
    Key key,
    this.selected = false,
    this.onTap,
  }) : super(key: key);

  final ValueChanged<bool> onTap;
  final bool selected;

  @override
  RotationTransitionExampleState createState() => new RotationTransitionExampleState();
}

class RotationTransitionExampleState extends State<RotationTransitionExample>
// The TickerProviderStateMixin makes it so that this state object can
// be used as the vsync for the AnimationController.
    with TickerProviderStateMixin<RotationTransitionExample> {
  AnimationController _controller;
  Animation<double> _rotationAnimation;

  @override
  void initState() {
    super.initState();
    // Create an animation controller, and rebuild the widget tree when we
    // listen to it, in order to generate frames.
    _controller = new AnimationController(
      duration: const Duration(seconds: 2),
      vsync: this,
    )..addListener(() {
      setState(() {
        // The value of the animation controller has changed, so we need to
        // rebuild the widget tree.
      });
    });

    // Set up the animation curve that we want to use.
    _rotationAnimation = new CurvedAnimation(
      parent: _controller,
      // There are many different types of curves.
      curve: Curves.elasticOut,
    );
  }

  @override
  void didUpdateWidget(RotationTransitionExample oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.selected == widget.selected) {
      return;
    }
    // Change the direction of the animation based on whether selected is true
    // or not.  Note that this means if you tap it twice quickly, the animation
    // will only just get started, and reverse to the original position.
    widget.selected ? _controller.forward() : _controller.reverse();
  }

  @override
  void dispose() {
    // The controller must be disposed of, so it is usually owned by a
    // StatefulWidget.
    _controller?.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        setState(() {
          widget.onTap(!widget.selected);
        });
      },
      child: Container(
        color: Colors.white,
        child: new RotationTransition(
          turns: _rotationAnimation,
          child: const FlutterLogo(),
        ),
      ),
    );
  }
}

```
{% endprettify %}

### SlideTransition example
<!-- The `AnimatedOpacity` implicit widget automatically transitions a child's opacity over a given duration whenever the specified opacity changes. -->
<table cellpadding="10">
  <tr>
    <td style="width:20%">
    <a href="" onMouseOver="document.MyImage2.src='/animations/images/slide_transition.gif';" onMouseOut="document.MyImage2.src='/animations/images/slide_transition.png';">
    <img src="/animations/images/slide_transition.png" name="MyImage2">
    </a></td>
    <td>
    The <code>SlideTransition</code> transition widget automatically animates the position of a widget relative to its normal position .<br>Mouseover the image to view the animation.
    </td>
  </tr>
</table>

<!-- <a href="" onMouseOver="document.MyImage1.src='images/AnimatedOpacity_blue.gif';" onMouseOut="document.MyImage1.src='images/AnimatedOpacity_blue.png';">
<img src="images/AnimatedOpacity_blue.png" name="MyImage1" height="180" width="160" name="MyImage1">
</a>
Mouseover the image to view the animation using the <code>AnimatedOpacity</code> implicit widget. -->

<!-- <div>
<img src="images/AnimatedOpacity_blue.png" name="img" height="180" width="160">

 <script>
 imgOn2 = new Image;
 imgOff2 = new Image;
 imgOn2.src = "images/AnimatedOpacity_blue.gif";
 imgOff2.src = "images/AnimatedOpacity_blue.png";
 </script>

 <a href="#C4" onClick="document.img.src=imgOn2.src;">▶︎</a>
 <a href="#C4" onClick="document.img.src=imgOff2.src;">◼︎</a>

 This example shows an animation using the <code>AnimatedOpacity</code> implicit widget.
</div> -->

 <!-- <br>
 The `AnimatedOpacity` implicit widget automatically transitions a child's opacity over a given duration whenever the given opacity changes. Notice that the listeners, tickers, and other animation elements are *implied* so you don't need to add them. The `AnimatedOpacity` class extends `ImplicitlyAnimatedWidget` and those elements are already included in the `ImplicitlyAnimatedWidget` class. -->

The code for the `SlideTransition` example is shown below.  
<!-- skip -->
{% prettify dart %}
```Dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    new MaterialApp(
      home: new HomePage(),
    ),
  );
}

class HomePage extends StatefulWidget {
  @override
  HomePageState createState() => new HomePageState();
}

class HomePageState extends State<HomePage> {
  bool selected = false;

  @override
  Widget build(BuildContext context) {
    return new SlideTransitionExample(
      selected: selected,
      onTap: (bool value) {
        setState(() {
          selected = value;
        });
      },
    );
  }
}

class SlideTransitionExample extends StatefulWidget {
  const SlideTransitionExample({
    Key key,
    this.selected = false,
    this.onTap,
  }) : super(key: key);

  final ValueChanged<bool> onTap;
  final bool selected;

  @override
  SlideTransitionExampleState createState() => new SlideTransitionExampleState();
}

class SlideTransitionExampleState extends State<SlideTransitionExample>
// The TickerProviderStateMixin makes it so that this state object can
// be used as the vsync for the AnimationController.
    with TickerProviderStateMixin<SlideTransitionExample> {

  // Because this animation doesn't just drive something that's a double,
  // we need a Tween to do the mapping from a double to an Offset.
  static final Tween<Offset> _offsetTween = new Tween<Offset>(
    // Start with no offset.
    begin: Offset.zero,
    // End at this offset, which is down and to the right. The numbers are in
    // multiples of the size of the child widget, so it'll travel half of a
    // full width to the right, and a full height of the child down.
    end: const Offset(0.5, 1.0),
  );

  AnimationController _controller;
  Animation<Offset> _slideAnimation;

  @override
  void initState() {
    super.initState();
    // Create an animation controller, and rebuild the widget tree when we
    // listen to it, in order to generate frames.
    _controller = new AnimationController(
      duration: const Duration(seconds: 1),
      vsync: this,
    )..addListener(() {
      setState(() {
        // The value of the animation controller has changed, so we need to
        // rebuild the widget tree.
      });
    });

    // Use a curved animation to drive the Tween<Offset> to give interpolated
    // values between the start and end of the Tween.
    _slideAnimation = _offsetTween.animate(
      new CurvedAnimation(
        parent: _controller,
        curve: Curves.fastOutSlowIn,
      ),
    );
  }

  @override
  void didUpdateWidget(SlideTransitionExample oldWidget) {
    super.didUpdateWidget(oldWidget);
    if (oldWidget.selected == widget.selected) {
      return;
    }
    // Change the direction of the animation based on whether selected is true
    // or not.  Note that this means if you tap it twice quickly, the animation
    // will only just get started, and reverse to the original position.
    widget.selected ? _controller.forward() : _controller.reverse();
  }

  @override
  void dispose() {
    // The controller must be disposed of, so it is usually owned by a
    // StatefulWidget.
    _controller?.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        setState(() {
          widget.onTap(!widget.selected);
        });
      },
      child: Container(
        color: Colors.white,
        child: new SlideTransition(
          position: _slideAnimation,
          child: const Padding(
            padding: const EdgeInsets.all(8.0),
            child: const FlutterLogo(size: 150.0),
          ),
        ),
      ),
    );
  }
}

```
{% endprettify %}


## Transition animations widgets

The Flutter SDK <a href="https://docs.flutter.io/flutter/widgets/widgets-library.html">widgets</a> library includes the following transition animation widgets.

<div>
<table class="table" width="100%">
  <col width="25%">
  <col width="70%">
	<thead>
		<tr>
			<th>Transition widget</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/DecoratedBoxTransition-class.html">DecoratedBoxTransition</a> </td>
			<td>This widget is the animated version of a <a href="https://docs.flutter.io/flutter/widgets/DecoratedBox-class.html">DecoratedBox</a> that animates the different properties of its <a href="https://docs.flutter.io/flutter/painting/Decoration-class.html">Decoration</a>.<br>

      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/FadeTransition-class.html">FadeTransition</a> </td>
			<td>This widget animates the opacity of a widget.
      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/PositionedTransition-class.html">PositionedTransition</a> </td>
			<td>This widget is the animated version of <a href="https://docs.flutter.io/flutter/widgets/Positioned-class.html">Positioned</a> which takes a specific <a href="url">Animation</a> to transition the child's position from a start position to and end position over the lifetime of the animation. This requires that the child is part of a <a href="https://docs.flutter.io/flutter/widgets/Stack-class.html">Stack</a>.
      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/RelativePositionedTransition-class.html">RelativePositionedTransition</a> </td>
			<td>This widget is the animated version of <a href="https://docs.flutter.io/flutter/widgets/Positioned-class.html">Positioned</a> which transitions the child's position based on the value of the <a href="url">rect</a> property relative to a bounding box with the specified size. This requires that the child is part of a <a href="https://docs.flutter.io/flutter/widgets/Stack-class.html">Stack</a>.
      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/RotationTransition-class.html">RotationTransition</a> </td>
			<td>This widget animates the rotation of a widget.
      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/ScaleTransition-class.html">ScaleTransition</a> </td>
			<td>This widget animates the scale of a widget.
      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/SizeTransition-class.html">SizeTransition</a> </td>
			<td>This widget animates its own size and clips and aligns the child.
      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/widgets/SlideTransition-class.html">SlideTransition</a> </td>
			<td>This widget animates the position of a widget relative to its normal position.
      </td>
		</tr>

	</tbody>
</table>
