# Different Languages

![[study_drive/software_development/0_imgs/Pasted image 20240808115402.png]]

Java, Python, and C++ all inherit ideas from C.

The C programming language was developed in the 1970s. It's used for programming operating systems, games, smart devices, and compilers for other languages. 
``` c 
#include <stdio.h>
int main() {
printf("Hello, World!");
return 0;
}
```
Here's an example of a short program in C. Let's walk through it. The include statement we see here on line number one is prevalent in C and C-based languages. It's how you as a programmer get to link to a library. Recall, a library is code that someone else wrote. 

Here in our example, we want to link to the standard input and output library available for the C language. This library includes the code for the printf function. We use printf to display our "Hello, World!" Message to the user. 

The main function is vital in C-based languages, because it's the function where the program begins. Next, the curly braces are used for the function's body. And finally, as our function's body, we call the printf function to display our message. Then the return 0 statement is the exit status of the program. In simple terms, the program ends with this statement. 

C is not your best choice if you're interested in making a desktop, web, or mobile application. It's a low-level language and requires more advanced programming knowledge. However, some of its successors are ideal for these types of applications.

Java. Java is a cross-platform programming language. This means that you can write your Java code, compile it, and then any device that has the needed program installed can run it. It's famous for desktop applications and backend services. It's a C-based language. But unlike C, everything in Java is in a class. We've already learned about classes and their importance in object-oriented programming. 

```java
public class Hello {
	public static void main(string[] args){
	System.out.println("Hello world!");
	}
}
```
Here's some Java code. It looks pretty close to what we saw with the C programming language with its use of curly braces, semicolons, and the main function. One of the new additions is the class keyword. This is how the Java language prefers to organize its code. 

C++, as the name hints at, is also a C-based language It's easier to learn and work with than C, because it adheres to the object-oriented programming style. C++ is used for games and game engines. 

```c++
#include <iostream>
using namespace std;

int main () {
cout << "Hello world!" << endl;
return 0;
}
```

Depending on what type of applications you want to develop, you'll have the chance to choose the one that suits your needs the most.

## Exploring variables across languages 

Variables in Java
```java
int age = 36;
System.out.println(age); //36
```
Variables in JavaScript
```js
const base = prompt('Enter the base of a triangle: ')
const height = prompt('Enter the height of a triangle: ')

const area = (base * height) / 2

console.log('The area of the triangle is ${area}');
```
Variables in Python

Rules 
- Contain letter, numbers, underscores
- Space are not allowed
- Names are case-sensitive
- Cannot be keywords
Use shorts, descriptive names.

## Making decisions in code across languages

Python 
```python
if 5 < 6:
print("yes")
```
Java
```java
if (5 < 6){
System.out.println("yes");
}
```
Ruby
```ruby
if 5 < 6
puts "yes"
end
```
Conditionals in Python start with an if followed by our condition, and we end the condition with a colon. Then on the next line, we indent the block of code that we want to be executed.

## Tech careers and their associated programming languages

Software developers create software applications that may include programs built for a specific task or those used in operating systems. Their job involves the design, testing and troubleshooting of software to meet the needs of various users. Several programming languages are commonly used, such as Java, Python, C++, C# and Ruby.

Web developer. How a website looks and functions are the direct results of a web developer's work. Web development can be a fulfilling career as you have a working website to show off your hard work at the end of a project. Web developers may specialize in front-end or backend development or work on both as a full stack developer. JavaScript, Python, Java and PHP are the primary languages that dominate this space. 

Network Systems administrator. These folks manage a company's servers, computer equipment, local networks and intranet. In addition, they have the challenging job of providing network security and preventing viruses from infecting the company systems. To work as a network systems administrator, you may write code in either Python, Bash or PowerShell. 

Mobile application developer.Mobile app developers create software applications that run on mobile devices like phones, watches and tablets. The most common programming languages for mobile development are Kotlin, Swift and Dart. 

Software quality assurance engineer, or QA engineer. QA engineers work to establish and maintain the high quality of software written by other programmers. You can find them writing code in Java, Python, JavaScript and more.