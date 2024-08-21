## What is a programming language?

There are thousands of programming languages available. Every programmer has their favorite. 

Programming languages It's a way of providing instructions to a computer, written in text, and based on a set of rules. 

Computers only understand one language and that's called machine language. Machine language is highly complex for us to write directly because it's a long series of numbers. 

We don't code in machine language. Instead we use other programming languages that are closer to human languages. The code that programmers write is written in a particular programming language.

For example, many websites are written in the JavaScript programming language. Android applications, the Kotlin programming language. And video games, the C# programming language. And the list goes on and on. 

Each of these languages comes with its own set of strengths and weaknesses. Some are ideal for programming small devices with limited memory, whereas others were made to handle complex mathematical computations. 

Software projects rely on programming languages to achieve the desired goal of the business. Therefore, choosing the appropriate one will impact the outcome.
## Basic components of a programming language

A programming language can be broken down into two components, its syntax or rules and its semantics or meaning.

Programming languages have unique syntax rules. Let's look at an example. 

```python
print "hello!"
```

```js
document.write('Hello!');
```
**Syntax and Semantics in Programming**

Understanding syntax and semantics is crucial in programming. These two concepts form the foundation of how we write and understand code.

### Syntax

**Definition:**
Syntax refers to the set of rules that define the structure and form of a valid program in a specific programming language. It is similar to grammar in natural languages.

**Key Points:**
- **Syntax Rules:** These are predefined rules that dictate how code should be written so that the compiler or interpreter can correctly understand it.
- **Syntax Errors:** If the code does not follow the correct syntax, the compiler or interpreter will throw an error. Common syntax errors include missing semicolons, incorrect indentation, unmatched parentheses, and incorrect use of keywords.
- **Examples:**
  - In Python, proper indentation is crucial:
    ```python
    def say_hello():
        print("Hello, World!")
    ```
  - In Java, each statement must end with a semicolon:
    ```java
    System.out.println("Hello, World!");
    ```

### Semantics

**Definition:**
Semantics refers to the meaning and behavior of the code. It deals with what the statements, expressions, and program constructs mean and how they interact when the program runs.

**Key Points:**
- **Correct Semantics:** Even if the syntax is correct, the program must also make sense logically. Semantics ensures that the code does what it is intended to do.

- **Semantic Errors:** These errors occur when the syntax is correct, but the logic is flawed. Examples include division by zero, accessing an array out of bounds, and infinite loops.

- **Examples:**

  - A semantic error in Python:
  
```python
x = "10"
y = x + 5  # This will cause a runtime error because you cannot add a string and an integer.
```

  - Correcting the semantic error:
  
  ```python
x = 10
y = x + 5  # Now y will be 15
   ```

### Relationship Between Syntax and Semantics

- **Syntax is About Structure:** It is concerned with the form of the code. For example, ensuring the parentheses and braces are matched and the keywords are correctly placed.
- **Semantics is About Meaning:** It focuses on the logic and the intended operations of the code. For example, ensuring that the operations performed are meaningful and lead to the correct outcomes.

### Importance in Programming

- **Compiler/Interpreter:** The compiler or interpreter checks the syntax first. If the syntax is correct, it then checks the semantics. If both are correct, the code runs.
- **Error Detection:** Understanding syntax and semantics helps in identifying and fixing errors in the code. Syntax errors are usually easier to spot and fix compared to semantic errors, which might require a deeper understanding of the code's logic.
- **Code Quality:** Writing syntactically correct and semantically meaningful code ensures high-quality, maintainable, and bug-free software.

Syntax is the concept that concerns itself only whether or not the sentence is valid for the grammar of the language. Semantics is about whether or not the sentence has a valid meaning.

## Source code

Source code is what we call the human-readable computer instructions written by programmers. It's written in plain texts. We write it without special formatting, like bold, italic, or different font types. It's mostly just the actual characters. This means that word processing applications aren't suitable for writing code because by default they insert bits of information in files that prevent them from being plain text. This is one reason why programmers choose to use IDEs, or Integrated Development Environments, to write code.

## Running source code 

Computers only speak one language, machine language. So when we write code in other programming languages, we need a way to have it converted into machine code.

For example, programmers often say, "I need to compile my code." Compiling is one way that human-readable code is converted into machine code. 
![[study_drive/software_development/0_imgs/Pasted image 20240807114201.png]]
You can think of it as an assembly line. On one end is the human-readable code and on the other, machine code. The compiler goes through various steps to transform the code you've written as a programmer into something that the computer can understand and execute.
## Variables
![[study_drive/software_development/0_imgs/Pasted image 20240807120415.png]]
In algebra class, you were introduced to variables. Remember seeing formulas like this where both x and y represented variables

A variable was a placeholder for an unknown value. Similarly, in programming, we use variables as a container for a value. When we run our programs, the computer gives us space in its memory where we can put data that we want to use as a reference for later. This data is called a variable. 

Variables are an essential part of programming, because we need a way to store information. 

Think about your favorite online game. A programmer will need to keep track of dozens of variables. For example, I like to play word games online. Besides the score for each player, there's the total number of tiles left to play, the value of each tile, bonus points, and more. Each of these values are variables stored in the computer's memory. 

To visualize this, imagine each variable is a jar on a shelf.
![[study_drive/software_development/0_imgs/Pasted image 20240807120622.png]]
One jar holds your score, another, the total number of tiles, and yet another, the number of tiles left. As the game proceeds, the values of the variables may change. 

For example, if you score, the value of their score variable changes, and the previous score is gone from memory. Also, when you make a new word, the number of tiles left will decrease. But then, when it comes to the total number of tiles, that's a variable that doesn't change during gameplay. 

These are special types of variables that we call constants, because their values don't change. Variables are vital in any programming language as they allow you to write flexible programs. Can you imagine playing your favorite game and the moves are always the same, the score, always the same, you would lost interest quickly. Rather than assuming how the user will interact with your program, variables allow you to leave it up to them, a more engaging experience all around.

## Basic statements and expressions

Statements are the building blocks of any program. They are the individual actions that you want your program to take. Each statement can be made up of keywords, expressions, and operators. 
![[study_drive/software_development/0_imgs/Pasted image 20240807120923.png]]
For example, if we take the following, 10 + 2, what does that equal? 12, correct.  Let's try another one. 10 / 2 + 3. What does this evaluate to? Yes, it's 8. 

The result of these operations that break down to a single value are called expressions. You'll encounter various types of expressions in programming. The result may be a number, as we've seen a few examples of here. Or it could be other values, like true or false or something else entirely.

Keyword is a reserved word that means something special to the given programming language. Let's take a look at a keyword in the Ruby programming language. The keyword is unless. 
![[study_drive/software_development/0_imgs/Pasted image 20240807121112.png]]
This means we can't create our own variable with the same name. The program would not compile, as the keywords need to be used according to the rules of the language. Bear in mind, each programming language has a set of reserved keywords useful for building robust statements. Computer programs are made up of statements, statements that can perform a mathematical calculation, print something to the screen, or even make a choice between two code paths. This powerful concept is the foundation of all code that programmers write.