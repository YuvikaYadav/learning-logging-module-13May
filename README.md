# learning-logging-module-13May
# Logging module
Logging is a means of tracking events that happen when some software runs.
Logging in Python lets you record important information about your program’s execution. You use the built-in logging module to capture logs, which provide insights into application flow, errors, and usage patterns.
With logging, can create and configure loggers, set log levels, and format log messages without installing additional packages. You can also generate log files to store records for later analysis.
The logging module in Python’s standard library is a ready-to-use, powerful module that’s designed to meet the needs of beginners as well as enterprise teams.
The main component of the logging module is something called the logger. You can think of the logger as a reporter in your code that decides what to record, at what level of detail, and where to store or send these records.
Exploring root logger
import logging 
logging.warning("Save your Code”)         WARNING:root:Save your Code
This output shows the default format that can be configured to include things like a timestamp or other details.
The example above, you’re sending a message on the root logger. log level of the message is WARNING.
Here are the five default log levels, in order of increasing severity:
Log Level          	Function	        Description
DEBUG	      logging.debug()	      Provides detailed information that’s valuable to you as a developer.
INFO	      logging.info()	      Provides general information about what’s going on with your program.
WARNING	    logging.warning()	    Indicates that there’s something you should look into.
ERROR	      logging.error()	      Alerts you to an unexpected problem that’s occured in your program.
CRITICAL	logging.critical()	    Tells you that a serious error has occurred and may have crashed your app.

>>> logging.debug("This is a debug message")
>>> logging.info("This is an info message")
>>> logging.warning("This is a warning message")
WARNING:root:This is a warning message
>>> logging.error("This is an error message")
ERROR:root:This is an error message
>>> logging.critical("This is a critical message")
CRITICAL:root:This is a critical message
##Notice that the debug() and info() messages didn’t get logged. This is because, by default, the logging module logs the messages with a severity level of WARNING or above. You can change that by configuring the logging module to log events of all levels.

# ADJUSTING LOG LEVELS
To set up your basic logging configuration and adjust the log level, the logging module comes with a basicConfig() function. 
Import logging
logging.basicConfig(level = logging.DEBUG )
logging.debug(“this will get logged.”)         DEBUG:root:this will get logged

Import logging
logging.basicConfig(level = logging.INFO )
logging.debug(“this will not get logged.”)        
This can be done by passing one of the constants available in the class.       
There are numeric values of the string (DEBUG, INFO, WARNING, ERROR, CRITICAL) numeric (10,20,30,40,50)

# Formattinng the output
import logging
logging.basicConfig(format="%(levelname)s:%(name)s:%(message)s:")
logging.error("This is an Error!")		 ERROR:root:This is an Error!

Modern formatting 
import logging
logging.basicConfig(format="{levelname}:{name}:{message}",style="{")
logging.error("This is an Error!") 		 ERROR:root:This is an Error!
Logging to a file
So far, you’ve logged the messages into your console. But if you want to archive your logs, then it’s a good idea to save log messages in a file that grows over time.
import logging
logging.basicConfig(
    filename="app.log",
    encoding="utf-8",
    filemode="a",
    format="{asctime} - {levelname} - {message}",
    style="{",
    datefmt="%Y-%m-%d %H:%M",
)

logging.warning("Save me!")
the output will be logged on app.log file as 2025-05-13 11:41 - WARNING - Save me!

Dynamic data
logging functions take a string as an argument. By leveraging Python’s f-strings, we can create verbose debug messages that contain variable information
import logging
logging.basicConfig(
    format="{levelname} - {message}",
    style="{",
    level=logging.INFO
)

user = "pylogger"
logging.info(f"{user=}")   		INFO:root:user='pylogger'

Exception tracking
Exception information can be captured if the exc_info parameter is passed as True, and the logging functions are called like this
import logging
logging.basicConfig(
    filename="save.txt",
    encoding="utf-8",
    filemode="a",
    format="{levelname} - {message}",
    style="{",
)

donuts = 5
guests = 0
try:
    donuts_per_guest = donuts / guests
except ZeroDivisionError:
    logging.error("DonutCalculationError", exc_info=True)
this output will be shown on the app.log file
2025-05-13 12:01 - ERROR - DonutCalculationError
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
ZeroDivisionError: division by zero
If exc_info isn’t set to True, the output of the above program wouldn’t tell you anything about the exception	

import logging
donuts = 1
guests = 0
try:
    donuts_per_guest = donuts / guests
except ZeroDivisionError:
    logging.exception("DonutCalculationError")
This function logs a message with the level ERROR and adds exception information to the message.
Calling logging.exception() is like calling logging.error(exc_info=True)
logging.exception(), shows a log at the level of ERROR. If you don’t want that, you can call any of the other logging functions from debug() to critical() and pass the exc_info parameter as True.
CREATING CUSTOM LOGGER
The default logger named root, which is used by the logging module whenever functions like logging.debug(), logging.warning(), and so on are called.
to define your own custom logger. You can do so by creating an object of the Logger class, which you can find in the logging module.

1	Instantiating Logger
You can instantiate a Logger class by calling the logging.getLogger() function and providing a name for your logger could use any string as the name, it’s good practice to pass __name__ as the name parameter. 
could use any string as the name, it’s good practice to pass __name__ as the name parameter. 
import logging
mylogger = logging.getLogger(__name__)
mylogger.warning("Created logger!")			Created logger!
HANDLERS
Handlers come into the picture when you want to configure your own loggers. For example, when you want to send the log messages to different destinations like the standard output stream or a file.
example of adding two handlers to a custom logger. Start by importing logging to create your logger and then two handlers.
import logging
logger = logging.getLogger(__name__)
console_handler = logging.StreamHandler()				//logs into console
file_handler = logging.FileHandler("app.log", mode="a", encoding="utf-8")//logs into file
after this we can use .addHandler() method
logger.addHandler(console_handler)
logger.addHandler(file_handler)
logger.handlers
[
  <StreamHandler <stderr> (NOTSET)>,
  <FileHandler /Users/RealPython/Desktop/app.log (NOTSET)>
]
For the StreamHandler, the logs are written to the standard error stream (stderr) stream, which is the output channel that Python uses by default
For the FileHandler, you can see the location where your log records will be saved.
log level is NOTSET. As you might expect, NOTSET means that the log level for the logging handlers is not set, yet.

ADDING FORMATTERS TO THE HANDLERS
import logging
logger = logging.getLogger(__name__)
console_handler = logging.StreamHandler()
file_handler = logging.FileHandler("app.log", mode="a", encoding="utf-8")
logger.addHandler(console_handler)
logger.addHandler(file_handler)
formatter = logging.Formatter(
   "{asctime} - {levelname} - {message}",
    style="{",
    datefmt="%Y-%m-%d %H:%M",
)

console_handler.setFormatter(formatter)
logger.warning("Stay calm!")			
o/p  Stay calm!
2025-05-13	2:44 - WARNING - Stay calm!

Setting the Log Levels of Custom Loggers
import logging
logger = logging.getLogger(__name__)
logger.level					// 0 (NotSet)
>>> logger				//<Logger __main__ (WARNING)>(Default)
>>> logger.parent			//<RootLogger root (WARNING)>

>>> logger.getEffectiveLevel()				//30(to know the effective level >>> logger.setLevel(10)
>>> logger					//<Logger __main__ (DEBUG)>
>>> logger.setLevel("INFO")
>>> logger					//<Logger __main__ (INFO)>)


>>> import logging
>>> logger = logging.getLogger(__name__)
>>> logger.setLevel("DEBUG")
>>> formatter = logging.Formatter("{levelname} - {message}", style="{")

>>> console_handler = logging.StreamHandler() 		//initiate handler
>>> console_handler.setLevel("DEBUG")			//set lvl
>>> console_handler.setFormatter(formatter)		//set formate
>>> logger.addHandler(console_handler)			//add handler to logger

>>> file_handler = logging.FileHandler("app.log", mode="a", encoding="utf-8") //initiate handler
>>> file_handler.setLevel("WARNING")				//set lvl
>>> file_handler.setFormatter(formatter)				//set formate
>>> logger.addHandler(file_handler)					//add handler to logger
>>> logger.debug("Just checking in!")		//DEBUG - Just checking in!
>>> logger.warning("Stay curious!")			//WARNING - Stay curious!
>>> logger.error("Stay put!")				//ERROR - Stay put!
In app.log file
WARNING - Stay curious!
ERROR - Stay put! [AS LEVEL IS WARNING IT DOESN’T SHOW DEBUG]


Memory management is the process by which applications read and write data. A memory manager determines where to put an application’s data.
there’s a finite chunk of memorythe manager has to find some free space and provide it to the application. This process of providing memory is generally called memory allocation.
ne of the main layers above the hardware (such as RAM or a hard drive) is the operating system (OS). It carries out (or denies) requests to read and write memory.
Python is an interpreted programming language. Your Python code actually gets compiled down to more computer-readable instructions called bytecode. (through PVM(.pyc))
The PyObject, the parent of all objects in Python, contains only two things:
•	ob_refcnt: reference count
•	ob_type: pointer to another type
The reference count is used for garbage collection. Then you have a pointer to the actual object type. 
The Global Interpreter Lock (GIL)
The GIL is a solution to the common problem of dealing with shared resources, like memory in a computer
Python’s GIL accomplishes this by locking the entire interpreter, meaning that it’s not possible for another thread to step on the current one. When CPython handles memory, it uses the GIL to ensure that it does so safely.
Garbage Collection
It’s frees the memory space..which is very old or not refrenced by any object
numbers = [1, 2, 3]		# Reference count = 1
more_numbers = numbers	# Reference count = 2
it will also increase if you pass the object as an argument:	total = sum(numbers)
The reference count will increase if you include the object in a list:  matrix = [numbers, numbers, numbers]
Python allows you to inspect the current reference count of an object with the sys module. You can use sys.getrefcount(numbers), but keep in mind that passing in the object to getrefcount() increases the reference count by 1.
When x = 10 is executed an integer object 10 is created in memory and its reference is assigned to variable x, this is because everything is object in Python.
here are two parts of memory:
stack memory
heap memory
The allocation happens on contiguous blocks of memory. We call it stack memory allocation because the allocation happens in the function call stack. The size of memory to be allocated is known to the compiler and whenever a function is called, its variables get memory allocated on the stack.
it is the memory that is only needed inside a particular function or method call. When a function is called, it is added onto the program’s call stack.
def func():  
        
    # All these variables get memory   
    # allocated on stack   
    a = 20
    b = []  
    c = "" 
The memory is allocated during the execution of instructions written by programmers. Note that the name heap has nothing to do with the heap data structure. It is called heap because it is a pile of memory space available to programmers to allocated and de-allocate. The variables are needed outside of method or function calls or are shared within multiple functions globally are stored in Heap memory.
# This memory for 10 integers 
# is allocated on heap. 
a = [0]*10

The methods/method calls and the references are stored in stack memory and all the values objects are stored in a private heap.
If refrence count = 0 then it deallocates the memory count
Python uses a portion of the memory for internal use and non-object memory.
Pools are composed of blocks from a single size class. Each pool maintains a double-linked list to other pools of the same size class. 
Pools themselves must be in one of 3 states: used, full, or empty. A used pool has available blocks for data to be stored. A full pool’s blocks are all allocated and contain data. An empty pool has no data stored and can be assigned any size class for blocks when needed.

# Conditional Statement (Filter, Map, Reduce, Recursive function)
 They allow one to write simpler, shorter code without needing to bother about intricacies like loops and branching. 
 

Map() : 
The purpose of map() is to generate / create a new iterable object from an existing iterable object by applying existing elements of the iterable object to the particular function.
varname=map(function name ,iterable obj) 
1) Varname is an object of <class ‘map’>
2) map() is a pre_defined function. 
The execution behaviour of map() is that it applies each element of an iterable object to a specified function and generates a new iterable object based on the logic we write in the function (normal function, anonymous function)
. 3) Function name can be either normal function or anonymous function. 
4) Iterable objects can be either sequence, list, set, dict type.
def sq_root(x):
    return x**2
mp = map(sq_root,(1,2,3,4,5))
for ele in mp:
    print(ele, end=” ”) 		1 4 9 16 25

# lambda function
print("value:")
old = [int (x) for x in input().split()]
sq = list(map(lambda x:x**2,old))
print("Old:",old)
print("SQ:",sq)

 filter() : 
The purpose of filter() is to filter out the elements by applying elements of iterable objects to the specified function. 
varname=filter(functionname,iterable_obj) 
1) Varname is an object of <class ‘filter’>
2) filter() is a pre_defined function, which is used for filtering out the elements of any iterable object by applying it to the function. The execution behaviour of filter() is that if the function returns True then filter that element. If the function returns False then don’t filter that element. 
3) Function name can be either normal function or anonymous function.
 4) Iterable objects can be either sequence, list, set, dict type
def pos(x):
    if x > 0:
        return True
    else:
        return False
    
def neg(x):
    if x < 0:
        return True
    else:
        return False
    
l= [1,2,3-6,-8,-5,-3,-6,4,6,7]
pl = list(filter(pos,l))
print("Pos list:",pl)
nl = list(filter(neg,l))
print("Neg list:", nl)
Lambda function
l= [1,2,3-6,-8,-5,-3,-6,4,6,7,-11]
pl = tuple(filter(lambda x:x>0,l))
print("Pos list:",pl)
nl = tuple(filter(lambda a:a<0,l))
print("Neg list:", nl)

Reduce():
The purpose of reduce() is to obtain a single element / result from the iterable object by applying it to the function. 
 The reduce() present in a predefined module called “function tools” 
varname=reduce(function name,iterable obj)
1) Varname can be either fundamental data types and strings. 
2) Function names can either be normal function or lambda function.
 3) Iterable objects can be either sequence, list, set, dict type. 
 Working functionality of reduce():
 1) At first step, First two elements of iterable objects are picked and the result is obtained. 
2) Next step is to apply the same function in such a way that the previously obtained result and the succeeding element of the iterable object and the result obtained again.
 3) This process continues till no more elements are left in the iterable object.
 4) The final result must be returned.
from functools import reduce
print("Values:")
lst = [int(x) for x in input().split()]
bg = reduce(lambda x,y:x if x>y else y, lst)
sm = reduce(lambda x,y:x if x<y else y, lst)
print(bg)
print(sm)

# Recursion
Recursion involves a function calling itself directly or indirectly to solve a problem by breaking it down into simpler and more manageable parts.
def recursive_function(parameters):
if base_case_condition:
return base_result
else:
return recursive_function(modified_parameters)
Base Case: This is the condition under which the recursion stops. It is crucial to prevent infinite loops.
Recursive Case: This is the part of the function that includes the call to itself. It must eventually lead to the base case. 
def fact(x):
    if x == 0 or x == 1:
        return 1
    return x * fact(x-1)

print(fact(6))
def fibo(x):
    if x == 0:
        return 0
    elif x == 1:
        return 1
    else:
        return (fibo(x-1) + fibo(x-2))

print(fibo(3))


List comprehensions can include conditional statements to filter or modify items based on specific criteria.
l = [1,2,3,4,5,5,7,9,8]
evn_l = [i for i in l if i%2==0]
print(evn_l)			[2,4,8]
creating a list from a range
evn = [i for i in range(10) ]
print(evn)			[0,1,2,3,4,5,6,7,8,9]
evn = [i for i in range(10) if i%2==0]
print(evn)			[2,4,6,8]

# Regular Expressions 
A Regular Expressions is one of the string data, which is searching / matching / finding in the Given Data and obtains Desired Result. Python provides **re module** for regular expression.
import re
match = re.search(r'hello', 'this is a "python tutorial", hello guys')
print(match.group())
print(match)
print("start index", match.start())
print("End index", match.end())
o/p   
hello
<re.Match object; span=(29, 34), match='hello'>
start index 29
End index 34

\	Used to drop the special meaning of character following it
[]	Represent a character class
^	Matches the beginning
$	Matches the end
.	Matches any character except newline
|	Means OR (Matches with any of the characters separated by it.
?	Matches zero or one occurrence
*	Any number of occurrences (including 0 occurrences)
+	One or more occurrences
{}	Indicate the number of occurrences of a preceding RegEx to match.
()	Enclose a group of RegEx

 why we should use Regular expression.
Data Mining:  It efficiently identifies a text in a heap of text by checking with a pre-defined pattern. Some common scenarios are identifying an email, URL, or phone from a pile of text.
Data Validation: It can include a wide array of validation processes by defining different sets of patterns. A few examples are validating phone numbers, emails, etc.

 






