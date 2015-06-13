title: Observer Pattern and Decoupling in Swift
tags:
- Swift
- iOS
- Design Pattern
---
[MVC](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) is central in Cocoa and Cocoa Touch,  while [Observer Pattern](http://en.wikipedia.org/wiki/Observer_pattern) is an important part of MVC to decouple model. There are several different variations of observer pattern and also other tools to help write [loose coupling](http://en.wikipedia.org/wiki/Loose_coupling) code in swift and Cocoa Touch: 

- [Cocoa Touch](#Cocoa_Touch)
  - [NSNotification](#NSNotification)
  - [Events](#Events)
  - [Target-Action](#Target-Action)
  - [Delegate](#Delegate)
- [Swift](#Swift)
  - [Property Observers](#Property_Observers)
- [Third Party Libraries](#Third_Party_Libraries)
  - [PromiseKit](#PromiseKit)
  - [Bond](#Bond)
  - [ReactKit](#ReactKit)
  - [RxSwift](#RxSwift)
  - [emitter-kit](#emitter-kit)
  - [SwiftEventBus](#SwiftEventBus)
  - [Signals](#Signals)

## Cocoa Touch

In Cocoa Touch frameworks there are some tools and patterns work both in Objective-C and Swift: 
<!-- more -->

### NSNotification

NSNotificationCenter works in two ways: Add observers to subject objects, or add to Event-Bus instead(pass nil to notificationSender). 

```swift
func addObserver(_ notificationObserver: AnyObject, 
          selector notificationSelector: Selector, 
                  name notificationName: String?, 
              object notificationSender: AnyObject?)
```

#### pros 

- Basic observer pattern concepts: addObserver/removeObserver.
- Work as system Event-Bus.

#### cons

- 'String' based: 
  - Error prone. 
  - Hard to refactor.
- Need to `removeObserver` carefully in `deinit`. 
- Weak typed payload.
- Can not define in `protocal`.

### Events

Use [Events](https://developer.apple.com/library/ios/documentation/General/Conceptual/Devpedia-CocoaApp/EventHandlingiPhone.html#//apple_ref/doc/uid/TP40009071-CH13-SW1) to handle user operations on device.

```objc
typedef enum {
   UIEventTypeTouches,
   UIEventTypeMotion,
   UIEventTypeRemoteControl,
} UIEventType;
```

### Target-Action

[Target-Action](https://developer.apple.com/library/ios/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) pattern is commonly used between views and view controllers in UIKit framework.

- Map [Events](#Events) and pass the mapped `UIEvent` if required by action method signature.
- Work together with XCode(`IBAction`).

### Delegate

[Delegation](https://developer.apple.com/library/ios/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html) is another commonly used pattern to decouple objects(usually views). It is better then `NSNotification` for one-to-one observers, like views and their controllers. 

## Swift

### Property Observers

[KVO(Key-Value Observing)](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html) provides a mechanism to observe object properties chagnes in Objective-C. It is not availabe in Swift but we have Property Observers instead. Differences between the two: 

- KVO allows objects to observe properties changes of other objects. Property Observers only work inside the object. 
- KVO notify after the property change. Property Observers have `willSet` and `didSet`. 
- Property Observers can work together with other tools like NSNotification, Delegation, etc. 

## Third Party Libraries

### PromiseKit

[PromiseKit](https://github.com/mxcl/PromiseKit) is an implementation of [Promise](http://en.wikipedia.org/wiki/Futures_and_promises) in both Swift and Objective-C.

```swift
UIApplication.sharedApplication().networkActivityIndicatorVisible = true

when(fetchImage(), getLocation()).then { image, location in
    self.imageView.image = image;
    self.label.text = "Buy your cat a house in \(location)"
}.finally {
    UIApplication.sharedApplication().networkActivityIndicatorVisible = false
}.catch { error in
    UIAlertView(…).show()
}
```

Without promises we usually need Events/Callbacks to handle asynchronous operation result and sometimes need to check the result/state first. Less code and less pain with promises now. 

### Bond

[Bond](https://github.com/SwiftBond/Bond) is a binding framework that listen to changes and update another value in listeners automaticly. 

```swift
textField ->> label
```

Simply change bind label to textField. Or do something before set the label: 

```swift
textField.dynText.map { "Hi " + $0 } ->> label
```
And there are some more powerful features like two way bindings. 

```swift
viewModel.username <->> usernameTextField.dynText
```

### ReactKit

[ReactKit](https://github.com/ReactKit/ReactKit)

### RxSwift

[RxSwift](https://github.com/kzaher/RxSwift)

### emitter

[emitter-kit](https://github.com/aleclarson/emitter-kit)

### SwiftEventBus

[SwiftEventBus](https://github.com/cesarferreira/SwiftEventBus)

### Signals

[Signals](https://github.com/artman/Signals)