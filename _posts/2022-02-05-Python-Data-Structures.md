---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: Python by R  
title: Python data structures (compared to R)

# post specific
# if not specified, .name will be used from _data/owner.yml
author: Amrit Koirala
# multiple category is not supported
category: Python
# multiple tag entries are possible
tags: [dictionary, python data structures]
# thumbnail image for post
img: ":builtinPython.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2022-02-05 08:11:06 +0900

# seo
# if not specified, date will be used.
meta_modify_date: 2022-11-01 08:11:06 +0900
# check the meta_common_description in _data/lang/[language].yml
meta_description: " Python for R users"

# optional
# if you enabled image_viewer_posts you don't need to enable this. This is only if image_viewer_posts = false
#image_viewer_on: true
# if you enabled image_lazy_loader_posts you don't need to enable this. This is only if image_lazy_loader_posts = false
#image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false
---

# Learning Python for R users

### Comparative view of R and Python
<!-- outline-start -->
R and python are both extensive used in the field of data science and
bioinformatics. Different people start learning programming from either
languages. So it will be very helpful to see what are the common data
structures in both languages so that you don't have to invent the wheel
again and again.
<!-- outline-end -->
## Data Structures

Both the languages have their own set of data formats to represent
similar kind of data. It is important to understand the subtle
differences between them.

### Data structures in R

Data structures available in base R were written in the early phase
during the evolution of R in S language. Although they are frequently
called 'Object' are not the true objects in the light of object oriented
programming (OOP). All R objects follow **copy-on-modify** semantics, so
if same data is linked to two variable names and we make modification to
one of the names, a new copy with modification is made for that variable
name. When there is only one variable name bound to a data, R works by
**modify-in-place** semantics. So, all the objects in R are mutable but
because of **copy-on-modify** semantics we will not have any issue of
indirectly modifying unwanted variables. The base R objects can be
categorized as follows:

1.  Vectors
    Vectors are the flat data structures (1D array) which can have zero
    to multiple number of items (except `NULL`). Therefore, can be
    empty, single item or sequence. Vector objects can hold only values
    and in maximum one attribute(`name`).
    1.  Atomic: These are the vectors of numbers, or string of
        characters and `NULL` which can hold only one type of data in
        one object. In R there are four types of atomic vectors:
        1.  integer : These are the vectors of integers.
        2.  double : These are the vector of decimal numbers.\
        3.  logical : These are the vector of 0 and 1 or FALSE and TRUE.
        4.  character : These are the vector of character strings.
    2.  Recursive: These are the vector of one or more atomic vectors
        and it can contain data type of itself. Only recursive datatype
        available in R is list. At user level it is similar to the tuple
        in python.
2.  Non-vectors
    When a vector has additional attribute (in addition to `name`), it
    is no more called a vector because those attributes significantly
    alter how the values contained in the object is treated by R.
    1.  Array
        The attribute, `dim` determines the shape of the vector
        contained. setting the `dim` to `c(i,j)` makes a vector behave
        like a matrix with i rows and j columns.
    2.  S3 objects The attribute, `class` makes any R object, a object
        of that class and the class of a object will govern how a
        generic function will treat that object. The R base type of a S3
        object could be any one of vector types (integer, double,
        logical, character or list). For example: factor is a S3 object
        with class `factor` and another attribute `levels` but the
        values it hold is all integer. Similarly, data frame is a S3
        object with class `data.frame` but the R data type under the
        hood is list.
    3.  S4 objects The S3 objects works in same principle as the S3
        objects but have strict construction requirements to reduce the
        error down the pipeline.

These R objects are described in detail in another post. None of these R
objects are true OOP objects and do not hold any methods inside the
object.

### Data structures in Python

Python is a more general purpose language (has wider range of
applications than just data science) therefore offers wider range of
data types and because of its wider range of applications objects used
in python are not as specialized for data analysis as R. The additional
modules like `numpy` and `pandas` are required to explore the full
potential of python for data analysis. Python follows strict object
oriented programming hence all the data structures are true objects
defined in OOP.

The built in function `type()` gives the **class** of an instance of a
object. The function `isinstance()` gives if an object is an instance of
a specific class. It is also true if the class being tested is parent to
the current class. Another attribute `cls.__mro__` gives name of class
and all parent classes in the method resolution order. Let us explore
the built in data structures in python using these functions.

![Objects in Python](:builtinPython.jpg)

#### 1. Scalars 
This includes
built-in objects which store only single value and are not iterable and
subscriptable. In R, any object created (except NULL) is a vector,
ordered, and subscriptable. An identifier holding a single value is
treated same as a vector of length one. But in python, objects of class
*float, int, bool, and NoneType* when outside any container are not
iterable and subscriptable. These are called scalars and has no length
(len() function is not defined for these objects)

-   NoneType: This is a special class which holds now value. It is
    simply a place holder. It has same function as NULL in R and only
    value this class can take is 'None' which Python interpreter takes
    as absence of value.\
-   float: These are the numbers with values after decimal. It
    corresponds to 'double' in R and all syntax applicable to double in
    R are also applicable to float.
-   int: These are 'integers' in R and follows the syntax used for
    syntax in R
-   bool: These are 'logical' in R, while in R it can take value of
    TRUE/FALSE/T/F in python boolean can take value of True/False/1/0
    only (case sensitive). Python class-wise, it is a child class of
    'int'.

``` python
#None object 
a_none = None #create an instance of class NoneType
type(a_none).__mro__ #get class and parent class to that object
```

    ## (<class 'NoneType'>, <class 'object'>)

``` python
dir(a_none) #get all attributes of that object
#len(a_none) #gives error

##Float - Decimal object 
```

    ## ['__bool__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']

``` python
a_float = 34.56 #create an instance of class float
type(a_float).__mro__ #get class and parent class to that object
```

    ## (<class 'float'>, <class 'object'>)

``` python
a_float.as_integer_ratio() #one of the example method in class float.
#help(float) ##see details of all attributes of that class
#len(a_float) #gives error
```

    ## (607985949695017, 17592186044416)

``` python
a_bool = True
type(a_bool).__mro__ #shows that bool is child class to int. 
```

    ## (<class 'bool'>, <class 'int'>, <class 'object'>)

Scalars in R are treated as vectors of length 1

``` r
R_none <- NULL
length(R_none)
```

    ## [1] 0

``` r
R_double <- 34.56
R_double[1] #subscriptable
```

    ## [1] 34.56

``` r
length(R_double) #has length of 1 #length function is defined
```

    ## [1] 1

#### 2. Containers

These data structures as name suggests are containers with hold zero,
one or more than one of the scalars discussed above, character strings,
or container object itself. All containers are iterable ie can iterated
one item at a time using the for statement. There are further 3 types of
containers:

##### a. Sequences:

These are the containers where position of items in the sequence has
value and an integer(starting from 0) can be used to extract an item
from a desired position. There are three classes of objects in Python
that fit this definition: - list : Sequence of any datatype of same
kind or mixed is called a list. It is identified by square brackets \[
\] and subset able using position similar to R with three major
differences.

i.  Python list don't have 'name' attributes.
ii. Python list are mutable (values are modified in place even when
    multiple identifiers are pointing to same list)
iii. Python list positions starts from 0 unlike 1 in R.

``` python
#mutable nature of list in python
a_list = [1,2,3,4,5]
b_list = a_list #both points to same list
b_list[0] = "new Item at first"
a_list[0] #this change is reflected in a_list as well.
```

    ## 'new Item at first'

``` r
#copy on modification semantics of R
aR_list = c(1,2,3,4,5)
bR_list = aR_list
bR_list[1] = "new Item at first"
aR_list[1] #this change doesn't affect  aR_list .
```

    ## [1] 1

``` r
bR_list[1] #bR_list is modified 
```

    ## [1] "new Item at first"

-str : These class hold character string data in Python. Unlike R, a
string of character is a subscriptable even without items being
separated by a coma.

``` python
a_string = "This a string."
len(a_string)
```

    ## 14

``` python
a_string[7:13] #last position is exclusive, start from 0
```

    ## 'string'

``` r
aR_string = "This a string."
length(aR_string)
```

    ## [1] 1

``` r
aR_string[8:14] #Gives NA because for R there is just 1 position 
```

    ## [1] NA NA NA NA NA NA NA

``` r
#need to use substr function 
substr(aR_string, 8,14) #last position is inclusive, start from 1 
```

    ## [1] "string."

-   tuple : This class is similar to list except that it is immutable
    and don't have methods like append, copy, extend, insert, pop,
    remove. and reassignment of values. It only has count and index
    methods that can be used to count the occurrence of a value and
    index to return the index of first occurrence of a value.

``` python
a_tuple = (1,2,3,4,5)
#b_tuple = a_tuple. "a new value" #gives error no reasssignment.
```

##### b. Sets:

Sets are unordered collection of unique items and can contain any data
types that are hashable (to identify the uniqueness). There are two
types of sets based on if they are mutable or immutable: set and
forzenset respectively. They are created using built-in functions
`set()` and `frozenset()`.

``` python
a_set = set([1,1,1,1,2,3,4,4,5])
a_set
```

    ## {1, 2, 3, 4, 5}

``` python
a_set.pop() #removes first item
```

    ## 1

``` python
a_set
#a_set[0] #gives error not subscriptable
```

    ## {2, 3, 4, 5}

``` python
a_frozenset = frozenset([1,1,1,1,2,3,4,4,5])
a_frozenset
#a_frozenset.pop() #gives error #are not mutable
```

    ## frozenset({1, 2, 3, 4, 5})

##### c. Dictionaries:

This is a collection of key:value pairs where key should be unique and
non-mutable but values can have any datatype and are mutable too. This
is very similar to named list in R but names of items in list in R
doesn't need to be unique.

``` python
a_dictionary = {'name': 'Sam', 'position': 'Manager', 'Address': 25, 'score': 90}
a_dictionary['name']  #Subscriptable by key
```

    ## 'Sam'

``` python
a_dictionary['name'] = "Peter" #values are mutable
a_dictionary
```

    ## {'name': 'Peter', 'position': 'Manager', 'Address': 25, 'score': 90}

``` python
for key in a_dictionary:
    print(a_dictionary[key])
```

    ## Peter
    ## Manager
    ## 25
    ## 90

### Numpy Array

These basic data structures offers very minimum resources for
applications in data science so additional data structures and functions
are offered by packages like NumPy and Pandas. One of the most important
data structures provided by Numpy and used extensively in other packages
like pandas, Scikit-Learn etc. Details on numpy available from [numpy
website](https://numpy.org/doc/stable/user/whatisnumpy.html).

``` python
import numpy as np
a_array = np.array([1,2,3,4,5,6]) # one dimensional array
a_array.size ##gives length of array
```

    ## 6

``` python
a_array.ndim ##gives number of dimensions 
```

    ## 1

``` python
a_array.shape ##gives dimensions
```

    ## (6,)

``` python
a_array.reshape(3,2) #makes 3 rows 2 columns (2 dim array)
```

    ## array([[1, 2],
    ##        [3, 4],
    ##        [5, 6]])

### Basic operations in R Vs Python.

#### Using Functions

In R, all functions are objects on their own and syntax
`function_name(arguments...)` is used universally to apply a function.
Python has diverse type of functions and syntax for using functions.

1.  Methods:

Any objects in Python being a true object (in OOP context) can store
functions within itself as an attribute. Such functions are called
methods. Syntax for using methods is
`instance.method(addn_arguments...)`.When used like this, that
particular object is automatically supplied as a first argument to that
method. To check available methods for a class, in-built
function`dir(class)` or `help(class)` can be used. As we will see below
methods suffixed and prefixed with \_\_ are called as magic/dunder and
will be called by other built-in functions.

``` python
a_float = 34.5 
a_float.as_integer_ratio() #using a method "as_integer_ratio" available in the class float
```

    ## (69, 2)

``` python
dir(float)
```

    ## ['__abs__', '__add__', '__bool__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getformat__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__int__', '__le__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__pos__', '__pow__', '__radd__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rmod__', '__rmul__', '__round__', '__rpow__', '__rsub__', '__rtruediv__', '__set_format__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', 'as_integer_ratio', 'conjugate', 'fromhex', 'hex', 'imag', 'is_integer', 'real']

2.  Built-in Functions:

Python interpreter comes packed with few [in-built
functions](https://docs.python.org/3/library/functions.html). These
functions belong to class "built-in_function_or_method". The syntax for
using these functions is similar to that of R (`function(argument)`)
where argument is an instance of a class/class name. It relies on the
dunder methods defined in the argument class. For example built-in
function "abs()" is applicable to numbers only which have dunder
"**abs**" defined. If we make a custom class of our own and define the
**abs** then we can use built-in abs() to an instance of that class as
well. Hence the built-in functions work just as a modulator directing to
appropriate method in the class on which function is being applied.
Mathematical operators like addition, subtraction all work by using
these dunders.

``` python
abs(-34.6) #abs function applied to an instance of float
#help function takes name of a class
#help(str) 

#make a custom class with abs defined 
```

    ## 34.6

``` python
class A_dummyClass:
    def __abs__(self):
        return("This dummy object is now positive.")
    
abs(A_dummyClass()) #apply abs function to an instance of this dummy class
```

    ## 'This dummy object is now positive.'

``` python
A_dummyClass().__abs__() ##calling it as a method works but is not a pythonic way
```

    ## 'This dummy object is now positive.'

3.  Functions of class 'function' The functions created using `def`
    outside of a class (not a method) are a special class of type
    "function" aka "first class objects". They can be used similar to
    the built-in function in the syntax `function_name(argurments...)`
    and behave very similar to a user defined function in R.

``` python
# Define a function to print given string three times.
def print3X(a_string) :
    return(print(a_string*3))

type(print3X) #check class of this function.
```

    ## <class 'function'>

``` python
print3X("A sample string.\n") #application of this function.
```

    ## A sample string.
    ## A sample string.
    ## A sample string.

References: 

For completely new users suggested read [Python in a
Nutshell by Alex Martelli, Anna Ravenscroft and Steve
Holden](https://www.academia.edu/39767840/Python_in_a_Nutshell_3rd_Edition)

<https://automatetheboringstuff.com/>
