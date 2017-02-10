# 学习笔记
####以后会经常总结学习笔记了,学习笔记暂时放到这里吧.
> 写给自己:最近在学习MVVM与ReactiveCocoa配合下的编程,如果在整个app开发过程中都是用这种模式的话会大大提高开发效率,提高代码的复用性,降低耦合度吧,但是前提是得花一大部分时间来学习ReactiveCocoa这个库,否则写出的代码可能难以调试,可读性也不好,不利于团队协作开发.所以我建议如果团队开发大规模使用这种模式的话,必须制定严格的代码规范和模板式的开发习惯!








# Overview

There is an established pattern where UICollectionViewLayout talks directly to the collection view's delegate to get additional layout info e.g. the size of a header in a given section.

This pattern is established by Apple's flow layout, and it is used by Pinterest and I'm sure others. It is dangerous when used with ASDK because we update asynchronously, so
for instance if you delete a section from your data source, the layout won't find out until later and in the meantime it may ask the delegate about a section that doesn't exist!

The solution is to capture this kind of information from the data source immediately when a section is inserted, and make it available to the layout as we update the UICollectionView so that everyone is on the same page.

Enter: ASSectionUserInfo

Internally, we use a private object ASCollectionSection to represent one version of a section of items and supplementaries. If the user wants, they can provide us with an ASSectionUserInfo object to accompany the section, which will be read synchronously when the section is inserted.

## Usage During Layout

The collection view will make these section infos available in the same way that finished nodes are currently available.

#### [Sequence Diagram:][diag-layout-usage]

![][image-layout-usage]

## Creation When Inserting Sections

The section infos for any inserted/reloaded sections are queried synchronously, before the node blocks for their items. The top part of this diagram is the same as the current behavior but I think it's useful info =)

#### [Sequence Diagram:][diag-inserting-sections]

![][image-inserting-sections]

<!-- Reference -->
[diag-inserting-sections]: https://www.websequencediagrams.com/?lz=dGl0bGUgSW5zZXJ0aW5nL1JlbG9hZGluZyBTZWN0aW9ucwoKRGF0YSBTb3VyY2UtPkNWOiBpACoFABkIQXRJbmRleGVzOgpDVi0-Q2hhbmdlU2V0IAAzBUNvbnRyb2xsZXIAHRsAURFlbmRVcGRhdGVzADQgAB4MAGIYLT4AehFiZWdpbgAFNACBYxlsb29wIGZvciBlYWNoIHMAgjgGIGluZGV4CiAgIACBAhJDVjoAHwhJbmZvAIJABzoAKAVDVgCBLQcAgnEGAA8aAIMQDACDFwZSZXR1cm4gaWQ8QVMAgz0HVXNlckluZm8-AFQIAIF-EwAWIQCBUg5pdGVtIGluAIFgCACBXQUAgU0Zbm9kZUJsb2NrAIQkB1BhdGgAgWIGAIFXFQARHgCBWRlub2RlIGJsb2NrAE8MAIFRGgAhD2VuZAplbmQAhBktAIUcCwCEYxAAhW0cAIUPGwCGWwYKAIMaCgCDeQgKCg&s=napkin
[image-inserting-sections]: https://www.websequencediagrams.com/cgi-bin/cdraw?lz=dGl0bGUgSW5zZXJ0aW5nL1JlbG9hZGluZyBTZWN0aW9ucwoKRGF0YSBTb3VyY2UtPkNWOiBpACoFABkIQXRJbmRleGVzOgpDVi0-Q2hhbmdlU2V0IAAzBUNvbnRyb2xsZXIAHRsAURFlbmRVcGRhdGVzADQgAB4MAGIYLT4AehFiZWdpbgAFNACBYxlsb29wIGZvciBlYWNoIHMAgjgGIGluZGV4CiAgIACBAhJDVjoAHwhJbmZvAIJABzoAKAVDVgCBLQcAgnEGAA8aAIMQDACDFwZSZXR1cm4gaWQ8QVMAgz0HVXNlckluZm8-AFQIAIF-EwAWIQCBUg5pdGVtIGluAIFgCACBXQUAgU0Zbm9kZUJsb2NrAIQkB1BhdGgAgWIGAIFXFQARHgCBWRlub2RlIGJsb2NrAE8MAIFRGgAhD2VuZAplbmQAhBktAIUcCwCEYxAAhW0cAIUPGwCGWwYKAIMaCgCDeQgKCg&s=napkin
[image-layout-usage]: https://www.websequencediagrams.com/cgi-bin/cdraw?lz=dGl0bGUgcHJlcGFyZUxheW91dCAtIHNlY3Rpb24gbWV0cmljcwoKQ1YtPgAYBjoAHw4KbG9vcCBmb3IgZWFjaAAxCAogICAgCiAgICAATQYtPkNWOgBOCEluZm9BdEluZGV4OgAkBUNWLQBUClJldHVybiBQSU1hc29ucnlTACsKCgBFDQAOFDogY29sdW1uQ291bnQAgQAFADQUAFwSACYQAEYed2lkdGhPZkMAZgUAgT0NAEgmADsFAIIRDQCCUwhVc2UAgmsJZW5kCgoAgjgHAII6BihPSykK&s=napkin
[diag-layout-usage]: https://www.websequencediagrams.com/?lz=dGl0bGUgcHJlcGFyZUxheW91dCAtIHNlY3Rpb24gbWV0cmljcwoKQ1YtPgAYBjoAHw4KbG9vcCBmb3IgZWFjaAAxCAogICAgCiAgICAATQYtPkNWOgBOCEluZm9BdEluZGV4OgAkBUNWLQBUClJldHVybiBQSU1hc29ucnlTACsKCgBFDQAOFDogY29sdW1uQ291bnQAgQAFADQUAFwSACYQAEYed2lkdGhPZkMAZgUAgT0NAEgmADsFAIIRDQCCUwhVc2UAgmsJZW5kCgoAgjgHAII6BihPSykK&s=napkin









<p align="center">
  <img src="https://raw.githubusercontent.com/Instagram/IGListKit/master/Resources/logo-animation.gif" width=400 />
</p>

<p align="center">
    <a href="https://travis-ci.org/Instagram/IGListKit">
        <img src="https://travis-ci.org/Instagram/IGListKit.svg?branch=master&style=flat"
             alt="Build Status">
    </a>
    <a href="https://coveralls.io/github/Instagram/IGListKit?branch=master">
      <img src="https://coveralls.io/repos/github/Instagram/IGListKit/badge.svg?branch=master"
           alt="Coverage Status" />
    </a>
    <a href="https://cocoapods.org/pods/IGListKit">
        <img src="https://img.shields.io/cocoapods/v/IGListKit.svg?style=flat"
             alt="Pods Version">
    </a>
    <a href="https://instagram.github.io/IGListKit/">
        <img src="https://img.shields.io/cocoapods/p/IGListKit.svg?style=flat"
             alt="Platforms">
    </a>
    <a href="https://github.com/Carthage/Carthage">
        <img src="https://img.shields.io/badge/Carthage-compatible-brightgreen.svg?style=flat"
             alt="Carthage Compatible">
    </a>
</p>

----------------

A data-driven `UICollectionView` framework for building fast and flexible lists.

|         | Main Features  |
----------|-----------------
&#128581; | Never call `performBatchUpdates(_:, completion:)` or `reloadData()` again
&#127968; | Better architecture with reusable cells and components
&#128288; | Create collections with multiple data types
&#128273; | Decoupled diffing algorithm
&#9989;   | Fully unit tested
&#128269; | Customize your diffing behavior for your models
&#128241; | Simply `UICollectionView` at its core
&#128640; | Extendable API
&#128038; | Written in Objective-C with full Swift interop support

`IGListKit` is built and maintained with &#10084;&#65039; by [Instagram engineering](https://engineering.instagram.com/).
We use the open source version `master` branch in the Instagram app.

## Requirements

- Xcode 8.0+
- iOS 8.0+
- tvOS 9.0+
- macOS 10.10+ *(diffing algorithm components only)*
- Interoperability with Swift 3.0+

## Installation

### CocoaPods

The preferred installation method is with [CocoaPods](https://cocoapods.org). Add the following to your `Podfile`:

```ruby
pod 'IGListKit', '~> 2.0.0'
```

### Carthage

For [Carthage](https://github.com/Carthage/Carthage), add the following to your `Cartfile`:

```ogdl
github "Instagram/IGListKit" ~> 2.0.0
```

> For advanced usage, see our [Installation Guide](https://instagram.github.io/IGListKit/installation.html).

## Getting Started

- Our [Getting Started guide](https://instagram.github.io/IGListKit/getting-started.html)
- Ray Wenderlich's [IGListKit Tutorial: Better UICollectionViews](https://www.raywenderlich.com/147162/iglistkit-tutorial-better-uicollectionviews)
- Ryan Nystrom's [talk at try! Swift NYC](https://realm.io/news/tryswift-ryan-nystrom-refactoring-at-scale-lessons-learned-rewriting-instagram-feed/)

## Documentation

You can find [the docs here](https://instagram.github.io/IGListKit). Documentation is generated with [jazzy](https://github.com/realm/jazzy) and hosted on [GitHub-Pages](https://pages.github.com).

## Contributing

Please see the [CONTRIBUTING](https://github.com/Instagram/IGListKit/blob/master/.github/CONTRIBUTING.md) file for how to help out. At Instagram we sync the open source version of `IGListKit` daily, so we're always testing the latest changes. But that requires all changes be thoroughly tested and follow our style guide.

## License

`IGListKit` is BSD-licensed. We also provide an additional patent grant.

The files in the `/Examples/` directory are licensed under a separate license as specified in each file. Documentation is licensed [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/).

