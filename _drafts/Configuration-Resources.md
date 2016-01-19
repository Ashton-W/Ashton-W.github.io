---
layout: post
title: Configuration Resources
---

You can create different builds of your app for different purposes, for example: Test and Release. Release is your regular production version of the app but Test might include debug features.

To achieve this you will want to leverage configurations in Xcode to create variants of your app in addition to the default Debug and Release builds.
Build settings can be changed per configuration but resources can't - they are assigned to targets in the Xcode project.

So I wrote a *Build Phase Script* to add resources at build time. It chooses them by which configuration we build.
With this script configurations can produce builds with different bundled resources.
Some examples of what you can do with this is to include a `Settings.bundle` with dev switches or debug flags only in the Test version.
If you are creating seperate Beta and Release builds you can include correctly named App icon assets (Check your build product `.app`) and have different icon variations for the different builds too.

*Check out the full gist and you'll see I even wrote tests for it!*
<script src="https://gist.github.com/Ashton-W/a47ec8b128ecbe470632.js?file=CopyConfigurationResources.sh"></script>

