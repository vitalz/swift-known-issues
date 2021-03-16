# swift-known-issues

When there is one executable target `HelloWorld` with `main.swift` in Package layout and a test target for it
```swift
targets: [
    .target(
        name: "HelloWorld"
    ),
    .testTarget(
        name: "HelloWorldTests",
        dependencies: ["HelloWorld"]
    )
]
```
then `Undefined symbols for architecture x86_64` error may occur for some types of running code/tests though everything looks normal (in past I reproduced the problem by adding some code to Apple Swift demo projects).  

It seems to happen because of Swift recognizes as there are two main entry points appear during C-style compilation: one is exec main, the second is an entry point to run tests - and there cannot be two main methods compiled into one binary file.

So, when Package layout changes like below then such error will be resolved:
```swift
targets: [
    .target(
        name: "HelloWorld"
        dependencies: ["MyFramework"]
    ),
    .target(
        name: "MyFramework"
    ),
    .testTarget(
        name: "MyFrameworkTests",
        dependencies: ["MyFramework"]
    )
]

```
