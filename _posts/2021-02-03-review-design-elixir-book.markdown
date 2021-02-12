---
layout: post
title: Review of the Book Design Elixir Systems with OTP
date: 2021-02-10 00:00:00 +0300
description: A review of the book and some thoughts about it. # Add post description (optional)
img: book.jpg # Add image post (optional)
fig-caption: book-review # Add figcaption (optional)
tags: [English, Book, Review] # add tag
---

# Review of the Book Design Elixir Systems With OTP

This book by Bruce Tate is, for sure, one of the best findings that I did this year.

For those of you who don't know me, my name is Thiago Ramos, I am a senior software engineer for a California based startup and I work mainly with ruby and rails.

Some years ago I discovered Elixir seeing one of Jose Valim's presentation and he got my attention. As a Brazilian I had heard of Elixir before in 2013, but it was in 2016 that I really looked into it.

Well, life gets in the way and I never really studied Elixir. But at the end of 2019 I decided that I would learn Elixir and all the PETAL stack because I really loved the language and the fact that it is functional and runs inside the Beam Virtual Machine which is a 40+ virtual machine that already solved the concurrent problems that we are mostly facing now, all of this got me thinking,

> Humm, I think this is a great language and ecosystem for the future. I most learn it now.

From there to now I read a lot of Elixir books including the Programming Elixir, Phoenix Programming and Programming Ecto, Functional Web Development with Elixir, OTP and Phoenix. All excellent books, specially the last one that I will do a review later.

This year I found Design Elixir Systems with OTP and I am glad I did.

This book is somewhat comparable to the Functional Web Development with Elixir in the sense that both authors code layer by layer which means that the book is structured in a way that each chapter or part of the book is responsible for one layer.

In the case of this book the author is splitting the code into 5 layers plus two chapters related to test.

### Layers Explained

First of all I got say that this book is not for a programmer that is learning elixir. For that I think you should go first to Programming Elixir book, to understand the data structures, the functional language idiom using elixir and to understand about how Elixir uses recursion, how do you create functions, how to use them other aspects of the language.

That being said I think if you understand a little bit about all those topics above you will find that this book will help greatly. First because it's almost a hands on book, where you can see the author implementing the code and discussing about why he choose to use Map instead of a Struct of vice-versa, you can see when he uses Tuples and when he prefers to use Keyword Lists.

What I am trying to say here is that seeing the choices being made regarding the problem at hand is great to understand better why and how to use particular data structures over others. And that's, for a person that is learning the language, it is awesome to see.

I am kind of biased because I always learned better by doing and I was never big fan of Bible type of books anyway.

Through all the book the author is creating a Quiz system where a particular user can build quizzes through templates and can apply those quizzes to other users.
There are two main core parts in the system:

- The Quiz Manager is where a quiz is created, all the templates and fields populated. You can have multiple templates for single addition, multiple addition and so on.
- The other is the Quiz Session where a user can take a quiz. In this process the user needs to correct answer a number of questions so he can pass in the quiz. All of that is configurable through the Quiz Manager. The Quiz Session also has a timeout to expire and can be scheduled through a scheduler module.

So, the author starts by creating the core of the application which consists solely of modules that contain some structs and functions. He does not talk about Ecto or changesets or schemas or anything like that, he is not worried about any persistence layer. Just the intricate relations between modules and their functions. He goes on and create all the core structure with logic for the Quiz problem. In this part you will have a fully function core that you can test and even work with in the console.

This approach has a name in the architecture world which is the Hexagonal architecture.

![Hexagonal Architectrure](https://apiumhub.com/wp-content/uploads/2018/10/Screen-Shot-2018-10-26-at-09.36.50.png)

In the hexagonal architecture your core business logic is in the center and all the infrastructure code is in the boundaries, which in this case means:

- Processes
- Supervisors
- Database (Ecto)
- Web Layer (Phoenix)

So, in the core the author does not create any GenServer or worries about processes or anything like that, it is just plain old Elixir modules with functions.
I have some thoughts about this approach that are conflicted.

I did a lot of projects where we did the separation of concerns using the hexagonal architecture. I will do another post about my experiences on this but one thing I can say now is that you should analyze if your application is too much data centric, which means that your web layers a almost always sending data back and forth from databases to web or other delivery channel.
Or if you application has a high "density" of business logic.

In my experience I think you don't need to adopt one structure and use it everywhere for all the cases in your system.
I think you can understand the parts of your system that would be good to be extracted from concerns about persistence layer and delivery mechanisms and extract them so you can work on them without worry too much about database schema structures or json/xml structures or whatever it is your delivery mechanism.

What I see a lot is that people tend to be rigid about their early choices and create code in the system using the choices they made early on even when it is clear that it doesn't need to be that way. And we tend to see a lot of layers with no logic and layers of conversion data being used where it is most unnecessary, just because the choices were made and who the hell is brave enough to go against the system in place, right?

I am rambling here. Going back to the book.

The next step is to create the boundaries where all the GenServer processes exists.

So, with all the core done it is easy enough to create the modules that will start the processes that will use the core modules to create quizzes and make possible for users to take the quizzes.

What I found good in this part of the book is that you can see how an experienced elixir developer thinks about it and how he approach the creation of the servers and the api for that servers.

In this part he goes on and create the GenServers and the api for the GenServers which consists in functions on the module that hide the calls and the logic that is executed in the GenServer callbacks like "handle_call" and "handle_cast"

The author does a great explanation why it is better to use [GenServer.call](http://genserver.call) instead of GenServer.cast and uses the Logger code in elixir to explain how they use those methods and callbacks to apply backpressure and limit the flow of data so the system continues healthy.

Basically all [GenServer.call](http://genserver.call) are synchronous and the GenServer.cast are synchronous which means that if you use only the "cast" function you can end up receiving more messages than you can process and once you mailbox is full you will start to reject messages and on away to deal with this is to use the "call" funciton instead.
Depending on your business requirements you can create logic to use "cast" and change to "call" if needed.

I will not spoil more than I did so far so you read the book too.

The next steps he goes to create the supervisors. In this book he creates both the configuration for static and dynamic supervisors. He does a great job explaining why we need dynamic supervisors for the Quiz Session part and why we don't need for the Quiz Manager part.

Quiz Sessions are created by user. So each user taking a quiz is a different process in the tree.

Quiz Managers don't need to have more than one process because they are not dynamic. Once we have a manager with all the templates we can start taking quizzes using the templates already created.

Of course that he goes on explaining that this is the case for the business logic he thought about.  We, of course, could have multiple Quiz Manager processes if we want to.

The last part of the book is related to the Workers. What are they exactly.

At first I thought about them as background jobs because I am a full time ruby / rails developer and everytime someone talks about workers in my mind they are talking about some work that is done in the background. But in Elixir every process you create has his own world there and does not share anything with any other process.

In this case the author says that we should, in general, avoid naked processes and use Tasks instead or always work with supervisors and he goes on to write examples using the erlang library poolboy. I may be wrong here since I don't really remember the words he wrote.

But in this part of the book the author creates a module called Proctor which the basic function of the module is to schedule quizzes, start them and terminate them when they timeout.

This layer uses all the other layers built in the book and the code specifically creates processes and terminates them when they should be terminate.

So, the worker word it is used in the meaningful way here, they are processes that do the work of schedule quizzes and create and kill quizzes processes and it should.

In a sense this layer is adding one more process in the tree that is responsible to create processes and kill them.

### What did I think of it?

My general feeling of the book is that I learned a lot about OTP. I learned when to create processes the type of code that should go within, I learned how to use Supervisors to leverage a system that is reliable and scalable. I lost the fear of create trees of processes and I got to see how an experienced developer usually thinks when coding in elixir.

Overall I am much better in elixir that I was before reading the book.

I have to be careful though, because following someone's path (code) does not mean that you really get the knowledge and by now I have baggage enough to know that I need to write and write a lot of code and solve a lot of problems to be good at something.
Keep in mind that I am not learning how to code and I do study elixir for some years now. So I do think that if you are starting now I am not sure if Elixir is the best language for you.
I do think that if you know the basics this book will greatly improve your Elixir idiom and you got to see pretty good tricks.

This book by Bruce Tate is, for sure, one of the best findings that I did this year.

For those of you who don't know me, my name is Thiago Ramos, I am a senior software engineer for a California based startup and I work mainly with ruby and rails.

Some years ago I discovered Elixir seeing one of Jose Valim's presentations and he got my attention. As a Brazilian, I had heard of Elixir before in 2013, but it was only in 2016 that I looked into it.

Well, life gets in the way and I never really studied Elixir. But at the end of 2019, I decided that I would learn Elixir and all the PETAL stack because I loved the language and the fact that it is functional and runs inside the Beam Virtual Machine which is a 40+ virtual machine that already solved the concurrent problems that we are mostly facing now, all of this got me thinking, hmm, I think this is a great language and ecosystem for the future. I must learn it now.

From there to now I read a lot of Elixir books including Programming Elixir, Phoenix Programming, Programming Ecto, and Functional Web Development With Elixir, OTP and Phoenix. All excellent books, especially the last one that I will do a review later.

This year I found Design Elixir Systems with OTP and I am glad I did.

This book is somewhat comparable to the Functional Web Development with Elixir in the sense that both authors code layer by layer which means that the book is structured in a way that each chapter or part of the book is responsible for one layer.

In the case of this book, the author is splitting the code into 5 layers plus two chapters related to test.

The following is an attempt to explain a little bit of the book for those of you who don't know it or already know but are not sure to buy it.

If you want to follow me:

* Youtube: [Youtube Channel(In Portuguese)](https://www.youtube.com/thiagoramosal)
* Twitter: [@thramosal](https://twitter.com/thramosal)
* Instagram: [@thiagoramosal](https://instagram.com/thiagoramosal)

Hope you have an awesome life as a developers.
