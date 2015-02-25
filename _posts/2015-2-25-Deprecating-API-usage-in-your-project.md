---
title: Deprecating API usage in your project
---

I discovered that you can deprecate external symbols in your application code. So you can warn on usage of certain APIs (even C functions) in **Foundation**, **CocoaTouch**, et al.


For example: If you are trying to enforce always using [CocoaLumberjack](github.com/CocoaLumberjack/CocoaLumberjack) for logging over the classic `NSLog` function.
You can copy the declaration of `NSLog` into your prefix header and add a deprecated notice.

```objc
FOUNDATION_EXPORT void NSLog(NSString *format, ...) NS_FORMAT_FUNCTION(1, 2) __deprecated_msg(
    "MyApp: Use DDLog");
```

This might not neccesicarly be a good idea, as framework headers can and do change between releases.
If you did want to use this, I would put them in a seperate file, `ProjectDeprecations.h`, and use a macro to standardise and define the deprecated attribute across your app.

For Classes it is a little bit better, as you can add the deprecated attribute in a *category*.

```objc


```
