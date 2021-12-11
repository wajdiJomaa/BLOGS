# Programming languages course part A

## Introduction

### About the blog

Hi there, in this blog I will talk about a course called "Programming Languages Part A",
the course has the purpose of teaching you the most relevant ideas of functional programming, we will use a language called "SML", it's a very simple language and will let us focus on the concepts of functional programming. But I won't talk about everything in the course and every topic that the course covers, I will share with you the parts that are the most important and specific to functional programming.  

### About the course

As the name tells this course has 2 other parts "B" and "c", the main idea of the course is simply  
not learning new programming languages but learning some important programming concepts.
This course is available for free on Coursera. 

## The Course briefly 

This course has 5 weeks, or maybe 4 because the first one is a simple introduction with optional fake homework. In the following sections, I will talk about the important ideas in each week.

### The second week

#### Defining expressions

First, we'll start talking about ML expressions and variable-bindings, when we want to define an expression in any programming language we have to ask 3 fundamental questions :

1. what is the syntax?
2. what are the type checking rules?
3. what is the evaluation of this expression?

for example the adding expression :

1. syntax: `e1 + e2`.
2. type check: e1 and e2 should both be expressions that have an integer type or it will not type check.
3. evaluation: evaluates e1 to v1 and e2 to v2 then returns the sum of v1 and v2.
 
#### no mutation

In SML the variables are not mutable, there is a way to define mutable variables using records but all other variables are not. With this approach, we don't have to worry about mutations 
which can cause a lot of problems especially when passing arguments and returning from functions.

#### Recursion everywhere

In imperative programming everyone use `for` and `while` loops, but this is not the case for functional programming
where you have to use recursion and only recursion and the reason is the lack of mutation. I will put a simple example of adding all the elements of a list.
`fun add ls = if null ls then 0 else hd ls + add tl ls`  the `hd` function is function that returns the first element of a list, the `tl` returns the list without first element and `null ls` return true if the list is empty. It was very funny to work with this type of recursion a style that you will never see in imperative programming.

#### local bindings 

Another feature that I find very important is the `let` expression which let us define local variables which are only available in the body of the let expression `let val x = 1 in x + 1 end `.

### The third week

#### Syntactic sugar 

The first time I heard of this expression was in this course, all programming languages do a lot of syntactic sugars and we don't notice this, in SML there is a type called records that could be defined like this 
`val rec ={a=1,b=2}` and you can access an item by using `#a rec`, and it seems that there is another type called tuple
`val tup = (1,2)` and you can access an item by using `#1` or `#2` but the truth is that this tuple is a record like this `val tup = {1=1,2=2}` but the syntax is so ugly so the programmers of the language decided to implement tuples as syntactic sugar.

#### Compound Types

Compound types are types that have other types in their definition and we have 3 different types of them :

1. "Each of": A compound type that contains each of values of types t1 and t2 and ... like records and tuples.
2. "One of": A compound type that contains one of the values of types t1 or t2 or ... 
3. "Self-reference": A compound type that can refer to itself in its definition for example list of integers is an 
Integer with another list.

#### Datatypes

In SML you can define your own "One of" datatype by using `datatype card = Number of int |  Diamond | ... `
and what is exciting about these data types is that they could be recursive by definition.

#### Pattern matching 

When I first see the pattern matching I was impressed, and I thought that why not just every programming language has this idea. A pattern is like `[]` that matches an empty list and `x::xs` x matched the first element of a list and xs is the part of the list without the head. If we implement the example above of adding elements in the list it will be like: `fun add ls = case ls of [] => 0 | x::xs => x + add xs ` you see no need to if-else no need to hd tl null a simple and beautiful syntax, and it becomes really helpful with datatypes that we talk about above for example 
`fun example c = case c of  Number x => x+1 | Diamond => ...`.

#### Tail recursion 

When we talk about recursive functions we talk about the stack, every time we call a function we will push it to the part of the memory called stack, so sometimes if a recursive function has a lot of calls we will run out of space, but with tail recursion, we don't have to worry about this. 

`example n = n*example n-1` and `examp n = examp n-1` The difference between these two examples is that in the first one after calling the function `example n-1` we have to multiply the result by n, while in the second example the function `example n` return the same result as `example n-1` and this is a tail case, we don't have to do any calculation after calling `example n-1`, so when we call it we can remove `example n` from the stack because it has no more calculation, the SML language can detect the tail recursion case and this is very elegant.

### The 4th week 

The main ideas of this week are closures, passing functions to other functions, and returning functions.

#### What is a closure

A closure is a function with the environment where this function is defined for example 
`val x = 2 ; fun double y = y*x `  in the function double the value of x is always 2 event when we pass the function 
to other functions, so we don't just pass the function we always pass its environment.

#### map function

One of the most popular functions in functional programming is the map function, the map function takes another function f and a list l and returns the list of {f(x)} where x is an element of l. 

#### Anonymous functions and syntactic sugar

SML lets us define anonymous functions, this is very useful when we want to pass a function as an argument, and we don't want to use it anywhere else. for example, we have a function `fun example f = ....` that takes another function.
So while calling it we can use `example (fn x=> x*2)`.

Also all funtions are syntactic sugar of anonymus functions `fun f x = x*2` is syntactic sugar of 
`val f = fn x => x*2`.

#### filter function 

As the name tells, the filter function takes a function f and a list l and returns a list of elements of l that evaluates to true with f(x). note: the function f should return always a bool.

#### fold function

the function fold takes 3 arguments a function f an accumulator acc and a list l, it will apply the function f on every element of l and every call will affect the accumulator, at the end, the function will return the accumulator.

#### Currying

The common way to pass arguments to a function is using a tuple `fun example (x,y) = x*y`, but with currying, we pass the arguments like this `fun example x = fn y => x*y`, the function example takes an argument x, and returns another function that takes y, and returns x*y `fun 8 10`. Some languages can use currying better than tuples and also currying allows us to do the partial application.

#### Partial application

This idea is cool, if we want to define a function only_positive that takes a list and remove all negative numbers, well this looks like a filter, so we can define the function as following 
`val all_positive = filter (fn x => x > 0)` now when we call the function with a list ls, it's like calling 
`filter (fn x=> x > 0) ls`. notice the filter function uses currying.

#### Unecessary wrapping

One of the simplest mistakes while programming is the unnecessary wrapping, for example, 
`fun all_positive ls = filter (fn x=> x>0) ls` the function here takes one argument a list ls and pass it directly to the function filter with the function that checks for positive numbers. But instead, we should define it as the example in the partial application section.

### The 5th week 

In this week, the course talks about 2 main topics :

1. The first is about type inference and how a type checker determines the type of a variable for example `fun ex x = x*2` x should be an integer because we multiply it by 2.

2. The second is about defining modules and restrictions on these modules using signatures. 

#### Polymorphic types

I should also mention the polymorphic types, for example, the following function that returns the length of a list, `fun ex ls = case ls of []=>0 | _::xs=> 1+ ex xs`, we don't do any calculation with list elements, notice I use _ here, so ls could be a list of anything, the type checker represent it as `'a list` alpha list.


## Finally...

Finally, I would say that this course taught me a lot of concepts, I encourage you to watch it, don't forget to do all your homework because it's the best way to practice new things, the course covers all the topics I mentioned in details and a lot of other topics.

###### Wajdi jomaa, 9 Dec 2021.