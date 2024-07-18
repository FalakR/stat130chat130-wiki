**TUT/HW Topics**

1. first data types... [`tuple`](02-Coding#Types-II), [`list`](02-Coding#Types-II), [`dict`](02-Coding#Types-II)
2. another key data type... [`np.array`](02-Coding#np.array) [and `np.random.choice`]
3. loops... [`for i in range(n)`](02-Coding#for-loops) and [`print()`](02-Coding#for-loops)
4. logical flow control... with [`if`](02-Coding#Logical-Flow-Control)/[`else`](02-Coding#Logical-Flow-Control)
    1. [`try-except` blocks](02-Coding#Logical-Flow-Control)

**LEC Extensions**

1. 
    1. `type()` not "types of data" like quantitative or qualitative
    2. `str` (and `sentence.split()`) as opposed to `int` versus `float` versus `bool`
    3. operator overloading polymorphism with `+` and `.sum()`
2. `from scipy import stats`, `stats.multinomial`, and probability [and `np.random.choice`]
    1. conditional probability Pr(Y=y|X=x)
3.
    1. [`for x in lst`](02-Coding#More-Loops) and [`for word in sentence.split()`](02-Coding#More-Loops) and 
        1. ~text manipulation with `.apply(lambda x: ...)`, `.replace()`, `re`~
    2. more loops... such as [`for i,x in enumerate(a_list)`](02-Coding#More-Loops)
        1. ~`for key,val in dictionary.items()` and `dictionary.keys()` and `dictionary.values()`~
4. [`elif`](02-Coding#Logical-Flow-Control)


# TUT/HW Topics

## Types II

> 1. first data types... [`tuple`](02-Coding#Types-II), [`list`](02-Coding#Types-II), [`dict`](02-Coding#Types-II)

A `tuple` is an object containing an "sequential collection" of "things" that is created and represented with round brackets.
Tuples are **immutable**, which means that after a tuple is created you cannot change, add, or remove items; so, tuples's are ideal for representing data that shouldn’t really change, like birthday dates or geolocation coordinates of natural landmarks.

```python
example_tuple = (1, 'apple', 3.14, 1) # tuples can contain duplicate elements
example_tuple
```

A `list` is another kind of object containing a "sequential collection" of "things" that is created and represented with square brackets.
Unlike tuples, lists are **mutable**, which means they can be altered after their creation. If you don't want to recreate a tuple from scratch each time you need to change your collection of things, then you should use a list!

```python
example_list = [1, 'banana', 7.77, 1] # lists can also contain duplicate elements like tuples
example_list.append('new item') # here we add a new element onto the list
# to do the same thing with a tuple you'd have to completely create a completely new tuple 
# as `example_tuple_update = (1, 'banana', 7.77, 1, 'new item')`
example_list
```

A `dict` ("dictionary") is an object that uses a "key-value" pairs "look up structure" of "things" instead of a "sequential collection" organization that are created and represented using "key-value" pairs inside of curly braces (as demonstrated below).
Dictionaries are **mutable** like lists, but each "key" of a dictionary is **unique** (so it uniquely references its corresponding "value"). Since a `dict` is based on a "look up" system, it is a so-called **unordered** object. This means that unlike tuples and lists which always remember their sequential order, dictionaries do not maintain their order of insertion and can change the order of items when the dictionary is modified.

```python
example_dict = {'id': 1, 'name': 'orange', 'price': 5.99} # There cannot be duplicate "keys" but there could be duplicate "values"
example_dict['quantity'] = 10
```

## np.arary

> 2. another key data type... [`np.array`](02-Coding#np.array)

NumPy is a `Python` library that contains the most efficient versions of standard numerical routines.
For example, a NumPy `np.array` is preferred over a list for its speed and functionality in numerical tasks.
The NumPy library is imported and a `np.array` is created from a list object as follows.

```python
import numpy as np
example_array = np.array([1, 2, 3])
```

An example numerical task that can be done with NumPy is to select a random value from an `np.array` object as follows.

```python
random_element = np.random.choice(example_array)
random_element
```

## for loops

> 3. loops... [`for i in range(n)`](02-Coding#for-loops) and [`print()`](02-Coding#for-loops)

The `range(n)` function in `Python` **generates** numbers from `0` to `n-1`.
> If you try to run the `range(n)` function to produce these values, it won't do anything because it is a so-called **generator** which means it will only produce the actual values within the context of a looping structure which sequentially requests the values.  This is actually clever because it means the actual numbers themselves don't actually have to be stored in memory anywhere, and can instead just be sequentially produced one at a time as needed. 

The `print()` function outputs a displays of its object argument. Since (as discussed above) `range(5)` is a so-called **generator**, if you run the code `print(range(5))`, you will get the following output. 

```python
range(0, 5) # output from running `print(range(5))`
```

The `for i in range(n):` template is the coding construct that is used used to specify the repetition of a block of code `n` times.

> The block of code that the `for` loop repeats will be executed "silently"; so, if you want to display anything inside of a `for` loop you need to explicitly use the `print()` function in the body of your `for` loop as demonstrated below.

```python
for i in range(5): # "iterates" `i` over the values 0, 1, 2, 3, 4
    # the "body" of a `for` loop --
    # the "indented code block" below the `for` statement syntax
    print(i) # is executed sequentially for each value of the `i` "iterator"
```

> `Python` code WILL NOT WORK unless properly indented... this is an interesting "feature" of `Python` coding that helps to make code for readable!

Here's a step by step break down of what the `for` loop code above is doing.

1. Initialization: the `for` loop starts with the keyword `for`
2. Iterator Variable: `i` is the "iterator" variable that will sequentially change with each "iteration" of the `for` loop
3. The `range()` function: `range(5)` will "iteratively" generate the sequence of numbers from `0` to `4` (since `Python` uses "0-indexing"), and these will be sequentially assigned to "iterator" `u` 
4. Loop Body: the code block indented under the `for` loop defines what happens during each "iteration" (in this case, the sequentially assigned values of `i` will be printed)
5. Iteration Process:
    1. In the first iteration, `i` is set to `0`, the first number produced by the `range` generator.
    2. The `print(i)` statement is executed, printing `0` to the screen.
    3. The `for` loop now iterates by setting `i` is set to the next number produced by the `range` generator, which is `1`.
    4. `print(i)` is executed again, this time printing `1`.
    5. This process repeats until `i` has "iterated" through all of the values produced by the `range` generator.
6. Termination: once `i` has reached `4` there are no more `i` "iterations", the loop ends, and the program continues with any code following the loop.

## Logical Flow Control

> 4. logical flow control... with [`if`](02-Coding#Logical-Flow-Control)/[`else`](02-Coding#Logical-Flow-Control)

FizzBuzz is a classic programming challenge that’s often used to teach basic programming concepts like loops and conditionals. 
In the FizzBuzz problem, you loop through a range of numbers, and then do the follow for each number.
1. If the number is divisible by 3, you print “Fizz”.
2. If the number is divisible by 5, you print “Buzz”.
3. If the number is divisible by both 3 and 5, you print “FizzBuzz”.
4. If the number is not divisible by 3 or 5, you print the number itself.

Here’s an example FizzBuzz program that illustrates the use of `if` and `else` conditional statements for logical flow control with comments explaining each step.

```python
for i in range(1, 101):  # Loop from 1 to 100

    if i % 3 == 0 and i % 5 == 0:  # Check if divisible by both 3 and 5
        print("FizzBuzz")
    elif i % 3 == 0:  # Check if divisible by 3
        print("Fizz")
    elif i % 5 == 0:  # Check if divisible by 5
        print("Buzz")
    else:  # If not divisible by 3 or 5
        print(i)
```

1. The `for` loop sets up the iteration (from `1` to `100` in this example).
2. The first `if` statement checks for the first condition (divisible by both 3 and 5).
    1. the **modulus** operation `%` returns the remainder of "i divided by 3"; so, it's just an operation like `i+3`, `i-3`, or `i/3`; but, if the remainder is `0` then it means that "i divides by 3 perfectly"
    2. The `and` construction in `i % 3 == 0 and i % 5 == 0` means that both `i % 3 == 0` AND `i % 5 == 0` must be `True` for the `if` statement to succeed
    3. If the `if` statement succeeds (because its "argument is `True`) then the "body" (the indented code block beneath the `if` statement) of the `if` statement is executed
3. The next two `elif` ("else if") statements each subsequently sequentially check if their respective conditions (divisible by 3, and then 5) are true statements, and execute their code block "bodies" if so
    1. Using `elif` instead of `if` "connects" the logical statements into a single logical control flow sequence whose conditions can be understood in an "else if" manner that can help improve the clarity of the checks. 
4. The `else` statement covers the case where none of the above conditions are `True` and concludes the logical control flow sequence.
5. The `print()` function outputs the result based on the condition that’s met.

This structure allows the program to make decisions at each iteration, using logical flow control structures within a `for` loop to print out a mix of numbers and the words “Fizz”, “Buzz”, and “FizzBuzz” based on the divisibility of each number. 

**`try-except` blocks**

A similar logical flow control structure to `if`/`else` statements is the `try-except` block. Rather than checking for `True` conditions however, `try-except` blocks check for the presence of "run time errors" which are stored as a kind of `Python` object known as an `Exception`. In the code below, `Python` tries to run "one"/"two" but doesn't know how to do this; but, the `except Exception as e` construct allows the nature of the error to be captured as an `Exception` object and named `e` which can then be printed out and examined without cause the code to fail (as it would if you tried to run `"one"/"two"` without wrapping a `try-except` block around it). 

```python
try:
    "one"/"two"
except Exception as e:
    print(f"An error occurred: {e}")
```


# LEC Extensions

## More Loops 

> 3.
>     1. [`for x in lst`](02-Coding#More-Loops) and [`for word in sentence.split()`](02-Coding#More-Loops) and 
>         1. ~text manipulation with `.apply(lambda x: ...)`, `.replace()`, `re`~
>     2. more loops... such as [`for i,x in enumerate(a_list)`](02-Coding#More-Loops)
>         1. ~`for key,val in dictionary.items()` and `dictionary.keys()` and `dictionary.values()`~

It is sometimes useful to iterate through a custom list rather than a `range(n)` generator. 
Below, instead of "iterator" `i`, we denote the "iterator" as `x` to emphasize that it's not a "numerical index iterator".
> This is not strictly necessary, since you can name your "iterator" variable whatever you want to and then access it as such in the body of the for loop.

```python
a_list = ['apple', 'banana', 'cherry'] # or, equivalently: `a_list = "apple banana cherry".split()`
for x in a_list: # note that we don't have to use the `range()` function here!
    print(x)     # we can just "iterate" through the "iterable" list `a_list`!
```

It is additionally sometimes useful to both iterate through a custom list but also still have a "numerical index iterator" as well.
This is done with by wrapping the `enumerate()` function around a list (or tuple) object.

```python
for i,x in enumerate(a_list):
    print(f"Index: {i}") # this useful syntax pastes `i` into the displayed string
    print(f"Value: {x}") # this useful syntax pastes `x` into the displayed string
    print("Iteration Completed")
```

The `enumerate(a_list)` "numerical index iterator" (`i`) to the "iterable" list (`a_list`) and returns it as an enumerate object which the `for` loop understands and unpacks into `i` and `x` at each "iteration" as indicated by the `i,x` syntax. 

One more `for` loop structure that can sometimes be useful is "iterating" through dictionaries based on the `.items()`, `.keys()`, or `.values()` methods of a dictionary.

```python
my_dict = {'a': 1, 'b': 2, 'c': 3}
for key in my_dict.keys():
    print(key)

for key, value in my_dict.items():
    print(f"Key: {key}, Value: {value}")

for value in my_dict.values():
    print(value)
```