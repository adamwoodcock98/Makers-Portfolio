In this lesson we will be covering the fundamental concepts behind MVVM, how they are applied in Apple developoment, discuss the major advantages and disadvantages and work through some example code demonstrating how to put these concepts into practice.

Before we dive in, it may be useful to note that that MVVM was invented by Microsoft in 2005, as a new way to build apps using Windows Presentation Foundation (WPF) framework, but since has spread very far.

The fundamentals of MVVM
========================

We already know from MVC, that this is often referred to as a *Massive View Controller* due to developers often dumping code into the view controller, resulting in a large ball of mud with terrible legibility, reusability and tight code coupling. It does not have to be this way however, in 2004 Martin Fowler suggested an alternative: introduce a new object called a **presentation model**, that stores the state of the app independently of the UI. Be aware, *presentation model* is synonymous with *view model*.

### 

---

*Warning: Despite sounding similar, the Presentation Model architecture is not the same as Model-View-Presenter architecture*

---

Using presentation models requires you to write classes that are able to represent your view fully, *without* any UIKit or AppKit attachment. The presentation model should be able to read all of your model data and perform the transformations needed to prepare this data for presentation, **without actually importing UIKit**.

The immediate advantage to this is that we can write tests for everything your model does. You can feed any data into your model and respond with elements you can actually check, a concept that is extremely difficult when any interaction with UI elements are present.

In the presentation model Apple’s view controllers are part of the **view **system as opposed to the **controller** system. The only role of the view controller becomes to respond to Apple system events that are posted there, such as `​viewDidLoad()`​. This approach works quite well on Apple systems.

Despite the immediate advantages, MVVM presents a complex issue: if all presentation models store the current application state, and all views display application state, how are these two synchronised? This is where MVVM comes in, it was designed to take the presentation model architecture, attach a system of data-binding to remove the need to write boilerplate code connecting your views and your presentation models.

For clarity, MVVM is the presentation model, with the addition of some functionality enabled by the WPF framework, it does not rely on WPF, however WPF makes it much more natural because of its support for data binding - the ability to connect a text field to a string in the model so that changing one changes the other.

All of this results in MVVM having the same testability of the presentation model, without the boilerplate issues of synchronising the various parts of your app. John Gossman, the developer who invented MVVM said: *“the ViewModel, though it sounds View-ish is really more Model-ish, and that means you can test it without awkward UI automation and interaction. If you’ve ever tried to test UI code, you know how hard that can be”*.

How Apple developer use MVVM
----------------------------

WPF has been mentioned several times so far as it directly affects how Apple developers use MVVM. It’s no suprise that a technology with Windows in its name is not available on Apple’s platforms, which makes the bindings that make MVVM work a very alien concept to Apple developers.

Because of this we’re faced with the situation of iOS developers crying out for bindings to get MVVM to work natively. iOS developers still need bindings otherwise MVVM is far too painful to be effective, so we have 3 choices:

1. Write your own binding code that keeps your view model in sync with your views
2. Use a library such as Bond, which lets you say how values should bound to views
3. Implement something large like RxSwift, which includes binding as part of its framework

zOf the 3, the first 2 are the only viable options as RxSwift is such a vastly different way of working, using it just for bindings is a bad move.

Once the method has been established, the rest becomes more straightforward. The view controller becomes responsible for presenting layouts, **binding** the components in those layouts to the fields inside the view-model, and all the business logic in your VC gets moved to the view-model. The amount of logic in the view-model and the amount in the model vary dependant on the individual (this dilemma is no different in MVC).

MVVM is only slightly more difficult to understand than MVC, but significantly harded to use due to the lack of native bindings on Apple platforms. MVVM for simple UIs is extreme overkill, especially on Apple platforms.

Advantages and disadvantages
----------------------------

MVVM is a great choice for a lot of projects, and as projects increase in size and scope, so does the usefulness of MVVM.

### Advantages

* By far the biggest advantage is **testability**. Going two ways, our unit tests for most of the app are much more simple as we have the ability to call directly into the view-model, in addition to our now very small view controllers, mock view models with stubbed data to ensure correct functionality are much easier to write.
* View controllers are freed up to focus on the view lifecycle - what they are actually meant to do.
  * It must be acknowledged that 80% of the code has most likely been moved elsewhere, and that place may well become a dumping ground of code, but nevertheless it is still an improvement.
* Just like MVC, MVVM is a simple enough conecept for beginners to understand - models store data, views store visual representations of data and ‘everything else’ goes into the view-model. Unlike MVC however, ‘everything else’ going into the view-model seems a little dubious, meaning developers may well be naturally inspired to split it up.
* MVVM has a clear data flow, as long as bindings aren’t made too difficult. Views and view-models are connected with two-way binding and the view-model manipulates your model. The VC does next to nothing.

### Disadvantages

* The one major disadvantage of MVVM is the need for boilerplate code throughout your application, or the use of bindings. Bindings aren’t too hard to write, but as understanding of these increases, the desire to switch to a framework will increase, subsequently increasing the complexity of their usage,
* MVVM does solve the issue of small views, mostly due to view controllers going into the ‘view’ category. Unfortunately, MVVM doesn’t do much for small models; this of course depends on the individual, however when discussing how much code goes into models for both MVVM and MVC, the answer is not much.
* MVVM introduces a lot of complexity for small projects, no Apple templates use MVVM, so bindings must be self-introduced each and every time. For small projects and prototypes this can quickly become a large waste of effort.
* MVVM frees up view controllers, however view-models still become a dumping ground of unrelated code.

Bindings in practice
====================

We’ll be walking through a lot of code in this section, despite this you may well find yourself using a framework like *Bond* anyway as there is so much more functionality. This practical knowledge however is essential as it allows a greater understanding of bindings (and MVVM as a whole), making a transition to something like *Bond*** **much more simple.

The principle behind MVVM’s bindings is to prevent you from copying data from your view to your view-model and visa versa; this principle of multi-directional data flow is known as **two-way binding**.

We will be covering 2 ways of creating bindings in Xcode; one is a harder, idiomatic *Swifty *type, one is an easier *un-Swifty* way of doing it. The idiomatic approach is significantly better than the easier approach.

Creating an observable type
---------------------------

We will be working with an extremely simple example, with the goal being to create a **text field ***(view)* that **synchronises its text ***(view-model)* with the `​**name**`**​ value in a struct*** (model)*. It’s one view-controller, one model object and one property, so this really is the most simple example we can use.

```swift
struct User {
  var name: String
}
```

* In the above example, we have simply done the coding for our **model** in this example, you can see that this model `​struct`​ has one property `​name`​.

In the next step we will be using the storyboard so I will list the instructions:

1. Open **Main.storyboard**
2. Create a new **UITextField**
3. Create an **IBOutlet **connecting to **ViewController.swift**
  1. Name the IBOutlet **username**

In order for our bindings to work, we need to **observe** changes to the values which is not something that Swift `​struct`​s have any general-purpose way of doing. We could use `​didSet`​, however that is an extremely specific way to observe changes, we would have to set that up for every single value that we need to watch.

So our next step is to create an **Observable** class that is designed to monitor one value. We’ll be using a class as it allows us to mutate it freely, as binding skills improve it is better to switch to a `​struct`​ as it provides the extra immutability (or move to *Bond*).

Our **Observable type** needs to be **generic**, because even though it’s just going to handle strings in this example it can easily be extended to any other type in the future.

Add the following code below the `​User`​ struct:

```swift
class Observable<ObservedType> {
  private var _value: ObservedType?
  
  init(_ value: ObservedType) {
    _value = value
  }
}
```

* The code above creates our **generic type**
* It contains 1 property to store the value
* An initialiser to enable us to create them easily.
* Note that the stored value is private, we don’t want this to be accessed anywhere else by accident

A problem has alridy arisen with this code, our private value matters because of two-way bindings; if the text field changes the model must be updated and visa versa. This can easily create an endless cycle as when one is changed it notifies the other, which then changes and notifies the first that it changed and so on.

To avoid this endless loop, we must create a `​valueChanged`​ property in the `​Observable`​ class, which gets called whenever the value changes. This will be a closure, and will point to some **view** code that will be written later.

```swift
var valueChanged: ((ObservedType?) -> ())? //Add this property to the Observable class
```

* This closure will send the current value to whatever is watching it.

With the above property in place, our next step is to create an area in `​Observable`​ class where we can safely manipulate the stored value. We will do this through a new property called `​value`​, note this is different to the previous `​_value`​ (the underscotes denotes it is private), this will be public so that it can be adjusted from anywhere changing both the `​_value`​ property and send its new value to the observer using the `​valueChanged`​ closure. 

To do this we will be using property observers; add the following to the `​Observable`​ class:

```swift
public var value: ObservedType? {
  get {
    return _value
  }
  
  set {
    _value = newValue
    valueChanged?(_value)
  }
}
```

* The `​get`​ part is the **read-only** element of our property, we include this so if we need to access the current value elsewhere in the code we can.
* The `​set`​ part is where we set the private `​_value`​ to the new value that has been input, using the default `​newValue`​ constant that comes with the `​set`​ computer property.
  * This means that our current value is always stored safely, the manipulations are done in the non-private property and only updating our private property when this is finished
  * We then send the updated private property into our `​valueChanged`​ closure to send this value to whatever is observing it.

The other element that our `class` needs to care about is when the other direction of the binding has changed (when the text field has been changed by the user). This should automatically update the value we’re storing, but it is imperative **not to do so using `​value`​ **otherwise this will trigger our closure block subsequently starting the endless loop.

The method to achieve this belongs in `​Observable`​ so we can just modify its `​_value`​ in place, without triggering the `​didSet`​ property observer. We can do this by adding the following method:

```swift
func bindingChanged(to newValue: ObservedType) {
  _value = newValue
  print("Value is now \(newValue)")
}
```

* We are creating a function that takes a new value, where the type is our generic `​ObservedType`​
* We then set the private property `​_value`​ to the new value input in the `​UITextField`​
  * We have simply updated the variable manually, therefore bypassing the `​set`​ in our computed public `​value`​ which would trigger the observer
* We then included a print statement to help legibility in some later stages

Now that we have a type dedicated to observing changes, we can modify the `​User`​ `​struct`​ to use it. This means we must update as follows:

```swift
---Previous Definition---
struct User {
  var name: String
}     
---New Definition---
struct User {
  var name: Observable<String>
}
```

* We can see that our name property is now of type `​Observable<String>`​
  * This means that property is our custom object, it was a generic type, but now we are specifying that this particular observer object will be observing `​String`​ types.

This completes our `​Observable`​ type so we can now move onto our text field. We need to figure out how to monitor changes in our text field to update our model, and observe changes in our model to update our text field. This is made more complicated as you **cannot attach closures** to any `​UIControl`​; so we need a workaround.

Luckily, text fields contain a dedicated `​editingChanged`​ event that is triggered by a users touch. We need to connect this `​editingChanged`​ event to a closure that will update the model value with the new text. As we cannot do this directly we must subclass `​UITextField`​, call a dedicated `​valueChanged()`​ method when the `​editingChanged`​ event happens, which can then call the closure.

To start we must add a new class underneath `Observable`:

```swift
class BoundTextField: UITextField {
  var changedClosure: (() -> ())?
}
```

* The `​changedClosure`​ is what we’ll call when the `​editingChanged`​ event is triggered. As it can’t be called directly, we need a little **thunk** method - a method that can bridge the world of Objective-C and Swift.

Add the following to `​BoundTextField`​:

```swift
@objc func valueChanged() {
  changedClosure?()
}
```

* This is a pure Objective-C method so we are abble to attach it to our text field as a selector, with its only functionality being to pass the call directly to the `​changedClosure()`​ call

We need one more method, which will do all the heavy lifting, this will:

1. Accept any `​Observable`​ type value that stores a `​String`​ as that’s what our text fields work with
2. Add the text field as its own handler for the `​editingChanged`​ event, pointing to the `​valueChanged()`​ method we just created
3. Set the text fields `​changedClosure`​ to some code that calls the `​bindingChanged()`​ method (from our `​Observable`​ class) on its observed object
4. Set the observables `​valueChanged`​ closure (again, in the `​Observable`​ class) to some code that updates the text in the text field

```swift
func bind(to observable: Observable<String>) {                                         //Step 1
  addTarget(self, action: #selector(BoundTextField.valueChanged), for: .editingChanged)//Step 2 - self assigns the text field as its own handler,                                                                                       the selector simply must point to an obj-c func, for is                                                                                        the event we want to action
  changedClosure = { [weak self] in                                                    //Step 3 - bindingChanged takes an input in a public var and
    observable.bindingChanged(to: self?.text ?? " ")                                   // safely updates the private var (aka the model data)
  }
  
  observable.valueChanged = { [weak self] newValue in                                  //Step 4
    self?.text = newValue
  }
}
```

The next step for us is to update our actual text field to conform to our custom class:

1. Open the storyboard, select the text field and head into the identity inspector, from there change its class to `​BoundTextField`​
2. Back in `​ViewController.swift`​ change `​var username: UITextField`​ to be `​var username: BoundTextField`​

The last step is the easiest, we need to add the following to our code:

```swift
var user = User(name: Observable("Adam Woodcock"))  //This property should be declared in the view controller

username.bind(to: user.name)                        //This call should go in the viewDidLoad() method
```

That’s it! This is all the code we need in our VC to make the bindings system work. This gives a glimpse of what VCs are like when using MVVM; they present their views (text field in our example), bind those views to data, then do little more than respond to lifecycle events.