# NodeJS Language Notes
_Reference: <https://www.udemy.com/understand-nodejs/learn/v4>_

# Vocabulary

# Concepts

Node is written in C++
V8 is also written in C++

## The V8 Javascript Engine
A javascript engine is a program that converts javascript code into something the computer processor can understand. It should follow the ECMAScript standard on how the language should work and what features it should have.

The V8 engine is written in C++, it can run as a standalone, but it can also be embedded into other C++ programs. 
Essentially, what V8 is doing is taking in javascript and compiling it down to machine code.
By embedding V8 into a C++ application, you can ADD features to Javascript by writing additional C++ hooks that trigger when certain phrases are typed in the js code. 

Adding features like this is extremly powerful and it is what has allowed NodeJS to become a reality. This is the reason you can now code full stack in JS!  

## Processors, Machine Code, and C++



