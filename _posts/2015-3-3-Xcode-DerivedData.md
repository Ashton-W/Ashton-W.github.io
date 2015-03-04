---
layout: post
title: Xcode DerivedData
---

The go-to fix for random build issues and bugs in Xcode is to delete the *DerivedData* directory. The shortcut for that is holding alt/option with the **Product** menu open: *Clean Build Folder*, or the [keyboard shortcut `⌥⌘⇧K`][1]

The venerable *DerivedData* directory is the Xcode equivalent of the *build* directory from your *Makefile* days. Except, by default it is not relative to your project source. Instead, it is in a unique (as in UUID) location under `~/Library/Developer/Xcode/`.

I prefer a relative build directory because it's closer to how you might be building your project from command line or CI scripts. Not having to divine where Xcode is keeping your object files is important for reasoning about how your code is (not) compiling.  
And of course, it's easier to blow it away with a quick `rm`!

So let's change that default and take our *build* back.

Open up Xcode's preferences window from the menu-bar, and change to the **Locations** tab. Change the **Derived Data** location to *Relative* as shown:
![Xcode Preferences Window Locations]({{ site.url }}/assets/xcode-preferences-locations.png)

Under the **Advanced** settings you can change the *Build* directory. Be warned, if you change this Xcode will no longer *Clean Build Folder* for you with the [keyboard shortcut ⌥⌘⇧K][1]:
![Xcode Preferences Window Locations Advanced]({{ site.url }}/assets/xcode-preferences-locations-advanced.png)

:thumbsup:

[1]: keyboard shortcut ⌥⌘⇧K ( Option Cmd Shift K )

---
**Update**

Some great feedback came through from Twitter.

First I want to clarify. The only reason I recommend this configuration is for when problems aren't being fixed by a *Command-Shift-K* Clean in Xcode.

Unit Tests not being recognised. Interface Builder Designables not building. Headers not being found.  
Usually you can fix it with a clean, building for test, refreshing all views, or restarting Xcode. But sometimes it just won't give &mdash; and [I'm not the only one](https://github.com/kattrali/deriveddata-exterminator).

Your milage may vary.

---
Then a tweet happened

<blockquote class="twitter-tweet" lang="en"><p>I wrote about my old friend, DerivedData. My preferred preferences.&#10;<a href="http://t.co/hnSaItNqGl">http://t.co/hnSaItNqGl</a>&#10;<a href="https://twitter.com/hashtag/iOSDev?src=hash">#iOSDev</a></p>&mdash; Ashton (@AshtonDev) <a href="https://twitter.com/AshtonDev/status/572891890250485760">March 3, 2015</a></blockquote>

[@tonyarnold](https://twitter.com/tonyarnold) and [@mokagio](https://twitter.com/mokagio) kicked off a discussion, pulling in [@joar_at_work](https://twitter.com/joar_at_work)

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/tonyarnold">@tonyarnold</a> Let me preface my other comments with: Everything is a tradeoff… <a href="https://twitter.com/AshtonDev">@AshtonDev</a> <a href="https://twitter.com/mokagio">@mokagio</a></p>&mdash; Joar Wingfors (@joar_at_work) <a href="https://twitter.com/joar_at_work/status/572973755997155330">March 4, 2015</a></blockquote>

<blockquote class="twitter-tweet" data-conversation="none" lang="en"><p><a href="https://twitter.com/tonyarnold">@tonyarnold</a> The default location provides correct behavior with no required user configuration. <a href="https://twitter.com/AshtonDev">@AshtonDev</a> <a href="https://twitter.com/mokagio">@mokagio</a></p>&mdash; Joar Wingfors (@joar_at_work) <a href="https://twitter.com/joar_at_work/status/572973804755922947">March 4, 2015</a></blockquote>

<blockquote class="twitter-tweet" data-conversation="none" lang="en"><p><a href="https://twitter.com/tonyarnold">@tonyarnold</a> The “next to the project” model doesn’t work out of the box when multiple projects contribute to the build. <a href="https://twitter.com/AshtonDev">@AshtonDev</a> <a href="https://twitter.com/mokagio">@mokagio</a></p>&mdash; Joar Wingfors (@joar_at_work) <a href="https://twitter.com/joar_at_work/status/572974149519339520">March 4, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/AshtonDev">@AshtonDev</a> Yeah, workspace-relative should provide the same benefit. <a href="https://twitter.com/tonyarnold">@tonyarnold</a> <a href="https://twitter.com/mokagio">@mokagio</a></p>&mdash; Joar Wingfors (@joar_at_work) <a href="https://twitter.com/joar_at_work/status/572975640703471616">March 4, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/AshtonDev">@AshtonDev</a> Don’t let that stand. Please try to analyze how you end up in these situations, and file bug reports! <a href="https://twitter.com/tonyarnold">@tonyarnold</a> <a href="https://twitter.com/mokagio">@mokagio</a></p>&mdash; Joar Wingfors (@joar_at_work) <a href="https://twitter.com/joar_at_work/status/572976862588084224">March 4, 2015</a></blockquote>

<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

