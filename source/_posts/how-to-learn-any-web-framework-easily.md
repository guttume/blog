---
extends: _layouts.post
section: "content"
title: "How to learn any web framework easily"
author: "guttume"
date: 2021-07-03
---

Learning a web framework can be difficult, even if you have been programming for a long time. There are new terminologies to learn, new directory structures to understand, new tools to acquire, new conventions to follow, new style guides to adhere to, and the list goes on.

If you are also trying to learn a new web framework and struggling with these difficulties, you are not alone. In this article, I have listed down few dos and don't that I think when followed, will make your learning journey a little easier. So, lets start with Dos first:

### Dos

- **Have enough practice in your programming language.**

  First, make sure you have coded enough in your choice of programming language. This doesn't mean that you should be master of that language. Neither are you supposed to know each and every function and classes the language offers. You should at least be able to perform basic tasks and find ways to do advance stuffs whenever needed.

- **Get the basics of Object-Oriented Programming**

  Almost every framework uses Object-Oriented Programming pattern, so knowing how to think in terms of objects is a must.

- **Know how to use a dependency manager**

  I have yet to see a web framework, which does not use any sort of dependency management tool, so it is always recommended having a basic understanding. For example, Laravel, Symfony and other PHP frameworks, uses ***composer***. Django and Flask uses ***pip.*** Spring gives a choice between ***Gradle*** and ***Maven***.

- **Learn the fundamentals of web**

  Before starting to learn any web framework, get the understanding of web fundamentals. You should be familiar with how the web works and what is request response model. You should also know the different http verbs (**GET**, **POST**, **UPDATE**, **DELETE**, etc) and error codes (404, 302, 500, etc.). Try to learn and understand the headers that are interchanged between the web server and the client.

- **Understand MVC architecture**

  Most of the web frameworks uses MVC architecture, so make sure you familiar with the concept of model, view and controller. What are their roles and responsibilities and how they interact with each other

- **Familiarize yourself with the framework's directory structure**

  Almost all frameworks today, follow MVC architecture, still they differ in terms of how they organize their files and folders (directories). For example, Laravel keeps everything related to core functions inside the ***app*** directory, however, Symfony and Spring keeps them under ***src***. Django on the other hand uses completely different directory structure. Some frameworks like Flask allow you to use your own style.

- **Start small**

  Once you have a good grasp of the above prerequisites, the first step is to create a page using the web framework you are trying to learn. A page is the basic building block of a website or a web application. You should learn to make a simple route, attach it to a controller and display some content to the user. Once you learn that, proceed further with integrating database to make your application dynamic. Now, you are ready to make CRUD (Create, Read, Update and Delete).

- **Move on to advance stuffs**

  Once you are comfortable creating basic CRUD pages, next step will be to learn ORM (Object Relational Mapping). It can help you simplify and streamline the communication of the app with the database. Then gradually move on to advanced stuffs such as authentication, sending mails, processing tasks in background using queues, etc.

### Don'ts

Finally we are going to discuss, what are the things that you should avoid as beginner while learning a new web framework.

- **Don't get overwhelmed by the number of features and functionalities that a framework offers**

  All good framework provides a plethora of inbuilt features such as authentication, CSRF protection, mailing solution, queues etc. You may not need these features while you have just started to learn the framework. Hence, you should not get overwhelmed by the features and functionalities that the framework offers, rather, focus on the basics.

- **Don't be intimidated by the terminologies that a framework uses**
  
  The job of a framework is to provide you with same set of tools in order for you to accomplish the same thing while developing a web application. However, each of them differs in terms of the naming conventions they use. Let's consider a scenario, you want to modify a request or take some action based on the passed request before you process it and return response. Laravel and Django has ***middleware*** for it while Spring calls it ***interceptor.***

- **Don't bother about conventions and style guides your framework uses**

  Despite using same programming language, each framework may have their own coding conventions and style guide as well. Though it is necessary to follow a framework's conventions and guidelines, it can slow down your learning process. These conventions and style guides are there to handle big enterprise level projects but while you are at the beginner level, you will be dealing with smaller projects. In the beginning, you should focus on learning the basics instead.