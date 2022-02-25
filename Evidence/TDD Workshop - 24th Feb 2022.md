### Why do we even write tests?

* Makes it easier for someone new to the codebase to understand what *should* happen
* Ensures the code is really robust
* EDGE CASES
* Tests give you the confidence that if you break something in your code, your tests are going to make this very obvious.

### Why do we write tests before the code?

* Gives the code direction, making the code *objective*.
* Forces you to think about exactly what each piece of code should do
* Keeps you focused on the task at hand, ensuring good practices like SRP.

### What steps should we follow in TDD?

1. Feature test / plan the code in IRB
2. Write a unit test (in ./spec)
3. Test fails
4. Implement (in ./lib)
5. Test passes (*in some cases, you just want your code to pass in the simplest way, which may mean the code can be:*)
6. Refactor / Improve (*At this point, if you break your code, you know the tests will pick it up*)

### How do we add state in a Ruby Class?

Instance variable

### How do we add behaviour in a Ruby Class?

Methods?

### Piggy Bank Live Demo (with Leo)

1. Creates empty directory
2. Initialise rspec (`rspec —init`), creates spec dir.
3. User stories
  1. Start with the overall class
  2. Usually the most important part of the story is the last line; this covers state, behaviour and storage
  3. Go into IRB and try to create an instance of our class, this will fail. Maybe try an element of behaviour.
  4. Go into the spec file. Describing our class with no methods, even though its not a test, it’s still going to fail because Rspec cannot find the class
  5. Implement our class, run the test, this should now pass.
  6. Next we want to our `​put_coins`​ method.
4. Our tests can usually be split into an arrange step, an act step and an assert.
  1. __Arrange__- setup before our test
  2. __Act__ - make the program "act" (depending on what behaviour we want to test specifically)
    1. It’s okay to use public methods from the program in the tests, as this is the behaviour. This would be for example calling the `​put_coins`​ method as opposed to accessing the instance variable directly.
  3. __Assert__ - (check the resulting value, or the behaviour, was correct, likely using RSpec matchers)
  4. Try to think of your methods in terms of their *inputs* and *outputs*, ie. **what values go into them, what values they *return* and what outside methods they call**. This is what the tests should be testing.

### Testing behaviour over state

Testing behaviour over state is a concept that should be deeply engrained in our throught process when it comes to desigining our tests. This coupled with the arrange/act/assert concepts, we should work them together.

Behaviour means: **what** is the class doing? 

State means: **how** does it do it?

This is how we should model our tests.
