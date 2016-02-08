---
layout: post
title: Configuration Resources
---

You can create different configurations of your app for different purposes, for example: Test and Release. Release is your production configuration of the app, while Test includes debug features not meant for the AppStore.

To achieve this you will want to leverage configurations in Xcode to create variants of your app in addition to the default Debug and Release builds.
Build settings can be changed per configuration but resources can't - they are assigned to targets in the Xcode project.

So I wrote a *Build Phase Script* to add resources at build time. It chooses them by which configuration we build with.
With this script configurations can produce builds with different bundled resources.

Some examples of what you can do:

- Add a [`Settings.bundle`](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html) in the Test configuration with [Feature Toggles](https://en.wikipedia.org/wiki/Feature_toggle).
- Use unique App icon assets for Beta and Release builds so you can easily identify them on your iPhone (Check your built `.app` for the correct asset names).
- Include staging server certificates in the Test build of the app for SSL Pinning.

*Check out the full gist and you'll see I even wrote tests for it!*
<script src="https://gist.github.com/Ashton-W/a47ec8b128ecbe470632.js?file=CopyConfigurationResources.sh"></script>

Note that without editing it this script looks for directories like this:

    Resources/
    Resources-Beta/
    Resources-Debug/
    Resources-Release/
