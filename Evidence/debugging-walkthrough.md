# Debugging Walkthrough
In this exercise I am going to walk through the steps of debugging a script, to demonstrate the way in which the process is executed.

## The problematic script
```ruby
def encode(plaintext, key)
  cipher = key.chars.uniq + (('a'...'z').to_a - key.chars)
  ciphertext_chars = plaintext.chars.map do |char|
    (65 + cipher.find_index(char)).chr
  end
  ciphertext_chars.join
end

# Intended output:
#
# > encode("theswiftfoxjumpedoverthelazydog", "secretkey")
# => "EMBAXNKEKSYOVQTBJSWBDEMBPHZGJSL"

```

### Let's execute
The first and most simple place I would start with this script would simply be to run it, to see exactly the error message we're receiving, to begin the process of tigthening the loop.

The following is the output from running the script in its current state:
```bash
./Debugging.rb:4:in `+': nil can't be coerced into Integer (TypeError)
        from ./Debugging.rb:4:in `block in encode'
        from ./Debugging.rb:3:in `map'
        from ./Debugging.rb:3:in `encode'
        from ./Debugging.rb:19:in `<main>'

```
### From bottom up
Next, I would read the stack trace from the bottom up. This shows that this lead to an error started when the method was called, the next trace tells us to look at the third line of our encode method, more specifically we can see from the next 2 that there is something happening inside of the `map` block. Finally the error happens when trying to sum some values.

We now know for certain that the error is happening on the calculation on line 4. I haven't yet encountered the `.chr` method so I do some research to understand this.

> Returns a string containing the character represented by the `int`'s value according to encoding.

So in essence, we receive a character from a given number.

Now I know this line is trying to search through an array (`cipher`), that stores the unique letters of our `key` and the alphabet, minus the characters present in the key. We know have a jumbled array containing all of the alphabet. We then create a new array `ciphertext_chars` by mapping all of the characters in the text we want to encode, for each letter of the text to encode, we search for an index number from our jumped alphabet, `cipher`, adding 65 to the value and using `.chr` to give us a new character based on that total, returning this character to `ciphertext_chars`.

### Tightening the loop
Now I have a good grasp of what this script is doing, I need to find what is causing the nil value, everything seems to be logical, so I will `p` some of the values around the method to try and diagnore this further.

```ruby
def encode(plaintext, key)
  cipher = key.chars.uniq + (('a'...'z').to_a - key.chars)
  p cipher
  ciphertext_chars = plaintext.chars.map do |char|
    
    (65 + cipher.find_index(char)).chr
  end
  ciphertext_chars.join
end

```

I `p`'d the cipher, to get a clear picture of what this created, and this was the outcome:
```bash
["s", "e", "c", "r", "t", "k", "y", "a", "b", "d", "f", "g", "h", "i", "j", "l", "m", "n", "o", "p", "q", "u", "v", "w", "x"]
./Debugging.rb:6:in `+': nil can't be coerced into Integer (TypeError)
        from ./Debugging.rb:6:in `block in encode'
        from ./Debugging.rb:4:in `map'
        from ./Debugging.rb:4:in `encode'
        from ./Debugging.rb:21:in `<main>'

```

The stack trace itself has not changed, so I go straight to the printed `cipher`. Immediately I notice that the 'Z' is missing, and our text to encode includes a 'Z'. So when we search for this index number, nothing will be returned. We have the culprit.

The fix is simple, change the `...` to `..` to *include* the final element in the range. Let's run our code with that implementation and print the outcome.

Now we simply have the desired output:
```bash
EMBAXNKEKSYOVQTBJSWBDEMBPHZGJSL

```
### Takeaways
We trace our stack to find where an error is happening, but often in more convoluted bugs this doesn't necessarily mean the issue is happening there, simply the execution fails at that point. In hindsight, and with more experience, when breaking down the functionality of the method I could've noticed that bug immediately.

It's very easy to get caught up in what the method does, and the intricacies, particulary when it comes to these kind of calculations, that we don't always rememeber to think everything through.