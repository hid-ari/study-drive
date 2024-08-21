## Introduction to python
![[study_drive/Software_Development/0_imgs/Pasted image 20240807235811.png]] 
Python is ideal for those who are starting out with programming, because it has few syntax requirements. Additionally, Python code tends to be more concise meaning it takes fewer lines of code to get things done.

Python was designed to be a general purpose language. You can use it to create web applications, perform scientific analysis, create games, and more.

## Basic Python syntax 

```python
print("Hello World!")

print('Hello Wordl!')

print(43)
```
## Saving data with Python
```python
# The user's score
score = 750
print(score)

# My favorite snack
favorite_snack = "Cookies"
print(favorite_snack)
```
## Making decisions with Python
Any expression that breaks down to either true or false is called a conditional or boolean expression.

Boolean expressions, boolean expressions are useful in helping you to make decisions in your code. The most common way for a computer to make decisions based on a condition is using an if statement. An if statement has this general structure, it starts with the word if, that's where it gets its name from then it checks some condition and if that condition is true then it will do the work you've placed inside of it. To visualize this, think of it as a flow chart 
![[study_drive/Software_Development/0_imgs/Pasted image 20240808001141.png]]
![[study_drive/Software_Development/0_imgs/Pasted image 20240808001423.png]]
```python
having_fun = "yes"
if (having_fun == "yes"):
print("Glad you're having fun")
```
## Functions in Python

Programmers attempt to write simple, easy to read, and unrepetitive code. Functions help us do this. A function is a block of code grouped together with a name. 

```python
def say_hello(name):
print(f"Hello, {name}!")

say_hello('Tiffany') 
```

