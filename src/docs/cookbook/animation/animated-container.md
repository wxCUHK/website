---
title: Animate the properties of a Container
next:
  title: Fade a Widget in and out
  path: /docs/cookbook/animation/opacity-animation
---

The [`Container`](https://docs.flutter.io/flutter/widgets/Container-class.html)
class provides a convenient way to create a widget with specific properties:
width, height, background color, padding, borders, and more.

Simple animations often involve changing these properties over time.
For example, you may want to animate the background color from grey to green to
indicate that an item has been selected by the user.

To animate these properties, Flutter provides the
[`AnimatedContainer`](https://docs.flutter.io/flutter/widgets/AnimatedContainer-class.html)
widget. Like the `Container` Widget, `AnimatedContainer` allows you to define
the width, height, background colors, and more. However, when the
`AnimatedContainer` is rebuilt with new properties, it automatically
animates between the old and new values. In Flutter, these types of
animations are known as "implicit animations."

This recipe describes how to use an `AnimatedContainer` to animate the size,
background color, and border radius when the user taps a button.

## Directions

  1. Create a StatefulWidget with default properties
  2. Build an `AnimatedContainer` using the properties
  3. Start the animation by rebuilding with new properties

## 1. Create a StatefulWidget with default properties

To start, create
[`StatefulWidget`](https://docs.flutter.io/flutter/widgets/StatefulWidget-class.html)
and [`State`](https://docs.flutter.io/flutter/widgets/State-class.html) classes.
Use the custom State class to define the properties you need to change over
time. In this example, that includes the width, height, color, and border
radius. In addition, you can also define the default value of each property.

These properties must belong to a custom `State` class so they can be updated
when the user taps a button.

<!-- skip -->
```dart
class AnimatedContainerApp extends StatefulWidget {
  @override
  _AnimatedContainerAppState createState() => _AnimatedContainerAppState();
}

class _AnimatedContainerAppState extends State<AnimatedContainerApp> {
  // Define the various properties with default values. Update these properties
  // when the user taps a FloatingActionButton.
  double _width = 50;
  double _height = 50;
  Color _color = Colors.green;
  BorderRadiusGeometry _borderRadius = BorderRadius.circular(8);

  @override
  Widget build(BuildContext context) {
    // Fill this out in the next steps
  }
}
```

## 2. Build an `AnimatedContainer` using the properties

Next, you can build the `AnimatedContainer` using the properties defined in the
previous step. Furthermore, you must provide a `duration` that defines how long
the animation should run. 

<!-- skip -->
```dart
AnimatedContainer(
  // Use the properties stored in the State class. 
  width: _width,
  height: _height,
  decoration: BoxDecoration(
    color: _color,
    borderRadius: _borderRadius,
  ),
  // Define how long the animation. 
  duration: Duration(seconds: 1),
  // Provide an optional curve to make the animation feel smoother. 
  curve: Curves.fastOutSlowIn,
);
```

## 3. Start the animation by rebuilding with new properties

Finally, start the animation by rebuilding the `AnimatedContainer` with
new properties. How to trigger a rebuild? When it comes to `StatefulWidgets`,
[`setState`](https://docs.flutter.io/flutter/widgets/State/setState.html) is the
solution. 

For this example, add a button to the app. When the user taps the button, update
the properties with a new width, height, background color and border radius 
inside a call to `setState`.

In a real app, you most often transition between fixed values (for example, from
a grey to a green background). For this app, generate new values each time the
user taps the button.

<!-- skip -->
```dart
FloatingActionButton(
  child: Icon(Icons.play_arrow),
  // When the user taps the button
  onPressed: () {
    // Use setState to rebuild the widget with new values.
    setState(() {
      // Create a random number generator.
      final random = Random();

      // Generate a random width and height.
      _width = random.nextInt(300).toDouble();
      _height = random.nextInt(300).toDouble();

      // Generate a random color.
      _color = Color.fromRGBO(
        random.nextInt(256),
        random.nextInt(256),
        random.nextInt(256),
        1,
      );

      // Generate a random border radius.
      _borderRadius =
          BorderRadius.circular(random.nextInt(100).toDouble());
    });
  },
);
```

## Complete example

```dart
import 'dart:math';

import 'package:flutter/material.dart';

void main() => runApp(AnimatedContainerApp());

class AnimatedContainerApp extends StatefulWidget {
  @override
  _AnimatedContainerAppState createState() => _AnimatedContainerAppState();
}

class _AnimatedContainerAppState extends State<AnimatedContainerApp> {
  // Define the various properties with default values. Update these properties
  // when the user taps a FloatingActionButton.
  double _width = 50;
  double _height = 50;
  Color _color = Colors.green;
  BorderRadiusGeometry _borderRadius = BorderRadius.circular(8);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('AnimatedContainer Demo'),
        ),
        body: Center(
          child: AnimatedContainer(
            // Use the properties stored in the State class.
            width: _width,
            height: _height,
            decoration: BoxDecoration(
              color: _color,
              borderRadius: _borderRadius,
            ),
            // Define how long the animation should take.
            duration: Duration(seconds: 1),
            // Provide an optional curve to make the animation feel smoother.
            curve: Curves.fastOutSlowIn,
          ),
        ),
        floatingActionButton: FloatingActionButton(
          child: Icon(Icons.play_arrow),
          // When the user taps the button
          onPressed: () {
            // Use setState to rebuild the widget with new values.
            setState(() {
              // Create a random number generator.
              final random = Random();

              // Generate a random width and height.
              _width = random.nextInt(300).toDouble();
              _height = random.nextInt(300).toDouble();

              // Generate a random color.
              _color = Color.fromRGBO(
                random.nextInt(256),
                random.nextInt(256),
                random.nextInt(256),
                1,
              );

              // Generate a random border radius.
              _borderRadius =
                  BorderRadius.circular(random.nextInt(100).toDouble());
            });
          },
        ),
      ),
    );
  }
}
```

![AnimatedContainer demo showing a box growing and shrinking in size while changing color and border radius](/images/cookbook/animated-container.gif){:.site-mobile-screenshot}
