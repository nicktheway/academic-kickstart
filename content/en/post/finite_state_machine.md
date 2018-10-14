+++
title = "A random week day in fms."

date = 2018-10-05T18:00:00
lastmod = 2018-10-05T18:00:00
draft = true

authors = ["nicktheway"]

tags = ["programming", "ai"]
summary = "Lets create a simple state machine using C++ and lua!"

+++

Today, at the first university lecture on intelligent systems the word "state machine" was used again and again.
So, I though it would be fun to make a C++ state machine but it would be even better if I could pair it with 
Lua to bring the benefits of scripting in the whole thing.

Lets get on it!

To start, a way to bind lua to C++ is necessary. I could write one alone but for one thing, there is no time for that
and for another, an already made and tested implementation will be faster and easier to use.

After a little search I decided to use [**LuaBridge**](https://github.com/vinniefalco/LuaBridge).

To work with it I copied the source header files into the /usr/local/include directory and tested if that would work.
```cpp
#include <iostream>
#include <lua.hpp>
#include <LuaBridge/LuaBridge.h>

int main(void)
{
    lua_State* L = luaL_newstate();

    luaL_openlibs (L);
    std::cout << "No include errors!" << std::endl;

    lua_close(L);
    return 0;
}
```
Compiled with g++ and...
![Console success message](./img/post/finite_state_machine/1st_test_success.png)
