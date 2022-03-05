# Programming languages Part B

## Intro

In a previous blog, I was talking about part A of a course called programming languages,
https://github.com/wajdiJomaa/BLOGS/blob/main/programming_languages.md, this course is about 
programming concepts and it's available for free on Coursera, the first part was about a 
statically typed functional language called SML, you can check the blog for part A. 

And now this blog I'll cover the main topics of part B, the language used in this part
is called racket, a functional dynamically typed language. There is a lot of topics and 
it's hard to talk about them all so I'll talk only about some topics.

## Small intro to racket

### Parentheses

Maybe the first thing you will notice about racket is "what the heck why all these parentheses",
racket uses a lot of parentheses but of course, they are not for decoration, parentheses have a 
special meaning, mainly they are function calls, for example calling the plus function the syntax is 
`(+ 1 2)` at first this is a weird syntax but by the time you'll enjoy closing a lot of parentheses.
Be careful `(1)` will lead to a nice error message because you are assuming that 1 is a function.

### List are Pairs??

The question is how many pieces of data we can fit in a pair? 
The answer is really simple of course not 2 it's infinity, as we know a pair is a set of 2 expressions
but what if the second one is another pair and this pair has another pair... 
By this idea we can define a list using multiple nested pairs for example :
`cons(1,cons(2,cons(3,null)))` this is a racket list notice that at the end there is a null, and cons
is just a piece of syntax to define a pair.

### Be careful when using mutation

You can use mutation in racket but by default, cons are not mutable, but you can use mcons
for mutable pairs, this course shows you that mutation should not be overused, for example 
you have an array A and B, by avoiding mutation you don't have to worry if A a copy of B or 
A has the same address as B because A and B won't change forever. 

## Programming concepts 

### A Function with no arguments - The power of A Thunk

What is a thunk? A thunk is a function with no arguments, that contain an expression e in its body 
`(lambda () e)` 

Why use a thunk? Imagine that there is an expression e, and we want to evaluate e only when it's necessary because evaluating e is expensive. As we know passing e to a function would evaluate e before
executing the body even it's not needed in the body, so what if we pass a thunk containing e instead of e, and then call the thunk when needed. for example:
function :
`(define example (e) (if #t) 1 (e)`
if we pass an expensive expression to this function as a thunk it will never evaluate because 
we don't need it (the expression is in an else branch and the condition is true), so by using a thunk
we don't waste time.

### Lazy evaluation - Delay and Force

We said that a thunk would not be called if we don't need e, that's great, but what if 
there is a possibility that we will call the thunk several times, now we will evaluate e
multiple times, and that's worse than a normal function (evaluate e only once).

Delay and Force, is the idea of delaying evaluation, but also remembering the result of evaluating e.
The delay function would return a mutable pair `(mcons #f thunk)` the thunk holds the expression e 
and this pair is passed to the function instead of passing e. The force function when called for the first 
time will call the thunk, and remembers the result, the pair becomes `(mcons #t result)`. 
the algorithm is something like this:
```python 
    if mcons[0] is true 
    then return the result  
    else  evaluate the thunk and change the pair to be (true result) 
```

### Streams - Infinite sequence of values

To understand streams I'll show you an example: We need a function that returns the powers of two
2,4,8,... the code is like this:
`(define pow_two x  (cons x (lambda () pow_two x*2 )))`
notice when calling pow_two will get a pair holding 2 and a thunk that calls pow_two 4 in its body,
and then when calling this thunk will get a pair holding 4 and a thunk .... pow_two 8 and continue like
this forever.
Note: without returning a thunk it's an infinite loop.

### Memoization

The memoization is a way to avoid duplicate calculations, by storing the results of previous function calls,
for example, in the famous Fibonacci algorithm, to calculate fib(n) we have to call fib(n-1) and fib(n-2) recursively, so fib(5) will call fib(4) and fib(3), and then fib(4) will call fib(3) also, with bigger numbers these duplications are catastrophic, so if we store the result of fib(3) we can use it again instantly and this is the memoization.

### Macros 

A macro is a way to introduce new syntax to an existing language, for example, if I don't 
like the conditional style I can define a new `if e1 then e2 else e3` that will be replaced,
with `if e1 e2 e3` when executing the program. In the preprocessing stage.

### Crafting a simple interpreter 

The video lectures of week 2, contain some tips and explain how to implement an interpreter.


#### How to represent the syntax of the new language?

We can use struct construct to add new types to racket, then we can use these types as a new language.
for example: `(struct Add e1 e2)` this code will define an Add type that takes two expressions e1 and e2,
and we'll get a function then return an element of type Add `(Add 1 2)`, a function to check if the type is
Add `(Add? e)`, and function to get e1 and e1 `(Add-e1) (Add-e2)`.
Add is just an example we can create, datatypes for functions, if-else statement...
 

#### How to evaluate a program of the new language?

We will create an eval function, in this function, we will define how we will treat and evaluate each expression, and evaluate them recursively. for example in the body of eval:

```python
if Add? e
    e1 = (eval Add-e1) 
    e2 = (eval Add-e2)
    return e1 + e2
```
The syntax of this example is like racket, so when evaluating Add e1 e2, we will evaluate recursively 
e1 and e2 (e1 could be for example another add, so we can't assume that e1 is an integer), then return the sum of e1 and e2. 

#### Env

When we are evaluating a program we are evaluating this program in an environment, this environment 
contains variables, closures... so each time we are calling the eval function it should have
2 arguments, one for the environment and, another for the expression, the program starts with an empty env.

#### Closures

When we evaluate the body of a function we should evaluate it where the function is defined not where 
the function is called, so when evaluating a function we should keep a version of the env where it's defined, so evaluating a function should return a closure containing the function and the environment. 

```python
    if (fun? e)
        (closure env e)
```
#### Racket functions as macros

We can use racket functions as "macros" to return a program in our new language,
for example, we can define a racket function f that returns a valid expression e in the new language, and then we use f as a macro.  

### Week 3 - the final week.

The main topic of this week is explaining the differences between static and dynamic typing and comparing
them.

#### What is static checking?

Static checking is everything done to reject a program after it parses successfully but before it runs.
The most common static checking is type checking system (every variable has a type, functions have signatures)
and we can reject something like passing a string to an arithmetic function, but of course, a type system doesn't 
prevent everything.

### Soundness and Completeness

If we have a type system that should prevent X.
This type of system is sound if there are "no false negatives" or it never accepts a program that does X.
This type of system is complete if there are "no false positives" or it never rejects a program that will not do X.
The type systems for programming languages are usually sound because if it's complete it would not reject some 
programs that have X. 
A type system can't be complete and sound at the same time, so our sound type systems would reject some
programs that make sense and have no errors.
 
### Static vs Dynamic typing

I'll show you arguments from both sides it's like a war!

Dynamic:
- Dynamic is more convenient because we can create a list with integers and strings, a function has different
return types
- Static prevents useful programs for example an if statement that has a number in one branch and bool in the
other one.
- Dynamic can catch bugs but later than static.
- Code reusing is easier with dynamic `fun add x y = x + y` we can use this function with numbers and  
concatenate strings for example.

Static:
- Static is more convenient because `fun cube x = x*x*x` we assume x is an integer, but in racket, we have to check
it by using `integer?`
- Static allows you to define datatypes as needed, so if you want a function that takes an int and string 
we can define a datatype that holds both `datatype type = Int of int | Str of string`.
- Static catches bugs earlier
- Static is faster, when passing a number in a language like  racket we have to pass a field indicating 
it's a number.

## Finally...

I would say this is a great course. I encourage you to watch it on Coursera starting from part A.
I have finished this course on 14/1/2022.
