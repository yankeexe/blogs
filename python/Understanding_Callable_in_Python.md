## Understanding Callable in Python

Functions and classes are the most common things we use in our daily development. We invoke them, pass them around and yet never wonder what makes them so amazing. Well the short answer is **callable protocol;** for the long answer keep reading!

### **Definition** 📢

Objects defining the `__call__` method is known as callable. Or basically callable is anything you can call using the parenthesis () and pass arguments to it. Yes I am basically talking about a function.

### **`__call__` method** 🤙

`__call__` is one of the most interesting dunder method in Python. It is what most of the built-in functions make use of. If we peek into the type of some of these built-in functions then we often see the result as a class.

```python
>>> range
<class 'range'>

>>> zip
<class 'zip'>

>>> int
<class 'int'>
```

You get the gist. But how is a class acting as a function? I mean you can just `zip(iterable1, iterable2)` and you get your result without further invoking any other methods of the class.

**Just imagine using zip like if it were a normal class.**

```python
processor = ['Intel', 'Ryzen', 'Apple Silicon']
year = [2018, 2019, 2020]

zipped = zip(processor, year)
zipped.start()
```

This definitely isn’t intuitive to work with. So how does it work?

### **Using `__call__` method** 🔨

Well behind the scenes it’s because of the `__call__` method. This method is used in classes if we want the **instance of the class** to be callable.

What do I mean by that? Let’s take an example of a class that prints the square of a number.

```python
class Square:

    def __call__(self, num):
            print(num * num)

>>> sq = Square() # create an instance
>>> sq(5) # invoke
25
```
This is similar to executing:

```python
>>> sq = Square()
>>> sq.__call__(5)
25
```

But with the `__call__` dunder method we don’t have to do that. We can directly invoke the instance like a normal function.

> You may have noticed, we don’t need to use the `__init__` constructor to pass the value. Since the class instance acts like a function, then it can take arguments like a function without a need of a constructor.

### **Testing callable** 👨‍🔬

A class or a function is callable by default; but the instance is callable with `__call__` dunder method only. We can check this by using the `callable()` function.

Take for example a class without a `__call__` method.
```python
    class Random:
        pass

    >>> callable(Random)
    True

    >>> r = Random()
    >>> callable(r)
    False
```
On the contrary, let’s take our Square class from earlier example.
```python
    >>> callable(Square)
    True

    >>> callable(sq)
    True
```
### **Conclusion** 🚀
Next time when you’re creating a class for something and it needs to return a value in an instant; you can make use of the `__call__` method.

I hope this blog has been a help to understand an underlying concept involved in functions and classes. If you have any queries or suggestions, let’s discuss them in the comment section.
