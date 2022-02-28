Setup
=====

User Stories
------------

```text
As an air traffic controller 
So I can get passengers to a destination 
I want to instruct a plane to land at an airport

As an air traffic controller 
So I can get passengers on the way to their destination 
I want to instruct a plane to take off from an airport and confirm that it is no longer in the airport

As an air traffic controller 
To ensure safety 
I want to prevent landing when the airport is full 

As the system designer
So that the software can be used for many different airports
I would like a default airport capacity that can be overridden as appropriate

As an air traffic controller 
To ensure safety 
I want to prevent takeoff when weather is stormy 

As an air traffic controller 
To ensure safety 
I want to prevent landing when weather is stormy 
```

The first user story
--------------------

### Domain Model

We’ll start by jumping into the first user story, creating a domain model for the requirements.

```text
   Objects    |    Messages
------------------------------
Plane         |  
Airport       |   land(plane)
```

* I’ve gone with `​land(plane)`​ as I feel this is extremely clear for the methods function.

### Feature Test

The following was feature tested using IRB:

```sh
adamwoodcock@Adams-MacBook-Pro airport_challenge % irb -r ./lib/plane.rb
3.0.2 :001 > airport = Airport.new
 => #<Airport:0x00007fc6b71eb128> 
3.0.2 :002 > airport.land
(irb):2:in `<main>': undefined local variable or method `land' for main:Object (NameError)
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/lib/ruby/gems/3.0.0/gems/irb-1.3.5/exe/irb:11:in `<top (required)>'
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/bin/irb:23:in `load'
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/bin/irb:23:in `<main>'
```

* We have created the bare bones of this project, the lib directory containing a `​airport.rb`​ file and a `​airport_spec.rb`​ file in the spec folder.
* Note that to follow the RED-GREEN-REFACTOR principle of TDD, we will not yet create an argument as per the domain model.
* We can see from the feature test above that we are able to instantiate a new `​Airport`​ object, however we are unable to call the `​land`​ method as it does not yet exist, we now have our first:

### Unit Test

```ruby
describe Airport do
  it 'should respond to the land_at method' do
    expect(subject).to respond_to(:land)
  end
end
```

Results in the following error:

```sh
Failures:

  1) Airport should respond to the land method
     Failure/Error: expect(subject).to respond_to(:land_at)
     
     NoMethodError:
       undefined method `land' for #<Plane:0x00007f8ae305fd00>
     # ./spec/plane_spec.rb:5:in `block (2 levels) in <top (required)>'
```

Now that we have our RED test, we can implement code to satisfy the conditions:

```ruby
class Airport
  def land
    
  end
end
```

Our test now passes:

```sh
Airport
  should respond to the land method

Finished in 0.00274 seconds (files took 0.19626 seconds to load)
1 example, 0 failures
```

### Feature Test

Next, we need the `​instruct a plane`​ part of the user story, so we need to pass a plane as an argument. Let’s start with the feature test:

```sh
adamwoodcock@Adams-MacBook-Pro airport_challenge % irb -r ./lib/airport.rb 
3.0.2 :001 > airport = Airport.new
 => #<Airport:0x00007f79518db248> 
3.0.2 :002 > airport.land(plane)
(irb):2:in `<main>': undefined local variable or method `plane' for main:Object (NameError)
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/lib/ruby/gems/3.0.0/gems/irb-1.3.5/exe/irb:11:in `<top (required)>'
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/bin/irb:23:in `load'
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/bin/irb:23:in `<main>'
```

### Unit Test

Now we can write our unit test to expect a single argument.

```ruby
it 'should allow the air traffic controller to specify a plane to land' do
    expect(subject).to respond_to(:land).with(1).argument
  end
```

Resulting in the following error:

```sh
Failures:

  1) Airport should allow the air traffic controller to specify a plane to land
     Failure/Error: expect(subject).to respond_to(:land).with(1).argument
       expected #<Airport:0x00007f84679d90e8> to respond to :land with 1 argument
     # ./spec/airport_spec.rb:10:in `block (2 levels) in <top (required)>'

Finished in 0.02644 seconds (files took 0.30714 seconds to load)
2 examples, 1 failure
```

Now let’s implement the simple coding fix:

```ruby
class Airport
  def land(plane)
    
  end

end
```

Great, now our second test is passing.

The other user stories
----------------------

I followed these steps for the following user stories:

```text
As an air traffic controller 
So I can get passengers on the way to their destination 
I want to instruct a plane to take off from an airport and confirm that it is no longer in the airport

As an air traffic controller 
To ensure safety 
I want to prevent landing when the airport is full 

As the system designer
So that the software can be used for many different airports
I would like a default airport capacity that can be overridden as appropriate
```

These steps involved the standard feature tests in IRB, creating domain models, writing failing tests and implementing code to pass these tests. The [repo commit history]([https://github.com/adamwoodcock98/airport\_challenge/commits/main](https://github.com/adamwoodcock98/airport_challenge/commits/main)) shows these steps in more detail.

The final two user stories
--------------------------

Things became a little less clear when we look at the last 2 user stories:

```text
As an air traffic controller 
To ensure safety 
I want to prevent takeoff when weather is stormy 

As an air traffic controller 
To ensure safety 
I want to prevent landing when weather is stormy 
```

### Blocks

Before I even begin to write feature tests, or domain models, I usually like to think through what the story is asking in my head, to get an idea of which direction to take this in. In this instance I feel that was a mistake; I spent a lot of time going back and forth, thinking of the best way to do things and I ended up getting quite frustrated. I had to leave the code here and come back the following day with a fresh perspective. In hindsight, had I written the domain model, things would’ve been much clearer and I wouldn’t have ended up wasting time on the block.

### Domain Models

Eventually, I knew the best way to go about the weather would be a seperate class, so I had the following model:

```text
   Objects    |          Messages
--------------------------------------------
Plane         |  
Airport       | land(plane), take_off(plane
Weather       |           stormy?
```

* What I wanted was a predicate method, that would perform so kind of functionality, so that in my respective airport methods, I could raise errors depending on the outcome of that code.

Great, let’s get the feature:

### Feature Test

```sh
adamwoodcock@Adams-MacBook-Pro airport_challenge % irb -r ./lib/weather.rb
3.0.2 :001 > weather = Weather.new
 => #<Weather:0x00007ax0nh2eb365> 
3.0.2 :002 > weather.stormy?
(irb):2:in `<main>': undefined local variable or method `stormy?' for main:Object (NameError)
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/lib/ruby/gems/3.0.0/gems/irb-1.3.5/exe/irb:11:in `<top (required)>'
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/bin/irb:23:in `load'
        from /Users/adamwoodcock/.rvm/rubies/ruby-3.0.2/bin/irb:23:in `<main>'
```

As before, we implemented code to test and fix this simple `​NameError`​.

```ruby
Describe Weather do

it 'should respond to stormy' do
    expect(subject).to respond_to(:stormy?)
  end
  
end


class Weather
  def stormy?
    
  end
end
```

### Test 2

I knew I was building a predicate method, so I needed to ensure that `​stormy?`​ was to return a boolean, so I wrote the following test:

```ruby
it 'should return a random boolean' do
    expect(subject.stormy?).to eq(true). or eq(false)
  end
  
# passed the test with the following:
def stormy?
  [true, false].sample
end
```

* I passed this test in a simple way that could result in either outcome.

Testing for logic
-----------------

I needed to test the logic of the `​stormy?`​ method, again this would require some thinking. Should this be in the `​Weather` or `Airport`​ class? I decided on the airport class as this is where we actually *needed* the functionality.

This meant more than just a simple test, we had our `stormy?` method, which at this point has no logic, so before moving on I decided to get this method to *actually* do what we need it to do. Our spec for the challenge has the following:

\> You will need to use a random number generator to set the weather (it is normally sunny but on rare occasions it may be stormy). In your tests, you'll need to use a stub to override random weather to ensure consistent test behaviour.

I thought of using a `​.rand`​ number on a case statement, but in doing some research on the best way, I decided to go with an array of symbols, with duplicate sunny entries, and one stormy entry and then `​sample`​ this array:

```ruby
def stormy?
  ["sunny", "sunny", "sunny", "stormy"].sample == "stormy"
end
```

This satisfies our conditions, returns a boolean, however it does not conform to the **Single Repsonsiblity Principle**, so I pulled some of that logic out into a seperate method. Upon doing so, I marked this as private as it was purely internal logic, and was not necessary to provide access to, or to test.

```ruby
def stormy?
  randomise_weather == :stormy
end

private

def randomise_weather 
  [:sunny, :sunny, :sunny, :stormy].sample
end

#I changed to symbols as we do not need to mutate these variables, plus it looks much nicer. I also realised that in terms of 'ease of change' I could put this into a private constant, so refactored:

private

POSSIBLE_WEATHER = [:sunny, :sunny, :sunny, :stormy]

def randomise_weather
  POSSIBLE_WEATHER.sample
end
```

* The actual `​stormy`​ method only needed basic changes.

### Mocking behaviour

Now, to go back to testing our `​airport`​ class, in order to prevent dependancies in the code, and to satisfy the spec of the challenge, we would have to mock the weather and stub it’s functionality. We covered mocks in our [Boris Bikes]() challenge, so I went ahead and used **shorthand syntax**, to create some good and bad weather doubles:

```ruby
let(:good_weather) { double(:good_weather, :stormy? => false) }
let(:bad_weather) { double(:bad_weather, :stormy? => true) }
```

Now I was able to write some tests to prevent take off and landing in this kind of weather.

```ruby
context 'Weather dependant tests' do

    it 'should not land in bad weather' do 
      airport = Airport.new(10, bad_weather)
      expect { airport.land(plane) }.to raise_error 'Weather too stormy to land'
    end

    it 'should not take off in bad weather' do
      airport = Airport.new(10, bad_weather)
      airport.planes << plane # manually adding a plane to the airport, as bad weather would prevent landing a plane
      expect { airport.take_off(plane) }.to raise_error 'Weather too stormy to take off'
    end

  end
```

* I must admit, when writing these tests, I had to play around with some code at the same time. I was struggling to figure out how to actually *apply* the mocks to my airport code, it took me a while with some trial and error, but after some help from some peers and some research, initialising a default `​Weather.new`​ object on the airport, would mean that I could pass this weather mock into the airport.

To satisfy the tests, I only had to add 1 line of code to each relevant method:

```ruby
raise "Weather too stormy to land" if weather.stormy?

raise "Weather too stormy to take off" if weather.stormy?
```

This was very straightforward and did what I wanted it to do.

Edge Cases
----------

In our spec there were some edge cases mentioned:

\> Your code should defend against [edge cases](http://programmers.stackexchange.com/questions/125587/what-are-the-difference-between-an-edge-case-a-corner-case-a-base-case-and-a-b) such as inconsistent states of the system ensuring that planes can only take off from airports they are in; planes that are already flying cannot take off and/or be in an airport; planes that are landed cannot land again and must be in an airport, etc

I was really hoping to get these edge cases done, I knew it would involve creating a plane class, modifying some tests. But in the `​take_off`​ method, it meant I wouldn’t just pe `​pop`​ping the last element of the array, things could be much more specific.

Sadly, with a few of the blocks I hit, I did not get the time to complete these edge cases, hopefully I will get chance to come back to these challenges and finish them off.

Takeaways
---------

I definitely feel like there were times I did not trust my gut and this resulted in some extra time being spent on things that really didn’t need this. Also had I stuck to my domain models the whole way through, the process would’ve been sped up.

I enjoyed this challenge, I felt it really reinforced a lot of the [week 1]() learning and strengthened my understanding of mocking and stubbing a **lot**. I would definitely do things differently again, my confidence definitely needs to be worked on, but overall I’m pleased with what I achieved.

