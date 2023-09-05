# The fundamentals of Ruby

The fundamental syntax of Ruby looks like this:

```
2.add(3)
```

- The thing on the left side of the dot (`.`), in this example `2`, is called the **object**.
- The thing on the right side of the dot, in this example `.add(3)`, is called the **method**.
- Within the method, there are two parts:
	- `add` is called the **name** of the method.
	- `(3)` is called the **argument** to the method.

All together, the object, method, and argument is called an **expression**.

<aside markdown="1">
The `add` method increases the number that it was called upon (the number on the left side of the dot) by the number provided as its argument (the number within the parentheses), and returns the result.
</aside>

When **evaluated**, the method **returns** another object. The **expression** is then replaced by, or "substituted with", the return value. So the above expression boils down to:

```
5
```

---

Here's another example:

```
5.multiply(2)
```

<aside markdown="1">
The `multiply` method takes the number it was called upon, multiplies it by the number provided as its argument, and returns the result.
</aside>

- What would you guess the above expression evaluates to?
- 10
	- That's right!
- any
	- Not quite. Hint: The expression is equivalent to `5 × 2`.
{: .free_text_number #five_times_two title="Five times two" points="1" answer="1" }

---

Let's look at another program, this time containing multiple expressions within the same line:

```
3.add(4.multiply(2))
```

When evaluating this line, Ruby starts on the left and then proceeds to the right, searching for the first _complete_, _self-contained_ expression.

`3.add(4` is not complete and self-contained, so Ruby keeps going. The first complete, self-contained expression it finds is `4.multiply(2)`, which boils down to `8`. So the line becomes:

```
3.add(8)
```

Ruby proceeds evaluating methods and substituting with return values, and we're left with:

```
11
```

And now Ruby's done with the line, since there are no methods left.

---

Here's another example:

```
4.add(6.divided_by(2))
```

<aside markdown="1">
The `divided_by` method takes the number it was called upon, divides it by the number provided as its argument, and returns the result.
</aside>

- What would you guess the above expression evaluates to?
- 7
	- That's right!
- 5
	- This is a tempting response, but no. If Ruby just went from left to right, then you might think the expression was equivalent to `(4 + 6) ÷ 2`, in which case you would be correct and it would evaluate to `5`.

    But Ruby doesn't just go from left to right; it looks for the first _complete_, _self-contained expression_. `4.add(6` is not complete and self-contained. So Ruby keeps going, and finds `6.divided_by(2)`, which is complete and self-contained. So that is the first expression to be evaluated and substituted.
		
    So the expression is actually equivalent to `4 + (6 ÷ 2)`. Give it another try!
- any
	- Not quite. Hint: The expression is equivalent to `4 + (6 ÷ 2)`.
{: .free_text_number #order_of_operations_1 title="Order of operations 1" points="1" answer="1" }

---

Okay, enough reading. It's time for you to write and run some Ruby programs yourself! Try running this runnable code block:

```ruby
class Integer
	alias_method :add, :+
	alias_method :subtract, :-
	alias_method :multiply, :*
	alias_method :divided_by, :/
end

2.add(3)
```
{: .repl #without_pp title="without pp" points="1" setup_code="1-7" }

Wait a second — why didn't a `5` appear in the terminal?

It turns out that Ruby doesn't automatically display the result of each and every line — we have to explicitly command it to print the lines that we want to see. Try this instead:

```ruby
class Integer
	alias_method :add, :+
	alias_method :subtract, :-
	alias_method :multiply, :*
	alias_method :divided_by, :/
end

self.pp(2.add(3))
```
{: .repl #with_self_pp title="with self.pp" points="1" setup_code="1-7" }

Now when you run the program, you should see some output in the terminal! But what is `self`, and what is `pp`?

- `self` is a special object. The short version: as the program is being executed, `self` represents different objects at different times — whichever object Ruby is currently operating within.

    In this example, `self` represents the program itself, also known as **the main object**. If this is confusing, then that's good; it shouldn't make sense yet. We'll talk a _lot_ more about `self` later; you'll have to trust me for now.
- `pp` (pronounced "pretty print") is a method that we are allowed to execute, or "call", on the main object. `pp` takes its argument and displays it, or "prints" it, in the terminal.

Great! Now we can see the results of our expressions.

---

So far, we've only been looking at one-line programs. But programs can have many lines — sometimes hundreds, or even thousands! For example:

```ruby
class Integer
	alias_method :add, :+
	alias_method :subtract, :-
	alias_method :multiply, :*
	alias_method :divided_by, :/
end

self.pp(2.add(3))
self.pp(5.multiply(2))
self.pp(4.add(6.divided_by(2)))
```
{: .repl #multiline_self_pp title="multiline self.pp" points="1" setup_code="1-7" }

But does this mean that every time we want to see the output of a line, we're going to have to wrap the whole line within `self.pp(  )`? Hundreds, or thousands of times? Ugh, what a pain!

Fortunately, Ruby includes many handy shortcuts, known as "syntactic sugar", to make writing and reading Ruby more enjoyable. Here are our first two:

#### `self` as the implicit receiver

If you're calling a method on `self`, you can leave out the `self` and the `.`; Ruby will figure out what you mean. So:

```
self.pp(4.add(6.divided_by(2)))
```

Can also be written as:

```
pp(4.add(6.divided_by(2)))
```

You can _only_ use this shortcut when calling methods on `self`! For example, it wouldn't make sense to, rather than `2.add(3)`, write just `add(3)` by itself.

`self` is known as the "implicit receiver" of all method calls that are not explicitly attached to an object with a `.`.

#### Dropping the parentheses around arguments

You are allowed to leave out the parentheses around arguments. So:

```
pp(4.add(6.divided_by(2)))
```

Can also be written as:

```
pp 4.add(6.divided_by(2))
```

You could also keep going with this shortcut and write the above as:

```
pp 4.add 6.divided_by 2
```

But I recommend against this. If you drop too many parentheses, it quickly becomes confusing which arguments belong to which methods, which expressions are complete and self-contained, and therefore which expressions should be evaluated first.

Look at the above example. Should:

```
4.add 6.divided_by 2
```

Be evaluated as:

```
(4 + 6) ÷ 2
```

And return `5`? Or should it be evaluated as:

```
4 + (6 ÷ 2)
``` 

And return `7`? The answer is `7`. It was hard enough to parse the expression _with_ parentheses; _without_ parentheses it's even harder.

Therefore, my advice: only drop the parentheses around arguments when calling the `pp` method, and that's it. For all other methods, I keep the parentheses.

---

Together, these two shortcuts are going to save us a _lot_ of typing. The above example:

```
self.pp(2.add(3))
self.pp(5.multiply(2))
self.pp(4.add(6.divided_by(2)))
```

Can instead be written as:

```
pp 2.add(3)
pp 5.multiply(2)
pp 4.add(6.divided_by(2))
```

This is good, because it makes it easier to print often — all you have to do to _see_ what a line of Ruby is doing is pop a `pp ` at the start of the line, and no other changes are required.

And we should be printing _all the time_. Remember our golden rule of programming: **Make the invisible visible.** Printing values is an enormously helpful tool for that.

---

We rushed to this point in order to gain the ability to write expressions and print them. Now, let's slow down and explore what we've done so far in more detail.


---

Let's examine the first example of a Ruby expression again:

```
2.add(3)
```

- The thing on the left side of the dot is the **object** (a.k.a. "receiver") and the thing on the right side of the dot is the **method** (a.k.a. "message").
- I think of objects as _nouns_, and methods as _verbs_. An object is some information that represents a thing, and methods are actions that the thing is able to perform.
- Together, the object, dot, and method are called one **expression**.
- The expression is **evaluated** — i.e. the method is executed on the object. The result is a new object, which replaces the original expression. This object is called the **return value** of the method.

## Ruby's vocabulary

Ruby can work with many more kinds of data than just numbers. Ruby has:

- Numbers
- Text
- Dates
- Times
- True/false
- Lists containing multiple pieces of data
- And a lot more

Each kind, or **class**, of data has its own set of **methods** that it can perform. For example:

- Numbers can do the usual things — add, subtract, multiply, etc.
- Two dates can tell you how far apart they are from one another.
- Lists can tell how you long they are, or sort themselves.

Ruby is known as a "batteries included" language because it comes with _so many_ methods out-of-the-box, saving the programmer the trouble of having to re-invent the wheel.

Finally, *we can even make up our own nouns and verbs* and add them to the language. For example, we can create a data type `Venue`, give it a method that calculates the average rating from its reviews, and then use that method whenever we want.

One of the best things about Ruby is its wonderful **open-source** community: programmers very often share these new classes that they write with one another, making the language ever easier to use and ever more powerful.

## Ruby's primary syntax

So, in terms of **data** and **methods**, Ruby comes with a powerful set out-of-the-box *and* is always getting better. That's good news! Here's even more good news: to access all of this power, the **primary syntax** is straightforward. It looks like this: `some_object.some_method`. For example:

```ruby
pp "Hello, world!".upcase
```
{: .repl #hello_world_upcase title="Hello, world! Upcase" points="1"}

If all went well, you should have seen `"HELLO, WORLD!"` output by the command. Yay! What just happened?

<aside markdown="1">
It is [a time-honored tradition](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) that the very first thing a programmer does in a new language is print out "Hello, World!" Congratulations — you're now one of us 🙌🏾
</aside>

The primary way to write an expression in Ruby is: `object.method`. We ask the _thing_, or noun, on the left side of the dot to perform the _action_, or the verb, on the right side of the dot.

The computer then evaluates that expression and **returns** a new piece of data in its place (just like with the calculator).

In this case, we asked `"Hello, world!"`, which is a string (Ruby's name for a piece of text), to `upcase` itself, which it (very) happily does, and we're left with `"HELLO, WORLD!"` at the end of the day.

<aside markdown="1">
The name "string" is used in pretty much every programming language for the datatype that holds a piece of text, and refers to a string of _characters_; a holdover from back when we used to have to worry about conserving the computer's physical storage space and had a separate datatype for an individual character. Now we usually don't have to worry about storage space anymore, but the name "string" stuck with us.
</aside>


- From our first program, in the framework of nouns, verbs, and grammar...
- `"Hello, world!"` is the grammar, `upcase` is the noun, and `.` is the verb
    - Not quite, try looking through the previous sections again.
- `"Hello, world!"` is the verb, `upcase` is the grammar, and `.` is the noun
    - Not quite, try looking through the previous sections again.
- `"Hello, world!"` is the noun, `upcase` is the verb, and `noun.verb` is the grammar
    - Yes! And remember noun = data; verb = method; and grammar = syntax.
- There is only a double verb syntax, with no data in our program.
    - Not quite, try looking through the previous sections again.
{: .choose_best #hello_world_grammar title="Hello, world! Grammar" points="1" answer="3" }

## Every class has different methods

Different **classes** (`String` being one) can perform different **methods**. Here are a few expressions to try out. Type each one into the editor below, then press Run to see what it returns.

<aside markdown="1">
Almost everything in Ruby is an **object**. That's the idea behind [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming). Text like `"Hello, world!"` is an object — specifically a **`String` class object**.
</aside>

```ruby
pp 7.odd?
pp 7.even?
pp "Raghu Betina".reverse
pp "Your Name".swapcase
pp "Mississippi".length
```

```ruby
# put the code here
```
{: .repl #class_experiment title="Experiment with classes"}

After you experiment with the different inputs, write a program that prints how many characters are in the word "Mississippi". Press Run below the editor to see your output. When you think you've got it, press Run next to the test "How long is Mississippi?"

```ruby
describe "Experiment with classes" do
  it "should print the length of Mississippi" do
    path = "/tmp/code.rb"

    File.foreach(path) do |line|
      if line.include?("11")
        expect(line).to_not match(/11/),
          "Expected code block to NOT print the String literal '11', but did."
      end
    end

    expect { require_relative(path) }.to output(/11/).to_stdout
  end
end
```
{: .repl-test #class_experiment_test_1 for="class_experiment" title="Experiment with classes should print the length of Mississippi" points="1"}

---

What do you expect will happen if we ask `"Mississippi"` if it is `even?` (with `pp "Mississippi".even?`); try it in the editor to find out:

```ruby
# put the code here
```
{: .repl #first_error_repl title="Is Mississippi even?" points="1"}

- `"Mississippi".even?` returned...
- true
    - Really? Did you try it?
- false
    - Really? Did you try it?
- nothing
    - Really? Did you try it?
- a long message
    - Sweet, you got your first error message!
{: .choose_best #first_error title="First error message" points="1" answer="4" }

## Error Messages

### _Do_, or do not. There is no  _read_. 

Before we forced you to by asking a question, were you typing out every expression that you were reading about into an editor to experiment with it?

If not, then you're going to struggle. If you're just _reading_, you won't be successful at learning programming; you have to _do_ in order to build up some muscle memory and intuition. _Experimentation and practice are both crucial._ 

And don't just copy-paste; you won't retain anything. Type it out.

In fact, not only should you be typing the things I ask you to type, but you should also be trying out random other things that occur to you. (E.g. "What if I tried `"Mississippi".length.even?`")

_Experiment!_

### Read. The. Error. Message. (A.k.a., "RTEM") 

Aha! If you were typing out every expression and running it, then

```ruby
pp "Mississippi".even?
```

should have produced your very first error message! 🎉

Error messages can look scary, but the **most important skill** you must develop when learning to program **is to not panic** when you see them. Slow down, **read the error message**, and see if you can make any sense of it at all. Over time, you will find that they are _very_ helpful (and you will miss them if something goes wrong silently).

So, can you glean any information from:

```bash
(repl):2:in `<main>': undefined method `even?' for "Mississippi":String (NoMethodError)
```

In this case, it is saying: "Hey, friend — on line #2 of the program, you tried calling a method called `even?` on the object `"Mississippi"`, which is a `String`. Sorry, but `String`s don't have a method named `even?`." Fair enough, that makes sense.

### The bottom line 

The bottom line is — at all times as you are writing Ruby, you should be thinking: "What **class** is this object? What **methods** does _this_ class have available?" Then, the syntax itself is simple — `my_object.cool_method`.

## Arguments are inputs

Alright, so the **primary syntax** in Ruby is straightforward — `object.method`. However, there's a wrinkle: some methods require additional inputs. For example, there is a method called `gsub` which we can call on a `String`, which will substitute characters with other characters. Try it:

```ruby
pp "Java is a joy".gsub("Java", "Ruby")
```
{: .repl #gsub_arguments title="Experiment with gsub" points="1"}

`gsub` is short for "globally substitute", because it will replace _all_ occurrences of one _substring_ with another _substring_.

In order to do its job, the `gsub` method needs to know what substring to get rid of and what to replace it with. So we give it inputs, or **arguments**, which must come in parentheses _immediately_ following the method. If the method takes multiple arguments, as `gsub` does, then they are separated by commas.

Here's another graded code block. Modify the code to print `"Hello, world?"` to get the test to pass:

```ruby
pp "Hello, world!".gsub()
```
{: .repl #gsub_arguments_tested title="Hello, world?"}

```ruby
describe "Hello, world?" do
  it "should print 'Hello, world?'" do
    path = "/tmp/code.rb"
    expect { require_relative(path) }.to output(/Hello, world?/).to_stdout
  end
end
```
{: .repl-test #gsub_arguments_tested_test_1 for="gsub_arguments_tested" title="Hello, world? should print 'Hello, world?'" points="1"}

What is the purpose of `gsub`'s first argument, and what is the purpose of the second argument?

In reality, `gsub` is more often used to do things like removing illegal characters from usernames before saving, e.g.:

```ruby
pp "Raghu@Bet@ina".gsub("@", "")
```
{: .repl #gsub_remove_illegal title="Experiment with gsub: illegal characters" points="1"}

(`""` is an empty string, so all `@`s will be replaced with nothing, i.e. removed.)

## One of the only times when whitespace matters

Unlike some other languages (e.g. Python) where indentation and spacing can change the entire meaning of a program, Ruby is, generally, very permissive about how you use whitespace. You can _usually_ use spacing according to your own taste, and Ruby will be able to make sense of your code.

So, for example, whether you have spaces between arguments doesn't matter; these two are equivalent:

```ruby
"Raghu@Bet@ina".gsub("@",              "")
"Raghu@Bet@ina".gsub(          "@","")
```

(The most common _style_ is to have one space after each comma, and that's it.)

However, one situation in which whitespace _does_ matter has to do with the **parentheses** around arguments:

```ruby
"Raghu@Bet@ina".gsub("@", "") # good
"Raghu@Bet@ina".gsub ("@", "") # bad!
```

Can you spot the difference? **Don't put a space between the method and the opening parenthesis.**

It's a very easy mistake to make, so I just wanted to warn you early on so that you can begin developing good muscle memory. Try the bad version in the runnable code block and see what the error message looks like:

```ruby
pp "Raghu@Bet@ina".gsub ("@", "") # bad!
```
{: .repl #bad_whitespace title="Bad whitespace" points="1"}

## Seriously: please read the error message

Programming boils down to:

 1. Forming a plan of what you want to do (e.g. "I want to remove any `@`s in this input.")
 1. Typing some code to try and do it.
 1. It _never_ works the first time.
 1. Seeing an error message.
 1. Learning how to deal with that particular error message.
 1. The next time you encounter that error message, it will be vaguely familiar and it will take slightly less time to debug it.
 1. After you've encountered that error message for the 25th time, you will debug it instantly.

Your skill level as a programmer is essentially **the number of error messages that you have encountered in the past and now recognize**. So start paying attention to them now — we want to collect 'em all!

- What do you call the things we put inside of the parentheses after a method?
- arguments
    - Yes! 
- any
    - Try again!
{: .free_text #arguments title="Arguments in parentheses" points="1" answer="1" }

## An aside: Code comments

Here's a debate that will rage until the end of time: what do you call this symbol?

```
                 #
```

Is it a number sign? Is it a pound symbol? Is it a hashtag? Is it a waffle? Is it an [octothorpe](https://en.wiktionary.org/wiki/octothorpe)?

In this text, I'm going to refer to it as the "pound symbol".

The pound symbol is used quite a bit in Ruby. You can see one important way in the example above, where I said `# bad!` after some offending code. That is known as a "code comment". The Ruby interpreter, when it sees the `#`, will ignore it and everything that comes after it; allowing us to leave notes to ourselves and to each other. **Use comments liberally.**

<aside markdown="1">
Every programming language has its own syntax for leaving comments in the code.

Comments in Ruby are done with one `#` at the start of the line.

In HTML files, comments are wrapped within:

`<!-- comment here -->`

In CSS, comments are wrapped within:

`/* comment here */` 

Luckily in most text editors, the keyboard shortcut <kbd>Ctrl</kbd>+<kbd>/</kbd> (Windows) or <kbd>Cmd</kbd>+<kbd>/</kbd> (Mac) will automatically detect the language you're writing and comment the line for you.
</aside>

Another nice trick is: when experimenting with some code and it's not working, comment it out and try a different approach on the next line. That way you can keep the old code around for reference without having to delete it, but it won't break the program.

The below program doesn't work, because of the bad-whitespace issue. But I want to keep it around while I try to fix the issue. Run the code and make the test below it pass:

```ruby
pp "Raghu@Bet@ina".gsub ("@", "")

# The code above was throwing an error message,
# but I want to save it for reference as I try
# to debug, so I'll put a #-symbol before it. 
# Now, let me see if this thing below fixes it
# when I remove the leading #-symbol!

# pp "Raghu@Bet@ina".gsub("@", "")
```
{: .repl #code_comments title="Code comments"}

```ruby
describe "Code comments" do
  it "should print 'RaghuBetina'" do
    path = "/tmp/code.rb"
    expect { require_relative(path) }.to output(/RaghuBetina/).to_stdout
  end
end
```
{: .repl-test #code_comments_test_1 for="code_comments" title="Code comments should print 'RaghuBetina'" points="1"}

## Variables are boxes

Now that you've seen objects, methods, and arguments, you know all there is to know about crafting _expressions_ in Ruby. No kidding: `object.method(arguments)` is the *vast* majority of what we'll be doing. That's it.

However, so far we haven't been doing much with the **return value** of each expression. We've just been reading it off the screen, and then dropping it on the ground. For example, try the following (uncomment each line one at a time by removing the `#`, and re-"Run" each time):

```ruby
# pp "hello world!".upcase
# pp "hello world!".reverse
```
{: .repl #upcase_reverse_part1 title="Upcase and reverse part 1" points="1"}

We're not really able to make any forward progress when we only perform one operation at a time. Programs get interesting only when we start to take the return value of one expression and feed it into the _next_ method. That's how we craft our own novel, useful applications from the basic building blocks of Ruby.

So: let's start to store our return values for future reference, instead of dropping them on the ground. We do this using **variables**, or, as I like to think of them, _boxes_. Let's try it:

Run the following:

```ruby
upcase_string = "hello world!".upcase
pp upcase_string
```
{: .repl #upcase_reverse_part2 title="Upcase and reverse part 2" points="1"}

This creates a box, labels it `upcase_string`, and stores the string `"HELLO WORLD!"` in it. Then we print the contents of `upcase_string` with `pp upcase_string`.

### The variable assignment operator 

The single equals sign, `=`, is called the **variable assignment operator**.

When I read

```ruby
upcase_string = "hello world!".upcase
```

out loud, I say "the string hello world dot upcase _is assigned_ to the variable `upcase_string`".

**I read the right side first, because that's how Ruby reads it too**; it first evaluates the expression on the right side of the `=`, and _then_ it stores the resulting value in the variable on the left.

Now add the `reverse` method to the runnable code block on the `upcase_string` variable:

```ruby
upcase_string = "hello world!".upcase
pp upcase_string.reverse
```
{: .repl #upcase_reverse_part3 title="Upcase and reverse part 3" points="1"}

Great! Now we're making progress.

### Storing the next return value 

What would you expect to happen if you add a third line so that your program reads:

```ruby
upcase_string = "hello world!".upcase
pp upcase_string.reverse
pp upcase_string.gsub("L", "Z")
```
{: .repl #upcase_reverse_part4 title="Upcase and reverse part 4" points="1"}

What does `upcase_string` contain now? Try it, by adding `pp upcase_string` to the end of the program.

Did it match your expectations?

I was surprised when I first tried something like that. Most Ruby methods don't modify the object that they are called upon; they just return a **modified copy**. The original variable is untouched, so if we want to hold on to the new value then we better store that too. How about:

```ruby
# create a variable
upcase_string = "hello world!".upcase

# reverse it
upcase_string_reverse = upcase_string.reverse

# gsub it
upcase_string_reverse_gsub = upcase_string_reverse.gsub("L", "Z")

# print it!
pp upcase_string_reverse_gsub
```
{: .repl #upcase_reverse_part5 title="Upcase and reverse part 5" points="1"}

Fortunately, we can create as many variables as we want.

In the graded code block below, move line-by-line through the code and **debug it**, so that the terminal prints "raghubetina" for the test to pass:

```ruby
username = "Raghu@Bet@ina"

username_fixed = username.gsub ("@", "")

# add the method ".downcase" on the end of this line
username_fixed_downcased = username_fixed

pp username_fixed_downcased
```
{: .repl #debug_variables title="Debug it"}

```ruby
describe "debugging" do
  it "should print 'raghubetina'" do
    path = "/tmp/code.rb"
    expect { require_relative(path) }.to output(/raghubetina/).to_stdout
  end
end
```
{: .repl-test #debug_variables_test_1 for="debug_variables" title="Debug it should print 'raghubetina'" points="1"}

### Updating variables 

It can get old coming up with different variable names for every step of the program. Instead, we usually want to re-use existing variables. We can throw away what we have in the box and put in something entirely different with the same assignment operator, `=`. Try this:

```ruby
# put one thing in our "my_variable" box
my_variable = "hi"
pp my_variable

# replace it with something else
my_variable = 2.odd?
pp my_variable
```
{: .repl #updating_variables_pt1 title="Updating variables part 1" points="1"}

We can even replace the value in the box with an updated version of the _old_ value, because _the expression on the right side of the assignment operator is evaluated before the assignment takes place_. Try this:

```ruby
my_variable = "hello, world!"
pp my_variable

my_variable = my_variable.capitalize
pp my_variable

my_variable = my_variable.reverse
pp my_variable
```
{: .repl #updating_variables_pt2 title="Updating variables part 2" points="1"}

That may look strange — how can we use `my_variable` on the left side _and_ the right side of the `=`?

But it's because this is _not the equals sign from math class_; this is the **variable assignment operator**, and the right side is evaluated first until only a single value is left; and _then_ that object is assigned into the box on the left (replacing whatever was there before).

So you will very often see something like this:

```ruby
counter = counter + 1
```

When we're keeping track of e.g. how many times we've printed something out on the screen. We are taking the original value of `counter`, adding `1` to it, and then replacing the contents of `counter` with that new total.

### Variable syntax 

You may have noticed that the variable assignment syntax is a departure from the primary syntax of `object.method`. But we do it all day long, so we need to know it just as well. Our programs will end up looking like this (these are made-up method names, so this code won't work):

```ruby
storage_box_1 = "starting data".first_transformation
storage_box_2 = storage_box_1.second_step
storage_box_3 = storage_box_2.third_method.maybe_even("another", "one")
# etc for dozens or hundreds of lines
```

*First*, the expression on the right side of the assignment operator will be evaluated until there are no methods left and there's just a piece of data remaining.

*Then*, that value will be placed in the variable named on the left side of the assignment operator, which will be created if it doesn't exist, or will have its value replaced if it does exist.

Most programs are just a long succession of statements where we do some work with `object.method` and store the result in some variable, then we do some more work on that variable and store the result in yet another variable, and a hundred steps later we've produced our final result and we display that to our user.

- The `=` character in Ruby is...
- called the variable assignment operator.
    - Yes! 
- works the same as the classic `=` sign from math class.
    - Not quite.
- used to store objects (e.g., data) in variables.
    - Yes!
- can have the same variable on the left and right side.
    - Yes!
- can only have one operation on the right side.
    - Not quite.
{: .choose_all #assignment title="Assignment operator" points="3" answer="[1,3,4]" }

### Variable naming rules 

When you are choosing your variable names, there are some rules:

- Variable names can only contain **lowercase** letters (`a..z`), numbers (`0..9`), and underscores (`_`) — they can't contain spaces.
- Variable names cannot begin with a number.
- Rubyists strive to choose **descriptive** variable names, no matter how long they are, so that it's obvious to teammates what the contents are at a glance. 

_Please_ avoid naming your variables `x`, `y`, and `z`. Use underscores to separate words in multi-word variable names, since we can't use spaces.

## That's it

That's it for the fundamental grammar of Ruby!

```ruby
storage_box = noun.verb(input1, input2)
```

Of course, there's a bit more syntax (like how to define our own nouns and verbs) that we need to learn, but for the most part, `object.method` is the bulk of what we do.

Now we need to spend some time expanding our _vocabulary_ — what are the most commonly used data types in Ruby, and what are some of their methods? That's coming up next.

- Approximately how long (in minutes) did this lesson take you to complete?
{: .free_text_number #time_taken title="Time taken" points="1" answer="any" }

<span style="font-size: large">**When you are done here, close the window and return to Canvas for the next lesson in the series.**</span>

----
