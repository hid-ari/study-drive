## Introduction to object-oriented programming


Object-oriented programming involves using code to represent real world things and situations. 

Let's look at an example. We'll take a puppy. What are some of their characteristics? They have a name, an owner, a breed, and maybe even a favorite toy. 
![[study_drive/software_development/0_imgs/Pasted image 20240808093645.png]]
What do they do? They can sit, play, and sleep. 
![[study_drive/software_development/0_imgs/Pasted image 20240808093711.png]]
With object-oriented programming, a puppy would be represented as a class with certain properties and functions. 
![[study_drive/software_development/0_imgs/Pasted image 20240808093738.png]]A class is a blueprint for how other objects should be created. For example, we can have a puppy named Marble and one named Onyx. 
![[study_drive/software_development/0_imgs/Pasted image 20240808093925.png]]They would represent two instances of our puppy class. Both marble and Onyx have the same types of properties and actions, but they are not the same. They have unique names, breeds, owners, and favorite toys. Moreover, Onyx could be playing while Marble is sitting. But what is the benefit of structuring code this way? One major benefit is that it helps to isolate code changes. 

There might be multiple actions that we want our puppies to take when they play. They could run, jump or roll over, and then maybe later we decide we actually want puppies to run, beg, and lie down. With object-oriented programming, We only need to make the changes in one place. Score. Additionally, this type of programming style has wide adoption across programming languages. You will be able to find numerous resources and examples to aid you in learning the concepts. By adding object-oriented programming to your arsenal, you'll be able to develop more flexible software with fewer bugs.

```python
class Puppy():
    def __init__(self, name, favorite_toy):
        self.name = name
        self.favorite_toy = favorite_toy

    def play(self):
        print(self.name + " is playing with the " + self.favorite_toy)

marble = Puppy('Marble', 'teddy bear')
marble.play()
```

## How to organize your code

Your files can get long and unruly as you add more code to your classes. To keep things neat and organized, it's best to store your classes in modules. A module is a file consisting of Python code. It can define functions, classes, variables, and more. There are several advantages to breaking up code in a large application. 

![[study_drive/software_development/0_imgs/Pasted image 20240808114112.png]]

The first is simplicity. A module typically focuses on one piece of the software puzzle. This makes development easier and less error-prone, as you don't have to keep the entire domain in your head at the same time. 

Maintainability, if you write your modules so that there's little dependency between them, it's easier to make changes that won't impact other parts of the program. This is also a great way to support collaboration on a shared code base among programmers, as you've reduced the chances of conflicts when changes are made. 

Finally, reusability. By placing code in modules, you open the door to reuse that code in other places throughout your application. 

Remember, programmers aim for code that adheres to the DRY principle of don't repeat yourself. By using modules, you'll be keeping with the overall Python philosophy of clean, uncluttered code.

## Adding modules to your programs 

 The Python standard library is a set of modules that you get with every Python installation. You can use code other programmers have written to handle more complex logic in your own programs. 
 
 To get a feel for how this works, we're going to make use of the random module. The random module is used to generate pseudo-random numbers. This means that numbers are not completely random. You're likely to see the same one again. 
 
If you were to work in artificial intelligence or data science, you'd run into this use case quite often. These programmers use random numbers to shuffle their data sets, establish configuration values for machine models, or even to prepare values for experiments.
 ```python
import random

numbers = [1, 2, 3, 4, 5]

random.shuffle(numbers)
print(numbers)

number = random.choice(numbers)
print(number)
```
 Use the import statement and then provide the name of the module that we want to import. In this case, it's going to be random. 
 
 When Python executes the import statement, it searches for random.py in the list of directories. If it finds the file, then it will make the module's functionality available to you. 
 
 Once the module is available, you can begin to access its functionality by using the module's name followed by a dot and then the functional variable you'd like to use. 
 
## How Python is used in the real world

Python is capable of handling almost any software development requirement. It heads libraries and packages for web browsers, databases, image manipulation, email, and much more.

1. Web development. Django is one of the most used Python web frameworks. It allows teams to not only create websites quickly but to keep scaling them for millions of new users. Some well known sites using Django are Spotify, Discus and Instagram. Next up, 

2. Animation. Blender is a free and open source tool used for 3D modeling, texturing, and animation. It is powered by Python and even exposes a way for programmers to create animations using Python code. 

3. Image manipulation. For example, the Scikit-Image library is used for analysis of medical images and to classify images for object detection in machine learning applications. 

4. Python's use in machine learning and artificial intelligence has caused it to explode in popularity over the past several years. 

5. Pandas is a data analysis and manipulation tool. It's built on top of the Python programming language. Machine learning engineers use libraries like Pandas to process large amounts of data from various sources. Python's ease of use, simplicity, and security make it a go-to language for software development.