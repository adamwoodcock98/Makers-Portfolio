### What is a dependancy?

* If one class A depends on another class, B , if it calls methods or attributes from this class.
* We should avoid 2-way dependancies.

### When to mock?

* If one class A depends on one class , we need to change A’s unit tests in order to mock B (using doubles or stubs).
* You don’t want one class or class of unit tests to break, when the error is actually in a dependancy.
* A mock is simply *mocking* the behaviour of one of our dependancies, to avoid the breakages mentioned from happening.

### Dependancy injection

There are 2 different ways in which we can pass our mocks into our testing code. One uses initialisation, one uses `attribute_accessor`s. The preferred method is the former.

* Constructor injection
  * `airport = Airport.new(fake_weather)`
* Setter injection
  * `airport = Airport.new`
`airport.weather = fake_weather`​

### What are doubles?

A double is something that we can create the acts as our dependancy. We name them, provide them with some context and what they should receive (in terms of the dependancies methods) and how they should respond to that context. For example, if we had a predicate method, we could create a double that should respond to that method, and the boolean value we want it to return.

This means when we pass this into our testing class, it can call the methods and even tho it’s not one of the object, a response will still be received, the program will not crash and it provides the benefit of force testing a specific outcome.

### Before / do

We may want to create these mocks and use them throughout various tests, we can use a `​before do … end`​ block to run some setup, create doubles and method stubs, then use them throughout our code.

### Naming conventions

One simple naming convention would be to call doubles the name of the class suffixed with `​_double`​.