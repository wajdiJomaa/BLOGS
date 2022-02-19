# Programming languages Part B

The language used in this course is called racket, a functional dynamically typed language.
There is a lot of topics and it's hard to talk about them all so I'll talk only about some topics.

### Parentheses

Maybe the first thing you will notice about racket is "what the heck why all these parentheses",
racket uses a lot parentheses but of course the they are not for decoration, parentheses have a 
special meaning, mainly they are function calls, for example calling the plus function the syntax is 
`(+ 1 2)` at first this is a weird syntax but by the time you'll enjoy closing a lot of parentheses.
Be carefull `(1)` will lead to a nice error message because you are assuming that 1 is a function.

### List are Pairs??

The question is how many piece of data we can fit in a pair? 
The answer is really simple of course not 2 it's infini, as we know a pair is a set of 2 expressions
but what if the second one is another pair and this pair has another pair... 
By this idea we can define a list using multiple nested pairs for example :
`cons(1,cons(2,cons(3,null)))` this is a racket list notice that at the end there is a null, and cons
is just a piece of syntax to define a pair.

### Be careful when using mutation

You can use mutation in racket but by default cons are not mutable, but you can use mcons
for mutable pairs, this course really show you that mutation should not be overused, for example 
you have array A and B, by avoiding mutation you don't have to worry if A a copy of B or 
A has the same address as B, beacause A and B won't change forever. 

### A Function with no arguments - The power of A Thunk

What is a thunk? A thunk is function with no arguments, that contain an expression e in its body 
`(lambda () e)` 

Why using a thunk? Imagine that there is an expression e, and we want to evaluate e only when it's necessary, because evaluating e is expensive. As we know passing e to a function would evaluate e before
executing the body even it's not needed in the body, so what if we pass a thunk containing e instead of e,and then calling the thunk when needed. for example:
function :
`(define example (e) (if #t) 1 (e)`
if we pass an expensive epression to this function as a thunk it will never evaluate because 
we don't need it (the expression is in an else branch and the condition is true), so by using a thunk
why don't waste time.

### Lazy evaluation - Delay and Forth

We said that a thunk would not be called if we don't need e, that's great, but what if 
there is a possibility that we will call the thunk several times, now we will evaluate e
multiple times, and that's worse than a normal function (only once).

Delay and Force, is the idea of delaying evaluation, but also remembering the result of evaluatig e.
The delay function would return a mutable pair `(mcons #f thunk)` and this pair is passed to the function.
The forth function when called for first time it will call the thunk, and remeber the result the pair become
`(mcons #t result)` the algorithm is something like this:
`if mcons[0] is true then return the result  else  evaluate the thunk and change the pair to be (true result)`

### Streams - Infinite sequence of values

To understand streams I'll show you an example: We need a function that returns the powers of two
2,4,8,... the code is like this:
`(define pow_two x  (cons x (lambda () pow_two x*2 )))`
notice when calling pow_two will get a pair holding 2 and a thunk that call pow_two 4 in its body,
and then when calling this thunk will get a pair holding 4 and a thunk .... pow_two 8 and continue like
this forever.
Note: without returning a thunk it's an infinite loop.

### Memoization

The memoization is a way to avoid duplicate calculations, by storing the results of previous function calls,
for example the famous fibonnaci algorithm, to calculate fib(n) we have to call fib(n-1) and fib(n-2) recursivly, so fib(5) will call fib(4) and fib(3), and then fib(4) will call fib(3) also, with bigger numbers these duplications are catastrophique, so if we store the result of fib(3) we can use it again instantly and this is the memoization.

### Macros 

A macro is a way to introduce new syntax to an existing language, for example if I don't 
like the conditionalas style I can define a new `if e1 then e2 else e3` that  will be replaced,
with `if e1 e2 e3` when executing the program. In the preproccesing stage.

### Crafting a simple interpreter 

The video lectures of week 2, contains some tips and explains how to implement an interpreter.


#### How to represent syntax of the new language?

We can use struct constuct to add new types to racket, then we can use these types as a new language.
for example: `(struct Add e1 e2)` this code will define an Add type that takes two expressions e1 and e2,
and we'll get a function then return an element of type Add `(Add 1 2)`, a function to check if the type is
Add `(Add? e)`, and function to get e1 and e1 `(Add-e1) (Add-e2)`.
Add is just an example we can create, datatypes for functions, if-else statement...
 

#### How to evaluate a program of the new language?

We will create an eval function, in this function we will define how we will treat and evaluate each expression, and evaluate them recursivly. for example in the body of eval:

``` 
if Add? e
    e1 = (eval Add-e1) 
    e2 = (eval Add-e2)
    return e1 + e2
```
The syntax of this example is like racket, so when evaluating Add e1 e2, we will evaluate recursivly 
e1 and e2 (e1 could be for example another add, so we can't assume that e1 is an integer), then return the sum of e1 and e2. 

#### Env

When we are evaluating a program we are evaluating this program in an environment, this environment 
contains variables, closures... so each time we are calling the eval function it should has
2 arguments for the environment and for the expression, the program start with an empty env.

#### Closures

When we evaluate the body of a function we should evaluate it where the function is defined not where 
the function is called, so when evaluating a function we should keep a version of the env where it's defined, so evaluating a function should return a closure containing the function and the environment. 

```
    if (fun? e)
        (closure env e)
```
#### Racket functions as macros

We can use racket functions as "macros" to return a program in our new language,
for example we can define a racket function f that returns a valid expression e in the new language, and then we use f as a macro.  
