# Swift 5.4 新特性速查

> [Swift 5.4 Released!](https://swift.org/blog/swift-5-4-released/)

## 语言特性

- Support for multiple variadic parameters in functions, subscripts and initializers [(SE-0284)](https://github.com/apple/swift-evolution/blob/main/proposals/0284-multiple-variadic-parameters.md)
- Extend implicit member syntax [(SE-0287)](https://github.com/apple/swift-evolution/blob/main/proposals/0287-implicit-member-chains.md)
- Result builders [(SE-0289)](https://github.com/apple/swift-evolution/blob/main/proposals/0289-result-builders.md)
  - 更多参考： [awesome-function-builders](https://github.com/carson-katri/awesome-function-builders)
- Local functions supporting overloading
- Property wrappers for local variables

### Multiple variadic parameters in functions

```swift
func summarizeGoals(times: Int..., players: String...) {
    let joinedNames = ListFormatter.localizedString(byJoining: players)
    let joinedTimes = ListFormatter.localizedString(byJoining: times.map(String.init))
    
    print("\(times.count) goals where scored by \(joinedNames) at the follow minutes: \(joinedTimes)")
}
/*:
To call that function, provide both sets of values as variadic parameters, making sure that all parameters after the first variadic are labeled:
*/
summarizeGoals(times: 18, 33, 55, 90, players: "Dani", "Jamie", "Roy")
```

### Improved implicit member syntax

```swift
struct ContentView1: View {
    var body: some View {
        Text("Hello, World!")
            .foregroundColor(.red)
    }
}
/*:
Prior to Swift 5.4 this did not work with more complex expressions. For example, if you wanted your red color to be slightly transparent you would need to write this:
*/
struct ContentView2: View {
    var body: some View {
        Text("Hello, World!")
            .foregroundColor(Color.red.opacity(0.5))
    }
}
/*:
From Swift 5.4 onwards the compiler is able to understand multiple chained members, meaning that the `Color` type can be inferred:
*/
struct ContentView3: View {
    var body: some View {
        Text("Hello, World!")
            .foregroundColor(.red.opacity(0.5))
    }
}
```

### Result builders

```swift
@resultBuilder
struct SimpleStringBuilder {
    static func buildBlock(_ parts: String...) -> String {
        parts.joined(separator: "\n")
    }
}

@SimpleStringBuilder func makeSentence3() -> String {
    "Why settle for a Duke"
    "when you can have"
    "a Prince?"
}
    
print(makeSentence3())
```

- If you’d like to see more advanced, real-world examples of result builders in action, you should check out the [Awesome Function Builders repository on GitHub](https://github.com/carson-katri/awesome-function-builders).

### Local functions supporting overloading

```swift
struct Butter { }
struct Flour { }
struct Sugar { }
    
func makeCookies() {
    func add(item: Butter) {
        print("Adding butter…")
    }
    
    func add(item: Flour) {
        print("Adding flour…")
    }
    
    func add(item: Sugar) {
        print("Adding sugar…")
    }
    
    add(item: Butter())
    add(item: Flour())
    add(item: Sugar())
    print("Come and get some cookies!")
}
    
makeCookies()
```

### Property wrappers for local variables

```swift
@propertyWrapper struct NonNegative<T: Numeric & Comparable> {
    var value: T
    
    var wrappedValue: T {
        get { value }
    
        set {
            if newValue < 0 {
                value = 0
            } else {
                value = newValue
            }
        }
    }
    
    init(wrappedValue: T) {
        if wrappedValue < 0 {
            self.value = 0
        } else {
            self.value = wrappedValue
        }
    }
}
/*:
And from Swift 5.4 onwards we can use that property wrapper inside a regular function, rather than just attaching to a property. For example, we might write a game where our player can gain or lose points, but their score should never go below 0:
*/
func playGame() {
    @NonNegative var score = 0
    
    // player was correct
    score += 10
    
    // player was correct again
    score += 10
    
    // player got one wrong
    score -= 50
    
    print(score)
}
    
playGame()
```

## 运行性能与代码大小优化

Further, consecutive array modifications now avoid redundant uniqueness checks.

```
func foo(_ a: inout [Int]) {
  // Must do copy-on-write (CoW) check here.
  a[0] = 1
  // The compiler no longer generates
  // a redundant CoW check here.
  a[1] = 2
}
```

- `String` interpolations are more aggressively constant-folded
- Fewer retain/release calls, especially for `inout` function arguments and within loops
- Generic metadata in the Standard Library is now [specialized at compile time](https://forums.swift.org/t/generic-type-metadata-prespecialization/31659), reducing dirty memory usage and improving performance

## 开发体验优化

- Build Performance
- Code Completion
- Type Checker 🎉 🎉 🎉
  - “too complex to solve in reasonable time” 👋👋👋

For example, consider:

```swift
struct S { var s: String? }

func test(_ a: [S]) {
   _ = a.reduce("") {
     ($0 + "," + ($1.s ?? "")) + ($1.s ?? "") + ($1.s ?? "")
   }
}
```

For this code, the type checker completes in **under 100 ms**, where previously it would **time out.**

- Debugging

