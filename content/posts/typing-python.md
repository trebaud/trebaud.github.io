---
title: "Typing Python with typing"
description: "Learn how to add types to your Python code"
date: 2017-12-27T00:00:00-05:00
tags: [python]
draft: false
---

I've always been a big fan of the Python programming language. It's great for prototyping, it has great libraries for data processing and machine learning and it's overall just a joy to read and write with. But I also like types and if you're at all familiar with the language then you know that variables are dynamically typed in Python and that type information is absent from the basic syntax.

Don't get me wrong I love Python's dynamically typed model, it's what gives the language its distinct flexibility and fast prototyping capabilities. But it's undeniable that for certain tasks and projects, types add great value. They give you type hinting and help in catching obvious and not so obvious bugs, some that may only manifest themselves under some specific corner cases.

## Typing as your new best friend

Now since Python 3.6 you can use a module called typing to static type your variables if you want to. The neat thing about typing is that you can choose to include type information at specific points in your code where you feel there is a need to do it. The syntax to declare a variable type goes like this:

```python
variable_name: type
```

In functions you can also declare the return type like this:

```python
def function_name(param: type)-> return_type:
```

Typing provides you with container types like `Dict`, `List`, `Set` or `Tuple`.

```python
from typing import Dict, List, Tuple

primes: List[int] = [2,3,5,7,11,13]

primes_dict: Dict[str, int] = {
    "two": 2,
    "three": 3,
    "five": 5,
    "seven": 7,
    "eleven": 11,
    "thirteen": 13
}

isPrime_tuple: Tuple[str, int, bool] = ("one", 1, False)
```

I personnaly like this kind of syntax even if it might make the most hardcore python purist silently scream in cringe and agony. I feel it makes my code more readable and predictable but that might only be my Java part of the brain talking.

You can even define your own types by composing primitive types.

```python
from typing import Tuple

Vector = Tuple[float,float,float]

vector: Vector = (1.0,1.0,1.0)

Matrix = Tuple[Vector,Vector,Vector]
matrix: Matrix = (
    (1.0,0.0,0.0),
    (0.0,1.0,0.0),
    (0.0,0.0,1.0)
)
```

Or with classes.

```python
import typing

class Like:
    def __init__(self: Like)-> None:
        pass

class Pie(Like):
    def __init__(self: Pie)-> None:
        pass

I: Like = Pie()
```

If you want to declare a function that takes parameters of multiple types you can use `Any`.

```python
from typing import Any

def whatIsMyType(param: Any):
    return type(param)

print(whatIsMyType(42))

print(whatIsMyType("42"))
```

## Using mypy as your new tool for type checking

To type check your newly written programs with typing your best choice is probably to use [mypy](https://github.com/python/mypy), a static type
checker you can run before calling the python interpreter.

A cool thing you can do with this is build a new execution pipeline combining the static type checker and the Python interpreter in a single program
executable. If there is a type error the program won't run the interpreter and will output the type error messages, otherwise it will execute the python script.

```python
#!/usr/bin/python3.6

import subprocess
import sys

def type_check(script):
    output = subprocess.run(['mypy', script], stdout=subprocess.PIPE).stdout
    return output.decode('utf-8')

def run(script):
    output = subprocess.run(['python3.6', script])


if __name__ == '__main__':
    script = sys.argv[1]
    mypy_output = type_check(script)

    if mypy_output != '':
        print(mypy_output)
    else:
        run(script)
```

Make this program executable and send in the python script you want to run as an argument.

```bash
~$ chmod +x myPython
~$ ./myPython script.py
```

And voila you now have a brand new python interpreter which runs a type checker prior to running the actual program.

That's it! I hope you found this useful and might at least try it out of curiosity in some of your codebases. I think you'll find there's some merit to typing your variables in Python, even if it might slow you down in the short term, it might actually make you gain time in the longer run.

Bye for now!
