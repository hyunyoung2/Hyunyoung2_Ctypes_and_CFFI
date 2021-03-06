# Ctypes tutorial 3

This is an example about how to use ctype to wrap the function written in c from python with library.

In this time,only I write the code, that is enough for you to understand how to use ctype for c function on python. 

If you read the tutorial1 and tutorial2. 

Let's see the code :

```c
/*flags.c : Source file*/
# include <stdio.h>
# include "flags.h"

int gFlag = 0;

void welcome_msg(char *msg){
   printf("%s\n", msg);
   return;
}

int get_flag(){
   return gFlag;
}

void set_flag(int flag){
   gFlag = flag;
   return;
}

/*flags.h : Header file*/
void welcome_msg(char *msg);
int get_flag();
void set_flag(int flag);
```

Let's make shared object with the codes above : 

> gcc -Wall -fPIC -c flags.c  
> gcc -shared -Wl, -soname, libflags.so.1 -o libflags.so flags.o

then Let's use the library called shared object, libflags.so. with ctypes

```python
import ctypes

mylib = ctypes.CDLL("./libflags.so")
mylib.welcome_msg("Hi C, I am from Python!")
```

In this case, that is differet result between python2 and python3.

```shell
# hyunyoung2 @ hyunyoung2-desktop in ~/konltk/Hyunyoung2_Ctypes-CFFI-SWIG/Ctypes/tutorial3 on git:master x [21:39:18] 
$ python3 example.py
H

# hyunyoung2 @ hyunyoung2-desktop in ~/konltk/Hyunyoung2_Ctypes-CFFI-SWIG/Ctypes/tutorial3 on git:master x [21:39:22] 
$ python example.py 
Hi C, I am from Python!
```

So if you have to change some in python file like the following :

```python
import ctypes

mylib = ctypes.CDLL("./libflags.so")

original_string = "Hi C, I am from Python!"

mutable_string = ctypes.create_string_buffer(str.encode(original_string))

mylib.welcome_msg(mutable_string)
~                                          
```

So the result is : 

```shell
# hyunyoung2 @ hyunyoung2-desktop in ~/konltk/Hyunyoung2_Ctypes-CFFI-SWIG/Ctypes/tutorial3 on git:master x [21:49:04] 
$ python3 example_py3.py
Hi C, I am from Python!

# hyunyoung2 @ hyunyoung2-desktop in ~/konltk/Hyunyoung2_Ctypes-CFFI-SWIG/Ctypes/tutorial3 on git:master x [21:49:11] 
$ python example_py3.py 
Hi C, I am from Python!
```

this article is from [here](http://karuppuswamy.com/wordpress/2012/01/28/how-to-use-c-library-in-python-generating-python-wrappers-for-c-library/) for my practice of how to use ctypes.


# Reference

  - [The basic exmample of ctypes](http://karuppuswamy.com/wordpress/2012/01/28/how-to-use-c-library-in-python-generating-python-wrappers-for-c-library/)
