# Stevia 🍃

[![Language: Swift 2](https://img.shields.io/badge/language-swift2-f48041.svg?style=flat)](https://developer.apple.com/swift)
![Platform: iOS 8+](https://img.shields.io/badge/platform-iOS%208%2B-blue.svg?style=flat)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Build Status](https://www.bitrise.io/app/4478e29045c5f12e.svg?token=pti6g-HVKBUPv9mIR3baIw&branch=master)](https://www.bitrise.io/app/4478e29045c5f12e)
[![Join the chat at https://gitter.im/s4cha/Stevia](https://badges.gitter.im/s4cha/Stevia.svg)](https://gitter.im/s4cha/Stevia?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![License: MIT](http://img.shields.io/badge/license-MIT-lightgrey.svg?style=flat)](https://github.com/s4cha/Stevia/blob/master/LICENSE)


Elegant view layout for iOS

```swift
layout([
    100,
    |-email-| ~ 80,
    8,
    |-password-| ~ 80,
    "",
    |login| ~ 80,
    0
])
```

### Why
Because **nothing holds more truth than pure code** 🤓  
Xibs and storyboards are **heavy, hard to maintain, hard to merge.**  
They split the view concept into 2 separate files making making debugging a **nightmare**    
*There must be a better way*

## How
By creating a tool that makes Auto layout code finally **readable by a human being**.  
By coupling it with live code injection such as *injectionForXcode* we can **design views in real time**  
View layout becomes **fun**, **concise**, **maintainable** and dare I say, *beautiful* ❤️

## What
- [x] Auto Layout DSL
- [x] Pure Swift
- [x] Simple, this is just NSLayoutConstraint shortcuts, pure UIKit code, no voodoo magic
- [x] Chainable api

## Advantages of Stevia 🍃over Xibs or storyboards
- [x] No more constraints hell in Interface builder.
- [x] No more debugging in Interface builder toggling checkboxes.
- [x] The view code is not split between 2 files anymore
- [x] What you see is what you get, your view code is in one place, there is no hidden logic elsewere (in the xib)
- [x] No more referencing Storyboards or Xibs by their names "ProfileStoryboard". We all know strings are bad identifiers.
- [x] Clear view Hierarchy
- [x] Live reload, WHAT YOU SEE IS WHAT YOU GET
- [x] Events are a breeze
- [x] Code views Faster
- [x] No more XML (Thank God!)
- [x] Better readability 1000lines XML file vs. 30lines code
- [x] Readable constraints (they actually look like the layout itself \o/)
- [x] Horizontal & vertical layout can be described at the same time
- [x] Styles are well separated, concise, reusable and can be composed
- [x] Content like text, placeholders are easier to visualize

###






## Real life example : a classic Login View
![alt text](https://raw.githubusercontent.com/s4cha/Stevia/master/Stevia/LoginExample/login.png "Login view")

#### Before (Native Autolayout)

```swift
class LoginViewNative:UIView {

    let email = UITextField()
    let password = UITextField()
    let login = UIButton()

    convenience init() {
        self.init(frame:CGRectZero)
        backgroundColor = .whiteColor()

        addSubview(email)
        addSubview(password)
        addSubview(login)

        email.translatesAutoresizingMaskIntoConstraints = false
        password.translatesAutoresizingMaskIntoConstraints = false
        login.translatesAutoresizingMaskIntoConstraints = false

        addConstraint(NSLayoutConstraint(
                item: email,
                attribute: .Left,
                relatedBy: .Equal,
                toItem: self,
                attribute: .Left,
                multiplier: 1,
                constant: 8)
        )
        addConstraint(NSLayoutConstraint(
                item: email,
                attribute: .Right,
                relatedBy: .Equal,
                toItem: self,
                attribute: .Right,
                multiplier: 1,
                constant: -8)
        )
        addConstraint(NSLayoutConstraint(
            item: password,
            attribute: .Left,
                relatedBy: .Equal,
                toItem: self,
                attribute: .Left,
                multiplier: 1,
                constant: 8)
        )
        addConstraint(NSLayoutConstraint(
            item: password,
            attribute: .Right,
            relatedBy: .Equal,
            toItem: self,
            attribute: .Right,
            multiplier: 1,
            constant: -8)
        )
        addConstraint(NSLayoutConstraint(
            item: login,
            attribute: .Left,
            relatedBy: .Equal,
            toItem: self,
            attribute: .Left,
            multiplier: 1,
            constant: 0)
        )
        addConstraint(NSLayoutConstraint(
            item: login,
            attribute: .Right,
            relatedBy: .Equal,
            toItem: self,
            attribute: .Right,
            multiplier: 1,
            constant: 0)
        )
        for b in [email, password, login] {
            addConstraint(NSLayoutConstraint(
                item: b,
                attribute: .Height,
                relatedBy: .Equal,
                toItem: nil,
                attribute: .NotAnAttribute,
                multiplier: 1,
                constant: 80)
            )
        }
        addConstraint(NSLayoutConstraint(
            item: email,
            attribute: .Top,
            relatedBy: .Equal,
            toItem: self,
            attribute: .Top,
            multiplier: 1,
            constant: 100)
        )
        addConstraint(NSLayoutConstraint(
            item:email,
            attribute: .Bottom,
            relatedBy: .Equal,
            toItem: password,
            attribute: .Top,
            multiplier: 1,
            constant: -8)
        )
        addConstraint(NSLayoutConstraint(
            item: login,
            attribute: .Bottom,
            relatedBy: .Equal,
            toItem: self,
            attribute: .Bottom,
            multiplier: 1,
            constant: 0)
        )

        email.placeholder = "Email"
        email.borderStyle = .RoundedRect
        email.autocorrectionType = .No
        email.keyboardType = .EmailAddress
        email.font = UIFont(name: "HelveticaNeue-Light", size: 26)
        email.returnKeyType = .Next

        password.placeholder = "Password"
        password.borderStyle = .RoundedRect
        password.font = UIFont(name: "HelveticaNeue-Light", size: 26)
        password.secureTextEntry = true
        password.returnKeyType = .Done

        login.setTitle("Login", forState: .Normal)
        login.backgroundColor = .lightGrayColor()
        login.addTarget(self, action: "loginTapped", forControlEvents: .TouchUpInside)
        login.setTitle(NSLocalizedString("Login", comment: ""), forState: .Normal)
    }

    func loginTapped() {
        //Do something
    }
}
```

### With Stevia 🍃

```swift

class LoginViewStevia:UIView {

    let email = UITextField()
    let password = UITextField()
    let login = UIButton()

    convenience init() {
        self.init(frame:CGRectZero)
        backgroundColor = .whiteColor()

        sv([
            email.placeholder("Email").style(fieldStyle), //.style(emailFieldStyle),
            password.placeholder("Password").style(fieldStyle).style(passwordFieldStyle),
            login.text("Login").style(buttonSytle).tap(loginTapped)
        ])

        layout([
            100,
            |-email-| ~ 80,
            8,
            |-password-| ~ 80,
            "",
            |login| ~ 80,
            0
        ])
    }

    func fieldStyle(f:UITextField) {
        f.borderStyle = .RoundedRect
        f.font = UIFont(name: "HelveticaNeue-Light", size: 26)
        f.returnKeyType = .Next
    }

    func passwordFieldStyle(f:UITextField) {
        f.secureTextEntry = true
        f.returnKeyType = .Done
    }

    func buttonSytle(b:UIButton) {
        b.backgroundColor = .lightGrayColor()
    }

    func loginTapped() {
        //Do something
    }
}
```

**Number of lines** From 144 -> 57 **( ~ divided by 2.5)**  
**Number of characters** From 4231 -> 1338 **( ~ divided by 3)**

Write **3 times less code** that is actually **10X more expressive and maintainable** <3

## Live Reload

You can even enable LiveReload during your development phase! 🎉🎉🎉  

Stevia + [InjectionForXCode](http://injectionforxcode.com/) = <3 (WhoNeedsReactNative??) 🚀

![Output sample](http://g.recordit.co/i6kQfTMEpg.gif)

In order to support live reload with InjectionForXCode, we simply need to tell our ViewController to rebuild a view after an injection occured.

```swift
on("INJECTION_BUNDLE_NOTIFICATION") {
    self.v = CodeRequestView()
    self.view = self.v
}
```

Currently InjectionForXcode doesn't seem to swizzle "init" methods for some reason. So we have to move our view code in another methods
```swift
convenience init() {
    self.init(frame:CGRectZero)
    //View code
}
```
Becomes
```swift
convenience init() {
    self.init(frame:CGRectZero)
    render()
}

func render() {
  //View code
}

```

And Voila :)


## Installation

### Manual
Copy Stevia source files to your XCode project

### Carthage
```
github "s4cha/Stevia"
```

##Rationale behind the project

On the [Yummypets](http://yummypets.com) app, we needed to deal with looooooots of views.  
After trying different methods for building views (Xibs, Storyboards, Splitting Storyboards, React Native even(!).  
We found that coding views programmatically was the best solution for us.
But coding views programmatically had its issues too : UIKit exposes an imperative verbose API, and it's really easy to create a mess with it.
That's why we created Stevia.


## Contributors

[YannickDot](https://github.com/YannickDot),  [S4cha](https://github.com/S4cha),  [Damien](https://github.com/damien-nd),
[Snowcraft](https://github.com/Snowcraft)


## Other repos ❤️
Stevia 🍃 is part of a series of lightweight libraries aiming to make developing iOS Apps a *breeze* :
- Async code : [then](https://github.com/s4cha/then)
- JSON WebServices : [ws](https://github.com/s4cha/ws)
- JSON Parsing : [Arrow](https://github.com/s4cha/Arrow)
