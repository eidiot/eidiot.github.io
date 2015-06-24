title: FormationLayout and Protocol-Oriented Programming in Swift
tags:
- Swift
- iOS
---
My favourite talk of [WWDC 2015][1] was [Protocol-Oriented Programming in Swift][2]. However, when [Dave Abrahams][3] says "Swift is the first Protocol-Oriented Programming launguage", I didn't get it. Isn't `protocal` in Swift the same as `interface` in other languages? Isn't "Protocol-Oriented Programming" the same as one of the [SOLID principles][4] - [Dependency inversion principle][5] - "Depend upon Abstractions. Do not depend upon concretions"? It is not new at all.

Then in the later part of the talk I learned from [Dave][3] how powerful protocol is in Swift 2, especially **Self Requirement** and **Protocol Extensions**, and why we should use protocols instead of classes for abstration.  I felt excited and already Protocol-Oriented myself after watching the talk. I need to do something to make sure I am :)

So comes [FormationLayout][6], a Protocol-Oriented AutoLayout library in Swift 2. 









[1]: https://developer.apple.com/videos/wwdc/2015/
[2]: https://developer.apple.com/videos/wwdc/2015/?id=408
[3]: https://twitter.com/daveabrahams
[4]: https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)
[5]: https://en.wikipedia.org/wiki/Dependency_inversion_principle
[6]: https://github.com/evan-liu/FormationLayout