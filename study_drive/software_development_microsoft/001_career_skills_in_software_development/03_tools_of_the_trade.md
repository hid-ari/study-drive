## Version control systems 

Programmers use version control systems to make updates to their code safely. Version control is the practice of tracking and managing changes to software code. 
#### Version control systems (VCS)

Version control systems, or VCS, are software tools that help software teams manage code changes over time. You can think of it as an account ledger. Each transaction, whether it's a deposit or withdrawal, is tracked. And each transaction contains information about the date, the amount, and the payee. Version control software behaves similarly. It keeps track of every change to the code. In a special kind of database, information is stored about what files changed, who changed them, when, and the purpose of the change. 

Version control is an essential part of a programmer's everyday practice. I don't know anyone who develops without it. 
#### The benefits of using version control:

![[Pasted image 20240807035624.png]]

Having the history of changes allows you to go back if you make a mistake. Recall, writing code is a lot of trial and error. You're bound to make mistakes. So having version control is your safety net. 

It allows multiple programmers to work on the project simultaneously while minimizing conflicts. Modern VCS lets programmers create copies of the code and make modifications that don't prevent the other programmers on the team from doing the same. Then, once everyone is ready, they can safely combine or merge their changes. 

And finally, having the code changes annotated helps with troubleshooting when a bug is discovered. Like our account ledger analogy, if the balance goes in the negative, you can find out which transaction was the culprit. Similarly, when there is a bug in the code, you can use the log of changes from your VCS to debug the cause. 

![[Pasted image 20240807040000.png]]
![[Pasted image 20240807043547.png]]One of the most popular version control systems today is Git. Git is free and open source. It exploded in popularity due to its branching model. A branching model defines the rules for making isolated code changes and then having those changes brought back into the main project. 

Git's branching model allows you to have multiple local branches that can be entirely independent of each other. And then, Git makes it fast and easy to merge those changes back into the main branch or discard them altogether. It's up to you. Version control systems help software teams work faster and smarter. It's an essential practice of modern-day software development.

### Code repository services

-Repository Hosting Services are third-party web applications that wrap and enhance a version control system.

One of the most popular version control systems today is Git, and the most popular hosting service for Git is GitHub. GitHub lets you an others work together on projects from anywhere.

It allows you to store your code in the cloud. This makes it available to you and others, regardless of your location. 

GitHub has a software package registry. The registry helps you to find libraries that are community approved. Third, GitHub has code review builtin. A code review lets other programmers on your team share feedback and ask questions about your work. It's an invaluable tool in many organizations to ensure the quality of the code changes. 

GitHub workflows. The workflows allow you to create a series of checks that must pass before a new version of the software is shipped to customers. It's a powerful feature that aids teams in delivering software constantly.

However, GitHub is not the only player in the repository hosting game. There are several others that offer similar functionality. 

Bitbucket  is great for teams that prefer a seamless integration with their other planning tools. Similar to GitHub, Bitbucket offers code review. Yet, if you're using their software tracking tool, Jira, you can take additional actions like creating tasks and issues directly in your code review.

Bitbucket also has pipelines. These pipelines allow you to run checks and ship software when everything looks good. And finally, you can receive real-time security alerts while you're developing code that can help to keep your project secure. 

Introduction to libraries and frameworks

As programmers, it's our job to develop software that meets a business need. And we often do this with a specific deadline in view. So it wouldn't make sense to write the same code again and again for each different software project we work on, especially if the code is already available and verified by other programmers. 

![[Pasted image 20240807105619.png]]

This is so common in computer programming that we call code that someone else has written and verified a library. Libraries are essential for modern software development, because otherwise, we would solve the same fundamental problems repeatedly. 

There's hardly any relevant software project that would not depend on software libraries one way or another. There are multiple types of libraries available. Many are used for general programming tasks like reading a file or performing mathematical calculations, while others may focus on graphics like displaying images or editing images. And the list goes on and on. 

Each library serves a particular purpose. Think of it as a tool in your toolbox. Here are some examples of programming libraries in various languages. 

Ktor is used with the Kotlin programming language. Ktor allows you to make network requests and handle the responses in your application, a common task in programming. 

NumPy is a Python library. It's often used in machine learning to help simplify the processing of large data sets, a handy tool in any ML engineer's toolbox. 

Now let's contrast a library with that of a framework. A framework is more than a simple tool. It serves as the blueprint for how your software project should be configured and developed. This is best understood with an example. 

Next.js is a popular framework for developing web applications. It defines the file structure you should use for your code to get the most out of the framework. It also defines rules when followed that allow your website to load quickly and efficiently. 

## Differente types of IDEs

![[Pasted image 20240807110343.png]]
Integrated development environment (IDE)

Is an application that provides special tools for programmers to write, debug, and compile code. 

Visual Studio Code, or VS Code for short, is a lightweight IDE. It was initially designed for the JavaScript and type script programming languages. However, it now supports many more languages through powerful extensions. One of its unique features is IntelliSense. IntelliSense allows you to get code suggestions while typing. This works a lot like auto complete when you're sending a text or searching in Google, and it guesses the rest of the phrase you're typing. 

IntelliJ IDEA. This IDE is optimized for the Java programming language. What's unique about IntelliJ is the built in support for spotting errors in your code and helping you to write code that follows best practices. It's one of my favorite IDEs. 

Sublime Text is blazing fast, lightweight, and powerful. So if you're working on projects that don't require the powerful features of a traditional IDE, Sublime Text is a great option. IDEs can ease the burden of software development by providing tools to help you write, debug, and run your source code more efficiently.

Learn where to get help

Finding answers when you run into issues writing your programs will make you a more successful programmer. It's a known fact that software tends to have bugs. 

Official documentation for your programming language. For example, here's the documentation for the Python programming language. You can find numerous code examples and explanations of what the various commands do and their expected behavior.

This will help you understand why your code may not be doing what you're expecting. When you come across errors that you don't understand, 

[Stackoverflow](https://stackoverflow.com/). Here you can copy and paste the error message, and then look for similar questions to what you're experiencing. Then, when you click on the question, you will often find a suitable answer.

You can turn to the numerous online tech communities that exist to provide support. For example

Kotlin community page. there are multiple ways to ask questions and get help. 

Don't give up when you run into issues. It's all part of the process.