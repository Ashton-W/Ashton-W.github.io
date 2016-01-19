---
layout: post
title: Asynchronous Testing
---

Asynchronous Testing was made much easier in Xcode with the additon of expectations and the `XCTestExpectation` class.
In addition to the basic expectation support included are helper methods for testing KVO, Notifications, and using Predicates.
Expectations are created by helper methods on `XCTestCase`.

```swift
func testExample() {
    let expectation = expectationWithDescription("Example")
    
    AsyncObject.operationWithCallback {
        expectation.fulfill()
    }
    
    waitForExpectationsWithTimeout(10, handler: nil)
}
```

## Expectation basics

```swift
expectationWithDescription(description:)
```

This is the basic expectation, you call fulfill on it 

```swift
waitForExpectationsWithTimeout(timeout:, handler:)
```

You need to wait for expectations to be fulfilled by whatever asynchronous process you are testing. As the name implies it will wait for all expectations you have created within the test. I usually only have one.

## Complex Expectations

I wrote the test code for an `NSNotification` myself once - it sucked and was a waste of time. There are a few nice built in expectation factory methods provided that cover some tricky cases that you should definitely think about using first, not only does it make your test leaner, but it's written for you.

### KVO Expectations

```swift
keyValueObservingExpectationForObject(objectToObserve:, keyPath:, expectedValue:)
keyValueObservingExpectationForObject(objectToObserve:, keyPath:, handler:)
```

If testing KVO compliance definitely use a KVO expectation. Even if not explicitly testing KVO consider if your test is interacting with a KVO compliant object.
Expectations with optional handlers will fulfill themselves if not provided.

### Notification Expectations

```swift
expectationForNotification(notificationName:, object objectToObserve:, handler:)
```

### Predicate Expectations

```swift
expectationForPredicate(predicate:, evaluatedWithObject object:, handler:)
```

I wish there was a delegate expectation helper in the standard library, it would tie things off nicely and have the most common asynchronous tests covered. An exercise for the reader, or file a radar :)
