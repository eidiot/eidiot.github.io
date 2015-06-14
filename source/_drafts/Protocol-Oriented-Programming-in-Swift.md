title: Protocol-Oriented Programming in Swift
tags:
- Swift
- iOS
---
My favourite talk of [WWDC 2015][1] was [Protocol-Oriented Programming in Swift][2]. However, when [Dave Abrahams][3] says "Swift is the first Protocol-Oriented Programming launguage", I didn't get it. Isn't `protocal` in Swift the same as `interface` in other languages? Isn't "Protocol-Oriented Programming" the same as one of the [SOLID principles][4] - [Dependency inversion principle][5] - "Depend upon Abstractions. Do not depend upon concretions"? I knew it for years!

Then [Dave][3] presented some cool features of `protocal' in Swift 1 and 2: 

- [`Self` Requirement](#Self_Requirement)

and what "Protocol-Oriented Programming" really means: 

- Use `protocal` instead of `class` for abstration.

## `Self` Requirement



<!-- more -->

[1]: https://developer.apple.com/videos/wwdc/2015/
[2]: https://developer.apple.com/videos/wwdc/2015/?id=408
[3]: https://twitter.com/daveabrahams
[4]: https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)
[5]: https://en.wikipedia.org/wiki/Dependency_inversion_principle