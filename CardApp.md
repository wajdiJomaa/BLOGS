# Android Concepts BlOG 
### Date: 6/6/2022

## Introduction

In this blog, I would share with my first try of building an android app using android studio.

I had a course for android programming in my university using Java, so that give me the idea 
to learn some extra topics using youtube and google to build my first small project using 
android studio and Kotlin.


## 1st step learning basics 

I have found a great tutorial on youtube to learn android basics with kotlin, I'll put the link below
https://youtube.com/playlist?list=PLQkwcJG4YTCTq1raTb5iMuxnEB06J1VHX

This tutorial covers the basics, and the essentiels of android developement, and by watching it (not neccesarly all videos) you can build so many small project. It's better to code while watching videos.

This Tutorial didn't cover how to connect your app with a db, so You can find some helpful docs on the website of android studio developers https://developer.android.com/training/data-storage/sqlite, and I recommend you to watch a small video on youtube.

## kotlin basics

If you don't know anything about kotlin it's not a problem, because even after building this project I
still don't know a lot about kotlin, I know it's not the best way to do things  but I don't have a lot of 
time to learn kotlin and my goal is only building a small app.

Kotlin is like java an opp programming language so nothing special for the majority of syntax, you can use google, watch small videos on youtube, also the recommandations of android studio when you have an error are very helpful. 
A lot of time I forget to put '?' when calling a function with a nullable variable "x?.tostring()"
and android studio fix this for me, btw I still don't know why kotlin treat nullable variables differently.

## why kotlin?

why kotlin? not java, because kotlin is the future of android developpement and it's becoming the
official language, for example jetpack compose the way to build modern application is only available
for kotlin.

## The project idea

The project is CardApp, where you add Cards(questions,answers), then you can add them to quiz, and test 
yourself with these information, you can also share a quiz with your friend with the export and import
functionality.

## Starting with andorid

A page in android is called an Activity each activity has two essential files:

1. XML file, where you define page components for example `<textview/><button/>...`, and you can
add some styles for each component. It's like an html file structure.

2. kotlin file, This file is where you put you code, the main method is `oncreate()` this method is 
called when the activity starts.

You can access the componenets of the page or the 'Views' by calling function `getViewById()`

## Layouts

There are several types of layout that you can specify for each file, for example:

LinarLayout: after specifying orientation(horizental or vertical) the elements will be placed horizentaly or
vertically, this layout is very simple.

Constraintlayout: this is the layout that I use for this project, it's better to use the graphical layout
editor instead of the XML code to position elements in this layout. You can position elments as you wish
in this layout.

## Intents

Intents are the links between activities, with intents you can pass data between activites, and
jump from an activity to another, even if the activity is another application, for example 
opening a web browser with a link, opening sms application, all these things are accomplished 
by intents

## RecyclerView

If you have a list of data let's say students and you want to display this list in your app,
you should use a recyclerview.
You have to define the layout for each item of the list in an XML file 'student.xml'

You have to define a students adapter class that inherits from recycler view adapter, and takes a list of students.

The function `onCreateViewHolder(parent: ViewGroup, viewType: Int)` in this class function the layout that you want for each element view, for example 'student.xml'.

The function `onBindViewHolder(holder,position)` is a function that will be called for each item in the list and return the view with data of the item, the holder contains the view of the item and the position is the index of the item in list, for example for each student the view should contain his name 
`holder.itemView.name = students[i].name` then we return the holder.