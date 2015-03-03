---
layout: post
title: Xcode DerivedData
---

The go-to fix for random build issues and bugs in Xcode is to delete the *DerivedData* directory.

The venerable *DerivedData* directory is the Xcode equivalent of the *build* directory from your *Makefile* days. Except, by default it is not relative to your project source. Instead, it is in a unique (as in UUID) location under `~/Library/Developer/Xcode/`.

I prefer a relative build directory because it's closer to how you might be building your project from command line or CI scripts. Not having to divine where Xcode is keeping your object files is important for reasoning about how your code is (not) compiling.  
And of course, it's easier to blow it away with a quick `rm`!

So let's change that default and take our *build* back.

Open up Xcode's preferences window from the menu-bar, and change to the **Locations** tab. Change the **Derived Data** location to *Relative* as shown:
![Xcode Preferences Window Locations]({{ site.url }}/assets/xcode-preferences-locations.png)

Check out the **Advanced** settings to clean up some sub-directories too:
![Xcode Preferences Window Locations Advanced]({{ site.url }}/assets/xcode-preferences-locations-advanced.png)

:thumbsup:
