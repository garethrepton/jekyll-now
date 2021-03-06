---
layout: post
title: Just Write Something - The Antidote to Analysis Paralysis 
tags: tdd design c# software-engineering process 
---
This is going to be an opinionated view as to the most efficient way I've personally found create a software design, heavily based on core OO & XP principles, allowing software to grow naturally, TDD and refactoring. It may not suit everyone, and I'm sure some people will disagree with it, but I've found these things to really help reduce unnecessary friction.

## Implementing something helps you to see the design
You can understand your requirements, understand what software needs to do, and think about how it will fit together, maybe even draw some diagrams to get a better feel for something, but ultimately, the best tool to understand how something should be designed is to write some code, and its very, very unlikely you'll see enough of the picture from diagrams alone to implement an effective and well structured solution. 

Write it cleanly, and evolve the design by refactoring with you're increased understanding of the problem/solution. This is sort of the approach that TDD encourages.

## Red-Green-Refactor
The red-green-refactor cycle is:

1. Write a failing test
2. Make it pass by implementing the functionality that needs to be performed
3. When it passes - refactor anything you can see in both the test and the target code to make it work cleanly and optimally. 

And then repeat for the next feature.

I find this approach works whether you are using tdd or not, but with tests you get a lot more confidence in the refactor step, and they can help you think about the problem, especially with more complicated behaviour implementations where its easier to build the algorithm up over multiple steps.

Lots is written about this approach on-line, heres one from uncle bob, but mostly its summarised above (yes it really is that simple). [Red-Green-Refactor](http://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)

## Write the new behaviour in the simplest way possible
Don't try to pre-empt the design, if something requires something as simple as an if statement, write the if statement, don't start by implementing a complex design pattern that you'll probably never need. Everyone has done this before, the problem is it makes the code much more complex and less readable, and may never get removed.

## Remove code that isn't used
Reduce the noise to anyone else reading the code in the future by removing superfluous code from the code base that you can. This is really part of the refactor step.

## Work in vertical chunks if you can
Working in vertical chunks allows an application to grow naturally without a heap of BDUF. It means implementing one feature at a time across the entire/majority of the stack, this means you aren't trying to predict things like... how will the UI call this service? Which methods will we actually need for data access?... You know this because you write them as you write the features. Its a very efficient way to write things.

## Let abstractions naturally fall out of the code, don't force them
So you've written some nice simple code that works, and you spot somewhere that needs to be separated into its own class, or maybe something that would suit the strategy design pattern, refactor it to reflect your increased understanding of the design at this point, especially if it will simplify the overall solution. 

If you've just tested the behaviour in any unit tests you've written (and not the innards of the class), the refactor should be relatively simple, and you're starting to build a design based on a much better understanding of the solution than you had before writing anything.

I also find this helps reduce any potentially lengthy and unproductive analysis process, by getting into the code early, even if at first its not that well structured, with practice you get quite good at this, and shortcut much of the process by seeing common patterns etc...

## Summary
And thats about it, theres a whole heap written about these things written all over the internet, and this post is far from comprehensive, but this hopefully gives you a good summary of the sorts of things that will help to drive the design process. Again these take practice, and may not work for everyone YMMV. 

