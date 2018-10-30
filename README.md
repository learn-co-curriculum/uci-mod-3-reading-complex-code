# Reading Complex Code

“Meaning lies as much
in the mind of the reader
as in the Haiku.”
― Douglas R. Hofstadter

## Learning Goals

- Identify methods for interpreting unfamiliar code
- Identify inputs of a method
- Identify outputs of a method
- Distinguish separate actions within an existing method
- Infer relationships between methods

## Introduction

Being able to approach and understand code that we are unfamiliar with is a
critical programming skill.

Imagine you've landed a job as an engineer. You've joined a team thats already
working on a project and your task is to add your own feature. You've cloned the
project to your local machine and open it up... to find thousands of lines of
code in dozens of files. Where do you begin? It's like a giant knot that you
must try and unravel. Someone else made it and they're not always around to help
explain. It's easy to get overwhelmed!

This problem is common. It can be very hard to interpret the actions of code
written by another. Even code that we've written _ourselves_ can seem strange if
enough time has passed, making it difficult to jump back into old projects.
However, we can push past this point of unfamiliarity by following a set of
steps to drill down through the code and make sense of it.

There are a few common problems we face when opening up and looking at
unfamiliar code:

- Unknown vocabulary
- Strange syntax and structure
- Spaghetti code
- Dense code

Each of these can slow down our abilities to interpret and understand code. They
create 'opaque' chunks of code - code that's purpose isn't quickly apparent. Too
much opaque code can stop our progress altogether.

The overall process to resolve this challenge:

1.  Understand the Problem
2.  Create a Plan
3.  Implement Solution
4.  Test

We know what the problem is here. In this lesson, we will be looking at a plan,
a way to approach and interpret any unfamiliar blob of code. We will use some
code examples to illustrate as we go.

### The Plan

This process aims to demystify unfamiliar code. To separate the known from the
unknown and systematically drill down until the code makes sense. In summary,
the process looks like this:

1.  Identify where to start
2.  Identify inputs and outputs
3.  Distinguish and separate which parts of the code are **clear** and which are
    **opaque**
4.  From your starting point, walk through the code linearly until your reach
    opaque code.
5.  Repeat steps 2-4 on the opaque code
6.  Continue through all opaque code until the end

#### 1. Identify Where To Start

If you're trying to understand a single method, start from the top. Figuring out
where to start with multiple related methods is not always as clear. If the
methods are related, at least one of them will likely call the other. One simple
approach then would be:

A. Find a method in which other methods are called.
B. Check to see if this method is called in any _other_ method
C. If true, repeat step B for the new method. If false, start process

Let's take a look at an example to apply this to. Without knowing the purpose
of the code below, using the above steps, where should we start?

```ruby
def print_results(player, results, answer)
  if results
    puts player + " guessed correctly!"
  else
    puts player + " guessed incorrectly! The answer was " + answer
  end
end

def calculate_results(guess, answer)
  guess == answer
end

def valid_guess?(guess)
  (1..10).include? guess
end

def make_guess
  guess = ""
  loop do
    puts "Guess a number between 1 and 10"
    guess = gets.chomp.to_i
    break if valid_guess? guess
    puts "Invalid entry"
  end
  guess
end

def start_new_game(player = "Player")
  answer = rand(10)+1
  guess = make_guess
  results = calculate_results(guess, answer)
  print_results(player, results, answer)
  results
end
```

In this example, following the provided steps will lead you to `start_new_game`
regardless of which method you choose first.

#### 2. Identify Inputs and Outputs

Before we start trying to read code line by line, its important to understand
the context the code is in. Similar to steps in the Flatiron Process, it is
valuable to recognize the inputs and outputs of the code you're starting with.

**Inputs:** Are there any inputs for the code you're looking at? How is the
input used? Is it clear what data types they are?
**Outputs:** What is the output supposed to be? Is it clear what data type
the output is?

```ruby
def start_new_game(player = "Player")
  answer = rand(10)+1
  guess = make_guess
  results = calculate_results(guess, answer)
  print_results(player, results, answer)
  results
end
```

Here, `player` is the only input and we can see that it has a `String` default
value. The last line of a method implicitly returns `results`, so thats our
output. It isn't clear what `results` is just yet, however.

#### 3. Distinguish and Separate Out Clear and Opaque Code

Our goal is to make the unknown, the unfamiliar, _known_. We need to pinpoint
what _exactly_ is unknown to us. To do this, take a brief glance over the code
you are working on and try to first find all parts of the code that are well
understood. This is **clear** code. Obvious at a glance. What is left is the
**opaque** code.

```ruby
def start_new_game(player = "Player")
  answer = rand(10)+1 # variable assignment using a random number

  guess = make_guess # ??? not really sure yet how this works
  results = calculate_results(guess, answer) # ??? maybe I could guess what this does
  print_results(player, results, answer) # ??? probably displays something from its arguments

  results # return variable
end
```

**Clear** code is well understood. Examples: Variable assignment, simple data
manipulation, logs, prints, etc.. things that have an obvious purpose. What is
clear to you may vary based on how well you know a language, and that is fine.

**Opaque** code is not immediately understood. Examples include calls to other
methods where the specific code is written somewhere else, 'built in' Ruby
class methods or unfamiliar gem methods, and code blocks like loops

#### 4. Walk Through The Code

Now that we've got some context - we have some ideas on inputs and outputs as
well as some of the parts of the code, we can step through the code until you
get to the first opaque line. Keep a mental narrative and use comments if you
need to keep track of something.

```ruby
def start_new_game(player = "Player")
  answer = rand(10)+1 # variable assignment using a random number

  guess = make_guess
  ...
```

After a variable assignment, we hit something we can't immediately understand,
`make_guess`. We aren't sure what the method does, but we can see that its
return value gets assigned to the variable `guess`. This is our first line of
opaque code.

### 5. Repeat Steps 2 through 4 on the Opaque Code

We can't get much further in understanding our code without
diving into a second method, `make_guess`. Briefly reapplying the steps on
`make_guess`:

```ruby
def make_guess
  guess = ""
  loop do
    puts "Guess a number between 1 and 10"
    guess = gets.chomp.to_i
    break if valid_guess? guess
    puts "Invalid entry"
  end
  guess
end
```

- **Identify Inputs and Outputs:** No input involved here. The output is `guess`,
  which we can see is assigned `""` initially, so its a `String` unless it is
  reassigned somewhere.
- **Distinguish and Separate Out Clear and Opaque Code:** Variable assignment and
  the return value are clear. While it may be obvious to some what the loop is
  doing in this method, there are some parts that may not be immediately
  clear. We can treat the loop as opaque.
- **Walk Through The Code:** Again we hit opaque code pretty quick with the
  loop. Repeating our process once again:

```ruby
loop do
  puts "Guess a number between 1 and 10"
  guess = gets.chomp.to_i
  break if valid_guess? guess
  puts "Invalid entry"
end
```

- **Identify Inputs and Outputs:** While it isn't _exactly_ the same
  as a method input, the `guess` variable comes from outside the loop and is
  getting reassigned here so we can consider it a sort of input. Output isn't
  clear here.
- **Distinguish and Separate Out Clear and Opaque Code:** We can see two
  lines of `puts` statements that are simply printing out messages.
  `guess = gets.chomp.to_i` is taking a command line input (`gets`), removing
  unnecessary [record separator characters] and converting the response to an
  integer.

The only line that is clearly opaque is `break if valid_guess?(guess)`

- **Walk Through The Code:** The user is prompted by the `puts` to guess a
  number between 1 and 10. The user's guess is stored in `guess`. Now we've
  reached our opaque code. The `valid_guess?` method happens to be one line:

```ruby
def valid_guess?(guess)
  (1..10).include? guess
end
```

We can quickly see that the input is our guess, the output is the result of the
expression `(1..10).include? guess`, which is a `Boolean`. This code is clear:
does the range 1 through 10 include the guess, true of false?

### 6. Continue Through All Opaque Code Until The End

Stepping back into our loop:

```ruby
loop do
  puts "Guess a number between 1 and 10"
  guess = gets.chomp.to_i
  break if valid_guess? guess
  puts "Invalid entry"
end
```

We can see that all of this code is now clear and we can walk through the entire
loop: the user is prompted to guess a number, their entry is chomped, converted
to an integer and stored as `guess`. If `valid_guess?(guess)` returns `true`,
break the loop. Otherwise, display a message, 'Invalid entry' and loop back to
the top. Clear!

Now that this loop is understood, we can step back again to the `make_guess`
method:

```ruby
def make_guess
  guess = ""
  loop do
    puts "Guess a number between 1 and 10"
    guess = gets.chomp.to_i
    break if valid_guess? guess
    puts "Invalid entry"
  end
  guess
end
```

Walking through this method, we can see a guess variable is declared and our
loop is initiated and continues until a valid guess is provided. Once the loop
breaks, the guess is returned.

We can now revisit our first method and move one line of code from opaque to clear:

```ruby
def start_new_game(player = "Player")
  answer = rand(10)+1 # variable assignment using a random number
  guess = make_guess # NEW! this prompts the user to enter a guess and returns the guess if valid

  results = calculate_results(guess, answer) # ??? maybe I could guess what this does
  print_results(player, results, answer) # ??? probably displays something from its arguments

  results # return variable
end
```

We can now continue to walk through our code to the next opaque line and apply

```ruby
results = calculate_results(guess, answer)
```

Stepping into the `calculate_results` method...

```ruby
def calculate_results(guess, answer)
  guess == answer
end
```

...we can see that this method takes in our guess and the randomly generated
answer, and returns a comparison, which will return `true` or `false`. We now
know that the `results` variable is a `Boolean`. Back to our starter code:

```ruby
def start_new_game(player = "Player")
  answer = rand(10)+1 # variable assignment using a random number
  guess = make_guess # this prompts the user to enter a guess and returns the guess if valid
  results = calculate_results(guess, answer) # NEW! returns true if guess equals answer

  print_results(player, results, answer) # ??? probably displays something from its arguments

  results # return variable
end
```

One last line of opaque code in `start_new_game` leads us to our final method:

```ruby
def print_results(player, results, answer)
  if results
    puts player + " guessed correctly!"
  else
    puts player + " guessed incorrectly! The answer was " + answer
  end
end
```

This method takes in our `player` variable, the `results` that was just assigned,
and the randomly generated `answer`. There is no output here, but all the code
is clear: this method will output one of two messages depending on the value
of `results`.

Finally, we can go back to our starting method and see that the entire method
is clear:

```ruby
def start_new_game(player = "Player")
  answer = rand(10)+1 # variable assignment using a random number
  guess = make_guess # this prompts the user to enter a guess and returns the guess if valid
  results = calculate_results(guess, answer) # returns true if guess equals answer
  print_results(player, results, answer) # NEW! prints out message based on result
  results # return variable
end
```

## Conclusion

This method may not be necessary in all situations. Even this example may
have been mostly understood at a glance. However, walking through through the
code in this way ensures you have a complete picture in your mind - a clear
enough understanding that you could now modify any of the example methods and
have a good idea of how it will affect the entire set.

As you develop as a programmer, much of this process will become automatic. At
the same time, as your code vocabulary expands, more and more code will be
immediately clear. However, getting to that point is a challenge and having a
reliable process is important for getting yourself unstuck.

[record separator characters]: https://ruby-doc.org/core-2.2.0/String.html#method-i-chomp
