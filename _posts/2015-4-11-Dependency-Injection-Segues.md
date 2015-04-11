---
layout: post
title: Dependency Injection Segues
---

Segues link ViewControllers together, they don't pass along any data (except layout data). So we have a tool to create and present our screens - how do we set it up with data?

ViewControllers are given an opportunity to set up each other in methods like `prepareForSegue:sender:`. But we don't want our ViewControllers tightly coupled.

Some argue that we don't get much from Segues if when we prepare, we cast it to a known type and initialise it anyway. Especially if you are performing the segue in code.

Here is an approach I've used to address this problem using **protocols**.

---

First we define a protocol to indicate an object can be assigned our Model. 

```objc
@protocol ModelAssignable <NSObject>

@property (nonatomic) Model *model;

@end
```

You could stop there. In your `prepareForSegue:` you can now simply check if the destination conforms to this protocol and if it does, assign the model object. Do this before any `segueIdentifier` checking – if you still have any.

I like to move this simple logic into a category on `UIStoryboardSegue`.

```objc
@implementation UIStoryboardSegue (ModelAssignable)

- (void)assignModel:(Model *)model
{
    id<ModelAssignable> destination = [self destinationViewControllerConformingToProtocol:@protocol(ModelAssignable)];
    
    destination.model = model;
}

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

I avoid having to subclass container ViewControllers by also checking if the destination is a UINavigationController, UISplitViewController, etc.

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

Now our `prepareForSegue:sender:`s look like this:

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
    [segue assignModel:self.giantModel.model];
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


You can find my example project here [Ashton-W/Example-SegueInjection](https://github.com/Ashton-W/Example-SegueInjection)
