# Structure

OpShin code is an imperative programming language.
As such, code is written by instructing the computer to perform
computations.
Each line of code corresponds to one instruction.
A simple instruction is the assignment.

```python
# Assign value 2 to variable named a
a = 2
```

If we want to not only store but also read the value,
we can print it like this.

```python
# Assign value 2 to variable named a
a = 2
# print the content of variable a
print(a)
```

When executed locally, this will print `2` to the command line.
When executed on chain, this code will append the string `"2"` to the 
on-chain debug logs - you can use this to inspect how your code is working!

Can you guess what the following program will print?

```python
a = 2
b = 4
print(a + b)
```

### Types

Every variable in OpShin has a type.
In general we do not need to tell OpShin in advance what type
variables has - it is able to derive the type on its own.
In the above example, variable `a` has type `int`.
This is the type for whole (integer) numbers.
There are a few more types that are introduced in the following sections.

Note that types are important in OpShin.
If types do not match at compilation time, the contract can not be compiled.
This is to avoid unnecessary errors and prevent bugs in the code.
For example the following is not allowed, since it is a non-sensical instruction:

```python
a = 2 + "hello"
```

The left part of this addition has type `int` while the right part has type `str` (the text/"String" type).
An addition between them is not unambiguous and generally does not make too much sense.
Hence, when trying to compile this statement, you will face a compiler error.

You may see these errors a lot (but hopefully not too often).
They are telling you in advance that an operation will not work as expected.
Don't fret! Take these error messages as guidance on what works and what not and adjust
your program as required by the compiler to obtain valid code.

> It is not possible to change the type of your variable later.
> The following program is forbidden
>
> ```python
> a = 10
> a = "hello"
> ```
> 
> Instead, we recommend simply changing the variable name if you want to change the type.
> This will also be less confusing to the reader!

### Control flow

In OpShin, control flow can be introduced using statements like `if`.

```python
if a == 2:
    print("a is equal to 2!")
    a = 10
print("hello!")
```

In this case, the expression `a == 2` has a boolean type.
It can evaluate to either `True` (a is indeed equal to 2) or `False`.
If it is `True`, then the indented part is executed, so the program will print `a is equal to 2!`
and afterwards assign the value `10` to `a`.

The unindented part will always be executed. Generally, indentation indicates that code
belongs to a different "layer" of the program, i.e. an if/else statement, a loop, a function or a class.