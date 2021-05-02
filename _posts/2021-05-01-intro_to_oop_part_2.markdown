---
layout: post
title:      "Intro to OOP (part 2)"
date:       2021-05-02 00:19:44 +0000
permalink:  intro_to_oop_part_2
---

​	Welcome to the second part of my Object Oriented Programming (OOP) blog series, if you haven't read the first one check it out [here](https://blog.devtylerjones.com/intro_to_object_oriented_programming). As we delve a bit further into OOP fundamentals and concepts ill include further readings and resources at the bottom to cover specificities.

​	Inheritance allows new objects and classes to inherit the existing attributes and methods of another object or class. There are two main kinds of inheritance, multiple and single, in which object can inherit one or more classes. Multiple Inheritance is, just as it sounds, when one child class inherits from more than one parent class. Not all languages support this however because multiple inheritance can be overly complicated and difficult to manage. Single Inheritance forces you to focus on abstraction. Think of single inheritance as a tree and at its base it should be the most abstract and dynamic. As the tree branches out it becomes more specific. 

​	Class-based inheritance in which the structure of objects are defined in classes. New objects created from a class become an instance of it and inherit the attributes and methods defined within the class. Prototype-based inheritance in which objects are based on copies or clones of existing objects. Structured objects with attributes and methods can share them to its prototype. For example if objectA has a name, age, and height then an instance of objectA will inherit the attributes along with their values and can change or add its own. If objectB has hairColor, eyeColor, and height it will have the same name and age as objectA but a different height.

​	Polymorphism is the ability to have many forms of the same interface and is commonly known for two different types. The first type, dynamic polymorphism, is allowing access to methods that come from the same interface and implementing them in different ways depending on the object. The second type is called static polymorphism which uses a method overloading feature. Method overloading also allows access to methods from the same interface but, uses a different set or amount of inputs.

  This concludes the two part intro series, I may continue the series in a more in depth way. Along with part one I’ve coved the main concepts and ideology of OOP be sure to look at the resources below and fall down the rabbit hole as I have.



Resources:

[SO - Polymorphism](https://stackoverflow.com/questions/1031273/what-is-polymorphism-what-is-it-for-and-how-is-it-used)

[Medium - Polymorphism](https://medium.com/@shanikae/polymorphism-explained-simply-7294c8deeef7)

[Wiki - Prototype-based](https://en.wikipedia.org/wiki/Prototype-based_programming)

[Wiki - Class-based](https://en.wikipedia.org/wiki/Class-based_programming)

[Wiki - OOP](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))

[Wiki - OOA](https://en.wikipedia.org/wiki/Object-oriented_analysis_and_design)

[SO - OOP](https://stackoverflow.blog/2020/09/02/if-everyone-hates-it-why-is-oop-still-so-widely-spread/)


