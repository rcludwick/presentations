
.. Exploring python through IPython slides file, created by
   hieroglyph-quickstart on Mon Nov 10 20:58:58 2014.

================================
Exploring python through IPython
================================
| Rob Ludwick
| 11/11/2014

::

    git clone https://github.com/rcludwick/presentations



Installing IPython
==================
|
|
|

Through pip::

    $ pip install ipython



Running IPython
===============
|

  ::

    $ ipython


    Python 2.7.6 (default, Mar 22 2014, 22:59:56) 
    Type "copyright", "credits" or "license" for more information.

    IPython 2.3.0 -- An enhanced Interactive Python.
    ?         -> Introduction and overview of IPython's features.
    %quickref -> Quick reference.
    help      -> Python's own help system.
    object?   -> Details about 'object', use 'object??' for extra 
                 details.

    In [1]: 


Useful Things
=============
|

Directives ::

      %paste 
      %cpaste

|

Shell commands ::

      ls
      cd

More Useful Things
==================
|

* Tab symbol name completion

  * Useful for navigating around a module 

|

* reimport 

  * editing python code on the fly
  * doesn't require restart



...And More
===========
|

Copy input lines to github ::

  pastebin 1-10

|

Save lines to a file ::

  save /tmp/file.txt 1-10

...And Lots More
================
|

Show the docstring for an object ::

    In [5]: pdoc list

    Class docstring:
        list() -> new empty list
        list(iterable) -> new list initialized from iterable's items
    Init docstring:
        x.__init__(...) initializes x; see help(type(x)) for signature

Bring up the quick reference card ::

  quickref


In and Out
==========

* Case Sensitive
* ``In[]`` is a list of strings typed by the user at the console
* ``Out[]`` is a corresponding list of output

    * If a function returns ``None`` it is placed in ``Out[]`` but not printed

* Assign the last output to a variable ::

    In[3]: x = Out[2]

* Rerun a statement typed before ::

    In[3]: exec In[2]


Tabbing around a package
========================

::

    In [31]: import itertools

    In [32]: itertools.<tab>
    itertools.chain
    itertools.combinations
    itertools.combinations_with_replacement
    itertools.compress
    itertools.count
    itertools.cycle
    itertools.dropwhile
    itertools.groupby
    itertools.ifilter
    itertools.ifilterfalse
    [...]


Exploring unknown packages
==========================

  ::

    In [2]: import itertools

    In [3]: pdoc itertools.ifilterfalse
    Class docstring::
        ifilterfalse(function or None, sequence) -->
                ifilterfalse object
        Return those items of sequence for which function(item) 
            is false.
        If function is None, return the items that are false.
    Init docstring:
        x.__init__(...) initializes x; see help(type(x)) 
            for signature


Ex: What's an iterator?
=======================

Two code segments

.. code-block:: python

    x = [ 1, 2, 3, 4, 5]
    for y in x:
        print y

.. code-block:: python

    x = [ 1, 2, 3, 4, 5]
    my_iter = iter(x)
    for y in my_iter:
        print y


Ex: What's an iterator? (cont'd)
================================

Hmmm... let's look at ``__length_hint__`` ::

    In [13]: y = iter(x)

    In [14]: y.__length_hint__
    Out[14]: <function __length_hint__>

    In [15]: y.__length_hint__()
    Out[15]: 5


What about ``next``? ::

    In [18]: y.next
    Out[18]: <method-wrapper 'next' of listiterator object at 0x7fd658042510>

    In [19]: y.next()
    Out[19]: 1


Ex:  What's an iterator? (cont'd)
=================================
|

The ``__iter__`` function returns itself ::

    In [16]: y.__iter__()
    Out[16]: <listiterator at 0x7fd658042510>

    In [17]: y
    Out[17]: <listiterator at 0x7fd658042510>


Make our own iterator
=====================
|

* Takes an iterator of integers

* Return all odd numbers

Code
====

.. code-block:: python

    class OddIter(object):
        '''
        Takes a list or iterator of integers.  Iterates through 
        the odd integers
        '''

        def __init__(self, iterable):
            self.iterator = iter(iterable)

        def next(self):
            while True:
                n = self.iterator.next()
                if n % 2 == 1:
                    break
            return n

        def __iter__(self):
            return self

        def __length_hint__(self):
            return self.iterator.__length_hint__()

StopIteration
=============

``OddIter`` stops when ``self.iterator`` throws ``StopIteration``

.. code-block:: python

    def next(self):
        while True:
            #This Throws StopIteration when done
            n = self.iterator.next()
            if n % 2 == 1:
                break
        return n

::

    In [32]: y.next()
    ------------------
    StopIteration 
    <ipython-input-32-75a92ee8313a> in <module>()
    ----> 1 y.next()
    [...]


What's a Function?
==================
|

Let's create one:

.. code-block:: python

    def my_func():
        return 2

What's this ``__call__``?::

    In [3]: my_func.__call__
    Out[3]: <method-wrapper '__call__' of function object at 0x7ffdd...>

    In [4]: my_func.__call__()
    Out[4]: 2


Build a function the harder way
===============================

The following are equivalent:

.. code-block:: python

    def my_func():
        return 2

.. code-block:: python

    class MyFunc(object):
        
        def __call__(self):
            return 2

    my_func = MyFunc()

Both return the same thing::

    In [5]: my_func()
    Out[5]: 2


Build Something More Useful
===========================

.. code-block:: python

    import sys
    class LogFunc(object):
        '''Count the number of times a function has been run'''

        def __init__(self, func):
            self.func = func
            self.count = 0

        def __call__(self, *args, **kwargs):
            self.count += 1
            sys.stderr.write('function called {} times'.format(
                    self.count))
            return self.func(*args, **kwargs)

Use it
======

Pass a function into ``LogFunc``::

    In [6]: l = LogFunc(my_func)

Then call ``l``::

    In [7]: l()
    function called 1 times
    In [8]: l()
    function called 2 times
    In [9]: l()
    function called 3 times

We can redefine ``my_func`` (think: decorator) 

.. code-block:: python

    my_func = LogFunc(my_func)


Abstract into python types
==========================
|
|

* Decorators

  * Functions that return a function

* Lists

  * Slices and Indexes

* Dicts

  * Keys and values


Function Pipelines
==================

http://code.activestate.com/recipes/577714-function-pipelines/

.. code-block:: python

  def add_1(x):
    return x+1

  def mul_2(x):
    return (x * 2)

  def identity(x):
    return x

  p = Pipeline(add_1, mul_2, add_1, mul_2, identity)
  assert(p(1) == 10)


That's It!
==========
|

email::

    rcludw@gmail.com

github repo of this presentation ::

    git clone https://github.com/rcludwick/presentations
