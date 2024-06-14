# PlatformColor and Color View Extensions

To use these extensions

1. Download the Zipped version of this repository and expand the Zipped folder.
2. Drag the two extensions **Color+Extension.swift** and **PlatformColor+Extension.swift** into your project navigator, making sure you select the correct target(s) and copy the files.

## PlatformColor+Extension

This is a cross platform extension for either NSColor or UIColor.  This is done via a conditional compilation block to specify the target operating system.

It imports either AppKit or UIKit and creates an appropriate typeAlias called PlatformColor to represent either NSColor or UIColor

This conditional block is created outside of the extension and is thus available anywhere in your code.

```swift
#if os(macOS)
import AppKit
typealias PlatformColor = NSColor
#else
import UIKit
typealias PlatformColor = UIColor
#endif
```

#### convenience initializer

The extension creates a convenience initializer that will allow you to enter a Hex string for any color. 

```swift
let myColor = Color(hex: "FF0000")  // Red
```

#### toHexString()

The toHexString() function will generate a string representing the hex value of the color provided.

This produces an optional string so you can either provide an optional string value or force unwrap if you are sure (not recommended)



```swift
let teal:PlatformColor = .systemTeal
// Above is equitvalient to let teal: UICOlor = .systemTeal or let teal: NSColor = .systemTeal
print(teal.toHexString() ?? "Unkwnown") // #30B0C7
print(teal.toHexString()!)  // #30B0C7
```

#### luminance

This computed property will generate the colour's luminance. It *describes the amount of light that passes through, is emitted from, or is reflected from* a particular area, and falls within a given solid angle.  The value is a **CGFloat** between 0 and 1 and we use this in the **Color+Extension.swift**

```swift
let yellowiOS: UIColor = .yellow  // if on iOS
let yellowMacOS: NSColor = .yellow // if on MacOS
let yellow: PlatformColor = .yellow // works for both

print(yellowiOS.luminance)
print(yellowMacOS.luminance)
print(yellow.luminance)
// all print 0.8859999999999999
```

#### adaptedTextColor

This computed property produces either an NSColor or UIColor based on the luminance of a given colour.  If the colour's luminance is less than 0.5, it returns white, otherwise black.  This is used in the Color+Extension to create an appropriate label colour for text that has a background colour.

```swift
let yellow: PlatformColor = .systemYellow
let adaptedColor = yellow.adaptedTextColor
```

## Color+Extension

#### Initializer

Like the extension for the NSColor/UIColor (PlatformColor), the Color view extension also has an initializer that will create a Color View when provided with a hex string

```swift
Color(hex: "0000FF") // Creates a Color view with the color blue
```



#### toHexString(includeAlpha: Bool = **false**)

This extension function for Color will return a string representing the hex value for the NSColor or UIColor that is used to create the Color View.

Like the PlaformColor (NSColor, UIColor), this produces an optional string so you can either provide an optional string value or force unwrap if you are sure (not recommended)

You also have the ability to include an alpha argument of true to get the alpha value

```swift
let redColorView = Color.red
print(redColorView.toHexString() ?? "Unknown") // #FF3B30
print(redColorView.toHexString()!)  // #FF3B30

```

#### luminance

This computed property will generate the colour's luminance value based on the Color View.

It uses the luminance computed property for the UIColor/NSColor used to create the view

```swift
let yellowView: Color = .yellow
print(yellowView.luminance)
```

#### adaptedTextColor

This computed property produces an appropriate adaptive Color view (either white or black) depending on the luminance of the provided Color View

```swift
let backgroundColor: Color = .red

Text("Hello World")
    .foregroundStyle(backgroundColor.adaptedTextColor)
    .background(backgroundColor)
```
