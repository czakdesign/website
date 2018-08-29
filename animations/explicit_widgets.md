---
layout: page
title: "Flutter Explicit Animations"
permalink: /animations/explicit_widgets/
---

* TOC Placeholder
{:toc}



Explicit animations allow you to build custom animations using the `AnimatedBuilder` widget. `AnimatedBuilder` lets you create an animation as part of a build method for another widget.


## Creating an explicit animation

To create an explicit animation, you need to create an [AnimationController](https://docs.flutter.io/flutter/animation/AnimationController-class.html) to control the animation. The controller renders the animation and holds the animation duration, the Tween, and the direction of the animation such as forward or reverse. The controller requires a `TickerProvider` that is configured using a `vsync` argument on the constructor.

<!-- 1. Declare your `StatefulWidget` class.
2. Declare the `State` class with the appropriate ticker (`SingleTickerProviderStateMixin` or `TickerProviderStateMixin`).
3. Declare an instance of `Animation<T>` and `AnimationController`.
4. Define optional and required parameters such as `vsync` and `duration`.
5. Specify the animation such as `CurvedAnimation`
6. Add the appropriate action in the `listener` callback such as `setState`.
7. Start the animation.
8. Define the `BuildContext` to return the animation.
9. Dispose and stop the animation. -->


<!-- 1. Declare your <code>StatefulWidget</code> class.  
2. Declare the <code>State</code> class with the appropriate ticker: <code>SingleTickerProviderStateMixin</code> or <code>TickerProviderStateMixin</code>.  
3. Declare an instance of <code>Animation<T\></code> and <code>AnimationController</code>.  
4. Define optional and required parameters such as <code>vsync</code> and <code>duration</code>.  
5. Specify the animation such as <code>CurvedAnimation</code>.  
6. Add the appropriate action in the <code>listener</code> callback such as <code>setState</code>.  
7. Start the animation.  
8. Define the <code>BuildContext</code> to return the animation.  
9. Dispose and stop the animation.   -->

<div>
<table class="table" width="100%">
  <col width="45%">
  <col width="55%">
	<tbody>
    <tr>
      <td><img src="/animations/images/explicit3.png" alt="Explicit animation widgets"></td>
      <td>
      1. Import the appropriate Flutter package.<br>
      2. Declare your <code>StatefulWidget</code> class.<br>
      3. Declare the <code>State</code> class with the appropriate ticker: <code>SingleTickerProviderStateMixin</code> or <code>TickerProviderStateMixin</code>.<br>
      4. Declare an instance of <code>Animation&lt;T&gt;</code> and <code>AnimationController</code>.<br>
      5. Define optional and required parameters such as <code>vsync</code>, <code>duration</code>, and <code>tween</code>.<br>
      6. Specify the animation such as <code>CurvedAnimation</code>. <br>
      7. Add the appropriate action in the <code>listener</code> callback such as <code>setState</code>. <br>
      8. Start the animation.  <br>
      9. Define the <code>BuildContext</code> to return the animation. <br>
      10. Stop and dispose of the animation. <br>
      </td>
    </tr>
   </tbody>
  </table>
</div>

Once you’ve created a controller, you can use it to build other animations. For example, you can create a ForwardAnimation then modify some of the parameters to create a ReverseAnimation that mirrors the original animation but runs in reverse. Similarly, you can create a CurvedAnimation whose value is adjusted by a curve and then reverse it.

## Flutter with and without an explicit animation
Two code samples are shown below to highlight the difference between a Flutter app with an animation and a Flutter app without an animation.

### Flutter logo without an animation
<!-- <table cellpadding="10">
  <tr>
    <td style="width:25%">
    <img src="/animations/images/explicit_animation1.png" name="MyImage">
    </td>
    <td>
    The widget renders the Flutter logo but without animation.
    </td>
  </tr>
</table> -->

The following application draws the Flutter logo but it doesn't include any way to control an animation. It doesn't include an `AnimationController`, or `vsync`, `ticker`, or `listener`.

```Dart
// Simple Flutter app without an animated logo

import 'package:flutter/material.dart';

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> {
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        margin: EdgeInsets.symmetric(vertical: 10.0),
        height: 300.0,
        width: 300.0,
        child: FlutterLogo(),
      ),
    );
  }
}

void main() {
  runApp(LogoApp());
}
```
<a name="pookie"></a>
### Flutter logo with an explicit animation
The following application animates the Flutter logo.

<table cellpadding="10">
  <tr>
    <td style="width:25%">
    <a href="" onMouseOver="document.MyImage2.src='/animations/images/explicit_animation.gif';" onMouseOut="document.MyImage2.src='/animations/images/explicit_animation.png';">
    <img src="/animations/images/explicit_animation.png" name="MyImage2">
    </a></td>
    <td>
    The widget displays an animated Flutter logo that grows from nothing to full size.
    </td>
  </tr>
</table>

To render the animation with an `Animation<T>` object, store the `Animation` object as a member of your widget, then use its changing value to draw the animation.

* When you define an `AnimationController`, you must pass in a `vsync` object. The presence of `vsync` prevents offscreen animations from consuming unnecessary resources. You can use your stateful object as the `vsync` by adding `SingleTickerProviderStateMixin` to the class definition.
* You must specify a ticker, [tween](/animations/concepts#tweens), and listener.  
* The `addListener()` function calls `setState()`, so every time the animation generates a new value, the current frame is marked dirty, which forces a rebuild.   
* During the build, the container changes size because its height and width now use `animation.value` instead of a hardcoded value.  
* Dispose of the controller when the animation is finished to prevent memory leaks.  


The changes to the non-animated example are highlighted.

{% prettify dart %}
```Dart
// Flutter app with an animated logo
[[highlight]]import 'package:flutter/animation.dart';[[/highlight]]
import 'package:flutter/material.dart';
class LogoApp extends StatefulWidget {
  _LogoAppState createState() =>  _LogoAppState();
}
class _LogoAppState extends State<LogoApp> [[highlight]]with SingleTickerProviderStateMixin {
  Animation<double> animation;
  AnimationController controller;
  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = Tween(begin: 0.0, end: 300.0).animate(controller)
      ..addListener(() {
        setState(() {
// the state that has changed here is the animation object’s value
        });
      });
    controller.forward();
  }[[/highlight]]
  Widget build(BuildContext context) {
    return Center(
        child: Container(
        margin: EdgeInsets.symmetric(vertical: 10.0),
    height: [[highlight]]animation.value,[[/highlight]]  
    width: [[highlight]]animation.value,[[/highlight]]  
    child: FlutterLogo(),
    ),
    );
  }
  [[highlight]]dispose() {
    controller.dispose();
    super.dispose();[[/highlight]]  
  }
}
void main() {
  runApp(LogoApp());
}
```
{% endprettify %}  

<a name="pookie3"></a>
## Simplifying with AnimatedWidget  
To simplify an explicit animation, use the `AnimatedWidget` class  instead of using `addListener()` and `setState`.  
<table cellpadding="10">
  <tr>
    <td style="width:25%">
    <a href="" onMouseOver="document.MyImage3.src='/animations/images/explicit_animation.gif';" onMouseOut="document.MyImage3.src='/animations/images/explicit_animation.png';">
    <img src="/animations/images/explicit_animation.png" name="MyImage3">
    </a></td>
    <td>This example shows how <code>AnimatedWidget</code> is used to produce the same animation as the animation shown above in the <a href="#pookie">Flutter logo with an explicit animation</a> section above.
    The app displays an animated Flutter logo that grows from nothing to full size.
    </td>
  </tr>
</table>

The `AnimatedWidget` class allows you to separate the widget code from the animation code in the `setState()` call. `AnimatedWidget` doesn’t need to maintain a `State` object to hold the animation.

In the refactored example below, LogoApp now derives from `AnimatedWidget` instead of `StatefulWidget`. `AnimatedWidget` uses the current value of the animation when drawing itself. The LogoApp still manages the `AnimationController` and the `Tween`.

The LogoApp passes the `Animation` object to the base class and uses `animation.value` to set the height and width of the container.


{% prettify dart %}
```Dart
// Simple Flutter animation using AnimatedWidget to create a separate animation widget

import 'package:flutter/animation.dart';
import 'package:flutter/material.dart';

[[highlight]]class AnimatedLogo extends AnimatedWidget {
  AnimatedLogo({Key key, Animation<double> animation})
      : super(key: key, listenable: animation);

  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return Center(
      child: Container(
        color: Colors.white,
        margin: EdgeInsets.symmetric(vertical: 10.0),
        height: animation.value,
        width: animation.value,
        child: FlutterLogo(),
      ),
    );
  }
}[[/highlight]]

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = Tween(begin: 0.0, end: 300.0).animate(controller);
    controller.forward();
  }

  Widget build(BuildContext context) {
    return AnimatedLogo(animation: animation);
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}

void main() {
  runApp(LogoApp());
}

```
{% endprettify %}

## Monitoring the progress of an animation

It’s often helpful to know when an animation changes state, such as starting, stopping, or reversing direction.

Use `addStatusListener` for notifications of state changes.

<!-- skip -->
{% prettify dart %}
```Dart
class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = Tween(begin: 0.0, end: 300.0).animate(controller)
      [[highlight]]..addStatusListener((state) => print("$state"));[[/highlight]]
    controller.forward();
  }
  //...
}

```
{% endprettify %}

The result might show the following status.

<!-- skip -->
{% prettify dart %}
```Dart
AnimationStatus.forward
AnimationStatus.completed
```
{% endprettify %}

To reverse the animation at the beginning or end (and create a "breathing" effect), add `animation.addStatusListener`.

<!-- skip -->
{% prettify dart %}
```Dart
class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = Tween(begin: 0.0, end: 300.0).animate(controller);

    [[highlight]]animation.addStatusListener((status) {
      if (status == AnimationStatus.completed) {
        controller.reverse();
      } else if (status == AnimationStatus.dismissed) {
        controller.forward();[[/highlight]]
      }
    });
    controller.forward();
  }
  //...
}

```
{% endprettify %}

## Refactoring with AnimatedBuilder

You can refactor or restructure your animation code to improve code readability and reduce complexity. Refactoring also improves the source-code maintainability and creates a more expressive internal architecture or object model to improve extensibility.

By separating the responsibilities, or refactoring the code into different classes, you can separate the animation code so that if you change the animation, you won't need to change the widget that renders the animated object.  

To refactor animation code, restructure your code into the following sections:

1. Render the image.
2. Define the animation object.
3. Render the animation.

In this section, we'll refactor the animation code described in the sections above.

### Render the image
Rendering the image in this example is straightforward—you create the object that is used as a parameter by `AnimatedBuilder` later when you define the Animation object.  
<!-- skip -->
{% prettify dart %}
```Dart
// Render the image (logo)

import 'package:flutter/animation.dart';
import 'package:flutter/material.dart';

class LogoWidget extends StatelessWidget {
  // Leave out the height and width so it fills the animating parent
  build(BuildContext context) {
    return Container(
        margin: EdgeInsets.symmetric(vertical: 10.0),
        child: FlutterLogo());
  }
}
```
{% endprettify %}  

### Define the animation object
When you define the animation object widget, it's stateless and holds the set of final variables that are used to define the animation. The `build()` method creates and returns the `AnimatedBuilder` which takes an anonymous builder method and the rendered image object as parameters. The anonymous `build()` method creates a Container of the appropriate size to force the image to shrink to fit.

Similar to `AnimatedWidget`, you don't need to call `addListener()` because `AnimatedBuilder` automatically listens to notifications from the `Animation` object.

Notice that it looks like the child is specified twice. The outer reference of child is passed to `AnimatedBuilder` which passes it to the anonymous closure—which then users that object as its child. The result is that the `AnimatedBuilder` is inserted between the two widgets in the render tree.

<!-- skip -->
{% prettify dart %}
```Dart
// Define the Animation object

class GrowTransition extends StatelessWidget {
  GrowTransition({this.child, this.animation});

  final Widget child;
  final Animation<double> animation;

  Widget build(BuildContext context) {
    return Center(
      child: AnimatedBuilder(
          animation: animation,
          builder: (BuildContext context, Widget child) {
            return Container(
                height: animation.value, width: animation.value, child: child);
          },
          child: child),
    );
  }
}
```
{% endprettify %}  

### Render the animation  

The code to render the animation looks very similar to the code shown above in the [Flutter logo with an explicit animation](#pookie) section.

The `initState()` method creates an `AnimationController` and specifies the duration, vsync, type of animation (`CurvedAnimation`), and Tween then binds them with `animate()`. The `build()` method returns a `GrowTransition` object with a `LogoWidget` as a child and an animation object to drive the transition.




<!-- skip -->
{% prettify dart %}
```Dart
// Render the animation

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with SingleTickerProviderStateMixin {
  Animation animation;
  AnimationController controller;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    final CurvedAnimation curve =
        CurvedAnimation(parent: controller, curve: Curves.easeIn);
    animation = Tween(begin: 0.0, end: 300.0).animate(curve);
    controller.forward();
  }

  Widget build(BuildContext context) {
    return GrowTransition(child: LogoWidget(), animation: animation);
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}

void main() {
  runApp(LogoApp());
}

```
{% endprettify %}



Putting it all together. You can find the source code for this example in [Refactoring animation code](https://raw.githubusercontent.com/flutter/website/master/src/_includes/code/animation/animate4/main.dart).  


## Simultaneous animations

To create simultaneous animations, you can use multiple tweens on the same AnimationController—where each tween manages a different effect in the animation.  

<div>
<table cellpadding="10">
  <tr>
    <td style="width:25%">
    <a href="" onMouseOver="document.MyImage5.src='/animations/images/multiple_animations.gif';" onMouseOut="document.MyImage5.src='/animations/images/explicit_animation.png';">
    <img src="/animations/images/explicit_animation.png" name="MyImage5">
    </a></td>
    <td>The sample code in this section shows how you can build on the example shown in <a href="#pookie3">Simplifying with AnimatedWidget</a> to create a continuous and simultaneous animation.
    </td>
  </tr>
</table>
</div>



The snippet below shows one AnimationController and two tweens: one tween manages the opacity and the another tween manages the size.

<!-- skip -->
{% prettify dart %}
```Dart
final AnimationController controller =
    AnimationController(duration: const Duration(milliseconds: 2000), vsync: this);
final Animation<double> [[highlight]]sizeAnimation =
    Tween(begin: 0.0, end: 300.0).animate(controller);[[/highlight]]
final Animation<double> [[highlight]]opacityAnimation =
    Tween(begin: 0.1, end: 1.0).animate(controller);[[/highlight]]
```
{% endprettify %}

You can get the size with `sizeAnimation.value` and the opacity with `opacityAnimation.value`, but the constructor for `AnimatedWidget` only takes a single Animation object. To work with this constraint, the example shows how to create separate Tween objects and explicitly calculate the values.

The LogoApp widget was changed to encapsulate its own Tween objects. Its build method calls the Tween `.evaluate()` function on the parent’s animation object to calculate the required size and opacity values. The code changes are highlighted.

<!-- skip -->
{% prettify dart %}
```Dart
import 'package:flutter/animation.dart';
import 'package:flutter/material.dart';

class AnimatedLogo extends AnimatedWidget {
  // The Tweens are static because they don't change.
  [[highlight]]static final _opacityTween = Tween<double>(begin: 0.1, end: 1.0);[[/highlight]]
  [[highlight]]static final _sizeTween = Tween<double>(begin: 0.0, end: 300.0);[[/highlight]]

  AnimatedLogo({Key key, Animation<double> animation})
      : super(key: key, listenable: animation);

  Widget build(BuildContext context) {
    final Animation<double> animation = listenable;
    return Center(
      [[highlight]]child: Opacity([[/highlight]]
        [[highlight]]opacity: _opacityTween.evaluate(animation),[[/highlight]]
        child: Container(
          color: Colors.white,
          margin: EdgeInsets.symmetric(vertical: 10.0),
          height: [[highlight]]_sizeTween.evaluate(animation),[[/highlight]]
          width: [[highlight]]_sizeTween.evaluate(animation),[[/highlight]]
          child: FlutterLogo(),
        ),
      ),
    );
  }
}

class LogoApp extends StatefulWidget {
  _LogoAppState createState() => _LogoAppState();
}

class _LogoAppState extends State<LogoApp> with TickerProviderStateMixin {
  AnimationController controller;
  Animation<double> animation;

  initState() {
    super.initState();
    controller = AnimationController(
        duration: const Duration(milliseconds: 2000), vsync: this);
    animation = [[highlight]]CurvedAnimation(parent: controller, curve: Curves.easeIn);[[/highlight]]

    animation.addStatusListener((status) {
      if (status == AnimationStatus.completed) {
        controller.reverse();
      } else if (status == AnimationStatus.dismissed) {
        controller.forward();
      }
    });

    controller.forward();
  }

  Widget build(BuildContext context) {
    return AnimatedLogo(animation: animation);
  }

  dispose() {
    controller.dispose();
    super.dispose();
  }
}

void main() {
  runApp(LogoApp());
}

```
{% endprettify %}




## Explicit animation widget examples

The table below lists some of the Flutter widgets that use explicit animations in their implementations. Links to the source code are provided so that you can view the code as a reference for writing your own explicit animations.

<div>
<table class="table" width="100%">
  <col width="25%">
  <col width="70%">
	<thead>
		<tr>
			<th>Explicit widget</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/material/BottomSheet-class.html">BottomSheet</a> </td>
			<td>This widget is a material design bottom sheet. There are two kinds of bottom sheets in material design:  <br>      
      <b>Persistent</b>—A persistent bottom sheet shows information that supplements the primary content of the app. A persistent bottom sheet remains visible even when the user interacts with other parts of the app.  <br>
      <b>Modal</b>—A modal bottom sheet is an alternative to a menu or a dialog and prevents the user from interacting with the rest of the app.  <br>
<!-- <span style="padding-left: 20px; display:block">
</span> -->  
			Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
		</tr>
    <tr>
			<td><a href="https://docs.flutter.io/flutter/material/ExpansionTile-class.html">ExpansionTile</a> </td>
			<td>This widget is a single-line <a href="https://docs.flutter.io/flutter/material/ListTile-class.html">ListTile</a> with a trailing button that expands or collapses the tile to reveal or hide the children.<br>
			Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
		</tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/PopupMenuButton-class.html">PopupMenuButton</a> </td>
      <td>This widget displays a menu when pressed and calls <code>onSelected</code> when the menu is dismissed because an item was selected. The value passed to <code>onSelected</code> is the value of the selected menu item. <br>
      	Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/ProgressIndicator-class.html">ProgressIndicator</a> </td>
      <td>This widget is a base class for Material Design progress indicators.<br>
      Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/RefreshIndicator-class.html">RefreshIndicator</a> </td>
      <td>This widget supports the Material <em>swipe to refresh</em> idiom.<br>
      Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/Scaffold-class.html">Scaffold</a> </td>
      <td>This widget implements the basic Material Design visual layout structure and provides APIs for showing drawers, snack bars, and bottom sheets.<br>
      Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/SnackBar-class.html">SnackBar</a> </td>
      <td>This widget is a lightweight message with an optional action which briefly displays at the bottom of the screen. To display a snack bar, call <code>Scaffold.of(context).showSnackBar()</code>, passing an instance of <code>SnackBar</code> that describes the message.<br>
      Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/TabBar-class.html">TabBar</a> </td>
      <td>This widget is a Material Design widget that displays a horizontal row of tabs.<br>
      Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/TextField-class.html">TextField</a> </td>
      <td>This widget is a Material Design text field that lets a user enter text, either with the hardware keyboard or with an onscreen keyboard. <br>
      Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
    <tr>
      <td><a href="https://docs.flutter.io/flutter/material/ExpansionTile-class.html">ExpansionTile</a> </td>
      <td>This widget is a single-line <a href="https://docs.flutter.io/flutter/material/ListTile-class.html">ListTile</a> with with a trailing button that expands or collapses the tile to reveal or hide the children. <br>
      Flutter library: <a href="https://docs.flutter.io/flutter/material/material-library.html">material</a>
      </td>
    </tr>
	</tbody>
</table>
</div>
