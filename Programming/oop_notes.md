---
title: OOP Notes
category: Programming
tags: [oop, composition, inheritance, typing]
description: Notes comparing composition vs inheritance and related typing concepts.
status: notes
---

# OOP Notes

Composition is often considered better than inheritance because it provides a high level of flexibility and reduces the complexity associated with inheritance hierarchies. Here are some reasons why:

Strong typing: Inheritance is a tight coupling and often requires the base class to know about its derivatives, which can lead to problems if the base class changes.

Avoids "inheritance hell": Deep inheritance hierarchies can become hard to understand and maintain. With composition, you simply build up classes with minimum functionality and then combine them as needed.

Easier code reuse: With composition, it's easier to reuse a class in multiple contexts. Inheritance can lead to a lot of duplicated code if subclasses have similar but not identical features.

Dynamic behavior change: With composition, you can change the behavior of an object at runtime by changing its components. With inheritance, the behavior of an object is set statically at compile time.

Follows the principle of least knowledge (Law of Demeter): An object communicates with its own immediate components and does not need to know about the components that are used internally by other objects.

In the context of your code, using Dict, Mapping, and MutableMapping from the typing module for type hinting is an example of composition. Each of these types can be used independently or they can be combined to create more complex types. This provides a lot of flexibility and makes the code easier to understand and maintain.