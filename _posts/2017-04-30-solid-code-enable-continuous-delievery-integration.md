---
layout: post
title:  "Writing SOLID Code to Enable Continuous Delivery & Integration"
date:   2017-04-30 17:00:00 +1000
categories: tech blogs
---
### Why do we need Continuous Delivery & Continuous Integration?
In today's highly competitive IT industry no one can afford to spend years developing a piece of software. There are always smart people with same or similar ideas and users can always find alternatives if they are unhappy with what you can provide. In this sense, it is always important to get to the market faster.
<!--more-->
Also, as it says in the book 'The Lean Startup', it is important to get user feedback at an early stage. Not only to justify your ideas but also to evolve your ideas. The feedback channel is an indispensable part of your iteration loop. We cannot rely on our own imagination about what the users want and what the market needs but to evolve our ideas in an interactive way with the real end users.

So, it is all about speed! This process is actually a loop:

release  ------> feedback ------> improve ------> next release

Developing software, Continuous Delivery & Continuous Integration are essentially about **ITERATIONS**. It is like wheels. The faster you spin your wheels the more likely you get the advantages against your competitors and with improved user satisfaction.

### Problems of CD/CI in real life
While the intentions for CD/CI are good, in practice, quite often it does not provide as many benefits as it should.

Working in the industry, I find many times people set it up as an overhead. It becomes a part of the corporate process programmers are required to follow. If this is the case, then it will provide no benefit but to slow down the development.

CD/CI is not a magic but a process to assist developers. To make it work, it essentially comes down to developers' programming skills, the code quality and the architecture of the software.

### SOLID Principles The Most Fundamental Software Principles
SOLID principles are around for many years. Unfortunately, this is still a skill hard to get from current education systems. Not until those graduates start working in the industry and collaborate with others on significant projects. A few will realize the benefits of these principles, while some others, who are not keen to improve will just keep repeatedly doing basic coding.

People always describe programmer is a discipline only for young people. I would say this is the case for the fall behinders who are satisfied with a coding position for the salary but not interested in paying attention to the big picture.

The SOLID principles are the fundamental principles one level above all other design patterns. It enables other techniques, like TDD/BDD. I am not intended to explain it in this article. You can find good examples by simply google the term. I cannot emphasize more on how important those principles are to programming and I cannot imagine any people working in the software industry can ignore it.

If your code is not SOLID, I believe it is 100% that no matter how well defined your CD/CI process is set up you will essentially get no benefits from it. SOLID are essentially the characteristics a maintainable and evolvable piece of software should have.

If you feel it is still too much for the SOLID principles, I would recommend following the following the simple rule below to start with:

**Program to interfaces and depend on abstractions.**

To wrap it up, engineering practices are to speed up your iterations. There is no magic to get the benefits out of the box straight way. So if you find the CD/CI slows the team down, the problem may lie in developers' coding skills.