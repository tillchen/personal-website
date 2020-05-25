+++
draft = false
date = 2019-08-13
title = "iOS Development Notes"
description = "Some notes for iOS development"
tags = ["Mobile Development"]
+++

* [Basics](#basics)
* [View Controller Lifecycle:](#view-controller-lifecycle)
* [MVC Architecture](#mvc-architecture)
* [Project Organization](#project-organization)
* [References](#references)

## Basics

1. Model-View-Controller is a design pattern in iOS.

2. Unlike in Android Studio, the Interface Builder is not a graphical representation of code. A storyboard file is an archive of object instances.

3. Outlets are references to objects: `@IBOutlet var questionLabel: UILabel!` (IB means interface builder.) (Right click the item and drag it to the assistant editor to create the outlet.)

4. Actions: `@IBAction func showNextQuestion(_ sender: UIButton) {}` (Right click the item and drag it to the assistant editor to create the action.)

5. Use `View Controller` to add new screens. Use Embed in > `Navigation Controller` to add a nav controller.

6. Use this to pass info between screens:

    ```swift
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        segue.destination.navigationItem.title = textField.text
    }
    ```

7. Use `performSegue` to create a segue programmatically (could be used for conditional segue.)

    ```swift
    performSegue(withIdentifier: "Foo", sender: nil)
    ```

8. Right click and drag to a view controller to add the new view controller to the Tab Bar Controller (select view controllers).

9. Connect the new ViewController file by setting the custom class attribute in storyboard.

## View Controller Lifecycle:

1. ![Lifecycle image](/images/ios_lifecycle.png)

2. Detailed methods: <https://guides.codepath.com/ios/View-Controller-Lifecycle>

## MVC Architecture

1. ![MVC image](/images/mvc.png)

## Project Organization

1. Example: ![project organization image](/images/ios_project_organization.jpeg)

## References

* [iOS Programming: The Big Nerd Ranch Guide](https://www.amazon.com/iOS-Programming-Ranch-Guide-Guides/dp/0134682335/ref=sr_1_2?keywords=ios+programming&qid=1564912891&s=gateway&sr=8-2)

* App Development With Swift
