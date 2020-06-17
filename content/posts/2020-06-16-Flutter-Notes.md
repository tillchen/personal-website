---
title: "Flutter Notes"
date: 2020-06-16T21:49:50+02:00
draft: false
description: "Some notes for the Flutter framework."
tags: ["Mobile Development", "Frameworks"]
---

* [Introduction](#introduction)
* [Declarative UI](#declarative-ui)
  * [Why](#why)
  * [How](#how)
* [Views](#views)
  * [The equivalent of a view in Flutter](#the-equivalent-of-a-view-in-flutter)
  * [How to update widgets](#how-to-update-widgets)
* [References](#references)

## Introduction

This post is assuming that the reader has Android development background.

## Declarative UI

### Why

1. Flutter lets the developer describe the current UI state and leaves the transitioning to the framework. This lightens the burden on developers from having to program how to transition between various UI states.

### How

1. Example:

    ![Declarative UI](/images/declarativeUIchanges.png)

    ```java
    // Imperative style
    b.setColor(red)
    b.clearChildren()
    ViewC c3 = new ViewC(...)
    b.add(c3)
    ```

    ```dart
    // Declarative style
    return ViewB(
        color: red,
        child: ViewC(...)
    )
    ```

2. In the declarative style, view configurations (such as Flutter's Widgets) are immutable and only lightweight blueprints. To change the UI, a widget triggers a rebuild on itself and constructs a new Widget subtree. The framework manages many of the responsibilities behind the scenes.

## Views

### The equivalent of a view in Flutter

1. In Flutter, the rough equivalent to a `View` is a `Widget`.
2. The difference is that widgets are immutable and lightweight.

### How to update widgets

1. Since widgets are immutable, we have to work with the state.

2. A `StatelessWidget` is a widget with no state info, similar to a `ImageView` with a logo, which doesn't change during runtime. An example is the `Text` widget, which is a subclass of `StatelessWidget`:

    ```dart
    Text(
        'I like Flutter!',
        style: TextStyle(fontWeight: FontWeight.bold),
    );
    ```

3. If we want the text to change dynamically, we wrap the `Text` widget in a `StatefulWidget`:

    ```dart
    import 'package:flutter/material.dart';

    void main() {
      runApp(SampleApp());
    }

    class SampleApp extends StatelessWidget {
      // This widget is the root of your application.
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Sample App',
          theme: ThemeData(
            primarySwatch: Colors.blue,
          ),
          home: SampleAppPage(),
        );
      }
    }

    class SampleAppPage extends StatefulWidget {
      SampleAppPage({Key key}) : super(key: key);

      @override
      _SampleAppPageState createState() => _SampleAppPageState();
    }

    class _SampleAppPageState extends State<SampleAppPage> {
      // Default placeholder text
      String textToShow = "I Like Flutter";

      void _updateText() {
        setState(() {
          // update the text
          textToShow = "Flutter is Awesome!";
        });
      }

      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: Text("Sample App"),
          ),
          body: Center(child: Text(textToShow)),
          floatingActionButton: FloatingActionButton(
            onPressed: _updateText,
            tooltip: 'Update Text',
            child: Icon(Icons.update),
          ),
        );
      }
    }
    ```


## References

* [Flutter for Android Developers](https://flutter.dev/docs/get-started/flutter-for/android-devs)

* [Introduction to Declarative UI](https://flutter.dev/docs/get-started/flutter-for/declarative)
