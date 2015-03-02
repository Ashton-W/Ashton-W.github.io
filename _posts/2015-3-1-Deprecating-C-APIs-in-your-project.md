---
layout: post
title: Deprecating C APIs in your project
---

I discovered that you can deprecate external symbols in your application code. So you can warn on usage of certain C APIs from **CoreFoundation** et al.


For example: If you are trying to enforce always using [CocoaLumberjack](github.com/CocoaLumberjack/CocoaLumberjack) for logging over the classic `NSLog` function.
You can copy the declaration of `NSLog` into your prefix header and add a deprecated notice.

```objc
FOUNDATION_EXPORT void NSLog(NSString *format, ...) NS_FORMAT_FUNCTION(1, 2) __deprecated_msg(
    "MyApp: Use DDLog");
```

This might not be a good idea, as framework headers can and do change between releases.
I would at least put them in a seperate file, `ProjectDeprecations.h`, and use a macro to standardise and define the deprecated attribute.

I haven't found a way to do this on Objective-C classes, maybe that's for the best.

**Update from the Twitters**

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/AshtonDev">@ashtondev</a> for Objective-C: <a href="https://t.co/EsXqMcVes4">https://t.co/EsXqMcVes4</a></p>&mdash; Tony Arnold (@tonyarnold) <a href="https://twitter.com/tonyarnold/status/572181271289315328">March 1, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
