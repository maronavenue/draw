
![title](/tree/resources/kimono.png)

# \#1. Python is and always will be an `Open Kimono` language.


## Open kimono
aka `open door policy`. To open the kimono means to reveal the inner workings of a project or company to an outside party.

In fact, Guido said it himself in one of his [web blogs](http://www.artima.com/weblogs/viewpost.jsp?thread=211430):
  
> **Q:** Will Python 3000 provide "real" private, protected and public?!  
> **A:** No. Python will remain an "open kimono" language.

## Nothing is ever really private in Python
Python trusts you. It tells you:

> "Hey, if you want to go poking around in dark places, I'm gonna trust that you've got a good reason and you're not making trouble." `After all, we're all consenting adults here.`

## It's very much like Perl
Perl culture is like Python in this respect, but Perl expresses the
sentiment a bit differently.

As the Camel book puts it,

> "A Perl module would prefer that you stayed out of its living room because you weren't invited, not because it has a shotgun."

But the sentiment is identical.

## What about dunders?!
Dunders (_aka "double underscores"_) are designed mostly to avoid accidents. However, all it ever does is some funky `name mangling`.

SINGLE UNDERSCORE | DUNDER
--- | ---
\_single_leading_underscore | \_\_double_leading_underscore
weak "internal use" indicator | invokes "name mangling"
does not import objects whose name starts with it | prevents accidental namespace collisions

However, it's still possible for a determined soul to access or modify a variable that is considered "private".

## Let's check it out:



```python
class Student:
    
    def __init__(self):
        self.__name = "Maron Montano" # Too uptight
        self.course = "Computer Science" # No mangling
        self._school = "DLSU" # Indicate private by convention

s = Student()

#dir(s)

#print(s._Student__name)

#print(getattr(s, '_Student__name'))
```

![title](/tree/resources/tag.png)

# \#2. Other languages have `variables`. Python has `name tags`.

## C: Variable assigment
> Can you describe how assigment statement works below?
```c
int a = 1;
```
> What happens when we change the value of the variable?
```c
int a = 2;
```
> How about assigning one variable to another?
```c
int b = a;
```

You may think of them as boxes that contain the values.

## Python: Name binding
Unlike boxes, Python variables work more like name tags.

> When you assign, it tags the value with the variable name:
```python
a = 1
```
> And if you change the value, it changes the tag to that new value in memory:  
```python
a = 2
```
> Assigning one variable to another makes a new tag to the same value:
```python
b = a
```

> _**NOTE: A value that is no longer bound with a name or tag will automatically be `garbage-collected`._

## Memory management
In summary, we learned that a value will have `only one copy in memory` and all the variables having this value will refer to this memory location.

## Let's check it out:


```python
foo = 10
bar = 10
baz = 10

id(foo), id(bar), id(baz)
```


```python
foo += 1

id(foo)
```

As we can see, we see the same identical IDs when we assigned `10` to all three variables (or names). However, what if I told you that this assumption does not hold true all the time? 


```python
x = 500
y = 500

id(x), id(y)
```

## Wait, whaa-?!
What happened here? We got two different IDs. No worries, these are actually effects of CPython implementation.

It keeps an array of integer objects for all integers between `-5 and 256`. In other words, the integers between -5 and 256 are `singletons`.

This is possible because integers are immutable, as one cannot change their value.

## Let's take a look at strings:


```python
s1 = 'hello'
s2 = 'hello'

id(s1), id(s2)
```


```python
s1 == s2
```


```python
s1 is s2
```


```python
s3 = 'hello, world'
s4 = 'hello, world'

id(s3), id(s4)
```


```python
s3 == s4
```


```python
s3 is s4
```

## Interning
Looks interesting isn't? When the string was simple and shorter, names refer to the same object in memory. But when they became bigger, this was not the case.

This is called `string interning` and CPython does it to some extent at `compile-time` and is `run-time independent`.

Interning is done automatically and it is always a trade-off between `memory use vs the cost of string comparison`.

## String equality test
![title](/tree/resources/string_equality_test.png)

## Conclusion
In general, Python creates a new object in memory for each value with the exception of interning to optimize performance.

There's a built-in `sys.intern(string)` (`intern()` in Python 2) that enforces interning string objects when necessary.

Hey. When do you think interning makes sense?


```python
class Foo:
    pass

bar = Foo()
baz = Foo()

id(bar), id(baz)
```

![title](/tree/resources/citizen.png)

# \#3. Everything in Python is `First-class Citizen`.

## First-class everything
From [Guido's blogpost](http://python-history.blogspot.com/2009/02/first-class-everything.html):
> 1. One of the goals in implementing Python is to make everything of equal status (e.g. integers, strings, functions, classes, modules, methods, etc.)
> 2. All Python objects were based on a common C data structure that were used all over the interpreter.
> 3. It didn't matter if the `struct` happened to be a simple object such as an Integer or something complicated like a class.

## What are first-class citizens?
In general programming context, these are objects that can be dynamically created, destroyed, passed to a function, returned as a value, and have all the rights as other variables in the programming language have.

Depending on the language, this can imply:
> 1. being expressible as an anonymous literal value
1. being storable in variables
1. being storable in data structures
1. having an intrinsic identity (independent of any given name)
1. being comparable for equality with other entities
1. being passable as a parameter to a procedure/function
1. being returnable as the result of a procedure/function
1. being constructible at runtime
1. being printable
1. being readable
1. being transmissible among distributed processes
1. being storable outside running processes

## WIIFY
Grokking these concepts intuitively will make understanding advanced features in Python like `lambdas` and `decorators` much easier. It also puts you on a path towards `functional programming` techniques.

## Prelude
Throughout this topic, let's use this function as an example for easy and intuitive recognition:


```python
def yell(text):
    return text.upper() + '!'

yell('hello')
```

## Functions can be assigned to variables
Like any other, there's nothing particularly special about functions in Python.

Because the `yell()` function is an object, you can assign it to another variable:


```python
bark = yell # Does not invoke the callable method of yell(); instead takes the function object reference
bark('woof')
```

A variable pointing to a function and the function itself are two separate concerns:


```python
del(yell)
yell('hello?')
```


```python
bark('hey')

# bark.__name__
```

## Functions can be stored in data structures
For example, you can add functions to a list:



```python
funcs = [bark, str.lower, str.capitalize]

funcs
```

Accessing the functions through the list works like it would with any other type of object:


```python
for f in funcs:
    print(f, f('hey there'))
```

You can even call a function object stored in the list without assigning it to a variable first:


```python
funcs[0]('heyho')
```

## Functions can be passed to other functions
Because functions are objects, you can pass them as arguments to other functions.

Here’s a `greet()` function that formats a greeting string using the function object passed to it and then prints it:


```python
def greet(func):
    greeting = func('New years resolution anuna')
    print(greeting)
```

You can influence the resulting greeting by passing in different functions. Here's what happens when we pass in `yell()`:


```python
greet(bark)
```

Of course, we can define a new function to generate a new flavor of greeting:


```python
def whisper(text):
    return text.lower() + '...'

greet(whisper)
```

The ability to pass functions to other functions is `very powerful`. It allows you to abstract away and pass around `behavior` in your codes.

Functions that can accept other functions as arguments are formally called `Higher-order functions`.

This is paramount to `functional programming` paradigm.

Feels familiar? Some of you might be using it already via the built-int `lambda operators`:
> 1. map(func, sequence)
2. filter(func, sequence)
3. reduce(func, sequence)

## Quick example:


```python
list(map(bark, ['hello', 'hey', 'hi']))
```

## Functions can be nested
Python allows functions to be defined inside other functions. These are often called `nested functions` or `inner functions`. For example:


```python
def speak(text):
    def jejemonize(t):
        return t.lower() + '...'
    return jejemonize(text)

speak('Hello World')
```

Now, what’s going on here? Every time you call `speak()` it defines a new inner function `whisper()` and then calls it.

And here’s the CATCH -- `whisper()` does not exist outside `speak()`:


```python
jejemonize('yo')
```


```python
speak.jejemonize
```

But what if you really wanted to access that nested whisper function from outside speak?

Well, functions are objects—you can return the inner function to the caller of the parent function:


```python
def get_speak_func(jeje_level):
    def jejemonize(text):
        return text.replace('o', '0')
    def jejemonize_v2(text):
        return jejemonize(text).replace('a', '@')
    if jeje_level < 2:
        return jejemonize
    else:
        return jejemonize_v2

tell = get_speak_func(1)
tell('Hello sa inyo!')

tell = get_speak_func(2)
tell('Hello sa inyo!')
```

## The famous `@property` decorator
To wrap it all up, let's dive into the `@property` decorator which is arguably the most popular decorator I saw in [gitlab.factset.com](gitlab.factset.com) and [github.factset.com](github.factset.com) codebases. We love to abuse it. But what does it actually do...


```python
# Typical example

class BenchmarksEngineer:
    def __init__(self, name, team):
        self._name = name
        self._team = team
    
    @property
    def name(self):
        return self._name
    
    @name.setter
    def name(self, name):
        self._name = name
    
    @property
    def team(self):
        return self._team
    
    @team.setter
    def team(self, team):
        self._team = team

me = BenchmarksEngineer('Maron Montano', 'BDI')
print("Hi, I am {} from {} team.".format(me.name, me.team)) 

me.name = 'Yvette Robles'
me.team = 'EQB'
print("Hi, I am {} from {} team.".format(me.name, me.team)) 
```


```python
#property()
#property().getter
#property().setter
#property().deleter

print(property())
print(property().getter)
print(property().setter)
print(property().deleter)

print(property().getter(None))

@property
def foo(self):
    return self._foo

# just a syntactic sugar :P

def foo(self):
    return self._foo
foo = property(foo)
```


```python
# Let's create a full-on property

def getter(self): print('Get!')
def setter(self, value): print("Set to {}!".format(value))
def deleter(self): print ('Delete!')

prop = property(getter)
prop.fget is getter
prop.fset is None
prop.fdel is None

prop = prop.setter(setter)
prop.fget is getter
prop.fset is setter
prop.fdel is None

prop = prop.deleter(deleter)
prop.fget is getter
prop.fset is setter
prop.fdel is deleter
```




    True



## `property()` function returns a `special descriptor` object:
Let's delve in deeper into its actual pure Python sample implementation:

```python
class Property(object):
    "Emulate PyProperty_Type() in Objects/descrobject.c"

    def __init__(self, fget=None, fset=None, fdel=None, doc=None):
        self.fget = fget
        self.fset = fset
        self.fdel = fdel
        if doc is None and fget is not None:
            doc = fget.__doc__
        self.__doc__ = doc

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        if self.fget is None:
            raise AttributeError("unreadable attribute")
        return self.fget(obj)

    def __set__(self, obj, value):
        if self.fset is None:
            raise AttributeError("can't set attribute")
        self.fset(obj, value)

    def __delete__(self, obj):
        if self.fdel is None:
            raise AttributeError("can't delete attribute")
        self.fdel(obj)

    def getter(self, fget):
        return type(self)(fget, self.fset, self.fdel, self.__doc__)

    def setter(self, fset):
        return type(self)(self.fget, fset, self.fdel, self.__doc__)

    def deleter(self, fdel):
        return type(self)(self.fget, self.fset, fdel, self.__doc__)
```


```python
# Touche

class Foo: pass

prop.__get__(Foo(), Foo)
prop.__set__(Foo(), 'bar')
prop.__delete__(Foo())
```

    Get!
    Set to bar!
    Delete!
    

![title](/tree/resources/coffee.png)

# That's all for now. Time for some coffee break. For the next episode:


```python
for i in range(0,216):
    if (i % 2 == 0):
        print("coffee", end=" ")
    else:
        print("break", end=" ")
```

    coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break coffee break 

![title](/tree/resources/hobgoblin.png)

# \#4. A Foolish Consistency is the Hobgoblin of Little Minds.
Understanding [PEP-8](https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds) is a big step in writing better Python code. Let's discuss what is and what not.


```python
import this
```

![title](/tree/resources/ducktype.png)

# \#5. It's Easier to Ask for Forgiveness than Permission (`EAFP`).

We'll talk about EAFP vs LBYL coding styles, and how Python's philosophy capitalizes on the former. Bonus: We'll demyth something about Python's `typesystem`.


```python
duck = ['if', 'it', 'looks', 'like', 'a', 'duck', 'and', 'quacks', 'like', 'a', 'duck', 'it', 'is', 'a', 'duck']

for type in duck:
    print(type)
```

![title](/tree/resources/oop.png)

# \#6. Everything is an Object in Python.
There is more to saying "everything is object in Python." I mean, like really.

`Everything is an object in Java` is completely different than `Everything is an object in Python`.

`Everything is an object in Ruby` tells a slightly different story too. :)


```python
z = lambda x: print(x.capitalize())

for word in ['metaclass', 'is', 'all', 'im', 'saying']:
    z(word)
```
