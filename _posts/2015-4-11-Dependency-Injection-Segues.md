---
layout: post
title: Dependency Injection Segues
---

Segues link ViewControllers together, they don't pass along any data (except layout data). 
ViewControllers are given a few chances to set up each other in methods like `prepareForSegue:sender:`. However we don't want to tightly couple our ViewControllers.

It is debatable that not much is gained from Segues if when in a prepare method, the viewController cast it to a known type and initialises it anyway. Especially if the segue is performed in code.

Here is an approach I've found successful at addressing this problem by using a **protocol**.

---

First define a protocol to indicate an object can be assigned a Model.

```objc
@protocol ModelAssignable <NSObject>

@property (nonatomic) Model *model;

@end
```

You could stop there. In `prepareForSegue:sender:` you can now simply check if the destination conforms to this protocol and if it does, assign the model object. Do this before any `segueIdentifier` checking – if you still have any.

I prefer to move this logic into a category on `UIStoryboardSegue` like so:

```objc
@implementation UIStoryboardSegue (ModelAssignable)

- (void)assignModel:(Model *)model
{
	// another category method for finding the right vc
    id<ModelAssignable> destination = [self destinationViewControllerConformingToProtocol:@protocol(ModelAssignable)];
    
    destination.model = model;
}

// be lazy and assign from source to destination automatically
- (void)assignModel
{
    if (NO == [self.sourceViewController conformsToProtocol:@protocol(ModelAssignable)]) {
        return;
    }
    
    id<ModelAssignable> source = self.sourceViewController;
    
    [self assignModel:source.model];
}

@end
```

You can avoid having to subclass container ViewControllers by also checking if the destination viewController is a `UINavigationController`, `UISplitViewController`, etc.

```objc
@implementation UIStoryboardSegue (FindProtocol)

- (id)destinationViewControllerConformingToProtocol:(Protocol *)protocol
{
    id destinationViewController = self.destinationViewController;
    
    if ([destinationViewController conformsToProtocol:protocol]) {
        return destinationViewController;
    }
    
    // check for container viewControllers
    if ([destinationViewController isKindOfClass:[UINavigationController class]]) {
        UINavigationController *navigationController = destinationViewController;
        UIViewController *topViewController = navigationController.topViewController;
        
        if ([topViewController conformsToProtocol:protocol]) {
            return topViewController;
        }
    }
    
    if ([destinationViewController isKindOfClass:[UISplitViewController class]]) {
        UISplitViewController *splitViewController = destinationViewController;
        
        for (UIViewController *viewController in splitViewController.viewControllers) {
            if ([viewController conformsToProtocol:protocol]) {
                return viewController;
            }
        }
    }
    
    return nil;
}

@end
```

Now `prepareForSegue:sender:` methods will look something like this:

```objc
// screen to screen
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    [segue assignModel:self.model];
}
```

```objc
// screen to more specific screen
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    [segue assignModel:self.giantModel.subModel];
}
```

```objc
// going from a table view to a detail view
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender
{
    Model *selectedModel = [self.modelCollection modelAtIndex:self.tableView.indexPathForSelectedRow.row];
    [segue assignModel:selectedModel];
}
```

---

This technique can be used to provide [Dependency Injection](http://en.wikipedia.org/wiki/Dependency_injection) for other objects too. `NSManagedObjectContext`, `NSUserDefaults`, networking, etc.


You can find my example project here [Ashton-W/Example-SegueInjection](https://github.com/Ashton-W/Example-SegueInjection)
