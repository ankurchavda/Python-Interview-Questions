# Python Interview Questions

#### Interview questions for junior to intermediate programmers. 

----------------------------

1. What are lambda functions? Why are they used?  
   A Lambda function, or an anonymous function, is a self-contained block of functionality that can be passed around and used in your code. They are syntactically restricted to a single expression. Semantically, they are just syntactic sugar for a normal function definition.
- Lambda functions are syntactic sugar and provide a cleaner way to write easy/small functions.

- Python supports the functional programming paradigm. It allows to supply a function as a parameter to a different function like map, filter, etc. In such cases, elegant one time functions can be created using lambda.
  
  [Why are python lambdas useful - Stack Overflow](https://stackoverflow.com/a/890188)

----------------------------

2. What are the built-in data types that Python provides? Which of them are mutable, which are immutable?  
   Numeric Types:    `int`, `float`, `complex`, `bool`  
   Sequence Types:    `list`, `tuple`, `range`, `str`  
   Mapping Type:    `dict`  
   Set Types:    `set`, `frozenset`  
   
   Mutable: `list`, `set`, `dict`  
   Immutable: `str`, `tuple`, `range`, `bool`, `complex`, `int`, `float`  

----------------------------

3. What is the difference between list and tuple?  
   [Tuples vs Lists - Stack Overflow](https://stackoverflow.com/a/1708538/5193334)  
   [Tuples have structure, lists have order -Stack Overfloww](https://stackoverflow.com/a/626871/5193334)

----------------------------

4. Why are tuples faster than lists?  
    Tuples are faster than lists because:
   - Tuples can be constant folded.
- Tuples can be reused instead of copied.
- Tuples are compact and don't over-allocate.
- Tuples directly reference their elements.  
  Read the first two answers on this thread - 
  [Tuples faster than lists - Stack Overflow](https://stackoverflow.com/questions/3340539/why-is-tuple-faster-than-list-in-python)

----------------------------

5. What is the difference between `is` and `==`?  
- `==` is for <em>value equality</em>. Use it when you would like to know if two objects have the same value.

- `is` is for <em>reference equality</em>. Use it when you would like to know if two references refer to the same object.  
  In general, when you are comparing something to a simple type, you are usually checking for value equality, so you should use `==`. For example, for `x==2`  the intention is probably to check whether x has a value equal to 2 (==), not whether x is literally referring to the same object as 2.  

- However, there is a catch to this as well. Please refer the example in the below link.
  
  [`==` vs `is` - Stack Overflow](https://stackoverflow.com/a/1085656/5193334)  

----------------------------

6. What could be the key in `dict`?  
   Unlike sequences, which are indexed by a range of numbers, dictionaries are indexed by keys, which can be any immutable type; strings and numbers can always be keys. Tuples can be used as keys if they contain only strings, numbers, or tuples; if a tuple contains any mutable object either directly or indirectly, it cannot be used as a key. You can’t use lists as keys, since lists can be modified in place using index assignments, slice assignments, or methods like `append()` and `extend()`.

----------------------------

7. Why lists can't be dictionary keys?  
   If lists are hashed by their ids -
   
   - Looking up different lists with the same contents would produce different results, even though comparing lists with the same contents would indicate them as equivalent.
   
   - Using a list literal in a dictionary lookup would be pointless -- it would always produce a `KeyError`.  
     
     If lists were hashed by their contents (as tuples are) then that would lead to problems explained below -
     
     ```python
     #Suppose that the hash() function returns x & y for the following list objects
     [1,2] -> x
     [1,2,3] -> y
     
     #Initiate a list l & dictionary d
     l = [1,2]
     d[l] = 42    
     
     #The representation of the inside of a dictionary's collision list/bucket would look something like this (simplified)
     x -> ([1,2], 42)
     
     #Now append 3 to the list
     l.append(3)
     >> [1,2,3]
     
     #The representation of the inside of a dictionary's collision list/bucket would change to this (simplified)
     x -> ([1,2,3], 42)
     
     #Actual hash value was
     [1,2] -> x
     #According to the collision list
     [1,2,3] -> x
     #So this defies the correctness condition - for all i1, i2, if hash(i1) == hash(i2), then i1 == i2
     
     #Actual hash value was
     [1,2,3] -> y
     #According to the collision list
     [1,2,3] -> x
     #So this defies the correctness condition - for all i1, i2, if hash(i1) != hash(i2), then i1 != i2
     ```
     
     Since the dictionary doesn't know when a key object is modified, such errors could only be produced at key lookup time, not at object modification time, which could make such errors quite hard to debug.
     
     [Why lists can't be keys - Python Documentation](https://wiki.python.org/moin/DictionaryKeys)  

----------------------------

8. What is `__init__.py` for?  
   [`__init__.py` - Stack Overflow](https://stackoverflow.com/a/4116384/5193334)

----------------------------

9. How can I swap values of variables in Python?  
   `left, right = right, left`  
   [Pythonic way to swap variables - Stack Overflow](https://stackoverflow.com/a/14836456/5193334)

----------------------------

10. What is the difference between packages and modules in Python?  
    [Module vs Package - Stack Overflow](https://stackoverflow.com/a/7948672/5193334)

----------------------------

11. Can you write multithreading applications in Python?  
    Yes, one can write multithreading applications in python for cases when a responsive UI is needed, I/O or network is the bottleneck. C extensions and C code can run in parallel in Python, however, only one active Python thread can run in Python. The GIL prevents making use of one CPU core or separate CPUs to run python threads in parallel.  
    [Multithreading in Python - Stack Overflow](https://stackoverflow.com/a/20939442/5193334)

----------------------------

12. Why is GIL needed?  
    In order to make the dynamic memory management in CPython work correctly, the GIL prevents multiple threads from running Python code at the same time. This is because CPython's dynamic memory management is not thread-safe - it can have those same problems of multiple threads accessing (or worse, disposing) the same resource at the same time. The GIL was a compromise between the two extremes of not allowing multi-threaded code, and having the dynamic memory management be very bulky and slow.  
    [Is multithreading a myth - Stack Overflow](https://stackoverflow.com/a/44793537/5193334)

----------------------------

13. Multithreading vs Mulitprocessing?  
    *Multiprocessing Pros -*  
    
    - Separate memory space
    - Code is usually straightforward
    - Takes advantage of multiple CPUs & cores
    - Avoids GIL limitations for cPython
    - Eliminates most needs for synchronization primitives unless if you use - shared memory (instead, it's more of a communication model for IPC)
    - Child processes are interruptible/killable
    - Python `multiprocessing` module includes useful abstractions with an - interface much like `threading.Thread`
    - A must with cPython for CPU-bound processing 
    
    *Multiprocessing Cons -*  
    
    - IPC a little more complicated with more overhead (communication model vs. - shared memory/objects)
    - Larger memory footprint  
    
    *Threading Pros -*
    
    - Lightweight - low memory footprint
    - Shared memory - makes access to state from another context easier
    - Allows you to easily make responsive UIs
    - cPython C extension modules that properly release the GIL will run in - parallel
    - Great option for I/O-bound applications
    
    *Threading Cons -*  
    
    - cPython - subject to the GIL
    - Not interruptible/killable
    - If not following a command queue/message pump model (using the `Queue` module)- , then manual use of synchronization primitives become a necessity (decisions are needed for the granularity of locking)
    - Code is usually harder to understand and to get right - the potential for - race conditions increases dramatically  
    
    [Difference between the modules - Stack Overflow](https://stackoverflow.com/a/18114882/5193334)

----------------------------

14. What is the `__dict__` attribute of an object in Python?  
    Basically it contains all the attributes which describe the object in question. It can be used to alter or read the attributes. Quoting from the documentation for `__dict__`
    
    > A dictionary or other mapping object used to store an object's (writable) attributes
    
    [`__dict__` attribute in python - Stack Overflow](https://stackoverflow.com/a/19907498/5193334)

15. What is `self` keyword used for in python?  
    `self` represents the current instance of a class which is used to access the properties and methods of a class.   
    [`self` explained to a beginner - Stack Overflow](https://stackoverflow.com/a/6990217/5193334)  
    [The purpose of self -Stack Overfloww](https://stackoverflow.com/a/2709832/5193334)

----------------------------

16. What is the `__init__` function used for?  
    The `__init__` function is called a constructor, or initializer, and is automatically called when you create a new instance of a class. Within that function, the newly created object is assigned to the parameter self.  
    [Why do we use `__init__` in python - Stack Overflow](https://stackoverflow.com/a/8609238/5193334)

----------------------------

17. What is pickling and unpickling in python?  
    “Pickling” is the process whereby a Python object hierarchy is converted into a byte stream, and “unpickling” is the inverse operation, whereby a byte stream (from a binary file or bytes-like object) is converted back into an object hierarchy. Pickling (and unpickling) is alternatively known as “serialization”, “marshalling,” or “flattening”; however, to avoid confusion, the terms used here are “pickling” and “unpickling”.  
    [Code Example - Stack Overflow](https://stackoverflow.com/a/7502013/5193334)

----------------------------

18. Pickling usecases?  
- Saving a program's state data to disk so that it can carry on where it left off when restarted (persistence)
- Sending python data over a TCP connection in a multi-core or distributed system (marshalling)
- Storing python objects in a database
- Converting an arbitrary python object to a string so that it can be used as a dictionary key (e.g. for caching & memoization).  
  [Common pickling usecases - Stack Overflow](https://stackoverflow.com/a/3439921/5193334)

----------------------------

19. What is the output of `-12 % 10`?  
    
    ```python
    >>> -12%10
    8
    ```

----------------------------

20. What is the output of `-12 // 10`?  
    
    ```python
    >>> -12//10
    -2
    ```
    
    -1.2 is rounded off to -2.

----------------------------

21. Why shouldn't you make the default arguments an empty list?  
    A new list is created once when the function is defined, and the same list is used in each successive call.  
    Python’s default arguments are evaluated once when the function is defined, not each time the function is called (like it is in say, Ruby). This means that if you use a mutable default argument and mutate it, you will have mutated that object for all future calls to the function as well.
    
    ```python
     def append_to_list(element, list_to_append=[]):
        list_to_append.append(element)
        return list_to_append
    
    >>> a = append_to_list(10)
    [10]
    >>> b = append_to_list(20)
    [10, 20]
    ```
    
    [How to avoid above issue - Stack Overflow](https://stackoverflow.com/questions/366422/what-is-the-pythonic-way-to-avoid-default-parameters-that-are-empty-lists)
    
    [Python Gotchas -  Python Guide](https://docs.python-guide.org/writing/gotchas/#mutable-default-arguments)

----------------------------

22. What is the `id()` function in Python?  
    The id() function returns a unique id for the specified object.  
    All objects in Python has its own unique id.  
    The id is assigned to the object when it is created.  
    The id is the object's memory address, and will be different for each time you run the program. (except for some object that has a constant unique id, like integers from -5 to 256)  

----------------------------

23. What is the `yield` keyword used for in Python?  
    `yield ` is used to create generators. If a compiler detects the `yield` keyword inside a function, that function no longer returns via the `return` statement. Instead it immediately returns a generator object. The python function now maintains state which helps the function to resume where it left off.

----------------------------

24. What is a generator?  
    A generator is simply a function which returns an object on which you can call next, such that for every call it returns some value, until it raises a `StopIteration` exception, signaling that all values have been generated. Such an object is called an iterator. Generator is a subtype of `Iterator`. 
    Normal functions return a single value using return, just like in Java. In Python, however, there is an alternative, called yield. Using yield anywhere in a function makes it a generator. Observe this code:  
    
    ```python
    >>> def myGen(n):
    ...     yield n
    ...     yield n + 1
    ... 
    >>> g = myGen(6)
    >>> next(g)
    6
    >>> next(g)
    7
    >>> next(g)
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    StopIteration
    ```
    
    As you can see, myGen(n) is a function which yields n and n + 1. Every call to next yields a single value, until all values have been yielded. for loops call next in the background, thus:
    
    ```python
    >>> for n in myGen(6):
    ...     print(n)
    ... 
    6
    7
    ```
    
    [Understanding generators in python - Stack Overflow](https://stackoverflow.com/a/1756156/5193334)

----------------------------

25. Why use generators?
- When you are calculating a large data set and are unsure whether you'll need the entire result.

- When you don't want to allocate memory for the entire result at once (space).

- When you don't want to wait for the entire result to be calculated (time).

- Generators allow for a natural way to create infinite streams. 
  
  ```python
  >>> def fib():
  ...     a, b = 0, 1
  ...     while True:
  ...         yield a
  ...         a, b = b, a + b
  ... 
  >>> import itertools
  >>> list(itertools.islice(fib(), 10))
  [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
  ```
  
    [What can you use python generators for - Stack Overflow](https://stackoverflow.com/a/102632/5193334)  
    [Understanding generators in python -Stack Overflow](https://stackoverflow.com/a/1756156/5193334)

----------------------------

26. What are iterators in python? Write a custom iterator.  
    Iterators are objects that allow you to traverse through all the elements of a collection and return one element at a time. More specifically, we say that an iterator is an object that implements the iterator protocol.  
    An iterator protocol is nothing but a specific class in Python which further has the `__iter__()`  and `__next__()`  methods.  
    [Python Iterators: A Step-By-Step Introduction – dbader.org](https://dbader.org/blog/python-iterators)
    
    [Step by Step Guide on Iterators (Broken)](https://www.mygreatlearning.com/blog/iterator-in-python/)
    
    ```python
    # A custom fibonacci iterator
    class Fiborator:
    
        def __init__(self, stop=100) -> None:
            self.stop = stop
            self.start_a = 0
            self.start_b = 1
    
        def __iter__(self):
            return self
    
        def __next__(self):        
            while self.start_a <= selxxf.stop:
                result = self.start_a
                self.start_a, self.start_b = self.start_b, self.start_a + self.start_b
                return result
            raise StopIteration
    
    fib = Fiborator()
    
    for i in fib:
        print(i)
    ```

----------------------------

27. Iterable vs Iterator vs Iteration?  
    An _iterable_ is an object that has an `__iter__` method which returns an iterator, or which defines a `__getitem__` method that can take sequential indexes starting from zero (and raises an `IndexError` when the indexes are no longer valid). So an iterable is an object that you can get an iterator from.  
    An _iterator_ is an object with a `next` (Python 2) or `__next__` (Python 3) method.  
    [Iterable vs Iterator vs Iteration - Stack Overflow](https://stackoverflow.com/questions/9884132/what-exactly-are-iterator-iterable-and-iteration)

----------------------------

28. What is the difference between `__iter__()` and `__next__()`?  
    Iterables have an `__iter__` method which returns an iterator. Iterators have a `__next__` method which returns either their next value or raise a `StopIteration`

----------------------------

29. What is a context manager? How are they different from `try ... finally`?  
    Context managers allow you to allocate and release resources precisely when you want to. This exempts the developer from managing external resources such as files, network connections and locks. Sometimes a program will retain those resources forever. This kind of issue is called a memory leak because the available memory gets reduced every time you create and open a new instance without closing it.  
    For example, a common problem that can arise when developers are working with databases is when a program keeps creating new connections without releasing or reusing them. In that case, the database backend can stop accepting new connections. This might require an admin to log in and manually kill those stale connections to make the database usable again.  
    The `with` statement allows you to setup and teardown the instances in a safer, cleaner and reusable manner. Whereas in a `try ... finally` you still have to explicitly close the connection.  
    
    The context manager object results from evaluating the `expression` after `with`. In other words, `expression` must return an object that implements the **context management protocol**. This protocol consists of two special methods:
    
    1. [`.__enter__()`](https://docs.python.org/3/library/stdtypes.html#contextmanager.__enter__) is called by the `with` statement to enter the runtime context.
       
       2. [`.__exit__()`](https://docs.python.org/3/library/stdtypes.html#contextmanager.__exit__) is called when the execution leaves the `with` code block.
       
       [Python With Statement - Real Python](https://realpython.com/python-with-statement/)

----------------------------

30. How can you copy an object in Python? How to make a deep copy?  
    One can use the `copy` module to create a true copy of the object.  
    The difference between shallow and deep copying is only relevant for compound objects (objects that contain other objects, like lists or class instances):  
    - A _shallow copy_ constructs a new compound object and then (to the extent possible) inserts references into it to the objects found in the original.  
    - A _deep copy_ constructs a new compound object and then, recursively, inserts copies into it of the objects found in the original.

----------------------------

31. How does garbage collection work in python?  
    Reference Counting -  
    The main garbage collection mechanism in CPython is Reference Counts. Whenever you create an object in Python, the underlying C Object has both a Python type and a reference count.  
    At a very basic level, a Python object's reference count is incremented whenever the object is referenced, and it is decremented when an object is delinked. If the reference count for the object is 0, the memory for the object is released.  
    A few ways to increase the reference count -
    
    - Assigning an object to a variable.
    - Adding an object to a data structure, such as appending to a list or adding as a property on a class instance.
    - Passing the object as an argument to a function.  
    
    ```python
    >>> import sys
    >>> a = 'my-string'
    >>> sys.getrefcount(a)
    2
    ```
    
    Generational Garbage Collection - 
    While reference counting is easy to implement and makes the life of a developer easier, it has its drawbacks. It will not clear objects with cyclical references. 
    
    A reference cycle is when a object refers to itself. The simplest example would be - 
    
    ```python
    >>> a = [1,2]
    >>> a.append(a)
    ```
    
    In such cases, the generational collector runs after a certain threshold is reached for it's generation and clears all the self referencing objects that are not being referenced by any other object externally.

----------------------------

32. What are decorators in Python?  
    By definition, a decorator is a function that takes another function and extends the behavior of the latter function without explicitly modifying it.    
    An excellent article on what are decorators - [Primer on Python Decorators - Real Python](https://realpython.com/primer-on-python-decorators/)

----------------------------

33. Is python pass-by-value or pass-by-reference?  
    [Amazing Explanation by Robert Heaton](https://robertheaton.com/2014/02/09/pythons-pass-by-object-reference-as-explained-by-philip-k-dick/)

----------------------------

34. How are lists built internally in python?  
    [Python List Implementation - Laurent Luce](http://www.laurentluce.com/posts/python-list-implementation/)
    
    [How is Python's List Implemented? - Stack Overflow](https://stackoverflow.com/questions/3917574/how-is-pythons-list-implemented)

----------------------------

35. What does if `__name__ == "__main__":` do?
    
    [Answer and examples - Stack Overflow](https://stackoverflow.com/a/419185/5193334)

----------------------------

35. Is Python interpreted, or compiled, or both?
    
    [Explanation - Stack Overflow](https://stackoverflow.com/a/6889798/5193334)

---

36. How is memory managed in python?
    
    There are two parts of memory:
    
    - stack memory
    - heap memory
    
    The methods/method calls and the references are stored in **stack memory** and all the values objects are stored in a **private heap**.
    
    ##### Work of Stack Memory
    
    The allocation happens on contiguous blocks of memory. We call it stack memory allocation because the allocation happens in the function call stack. The size of memory to be allocated is known to the compiler and whenever a function is called, its variables get memory allocated on the stack.
    
    It is the memory that is only needed inside a particular function or method call. When a function is called, it is added onto the program’s call stack. Any local memory assignments such as variable initializations inside the particular functions are stored temporarily on the function call stack, where it is deleted once the function returns, and the call stack moves on to the next task. This allocation onto a contiguous block of memory is handled by the compiler using predefined routines, and developers do not need to worry about it.
    
    **Example:**
    
    ```python
    def func():
    
    # All these variables get memory
    # allocated on stack`
    
    a = 20
    b = []
    c = ""
    ```
    
    ##### Work of Heap Memory
    
    The memory is allocated during the execution of instructions written by programmers. Note that the name heap has nothing to do with the heap data structure. It is called heap because it is a pile of memory space available to programmers to allocated and de-allocate. The variables that are needed outside of method or function calls or are shared within multiple functions globally are stored in Heap memory.
    
    **Example:**
    
    ```python
    # This memory for 10 integers
    # is allocated on heap.
    a = [0]*10
    ```

        [Memory Management in Python - GeeksforGeeks](https://www.geeksforgeeks.org/memory-management-in-python/)

---

37. What is the difference between `range` and `xrange`?
    
    While `xrange` has been deprecated starting from python 3 (because range now does `xrange` used to do), the difference was that `range` returned a static list while `xrange` returned a generator object. Hence the objects were created on the fly. Leading to lesser time and memory consumption.

---

38. More such useful questions/answers shared by [InterviewBit](https://www.interviewbit.com/python-interview-questions/#what-is-python)

---

### Programming Questions

1. How can I reload a previously imported module?  
   
   ```python
   from importlib import reload
   import foo
   
   if is_changed(foo):
       reload(foo)
   ```

2. What will be the output of the following code?
   
   ```python
   >>> a = [[]]*3
   >>> a[1].append(1)
   >>> print(a)
   [[1], [1], [1]]
   ```

3. Write a `timeit` decorator to measure the time of function execution -  
   
   ```python
   import functools
   from datetime import datetime
   
   def timeit(func):
       @functools.wraps(func)
       def wrapper_timeit(*args, **kwargs):
           start = datetime.now()
           value = func(*args, **kwargs)
           end = datetime.now() - start
           print(f"Finished {func.__name__} in {end}")
           return value
       return wrapper_timeit
   
   @timeit
   def test_decorator():
       for i in range(200000000):
           pass
   
   test_decorator()
   
   >>> Finished test_decorator in 0:00:03.730084
   ```

4. Write a decorator function that will catch errors and repeat the function execution n number of times -
   
   ```python
   def retry(num_times=3):
       def decorator_retry(func):
           @functools.wraps(func)
           def wrapper_retry(*args, **kwargs):
               for i in range(num_times):
                   try:
                       return func(*args, **kwargs)
                   except Exception as e:
                       print("Retry number: ", i+1)
                       if i+1 == num_times:
                           print("Retry limit reached")
                           raise
   
           return wrapper_retry
       return decorator_retry 
   ```

5. We have the following code with the unknown function f(). In f(), we do not want to use a return, instead, we may want to use a generator.
   
   ```python
   for x in f(5):
       print(x,)
   ```
   
    The output looks like this:
   
   ```python
   0 1 8 27 64
   ```
   
    Answer -  
   
   ```python
   def f(x):
       for n in range(x):
           yield n**3
   
   for i in f(5):
       print(i,)
   ```

6. How to translate string containing binary code into a number? Write a function to do this -
   
   ```python
   def get_int(number):
       start = 2 ** (len(number) - 1)
       total = 0
   
       for i in number:
           total += int(i) * start
           start = start//2
   
       return total
   
   print(get_int('11111111'))
   ```

7. Write a function that returns a string of numbers from 0 to 100, "0123456789101112..."
   
   ```python
   for i in range(0,100):
       print(i, end='')
   ```

8. What will be the output of the following code?
   
   ```python
   class Person:
       def __init__(self, name): 
           __name__= name
   
       def getAge(self):
           print(__name__)
   
   p = Person("John")
   p.getAge()
   
   # Output
   >>> __main__
   ```

        While `__name__` is a built-in variable, since we are setting it inside a function, it's         scope is only limited to that function. So when we refer the variable outside of that         function, the value for `__name__` remains as `__main__`.

9. What is the output of the following code?
   
   ```python
   x = [[0], [1]]
   print(len(' '.join(list(map(str, x)))))
   
   #Output
   >>> x = [[0], [1]]
   >>> print(len(' '.join(list(map(str, x)))))
   7
   #Simplifying
   >>> print(' '.join(list(map(str, x))))     
   [0] [1]
   >>> print(list(map(str, x)))          
   ['[0]', '[1]']
   ```