---
layout: post
title: Unit Test More
---

Unit testing is hard, you aren't going to learn it overnight, you may not reap the benefits of it immediately, you may not have a code base that supports them, but persist and long term, the benefits will start to appear. This is especially true with code that is harder to test with more traditional methods. TDD is the next level of this, and tends to be even harder to apply at first, but can be a great benefit once you get the hang of it.

Some of the benefits are:
 - Can speed up development when used appropriately.
 - Can help to reduce bugs.
 - Can help to improve the design of the system by encouraging refactoring.
 - Encourages smaller, less coupled classes.
 - Empowers developers working on an area of the system to rework it with less fear.
 - Encourages the abstraction of third party components (to allow that area to be made testable).
 - TDD encourages thought about the how something should work, before writing it.
 - By testing behaviour, your tests can actually help describe what the class should be doing.

There are costs associated with unit tests, some of these are:
 - Perceived slow development, at least initially (this can make them a hard sell).
 - Applying them to the wrong part of the system can be costly.
 - It can be harder to rework large parts of the system with tests in place.
 - It is easy for developers to get bogged down in the innards of the class under test, rather than focussing on behaviour.

These are just some of the cost/benefits, and depending on the project you are working on, you may decide that forcing through unit testing everywhere you can is not a good fit. However, even the most archaic, monolithic of systems can have components split out into testable components, and I'd really encourage anyone writing code to think about doing this where they can.

For example, do you have a method stuck in a large class thats called multiple times, and nobody dares change it? Why not extract that into its own class, call it from there in the main class, and wrap the new class in unit tests covering all cases. Or even, make that method public and test it from the large class if you are afraid to move it.

There are always options to make code testable, and more robust, its just about seeing where they are and taking advantage of them, and it may just save you deploying a breaking change one day, and it will all be worthwhile. 






 


