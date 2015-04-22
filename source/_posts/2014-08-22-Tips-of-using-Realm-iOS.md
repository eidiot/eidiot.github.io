title: Tips of using Realm iOS
date: 2014-08-22 07:58:14
tags:
---
I am trying to use [Realm](http://realm.io/) on an iOS project by [CocoaPods](http://cocoapods.org/). After hours debugging, I finally got it working and there are 3 tips I'd like to record.

## Tip 1: Work on other [CocoaPods](http://cocoapods.org/) library project

I am using [CocoaPods](http://cocoapods.org/) to separate the iOS project to libraries. [Realm](http://realm.io/) works fine with the main app and libraries separately, but the libraries couldn't be compiled as Pods project in the main app. I solved this problem by adding [Realm](http://realm.io/) framework to the Pods project and add it to the library targets myself. But I need to do this every time after I run `pod update` which I need to do frequently. I need to write a script to do it for me later.

(**Update 21.09.2014**: Realm 0.85.0 use source code instead of framework file so Tip 1 is not needed now)

## Tip 2: Runtime errors when try to write [Realm](http://realm.io/) model objects

If you need to use a RLMArray of one model object `SomeObject` in another, you need to use `RLM_ARRAY_TYPE(SomeObject)` and declare it as `RLMArray<SomeObject> *someObjects`. At first I couldn't get it work so I marked the `someObjects` property ignored. Then I can't get it work any more and the runtime error tell me nothing about what happened. The problems is, when you change one RLMObject schema and you already stored some data as the old schema, you need to delete the old Realm file (or [migrate](http://realm.io/docs/cocoa/0.83.0/#migrations)).

## Tip 3: Do not work with `<NSObject>` protocol

After I upgraded to Realm 0.85.0 and Xcode 6, Realm complained about a 'hash' property. After debugging for a while I found out that it is because some of my RLMObjects conform to `<NSObject>` protocol. It was fine with Realm 0.83.0 and Xcode 5. I'm not sure if it's because of my upgrade of Realm or Xcode but it was fixed just by removing the `<NSObject>` protocol from the RLMObjects.
