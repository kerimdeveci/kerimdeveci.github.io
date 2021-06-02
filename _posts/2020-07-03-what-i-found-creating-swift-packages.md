---
title: "What I found creating swift packages with scenarios"
last_modified_at: 2021-06-03T10:12:41+03:00
categories:
  - Blog
tags:
  - Swift
  - Swift Packages
---

## Creating Local Swift Package Manager(SPM) in existing project

packages are platform independent
it is very easy to share code with different platforms
great for reusable code

1. create File -> New -> Swift Package
2. Add existing Project and its root groups. It is important that local packages must be under current project folder structure. so dont put it on other than project root.
3. put your swift files sources and resources
4. link the package with your app within frameworks libraries and embeded content section onf targets general tab. add new library from here.

## Publishing packages

use Semantic Versioning semver.org
1.2.4  
Major Version (Major behavioral changes) - minor version (backwards compatible) compatible addition add new mtethod or type - patch version bug fixes. backwards compatible

Major version zero is special use it for initial development
release with 1 major
0.x.y  
Initial 


2.0.0-alpha.1


1. drag and drop holding option the swift package to any location, it will copy 
2. double click Package.swift
3. fill readme 
4. create git reposutory in xcode add remote create remote or add existing remote
5. commit and tag master 
6. push including tags

## Package manifest

first line of manifest is swift tools version it is not commented code dont change it
 
```swift
// swift-tools-version:5.3
import PackageDescription

let package = Package(
    name: "MyLibrary",
    platforms: [
        .macOS(.v10_14), .iOS(.v13), .tvOS(.v13)
    ],
    products: [
        // Products define the executables and libraries a package produces, and make them visible to other packages.
        .library(
            name: "MyLibrary",
            targets: ["MyLibrary", "SomeRemoteBinaryPackage", "SomeLocalBinaryPackage"])
    ],
    dependencies: [
        // Dependencies declare other packages that this package depends on.
    ],
    targets: [
        // Targets are the basic building blocks of a package. A target can define a module or a test suite.
        // Targets can depend on other targets in this package, and on products in packages this package depends on.
        .target(
            name: "MyLibrary",
            exclude: ["instructions.md"],
            resources: [
                .process("text.txt"),
                .process("example.png"),
                .copy("settings.plist")
            ]
        ),
        .binaryTarget(
            name: "SomeRemoteBinaryPackage",
            url: "https://url/to/some/remote/binary/package.zip",
            checksum: "The checksum of the XCFramework inside the ZIP archive."
        ),
        .binaryTarget(
            name: "SomeLocalBinaryPackage",
            path: "path/to/some.xcframework"
        )
        .testTarget(
            name: "MyLibraryTests",
            dependencies: ["MyLibrary"]),
    ]
)
```