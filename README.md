# swift-known-issues
Swift known issues

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
then it may `Undefined symbols for architecture x86_64` error occur for some types of running code/tests though everything looks normal (in past I reproduced the problem by adding some code to Apple Swift demo projects).  

It seems to happen because of Swift recognizes as there are two main entry points appear on C-style compilation: one is exec main, the second is an entry point for run tests.

So, if package layout changes like below then such error will be resolved:
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
