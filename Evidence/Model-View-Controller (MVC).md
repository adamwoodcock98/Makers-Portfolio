# The Basics

The MVC architecture was invented in the 70s by Smalltalk developers. The fact it was invented before Windows, and even MS-DOS really shows how good this architecture actually is. MVC is very popular on Windows and across Apple platforms, meaning the knowledge of this process is extremely transferrable throughout our careers.

## Model

* Contains the logic of the application and is where data is manipulated or stored (usually we’d interface with our database here).
* In Sinatra, models are generally written as Ruby classes.

## Views

* The front-end, user-facing element of the web app. We usually build these with HTML, CSS and Javascript and typically this is the only part of the app that the user can directly interact with.
* In Sinatra, views are written as `​.erb`​ files, consisting of HTML and embedded Ruby.

## Controllers

* The intermediary between the views and the models, sending instructions to both of these elements.
* In Sinatra, controllers are written in Ruby and are made up of HTTP requests (GET, POST etc.), run code based on those requests, and render views to the user.

## How do the components of MVC communicate?

There is no predefined answer to this question, and this topic heavily debated. MVC leaves this open to interpretiation, the original author, Trygve Reenskaug said the following in 1979:

> "Models represent knowledge. A model could be a single object (rather uninteresting), or it could be some structure of objects”.

* The easiest component to understand as it has one-to-one mapping with real-world concepts.

> “A view is a (visual) representation of its model. It would ordinarily highlight certain attributes of the model and suppress others. It is thus acting as a presentation filter. A view is attached to its model (or model part) and gets data necessary for the presentation from the model by asking questions. It may also update the model by sending appropriate messages”.

* Views are visual manifestations of the data behind them

> “A controller is the link between a user and the system. It provides the user with input by arranging for relevant views to present themselves in appropriate places on the screen. It provides means for user output by presenting the user with menus or other means of giving commands and data. The controller receives such user output, translates it into the appropriate messages and pass these messages on to one or more of the views.”

* Controllers provide the user the ability to control their app, and translate those instructions into messages for the views

This means that:
1. The user uses the the controller, through the view
2. The controller manipulates the model
3. The model sends its data to the controller
4. The controller updates data in the view
5. The view renders itself to the user

This leaves the three components in the most decoupled way possible.

# Advantages and Disadvantages
Just because MVC is the default choice for so many developers and applications, it does not mean that we cannot be critical of it. Let's investivate what's good and what's bad:

## Advantages
* The biggest advantage of MVC is simply how ubiquitous it is. MVC is used *everywhere*, it really has a leading position and the perks that pretty much every developer anywhere knows what it is and how it works.
* The responsibilities of MVC are seperated so clearly that even a beginner developer can be sure of the various roles involved. 
* The data flow of an MVC is also extremely clear.

## Disadvantages
* The biggest disadvantage of MVC is that controllers can quickly end up becoming very, very large. It is extremely important to adhere to the (rather unpleasently named) 'keeping controllers skinny' principle. The 'business logic' should always be in the model.
* Controllers themselves can be very difficult to test; we can feature test our views, unit test our models, but testing the controller that is interacting with both is very challenging.
* MVC doesn't adequetly answer questions like 'where should networking go?'. Admittedly, this problem is present in other patterns like MVVM etc.