
.. The Algorithmic Coding Interview slides file, created by
   hieroglyph-quickstart on Mon Nov  9 22:28:33 2015.



================================
The Algorithmic Coding Interview
================================
|
|
|
| Rob Ludwick (@robludwick)
| 11/11/2015

git
===
|
|
|
|
|

::

    $ git clone https://github.com/rcludwick/presentations


Why am I giving this talk?
==========================

Great question...

.. image:: http://img.memecdn.com/my-friend-dropped-this-one-during-is-job-interview-never-called-him-back_o_2256999.webp
        :height: 450

What's the point of interviewing this way?
==========================================
|
|
|
|

It is perceived as the best predictor for a short interview process.

|

* Google
* Facebook
* Symantec
* LinkedIn


It's not that it's bad, per se...
=================================
|
|
|

    .. rst-class:: build

    * But it does seem unfair... 

    * ...especially if they ask you questions outside your immediate domain knowledge


Google KNOWS this.
==================
|
|
|

    .. rst-class:: build

    * They expect people to interview more than once

    * 60% of their employees did not succeed on the first interview


    * It's not the best way to interview people unless you have the resources, say, Google.


But not everyone else knows this!
=================================
|
|
|

:tw:`@mxcl`

.. tweet:: https://twitter.com/mxcl/status/608682016205344768



Interview Basics
================

    .. rst-class:: build

    * Whiteboard or Shared Document Platform (Google Docs)

    * Some problem of some algorithmic nature is presented

    * Timed (25 minutes) to do as well as you can



Simple Question
===============

    .. rst-class:: build


    - Question:  What's the expected order for quicksort?
    - Answer:  :math:`O(N log N)`
    - Question:  What's the worst case order?
    - Answer:  :math:`O(N^2)`


???
===
|
    Do you even know what I'm asking?
|
|

.. image:: http://cdn.meme.am/instances/500x/37607377.jpg 


Big O Notation
==============

    .. rst-class:: build

    * A way to describe time or space requirements for a given problem, useful for algorithms on large datasets

    * :math:`O(1)` means constant time or space

    * :math:`O(Log N)` means logarithmic time or space

    * :math:`O(N)` means linear time or space

    * :math:`O(N^2)` means polynomial time or space

    * :math:`O(2^N)` means exponential time or space


Example:  Bubble Sort
=====================

.. rst-class:: build

.. code-block:: python

    for y in range(len(x))[::-1]:
        for z in range(y):
            if x[z] > x[z+1]:
                x[z], x[z+1] = x[z+1], x[z]

.. rst-class:: build

* This is :math:`O(N^2)`

* Why?

    * The algorithm goes through a nested loop.


Example:  Searching a sorted list
=================================

.. rst-class:: build

.. code-block:: python

    def linear_search(x, l):
        """
        Finds x in a sorted list l
        
        :return: True if x is in l, False if x is not in l
        """
        
        for y in l:
            if y == x:
                return True
        return False

.. rst-class:: build

    * Order: :math:`O(N)`

    * Can we do better than :math:`O(N)`?


Example:  Binary Search
=======================

.. rst-class:: build

.. code-block:: python

    def binary_search(x, l):
        left = 0
        right = len(l) - 1
        old_mid = left
        while True:
            mid = (left + right) // 2
            if l[mid] == x:
                return True
            if old_mid == mid:
                return False 
            if x < l[mid]:
                right = mid
            else: 
                left = mid
            old_mid = mid

.. rst-class:: build
   
    * Order: :math:`O(Log N)`


BIG O
=====
|
|
|

.. rst-class:: build

    * On large data sets N>1M, Big O optimizations help immensely.

        * :math:`O(NLogN) << O(N^2)`



Binary Trees
============

.. rst-class:: build

.. graphviz::

    digraph G{
      graph [ordering="out"];
      5 -> 3;
      5 -> 8;
      3 -> 1;
      3 -> 4;
      8 -> 6;
      8 -> 12;
    }

.. rst-class:: build

    * Order:  :math:`O(LogN)`

    * Yes, these are fair game...

    * ...Because Google Says So.  


Binary Trees (cont'd)
=====================

Unbalanced Binary Tree

.. image:: tree/unbalancedtree.png
    :height:  400 px

.. rst-class:: build

    * Worst Case Order:  :math:`O(N)`


O(1)
====
    
.. image:: http://s2.quickmeme.com/img/c0/c043ca2c75608ca5170024702f2861d9755fd7975c6667476824fca99f5a7b42.jpg
    :width: 800 px


Depth First Search (DFS) vs Breadth First Search (BFS)
======================================================

Assume:  

.. code-block:: python

    class TreeNode(object):
        def __init__(self, val=None, left=None, right=None):
            self.val = val
            self.right = right  #    Right and left point to
            self.left = left    #    other TreeNode instances

Problem #1:
===========

Given a binary tree, mirror it, swapping the left and right nodes everywhere.


DFS Solution
============

.. code-block:: python

    def mirror(treenode):
        """DFS Solution"""
        if treenode.left is not None:
            mirror(treenode.left)
        if treenode.right is not None:
            mirror(treenode.right)

        trenode.left, treenode.right = treenode.right, treenode.left


.. rst-class:: build

    * What's the order?  

    * We're visitng every treenode once so O(N)

    * Is there a solution that doesn't use recursion?


BFS Solution
============

.. code-block:: python

    def mirror(treenode):
        """BFS Solution"""
        nodes = [treenode]

        while nodes:
            node = node.pop(0)
            if node.left is not None:
                nodes.append(node.left)
            if node.right is not None:
                nodes.append(node.right)
            node.left, node.right = node.right, node.left

            
DFS vs BFS
==========

.. rst-class:: build

* DFS 
    * Can get "lost" in a deeply unbalanced tree
    * Recursion makes it easy to write, but is limited by stack depth
    * Often hard to write without using recursion
    
* BFS 
    * Easy to write without recursion
    * Efficient if the answer is shallower than deeper in the tree

* Both solutions in this case are effectively :math:`O(N)`


Interview Strategies
====================

.. rst-class:: build

1.  Ask Questions

    - Make sure that you understand what is asked of you
    - Clarify anything that's confusing

2.  Build unit tests

    - Use test cases to clarify behavior with the interviewer
    - Think about corner cases, you might hit.

3.  If feasible, work through a couple of test cases by hand, see if there's an easy solution

    - Perhaps there's a pattern that you can use.
    - Some tests are looking to see how perceptive or insightful the candidate is.
    - Insight into the problem is valuable even if you can't solve it completely.


Problem #2
==========

Given two rectangles, return the area that they intersect.  If they don't intersect, return 0.

.. rst-class:: build

.. code-block:: python

    class Rectangle(object):
        def __init__(self, x1, y1, x2, y2):
            self.x1 = x1
            self.x2 = x2
            self.y1 = y1
            self.y2 = y2


.. rst-class:: build

* How many corner cases do you need to worry about?  

* Is there a way that you can simplify the solution?

* Maybe using only one dimension at a time?

* :math:`Area = overlap(Xdimension) * overlap(Ydimension)`


Problem #3 (From Project Euler)
===============================

Starting at the top left corner of a 2x2 grid, and only being able to move right and down, there are 6 routes to the bottom right corner

.. image:: https://projecteuler.net/project/images/p015.gif

.. rst-class:: build

* How many such routes are there though a 20x20 grid?  

Problem #3 (Cont'd)
===================

.. rst-class:: build

* What's the order of the solution?

* How long would it take to compute a grid of 1,000,000 squares on it's side?

* What's the fastest answer we can expect?


Problem #3 Solution
===================

.. rst-class:: build

* What's the fastest answer we can expect?

    * :math:`O(1)`

* Why?

    * Working out the solution for a 3 x 3 and 4 x 4 cube gives answers equal to Pascal's Triangle

    * The triangle is rotated 45 degrees and x and y need to be translated.

    .. image:: https://upload.wikimedia.org/wikipedia/commons/thumb/2/28/PascalsTriangle.png/250px-PascalsTriangle.png


Problem #3 Solution
===================

* Why :math:`O(1)`?

    .. rst-class:: build

    * Each number in pascals triangle can be computed directly.

    * math:`n! /(n - r)! r!` which is also from statistics (number of combinations)

        * :math:`n = rownum` where the first row is :math:`0'.

        * :math:`r = colnum` where the first row is :math:`0'.

    * Factorials can be computed using a lookup table.

	



What's Fair Game?
=================

.. rst-class:: build

* Basic data structures

    * binary trees

    * linked lists

    * queues (priority queues)

    * hash maps

* Optimization of algorithms

    * time efficient

    * space efficient

* Sorting and Searching Algorithms

    * Merge Sort

    * Quick Sort

    * Radix Sort

    * Binary Search

* Big O Notation







Resources 
=========

Introduction to Algorithms, Third Edition (Cormen, Leiserson, Rivest, Stein)

.. image:: https://mitpress.mit.edu/sites/default/files/9780262033848.jpg

Resources 
=========

Algorithms, Fourth Edition (Sedgewick)

.. image:: http://ecx.images-amazon.com/images/I/51UDgHU9z9L._SX404_BO1,204,203,200_.jpg


leetcode.com
============

.. image:: leetcode.com.png
    :width: 700px

Project Euler
=============

http://projecteuler.net

https://pypi.python.org/pypi/EulerPy/


