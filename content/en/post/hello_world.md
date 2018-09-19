+++
title = "Hello world!"

date = 2016-04-20T00:00:00
lastmod = 2018-09-18T18:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = []

tags = ["programming"]
summary = "The way to start learning a programming language."

#highlight_languages = ["C", "C++", "C#", "java","PHP", "python"]

[header]
image = "headers/hello_world.png"
caption = "Image credit: [**medium**](https://medium.com/@thiagonascimento/time-to-first-hello-world-11a4735602f2)"

+++

A program that prints **Hello world** is usually the first anyone will write when learning a new programming language.
It is now a tradition and if one does not uphold it who knows what might happen! Rumor has it that the bugs will 
torture h@ forever.

{{% alert note %}}
"h@" stands for him/her/whatever
{{% /alert %}}

The rumour might not be true but -just in case- lets see how "Hello world" can be printed with some of the most famous
programming languages in order to start -safely- our learning trip.


#### Python/Lua
```
print("Hello world")
```

Hmmm simple enough.

#### Coffeescript
```
console.log "Hello world"
```
That's easy.

#### Bash
```
#!/bin/bash
echo Hello world
```
Bash seems simple too :smirk:.

#### PHP
```
<?php echo "Hello world";
```

<?php What?!?.

#### Go
```
package main

import "fmt"

func main() 
{
    fmt.Println("Hello world")
}
```
Ok...lets see what *fmt* stands for:

1. full moon time :x:
2. free monster trigger :x:
3. format :o:

#### C\#
```
using System;

class HelloWorld
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello world");
    }
}
```
A Class for this?

#### Java
```
class HelloWorld
{
    public static void main(String[] args) 
    {
        System.out.println("Hello world");
    }
}
```
Again?

#### C
```
#include <stdio.h>

int main(void)
{
    printf("Hello world\n");

    return 0;
}
```
Good.

#### C++
```
#include <iostream>

int main(void)
{
    std::cout << "Hello world" << std::endl;

    return 0;
}
```
Ok, I guess.

### Bonus
#### Assembly
```
section     .text
global      _start                              ;must be declared for linker (ld)

_start:                                         ;tell linker entry point

    mov     edx,len                             ;message length
    mov     ecx,msg                             ;message to write
    mov     ebx,1                               ;file descriptor (stdout)
    mov     eax,4                               ;system call number (sys_write)
    int     0x80                                ;call kernel

    mov     eax,1                               ;system call number (sys_exit)
    int     0x80                                ;call kernel

section     .data

msg     db  'Hello, world!',0xa                 ;our dear string
len     equ $ - msg                             ;length of our dear string
```
PERFECT!

[Source for the assembly code](http://asm.sourceforge.net/intro/hello.html)