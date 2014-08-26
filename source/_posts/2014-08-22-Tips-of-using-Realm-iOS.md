title: Tips of using Realm iOS
date: 2014-08-22 07:58:14
tags:
---
I am trying to use [Realm](http://realm.io/) on an iOS project by [CocoaPods](http://cocoapods.org/). After hours debugging, I finally got it working and there are 2 tips I'd like to record.

## Tip 1: Work on other [CocoaPods](http://cocoapods.org/) library project

I am using [CocoaPods](http://cocoapods.org/) to separate the iOS project to libraries. [Realm](http://realm.io/) works fine with the main app and libraries separately, but the libraries couldn't be compiled as Pods project in the main app. I solved this problem by adding [Realm](http://realm.io/) framework to the Pods project and add it to the library targets myself. But I need to do this every time after I run `pod update` which I need to do frequently. I need to write a script to do it for me later.

## Tip 2: Running time error when try to write [Realm](http://realm.io/) model objects

If you need to use a RLMArray of one model object `SomeObject` in another, you need to use `RLM_ARRAY_TYPE(SomeObject)` and declare it as `RLMArray<SomeObject> *someObjects`. At first I couldn't get it work so I marked the `someObjects` property ignored. Then I can't get it work any more and the runtime error tell me nothing about what happened. The problems is, when you change one RLMObject schema and you already stored some data as the old schema, you need to delete the old Realm file (or [migrate](http://realm.io/docs/cocoa/0.83.0/#migrations)).
