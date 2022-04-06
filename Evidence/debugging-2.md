The first step when starting to work with any new tech stack is to get to grips with the best debugging process.

### What are the ways in which we can get visibility over our code?

* Read the stack trace
  * Reading and breaking down error messages
* Printing elements with `p`
  * Even printing the actual lines of codes
* Writing meaningful unit tests
* Breaking up single lines of code, particularly when they’re doing more than 1 thing
* Read it aloud loud line by line
* Comment out newer lines of code and bringing them back step-by-step / reviewing what’s changed since the previous working version
* Feature testing
* Follow the data flow / diagramming
* Rubber ducking
* Running a segment of the code elsewhere
* Check your assumptions

### Walkthrough

1. Starting with errors on the homepage is a good place to begin
2. We do of course through our stack trace, working from the bottom up to ascertain where the error is happening.
  * We must be vigilant that often times where the error is *triggered*, is not a reflection of what is actually *causing* the issue.
2. Lets check our assumptions. The objective is to go through exactly what it is we want our test to be doing, in plain English.
  * There is no right or wrong way to do this, you could rubber duck it, write a paragraph, comment the code etc. I'm going with comments:

![1](https://github.com/adamwoodcock98/MakersPortfolio/blob/main/Evidence/Screenshot%202022-03-17%20at%2012.21.28.png?raw=true)
![2](https://github.com/adamwoodcock98/MakersPortfolio/blob/main/Evidence/Screenshot%202022-03-17%20at%2012.27.34.png?raw=true)
![3](https://github.com/adamwoodcock98/MakersPortfolio/blob/main/Evidence/Screenshot%202022-03-17%20at%2012.27.52.png?raw=true) 
![4](https://github.com/adamwoodcock98/MakersPortfolio/blob/main/Evidence/Screenshot%202022-03-17%20at%2012.28.06.png?raw=true)

3. The next step for me would be printing various elements of our code using `p` statements. Adding and removing them as necessary to help us tighten our loops.
  * Often at this point it can be very handy to label your `p`'s, this makes it easier to both spot your values in the terminal, and so you know what is coming from where, especially if a test calls a method multiple times.

![p's](https://github.com/adamwoodcock98/MakersPortfolio/blob/main/Evidence/Screenshot%202022-03-17%20at%2012.32.49.png?raw=true)

4. We can then use combinations of the previous steps, moving prints around, finding where data might be missing and more, to help us determine _where_ our bugs were coming from.

5. I would show more screenshots from the printing loop, however, by this point we had gotten our first test passing, and in the process had inadvertently passed the rest of the tests in this exercise.

Despite the quick end to this process, this does also help to highlight how effective it is to go through our affected areas and write pseudocode to help us diagnose and fix the issue. So many times during debugging we believe our errors are coming from a certain place, whether that's through our assumptions (check them!) or because the stack trace is telling us that's where the issue is *encountered*.
