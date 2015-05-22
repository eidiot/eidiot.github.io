title: BDD Demo with GameOfLife in Swift and Quick
date: 2015-05-22 10:40:31
tags: 
- BDD
- Swift
- iOS
---

BDD([Behavior Driven development](http://en.wikipedia.org/wiki/Behavior-driven_development)) was firstly developed by [Dan North](http://dannorth.net/blog/) as a response to the issues encountered teaching test-driven development:

- Where to start in the process
- What to test and what not to test
- How much to test in one go
- What to call the tests
- How to understand why a test fails

That was [version 1](http://dannorth.net/2012/05/31/bdd-is-like-tdd-if/comment-page-1/#comment-10313) of BDD in mid-2003. [BDD v3](http://dannorth.net/2012/05/31/bdd-is-like-tdd-if) now is more about communications between all different roles in agile teams using the same language: given-when-then(context-event-behavior). 

I made [some videos](/2014/09/06/TDD-Demo-GameOfLife-with-Android-Studio/) before to demonstrate TDD on Android platform using [Conway’s Game of Life](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) as an example. This time I will use this example again to demonstrate BDD on iOS platform in Swift and a BDD framework [Quick](https://github.com/Quick/Quick). 
([Code at GitHub](https://github.com/evan-liu/BDD-GameOfLife-Swift))

## 1. Create Project and Add Quick Framework

- Create a "Single View Application" in XCode. 
- [Install Quick](https://github.com/Quick/Quick/blob/master/Documentation/InstallingQuick.md#cocoapods) by [CocoaPods](https://cocoapods.org/) 
- Open GameOfLife.xcworkspace

<!-- more -->

## 2. GameOfLifeSpec

- Rename GameOfLifeTests to GameOfLifeSpec

```swift GameOfLifeSpec.swift
import Quick
import Nimble
class GameOfLifeSpec: QuickSpec {
    override func spec() {   
    }
}
```
## 3. Rule #1

```swift GameOfLifeSpec.swift
import Quick
import Nimble
import GameOfLife
class GameOfLifeSpec: QuickSpec {
    override func spec() {
        var model:GameOfLifeModel!
        beforeEach {
            model = GameOfLifeModel(rows: 3, columns: 3)
        }
        describe("Rule #1") {
            context("A live cell with fewer than 2 live neighbours") {
                it("will die") {
                    model.makeCellBeAliveAtRow(1, column: 1)
                    // 0 live neighbours
                    expect(model.willCellBeAliveAtRow(1, column: 1)).to(beFalse())
                    // 1 live neighbour
                    model.makeCellBeAliveAtRow(0, column: 0)
                    expect(model.willCellBeAliveAtRow(1, column: 1)).to(beFalse())
                }
            }
        }
    }
}
```

```swift GameOfLifeModel.swift
import Foundation
public class GameOfLifeModel {
    public var rows: Int
    public var columns: Int
    private var aliveMap = [String:Bool]()
    public init(rows:Int, columns:Int) {
        self.rows = rows
        self.columns = columns;
    }
    public func isCellAliveAtRow(row: Int, column: Int) -> Bool {
        return aliveMap[keyOfCellAtRow(row, column: column)] == true
    }
    public func makeCellBeAliveAtRow(row: Int, column: Int) {
        aliveMap[keyOfCellAtRow(row, column: column)] = true
    }
    public func willCellBeAliveAtRow(row: Int, column: Int) -> Bool {
        var aliveNeighbours = 0
        for i in row - 1...row + 1 {
            for j in column - 1...column + 1 {
                if !(i == row && j == column) && isCellAliveAtRow(i, column: j) {
                    aliveNeighbours++
                }
            }
        }
        if isCellAliveAtRow(row, column: column) {
            if aliveNeighbours < 2 {
                return false
            }
        }
        return false
    }
    private func keyOfCellAtRow(row: Int, column: Int) -> String {
        return "\(row)_\(column)";
    }
}
```

## 4. Rule #2

```swift GameOfLifeSpec.swift
describe("Rule #2") {
    context("A live cell with two or three live neighbours") {
        it("will live") {
            model.makeCellBeAliveAtRow(1, column: 1)
            // 2 live neighbours
            model.makeCellBeAliveAtRow(0, column: 0)
            model.makeCellBeAliveAtRow(0, column: 1)
            expect(model.willCellBeAliveAtRow(1, column: 1)).to(beTrue())
            // 3 live neighbours
            model.makeCellBeAliveAtRow(0, column: 2)
            expect(model.willCellBeAliveAtRow(1, column: 1)).to(beTrue())
        }
    }
}
```

```swift GameOfLifeModel.swift
if isCellAliveAtRow(row, column: column) {
    if aliveNeighbours < 2 {
        return false
    }
    if aliveNeighbours == 2 || aliveNeighbours == 3 {
        return true
    }
}
```

## 5. Rule #3

```swift GameOfLifeSpec.swift
describe("Rule #3") {
    context("A live cell with more than three live neighbours") {
        it("will die") {
            model.makeCellBeAliveAtRow(1, column: 1)
            // 4 live neighbours
            model.makeCellBeAliveAtRow(0, column: 0)
            model.makeCellBeAliveAtRow(0, column: 1)
            model.makeCellBeAliveAtRow(2, column: 0)
            model.makeCellBeAliveAtRow(2, column: 1)
            expect(model.willCellBeAliveAtRow(1, column: 1)).to(beFalse())
            // 5 live neighbours
            model.makeCellBeAliveAtRow(2, column: 2)
            expect(model.willCellBeAliveAtRow(1, column: 1)).to(beFalse())
        }
    }
}
```

```swift GameOfLifeModel.swift
if isCellAliveAtRow(row, column: column) {
    return aliveNeighbours == 2 || aliveNeighbours == 3
}
```

## 6. Rule #4

```swift GameOfLifeSpec.swift
describe("Rule #4") {
    context("A dead cell with exactly three live neighbours") {
        it("will live") {
            // 2 live neighbours (die)
            model.makeCellBeAliveAtRow(0, column: 0)
            model.makeCellBeAliveAtRow(0, column: 1)
            expect(model.willCellBeAliveAtRow(1, column: 1)).to(beFalse())
            // 3 live neighbours (live)
            model.makeCellBeAliveAtRow(0, column: 2)
            expect(model.willCellBeAliveAtRow(1, column: 1)).to(beTrue())
            // 4 live neighbours (die)
            model.makeCellBeAliveAtRow(1, column: 0)
            expect(model.willCellBeAliveAtRow(1, column: 1)).to(beFalse())
        }
    }
}
```

```swift GameOfLifeModel.swift
if isCellAliveAtRow(row, column: column) {
    return aliveNeighbours == 2 || aliveNeighbours == 3
}
return aliveNeighbours == 3
```

## 7. View & Controller

Add a simple [view](https://github.com/evan-liu/BDD-GameOfLife-Swift/blob/master/GameOfLife/GameOfLifeView.swift) and [controller](https://github.com/evan-liu/BDD-GameOfLife-Swift/blob/master/GameOfLife/GameOfLifeViewController.swift), we will have a [Gosper Glider Gun](http://en.wikipedia.org/wiki/Gun_(cellular_automaton).

![Gosper Glider Gun](http://upload.wikimedia.org/wikipedia/commons/e/e5/Gospers_glider_gun.gif)