# Programming languages course part A

## Introduction

### About the blog

Hi there, in this blog I will talk about a course called "Programming Languages Part A",
the course has the purpose of teaching you the most relevant ideas of functional programming, we will use a language called "SML", well no one uses this language anymore but it's very simple and will let us focus on the concepts of funtional programming. But I wont talk about everything in the course and every topic that the course covers, I will share with you the parts that are the most important and specific to functional programming.  

### About the course

As the name tells this course has 2 other parts "B" and "c", the main idea of the course is simply  
not learning new programming languages but learning some important programming concepts.
This course is available for free on coursera. 

## The Course briefly 

This course has 5 weeks, or maybe 4 because the first one is a simple introduction with an optional fake homework. In the following sections I will talk about the important ideas in each week.

### The second week

#### Defining expressions

First, we'll start talking about ML expressions and variable bindings, when we want to define an expression in any programming language we have to ask 3 fundamental questions :

1. what is the syntax ?
2. what are the type checking rules ?
3. what is the evaluation of this expression ?

for example the adding expression :

1. syntax: `e1 + e2`.
2. type check: e1 and e2 should both be expressions that has an integer type or it will not type check.
3. evaluation : evaluates e1 to v1 and e2 to v2 then return the sum of v1 and v2.
 
#### no mutation

 As we said that SML is a functional language so the variables are not mutable, there is a way to define mutable variables using records but all other variables are not. With this approach we don't have to worry about mutations 
which can cause a lot of problems espacially when passing arguments and returning from functions.

#### Recursion everywhere

In imperative programming everyone use `for` and `while` loops, but this is not the case for a functional programming
where you have to use recursion and only recursion and the reason is the lack of mutation. I will put a simple example of adding all the elements of a list.
`fun add ls = if null ls then 0 else hd ls + add tl ls`  the `hd` funtion is funtion that returns the first element of a list, the `tl` returns the list without first element and `null ls` return true if the list is empty. It was very funny to work with this type of recursion a style that you will never see in imperative programming.

#### local bindings 

Another feature that I find very important is the `let` expression which let us define local variables which are only available in the body of the let expression `let val x = 1 in x + 1 end ` .

### The third week

#### Syntactic sugar 

The first time i heard of this expression was in this course, all programming languages does a lot of syntactic sugars and we don't notice this, in SML there is a type called records that could be defined like this 
`val rec ={a=1,b=2}` and you can access an item by using `#a rec`, and it seems that there is another type called tuple
`val tup = (1,2)` and you can access an item by using `#1` or `#2` but the truth is that this tuple is a record like this `val tup = {1=1,2=2}` but the syntax is so ugly so the programmers of the language decided to implement tuples as a syntactic sugar.

#### Compound Types

Compound types are types that has another types in their definition and we have 3 different types of them :

1. "Each of": A compound type that contains each of values of types t1 and t2 and ... like records and tuples.
2. "One of" : A compound type that contains one of values of types t1 or t2 or ... 
3. "Self reference" : A compound type that can refer to itself in its defenition for example list of integers is an 
Integer with a another list.

#### Datatypes

In SML you can define your own "One of" datatype by using `datatype card = Number of int |  Diamond | ... `
and what is really exiting about these datatypes that they could be recursive by definition.

#### Pattern matching 

When i first see the pattern matching I was really impressed, and I thought that why not just every programming language has this idea. A pattern is like `[]` that matches an empty list and `x::xs` x matched the first element of a list and xs is the part of the list without head. If we implement the example above of adding elements in the list it will be like : `fun add ls = case ls of [] => 0 | x::xs => x + add xs ` you see no need to if else no need to hd tl null a simple and beatiful syntax, and it become really helpful with datatypes that we talk about above for example 
`fun example c = case c of  Number x => x+1 | Diamond => ...`.

#### Tail recursion 

When we talk about recursive functions we talk about the stack, every time we call the function we will push it to the part of the memory called stack, so sometimes if the recursive function has a lot of calls we will run out of space, but with tail recursion we don't have to worry about this. 

`example n = n*example n-1` and `examp n = examp n-1` The difference between these two examples is that in the first one after calling the function `example n-1` we have to multilply the result by n, while in the second example the function `example n` return the same result as `example n-1` and this is a tail case, we don't have to do any calculation after calling `example n-1`, so when we call it we can remove `example n` from the stack because it has no more calculation, the SML language can detect the tail recursion case and this is very elegant.

### The 4th week 

The main ideas of this week are closures, passing functions to another functions and returning functions.

#### What is a closure

A closure is a function with the environment where this function is defined for example 
`val x = 2 ; fun double y = y*x `  in the function double the value of x is always 2 event when we pass the function 
to other functions, so we don't just pass the function we always pass its environment.

#### map function

One the most popular functions in functional programming is the map funcrtion, the map function takes another function f and a list l and return the list of {f(x)} where x is an element of l. 

#### Anonymus functions and syntactic sugar


#### filter function 

As the name tells, the filter function takes a function f and a list l and return a list of elements of l that evaluates to true with f(x). note : the function f should return always a bool.

#### fold function

the function fold takes 3 arguments a function f an accumulator acc and a list l, it will apply the function f on every element of l and every call will affect the accumulator, at the end the function will return the accumulator.

#### Currying


#### Partial application

This idea is a really cool idea, if we want to define a function only_positive that takes a list and remove all negtive numbers, well this looks life a filter, so we can define the function as following 
`val all_positive = filter (fn x => x > 0)` now when we call the function with a list ls, it's like calling 
`filter (fn x=> x > 0) ls`. notice the filter funtion uses currying.

#### Unecessary rapping 
