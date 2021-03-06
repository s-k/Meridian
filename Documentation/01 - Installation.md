# 01 - Installation

Meridian uses Swift Package Manager for installation. 

Add Meridian as a dependency for your package:

    .package(url: "https://github.com/khanlou/Meridian.git", from: "0.0.6"),

The version should be the latest tag on GitHub.

Add Meridian as a dependency for your target as well:

    .product(name: "Meridian", package: "Meridian"),

If you haven't used Swift Package Manager much, here's a complete Package.swift:

    let package = Package(
        name: "MyFirstApp",
        products: [
            .executable(name: "App", targets: ["App"]),
            .library(name: "MyFirstApp", targets: ["MyFirstApp"]),
        ],
        dependencies: [
            .package(url: "https://github.com/khanlou/Meridian.git", from: "0.1.2"),
            .package(url: "https://github.com/swift-server/swift-backtrace.git", from: "1.2.0"),
        ],
        targets: [
            .target(name: "App", dependencies: ["MyFirstApp"], path: "App"),
            .target(name: "MyFirstApp", dependencies: [
                .product(name: "Meridian", package: "Meridian"),
                .product(name: "Backtrace", package: "swift-backtrace"),
            ], path: "MyFirstApp"),
        .testTarget(name: "MyFirstAppTests", dependencies: ["MyFirstApp"], path: "MyFirstAppTests"),
        ]
    )

A few notes:

1. Executables can't be tested, so separating your app code into a library will enable you to test any code inside it. Anything in your library will need to be marked as public to be used from the App target.
2. Backtrace is a package that will print the stack trace whenever a crash occurs. This is invaluable for debugging and should be included in every server application.