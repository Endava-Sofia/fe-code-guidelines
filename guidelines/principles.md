## Table of Contents

- [Principles](#principles)
  - [KISS](#kiss)
  - [YAGNI](#yagni)
  - [DRY](#dry)
  - [WET](#wet)
  - [The boy scouting rule](#the-boy-scouting-rule)

# Principles

Software principles like KISS (Keep is simple, stupid) and DRY (Don't repeat yourself) are important to keep in mind when writing code. They are guidelines that can help you write maintainable and easy to read code.

## KISS

KISS is an acronym for "Keep it simple, stupid". This principle states that we should not add unnecessary complexity to our software.

Instead, we should keep our code simple and focused on the task at hand.This will make it easier to understand and maintain our code.
KISS is also used as an acronym for _keep it short and simple_, _keep it short and sweet_, and _keep it simple and straightforward_. However, all these variations refer to the same approach.

There are many ways to apply the KISS principle. Here are some ideas:

- **Simplify and explain concepts** - try to code as simple as possible. Unfold your concepts and name things properly. Explain with comments where the code itself can't.
- **Use descriptive names** - use descriptive names for variables, functions, classes, etc. This will make it easier to understand what they do and how they work.
- **Deconstruct the problem** - break down complex problems into smaller parts that are easier to solve. Try refactoring your code and solve a single problem at a time, but with maximum precision and readability.

## YAGNI

YAGNI, acronym for "You aren't gonna need it" states that we should implement functionality only when it is truly necessary.
We should focus on solving the problem at hand and only add functionality when it is needed.
In other words, add the code you need to answer the present needs, no more. Focus on delivering the core functionality first, ensuring that it meets the requirements.
Avoid adding features based on speculation or future needs that are not currently essential.

Regularly assess the project's requirements, gather feedback, and adapt accordingly. Following an iterative development approach, we can ensure that we are delivering what is truly needed.

## DRY

The DRY principle, an acronym for "Don't Repeat Yourself", emphasizes the importance of avoiding duplication in our applications.
The DRY principle ensures that any modification of a single part of code does not require a change in other, logically unrelated parts.

DRY extends beyond literal code duplication. It also encompasses the duplication of knowledge and intent. In other words, it's about expressing the same concept in multiple places,
potentially in different ways. To stick to the DRY principle, we should strive for clean, modular, and reusable code.

## WET

"Write everything twice" or WET is a challenge to the extreme of DRY, often associated with over-abstraction. If you have pieces of code that do the same, or similar things,
but are representing different knowledge it's fine to keep the repetition, otherwise you'd be coupling those concepts, making changes that affect only one harder to do.

> "Duplication is far cheaper than the wrong abstraction." - Sandi Metz

## The boy scouting rule

The Boy Scouting Rule emphasizes the importance of consistently improving code quality and minimizing technical debt. It encourages developers to
"always leave the code you are working on in a better state than you found it." This guiding principle draws inspiration from the Boy Scouts of America's Leave No Trace ethos,
which advocates for leaving the environment cleaner than it was discovered. When integrated into software development practices, the Boy Scouting Rule promotes the cultivation of a culture of ongoing enhancement,
resulting in more robust code and reduced technical liabilities.

The boy scouting rule comes down to leaving the code in a better shape then you found it meaning you do reasonable optimizations and improve code readability to the working files constantly
while you are working on normal features in them.
