# Stanford - Developing iOS 8 Apps with Swift 2015 学习笔记

虽然有点旧，但是还是可以跟着规范的课程学一发

## 1 Logistics, iOS 8 Overview

### What's in iOS

+ Cocoa Touch
	+ Multi-Touch, Alerts, Core Motion, Web View, View Hierarchy, Map Kit, Localization, Image Picker, Controls, Camera
+ Media
	+ Core Audio, JPEG/PNG/TIFF, OpenAL, PDF, Audio Mixing, Quartz(2D), Audio Recording, Core Animation, Video Playback, OpenGL ES
+ Core Services
	+ Collections, Core Location, Address Book, Net Services, Networking, Threading, File Access, Preferences, SQLite, URL Utilities
+ Core OS
	+ OSX kernel, Power Management, Mach 3.0, Keychain Access, BSD, Certificates, Sockets, File System, Security, Bonjour

### Platform Components

+ Tools
	+ Xcode 6 (Latest is 7)
+ Language(s)
	+ Swift, Modern Language
	+ `let value = formatter.numberFromString(display.text!)?.doubleValue`
+ Frameworks
	+ Foundation, Core Data, UIKit, Core Motion, Map Kit
+ Design Strategy
	+ MVC


### Calculator Tutorial

+ Organization Identifier 是你的身份验证，需要保证独立性
+ 尽可能把控件放到蓝线上，也就意味着某种对齐
+ weak 关键字，与 reference counting 相关，具体后面会说
+ 对象都在 heap 中存在
+ 能用 let 尽量用 let，增加可读性
+ option(alt) 点击变量可以查看帮助
+ optional 类型，可能是当前类型的值，或者是 `nil`，例如 `String?`
	+ `nil` 就是 not set
+ 在 optional 类型后面加 `!` 可以取出对应的值，但如果原来值是 `nil`，就会崩溃
+ 所有的变量都必须被初始化


## 2 More Xcode and Swift, MVC

+ 为什么 `UILabel` 之类的 outlet 可以直接声明 `!` 而不需要像之前所说的那样所有变量都必须被初始化呢？因为作为控件变量称为 implicit unwrap，走 storyboard 另外的机制
+ 右键点击一个控件会显示当前和该控件绑定的事件，可以在这里操作进行绑定和解绑定
+ 新建数组 `var operandStack = Array<Double>()`，可以被 infer 的类型最好就不要明写出来
+ 变量的 set get 中有神奇变量 `newValue`
+ command + / 可以快速注释
+ 利用 Swift 本身的特性可以写出非常精简的代码
+ 之后是 MVC 的介绍

## 3 Applying MVC

+ 新建一个 class：File -> New -> File -> Swift File
+ 虽然没有要求类名和文件名一致，但是最好还是一样，这样比较符合惯例
+ 理论上来说不应该在一个 Model 类中 `import UIkit`
+ 可以用快捷语法来生成数组 `[TypeName]()`
+ 可以用快捷语法来生成字典 `[KeyType: ValueType]()`
+ `*`, `+`, `sqrt` 这些内置函数可以直接当参数传
+ 在字典里返回的是 optional 类型
+ 传参时除非传的是 class，不然都是传入一个复制的值
+ 传值的时候参数隐含是 `let` 的，即不可变
+ 函数可以直接返回一个 tuple
+ option(alt) 加点击文件可以把文件放到另一边
+ if let xxx = xxx {} 是通常的处理 optional 类型的方式
+ switch 如果列举了所有情况就不需要 `default: break`
+ `private enum Op: CustomStringConvertible` 由 `Printable` 更名而来

## 4 More Swift and Foundation Frameworks

不是所有的都记录，只挑了一些我觉得和之前学的语言差异比较大的，很多内容在我之前的学习笔记里都有覆盖

### Optional

An Optional is just an `enum`

```swift
enum Optional<T>{
	case None
	case Some(T)
}
```

```swift
let x: String? = nil
// is
let x = Optional<String>.None

let x: String? = "hello"
// is
let x = Optional<String>.Some("hello")

var y = x!
// is
switch x {
	case Some(let value): y = value
	case None: // raise an exception
}
```

### Other Classes

+ `NSObject`
	+ Base class for all objective-C classes. Some advanced features will require you to subclass from `NSObject`
+ `NSNumber`
	+ Generic number-holding class
+ `NSDate`
	+ Used to find out the date and time right now or to store past or future dates
+ `NSData`
	+ A "bag of bits"

### Data Structures

Three Types: Classes, Structures, Enumerations

+ Similarities
	+ Declasration syntax
	+ Properties and Functions
	+ Initializers
+ Differences
	+ Inheritance (class only)
	+ Introspection and casting (class only)
	+ Value type (struct, enum) vs. Reference type (class) 


### Value vs. Reference

+ Value (`struct` and `enum`)
	+ **Copied** when passed as an argument to a function
	+ **Copied** when assigned to a different variable
	+ **Immutable** if assigned to a variable with `let`
	+ Remember that function parameters are, by default, constants
	+ You can put the keyword `var` on an parameter, and it will be mutable, but it's still a copy
	+ You must note any `func` that can mutate a struct/enum with the keyword `mutating`
+ Reference (`class`)
	+ Stored in the heap and reference counted (automatically)
	+ Constant pointers to a class (`let`) still can mutate by calling methods and changing properites
	+ When passed as an argument, does not make a copy (just passing a pointer to same instance)
+ Choosing which to use?
	+ Usually you will choose `class` over `struct`. `struct` tends to be more for fundamental types. Use of `enum` is situational (any time you have a type of data with discret values)

### AnyObject

+ Special "Type" (actually it's a Protocol)
	+ Used primarily for compatibility with exsiting Objective-C-based APIs
+ Where will you see it?
	+ As properties (eigher singularly or as an array of them), e.g. ...
	+ `var destinationViewController: AnyObject`
	+ `var toolbarItems: [AnyObject]`
+ How do we use `AnyObject`
	+ We don't usually use it directly
	+ Instead, we convert it to another, known type 
+ How do we convert it?
	+ We need to create a new vairable which is of a known object type
+ Casting AnyObject
	+ We "force" an `AnyObject` to be something else by "casting" it using the `as` keyword 

### 总结

语言的细节还有很多，需要在实践中不断熟悉，这里不需要过度在意细节，动手最重要

## 5 Objective C Compatibility, Property List, Views

+ 主要介绍了 Swift 与 Objc 的桥接，这里官方的指南都有介绍，具体不赘述
+ Property List is really just the definition of a term
	+ pass around data "blindly"
	+ "generic data structure", be passed to API
	+ example: `NSUserDefaults`, small database
	+ 持久化的方式
+ Views
	+ `UIView` subclass
	+ Defines a coordinate space, for drawing, and for handling touch events
	+ Hierarchical (only one superview, but many/zero subviews)
	+ The hierarchy is most often constructed in Xcode graphically
		+ But it can be done in code as well
	+ Where does the view hierarchy start?
		+ THe top of the (usable) view hierarchy is the Controller's `var view: UIView`
		+ This view is the one whose bounds will change on rotation
		+ This view is likely the ones you will programmatically add `subview` to
		+ All of your MVC's View's `UIView` will have this view as an ancestro
		+ It's wutomatically hooked up for you when you create an MVC in Xcode
	+ 在代码中初始化 `UIView` 需要实现两个必要 `init` 函数，因为有两种方式可以创建
	+ 或者也可以用 `awakeFromNib` 针对 storyboard
+ View Coordinate System
	+ Origin is upper left
	+ Units are points, not pixels
		+ Pixels are the minimum-sized unit of drawing your device is capable of
		+ Points are the units in the coordinate system
		+ Most of the time there are 2 pixels per point, but it could be only 1 or something else
		+ How many pixels per point are there? `UIView`'s `var contentScaleFactor: CGFloat`
	+ The boundaries of where drawing happends
		+ `var bounds: CGRect` // a view's internal drawing space's origin and size
		+ This is the rectangle containing the drawing space **in its own coordinate system**
		+ It is up to your views' implementation to interpret what `bounds.origin` means (often nothing)   
	+ Where is the `UIView`? 注意都是在 superview 里的，不是自己的
		+ `var center: CGPoint` // the center of a `UIView` **in its superview's coordinate system**
		+ `var frame: CGRect` // the rect containing a `UIView` **in its superview's coordinate system**
+ bounds vs frame
	+ Use `frame` and/or `center` to **position** a `UIView`
	+ `frame.size` 不总是等于 `bounds.size` -> 因为旋转的时候 frame 要大得多！
+ Creating Views
	+ Most often your views are created via your storyboard
	+ To draw, just create a `UIView` subclass and override `drawRect`
	+ NEVER call drawRect!! EVER! Or else!
+ 绘制的方法也有很多，主要是 Bezier 已经一些 UIKit 的设定
+ `UIColor`: RGB, HSB or even a pattern (using UIImage)
	+ have alpha (transparency) 0(fully transparent) 1.0(fully opaque)
	+ 如果需要透明需要把 `UIView` 的 `var opaque = false`
+ Drawing Text
	+ Usually we use a `UILabel` to put text on screen
	+ To draw in `drawRect`, use `NSAtrributedString`
	+ Mutability is done with `NSMutableAttributedString`
	+ `NSattributedString` is not a String, nor an NSString
+ Fonts
	+ The absolutely best way to get a font in code
		+ `class func preferredFontForTextStyle(UIFontTextStyle) -> UIFont
		+ `UIFontTextStyle.Headline`
		+ `UIFontTextStyle.Body`
		+ `UIFontTextStyle.Footnote`
	+ There are also "system fonts"
		+ Don't use for user's **content**. Use preferred fonts for that 
+ 在 storyboard 中按着 `ctrl + shift` 再点击控件会出现一个界面让你选择你想要选择的控件，很有用
+ 如果需要在更新某个值的时候同时更新绘制，利用 `didSet` 方式声明变量
+ 如果需要一个 view 在旋转之后更新，在其 attribute inspector 中把 Mode 改为 Redraw

## 6 Protocols and Delegation, Gestures

+ 在一个 UIView 的代码前加上 `@IBDesignable` 可以直接在 storyboard 中绘制图案
+ 在一个 UIView 中的变量前加上 `@IBInspectable` 就可以直接在 storyboard 中更改
+ Extension 只能添加，不能修改原来的方法或者属性
	+ 是一个很容易滥用的机制！注意场景
+ Protocol - A way to express an API minimally
	+ is a TYPE just like any other type, except 
		+ It has no storage or implementation associated with it
		+ 没有对应的实现，只是一个羊头，没有狗肉
+ Delegation
	+ A very important use of `protocols`
	+ How it plays out
		+ Create a delegation `protocol` (defines what the View wants the Controller to take care of)
		+ Create `delegate` property in the View whose type is that delegation `protocol`
		+ Use the `delegate` property in the View to get/do things it can't own or control
		+ Controller declares that it implements the `protocol`
		+ Controller sets `self` as the delegate of the View by setting the property in #2 above
		+ Implement the `protocol` in the Controller
	+ Now the View is hooked up to the Controlle
	+ weak 用来避免循环引用
	+ `let smiliness = dataSource?.smilinessForFaceView(self) ?? -0.0`
		+ `??` 符号表示，如果左边的表达式是 nil，那么用右边的值
	+ comman + shift + o 可以快速检索并打开文件
+ Gestures
	+ we can get notified of the raw touch events
	+ Gestrues are recognized by instances of `UIGestureRecognizer`
	+ Two sides to using a gesture recognizer
		1. Adding a gesture recognizer to a `UIView`
		2. Providing a method to "handle" that gesture
	+ Usually the first is done by a Controller
	+ The second is provided either by the `UIView` or a controller
+ UISplitViewController (Two MVCs side by side)
	+ 左边 Master，右边 Detail
