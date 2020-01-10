# ViewInspector for SwiftUI

![Platform](https://img.shields.io/badge/platform-ios%20%7C%20tvos%20%7C%20watchos%20%7C%20macos-lightgrey) [![Build Status](https://travis-ci.com/nalexn/ViewInspector.svg?branch=master)](https://travis-ci.com/nalexn/ViewInspector) [![codecov](https://codecov.io/gh/nalexn/ViewInspector/branch/master/graph/badge.svg)](https://codecov.io/gh/nalexn/ViewInspector)

**ViewInspector** is a library for unit testing SwiftUI views.
It allows for traversing a view hierarchy at runtime providing direct access to the underlying `View` structs.

## Why?

SwiftUI views are a function of state. We can provide the input, but couldn't verify the output. Until now!

## Features

#### 1. Verify the view's inner state

You can dig into the hierarchy and read actual state values on any SwiftUI View:

```swift
func testVStackOfTexts() throws {
    let view = VStack {
        Text("1")
        Text("2")
        Text("3")
    }
    let sut = try view.inspect()
    let array = try sut.map { try $0.text().string() }
    XCTAssertEqual(array, ["1", "2", "3"])
}
```

#### 2. Trigger side effects

You can simulate user interaction by programmatically triggering system-controls callbacks:

```swift
let button = try view.inspect().hStack().button(3)
try button.tap()

let view = try view.inspect().list().view(ItemView.self, 15)
try view.callOnAppear()
```

#### 3. Extract custom views from the hierarchy of any depth

It is possible to obtain a copy of your custom view with actual state and references from the hierarchy of any depth:

```swift
let sut = try view.inspect().tabView().navigationView()
    .overlay().anyView().view(CustomView.self).actualView()
XCTAssertTrue(sut.isUserLoggedIn)
```

## FAQs

### Which views and modifiers are supported?

Pretty much all! Check out the [detailed list](readiness.md).

The framework is still expanding, as there are hundreds of inspectable attributes in SwiftUI that are not included yet.

Contributions are welcomed! To get some inspiration, read the [story](https://nalexn.github.io/swiftui-unit-testing/?utm_source=nalexn_github) behind creating this framework.

### Is it using private APIs?

**ViewInspector** is using official Swift reflection API to dissect the view structures.

So this framework is production-friendly for the case if you accidentally (or intentionally) linked it with the build target.

### How do I add it to my Xcode project?

1. In Xcode select **File ⭢ Swift Packages ⭢ Add Package Dependency...**
2. Copy-paste repository URL: **https://github.com/nalexn/ViewInspector**
3. Hit **Next** two times, under **Add to Target** select your test target. There is no need to add it to the build target.
4. Hit **Finish**

### How do I use it in my project?

Please refer to the [Inspection guide](guide.md). You can also check out my other [project](https://github.com/nalexn/clean-architecture-swiftui) that harnesses the **ViewInspector** for testing the entire UI.

### Other questions, concerns or suggestions?

Feel free to contact me on [Twitter](https://twitter.com/nallexn) or just submit an issue or a pull request on Github.

---

[![blog](https://img.shields.io/badge/blog-github-blue)](https://nalexn.github.io/?utm_source=nalexn_github) [![venmo](https://img.shields.io/badge/%F0%9F%8D%BA-Venmo-brightgreen)](https://venmo.com/nallexn)