---
layout: post
title: Xcode Breakpoints
---

Xcode has many great debugging features, the most basic of which are **Breakpoints**. Let's take a deeper look at **Breakpoints** in Xcode.

# Breakpoint Navigator

The Breakpoint Navigator pane in Xcode has almost all the functions for creating and managing your breakpoints.

![Xcode Breakpoints Navigator]({{ site.url }}/assets/breakpoints/navigator.png)

# Types of Breakpoints 

#### All Exception breakpoint

Stop on any and all Exceptions.

#### Objective-C Exception breakpoint

Stop on Objective-C Exceptions, eg: `NSException`.

#### Swift Error breakpoint

Stop on Swift Errors, e.g.: types conforming to `ErrorType`.

New in *Xcode 7.1 beta 3*.

#### Test Failure breakpoint

Stop when a Test fails. `XCTest` and compatible frameworks only.

#### Symbolic Breakpoints

![Symbolic Breakpoint on viewDidLoad]({{ site.url }}/assets/breakpoints/viewDidLoad.png)

Stop on a Symbol. A Symbol in this context is a selector or method name, or a function name. Methods can be scoped to a class.

If a symbol is found multiple times, like a selector implemented in more than one class, Xcode will allow you to expand your breakpoint in the navigator and control each of these sub-breakpoints.

You may need to debug your app once to find the symbols. You should also restrict the modules to your own app or frameworks, since it will pick up all of *UIKit* too.

Examples:  

    viewDidLoad`  
    [SKTLine drawHandlesInView]`  
    people::Person::name()`  
    _objc_msgForward`  

![Symbolic Breakpoint on viewDidLoad scoped to Banana app]({{ site.url }}/assets/breakpoints/viewDidLoad-edit.png)

## Advanced Options

![Editing a Breakpoint]({{ site.url }}/assets/breakpoints/edit.png)

#### Conditions

Evaluate an expression to determine if the breakpoint should pause or perform any actions.

You can speed up your debugging by breaking when an array has 0 elements, or a string is nil, for example. Much better than adding temporary code to give you a branch to break inside with log statements.

#### Ignore

Ignore a breakpoint until it has been hit a given number of times.

## Actions

Actions allow you add Log Messages, Debugger Commands, and Sounds to your breakpoints.
You can even do AppleScript and Shell Commands, if you want to.

The best part is that you can add multiple actions. So you can have a single breakpoint log a message that counts how many times it has been hit (with the `%H` macro) and also ribbits like a frog. :frog:

![A Log Message breakpoint]({{ site.url }}/assets/breakpoints/message.png)

My favourite custom breakpoint is the [**Reveal**](http://revealapp.com/) breakpoint.
Automatically set up all apps you run in the simulator with the ability to inspect views in Reveal. It's [documented](http://support.revealapp.com/kb/getting-started/integrating-reveal-load-reveal-without-changing-your-xcode-project) so I won't repeat it.

#### Automatically Continue

Under *Options* you will see 'Automatically continue after evaluating actions', which does what it says, the breakpoint will keep running the app.

This is extremely useful if you just want the side affects of the breakpoint, like the **Reveal** breakpoint.

Sometimes you will want this for debugging too. If you add a *log* action to debug something in real-time, you can monitor the console while using the app normally without interruptions.

A neat trick is to use the *play sound* action with *continue*. It's great for when you just want to confirm some code is executed at the right time in your app without slowing you down with a bunch of pauses.

# Shared Breakpoints

I don't encourage the use of this feature. I have not found a good reason for sharing breakpoints with other developers on a project, breakpoints are fleeting - in the moment debugging tools.
Even if you wanted to have a collection of project breakpoints within your team, the biggest problem is the state of the breakpoints is shared too. If one developer enables a breakpoint and commits that change, everyone that does a pull will have that breakpoint turned on.
You could fix this with some git hooks if you really wanted to use this feature, but I shy away from that stuff as it adds complexity to your workflow.

# User Breakpoints

![User Breakpoints]({{ site.url }}/assets/breakpoints/user.png)

Breakpoints can be made available in all projects by moving them to your User Breakpoints. I highly recommend using this setting for all breakpoints not referencing a specific project.

I've made *my* User Breakpoints publicly available in [this gist](https://gist.github.com/Ashton-W/5c1ede17f8cec1f8b529). I don't use them all every day, but I feel this is a good collection of breakpoints everyone should start with.
