---
layout: post
title: Configuration Resources
---

Sometimes we create different versions of the same app so that beta testers can install a pre-release app alongside the latest AppStore release.
In larger projects things can get even more complex, with different builds closer to the bleeding edge, nightly builds with dev switches and debug features.

To achieve this you will want to leverage configurations in Xcode to create variants of your app in addition to the default Debug and Release builds.
Build settings can changed per configuration, but resources can't - they are assigned to targets.

So I wrote a little *Build Phase Script* to add resoures to the app at build time. It chooses them by which configuration we build.

<script src="https://gist.github.com/Ashton-W/a47ec8b128ecbe470632.js?file=CopyConfigurationResources.sh"></script>
*Check out the full gist and you'll see I even wrote tests for it!*

With this script our configurations can produce builds with different bundled resources.
Some examples of what you can do with this is to include a `Settings.bundle` with dev switches or debug flags in your Debug builds.
If you are creating seperate Beta and Release builds, you can include correctly named App icon assets and have different coloured icons for the different builds too.
