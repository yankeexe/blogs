##  Understanding Iterators and Iterables in Python

Iterator and Iterable are general terms that gets thrown around a lot in Python. But what do they mean? Are they the same? We will try to understand what these objects in Python are and debunk some misunderstandings.

Along the line we will also see a high-level overview of how for loop and range function might be working under the hood. FYI these concepts as a whole is also known as the **Iterator Protocol.**

So let’s get started 🔨

### Iterable ➰

In general terms, anything that we can loop over is an iterable. I think the name itself gives it away. Most of the data structures in Python is an iterable like List, Tuple, Dict and so on.

You don’t have to believe me if I say a List is an iterable, you can see for yourself. We will be using the built-in `dir()` function to analyze the properties and methods that a list object consists.

```python
fruits = ['apple', 'banana', 'mango'] # Create a list.
print(type(fruits)) # make sure it is a List object. :D
print(dir((fruits))
```

![Checking for iter method in an object. 🔎](https://cdn-images-1.medium.com/max/2722/1*AZSzv6QKI_bR__eKICWddA.gif)*Checking for iter method in an object. 🔎*

You can find the `__iter__` method in the list, which means it is an iterable. All the objects that are iterable has the method `__iter__`. If you ever get confused, this handy knowledge might help you identify an iterable.

### Iterators ⏭

Now that we know what iterables are and how to identify them, let’s get our feet wet with iterators.

In general, iterator is an object that makes iteration possible. Sounds vague but let me explain. While iterables are the object that we can iterate over; iterator is the implementation that keeps track of the state of the iterable like what value has been returned, what value to return next and also returns the value accordingly.

**So how do we use it ?**
Remember the `__iter__` method we analyzed in our iterable example? Well, if we invoke the `__iter__` method of our iterable, it returns an iterator object.

![Identifying `iterator` type and its methods. 🔎](https://cdn-images-1.medium.com/max/2748/1*_GkGqTJK9Bs6lONV6Yn6RQ.gif)*Identifying `iterator` type and its methods. 🔎*

```python
fruits = ['apple', 'banana', 'mango']
fruity = fruits.__iter__()
print(type(fruity))
```

Again, you don’t have to believe what I say, you can check for yourself if it is an iterator using the `dir()` method. Like iterables, iterators can be identified using the `__next__` method.

```python
print(dir(fruity))
```

**So what does __next___ do?**

Next returns the item in a list, one at a time.

### Differences?

You might have noticed the iterator also consists of the `__iter__` method, so we can say that:
> Every iterator is also an iterable but not every iterable is an iterator.

## Hands on 🙌

### Implementing for loop manually

For loop makes use of the concepts of iterator and iterables to return value. Let’s see how we can implement the for loop ourselves.

```python
fruits = ['apple', 'banana', 'mango']
fruity = fruits.__iter__() # alternative:  iter(fruits)
fruity.__next__() # alternative: next(fruity)
```

You may notice that our next method only returns a single value. What if we want the next item in the list? Well, you call the same method again.

But did you notice, when calling the method again, it doesn’t return value from the start of the list, but carries on to return the next item? This is because iterator is maintaining the state of the items and knows exactly which item to return next.

![Manual implementation of `for` loop ➿](https://cdn-images-1.medium.com/max/2748/1*cR3jN_bvqNQI1imOOsTQ7g.gif)*Manual implementation of `for` loop ➿*

So in our example, we have to call the next function on our iterator object three times to return all the values in the list. But what if we run it four times? Well there is nothing to return as we have already exhausted our list by returning everything. So the iterator throws an `StopIteration` error.

Well this was a bit redundant isn’t it? What if our list had 1000s of items? We already know the internals so let’s move ahead and automate the process. We are not going to invoke `next()` for each item 🤷‍♂️.

For that we can make use of class to implement them automatically. For this you need some knowledge of OOP, but I will try my best to explain it using code comments.

### Abstracting for loop implementation using a class.📦

We will create a class that will somewhat act like a for loop and return all the elements of an iterable.

```python
class Loop:
    def __init__(self, some_iterable):  # constructor
        self.some_iterable = some_iterable
        self.index = 0

    def __iter__(self):
        return self  # next method is in the same class so return iterator here.

    def __next__(self):
        while self.index < len(self.some_iterable):
            try:
                index = self.index
                self.index += 1
                print(self.some_iterable[index])
            except StopIteration:
                break

```

![Custom loop in action ⚡](https://cdn-images-1.medium.com/max/2748/1*mso170Byz1ytbXx3X7PzJg.gif)*Custom loop in action ⚡*

Since we are already peeking inside how loop works, lets see how we can implement the `range()` function using these concepts.

### Abstracting range() function implementation 🏹

The concept is pretty similar, we need to print out a series of number one after another taking into consideration the steps to increase.

```python
class Range:
    def __init__(self, start, end, step=1):
        self.start = start
        self.end = end
        self.step = step

    def __iter__(self):
        return self

    def __next__(self):
        while self.start <= self.end:
            try:
                current = self.start
                self.start += self.step

                print(current)
            except StopIteration:
                break

```

![Custom range in action ⚡](https://cdn-images-1.medium.com/max/2748/1*m1zLd32Z2TKkDU7RrubxJg.gif)*Custom range in action ⚡*

### Conclusion 🙏

I hope this article has been of help to make things clear about Iterable and Iterator. If you have any suggestions or questions let’s talk in the comment section.

Another implementation of Iterator protocol is the generator functions which we will cover in the future blog.
