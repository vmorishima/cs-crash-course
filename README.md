Useful links:

- [Problem Solving with Algorithms and Data Structures](http://interactivepython.org/runestone/static/pythonds/index.html)
- [Introduction to Algorithms](http://bayanbox.ir/view/4177858657730907268/introduction-to-algorithms-3rd-edition.pdf)
- [Cracking the Coding Interview](http://www.eenadupratibha.net/Engineering-Colleges/Engineering-Jobs/Documents/crackingthecodinginterview.pdf)

#### CS 252 Table of Contents

1. [Software Engineering Principles](#1)
- Software life cycle
- Modules and modular design
- Top down and object oriented design

2. [Data Abstraction and Classes](#2)
- Abstract data types and classes in C++
- Constructors, destructors, friends, and operator overloading

3. [Concrete Data Structures](#3)
- Data structure overview
- Concrete vs abstract data structures, linear vs nonlinear data structures
- Fundamental data structures in C++: arrays and pointers, structs and unions, linked lists

4. [Recursion](#4)
- Recursive functions
- Recursion vs iteration
- Recursive data structures: binary trees

5. [Basic Abstract Data Types](#5)
- General concepts, templates
- Basic abstract data types: vectors, sets, bags, lists, stacks, queues, tables

6. [Object Oriented Design](#6)
- Inheritance
- Virtual functions
- Inheritance and program design

7. [Search Structures and Searching Algorithms](#7)
- Complexity analysis and big-O notation
- Linear and binary search
- Hashing, linear-probed hash tables, chained hashing
- Binary search trees, AVL trees
- Heaps - priority queues

8. [Sorting Algorithms](#8)
- Insertion, selection and bubble sort
- Quicksort, average and worst case analysis
- Tree sort, heap sort, finding nth largest

9. [External Searching and Sorting](#9)
- Searching on disk: m-way search trees, b-trees
- Sorting on disk - mergesort

10. [Graphs](#10)
- Adjacent matrix and adjacency list representation
- Depth-first and breadth-first search, topological sorting
- Spanning trees, shortest paths


# 1. Software Engineering Principles
In this course you will learn how to apply good software engineering principles to the development of collections of data. Such collections of data are called **data structures**. You may have used data structures, but in this course you will learn to develop them.

Good software should be:
- Easy to understand and use
- In a compact executable file
- Reusable by other programmers
- Reliable and well-tested
- Easily maintainable
- Cheap in dollar value
- Efficient when executing (algorithms)

Good software is based on good design. One prevailing principle of software design is modularity: the decomposition of the program into small, reusable modules.

Modules should be:
- Cohesive - all things in a module should be related
- Loosely coupled - one module should not depend too heavily on other modules

A module consists of two parts:
- Public - accessible to the client (interface)
- Private - inaccessible to the client, leads to information hiding or encapsulation, in which the inner workings of the module are hidden behind the public interface.

**Functional/Top-Down Design** decomposes a problem based on the tasks involved in solving the problem.

As an example, consider the design for a banking system where a customer can deposit or withdraw from an account and get the balance on an account. The bank must be able to open and close accounts as needed, and each account has a unique ID. The tasks involved can be modularized.

**Object Oriented Design** decomposes the problem based on data and associated operations on that data. The end result is the development of a set of classes (each representing data with associated operations) as well as a driver program.

In an example “account” class, public methods may include:
- Constructor: account ID, customer name, initial balance
- Deposit
- Withdraw
- Get balance
- Get name
- Get ID

In an example “bank” class, public methods may include:
- Constructor: max size (int)
- Open account: customer name, ID, initial balance
- Close account: account id (str/int)
- Deposit: account ID, amount
- Withdraw: account ID, amount
- Get balance: account ID

To complete the design of each class, we would specify a class invariant: a set of rules that must hold from the time that an object is constructed until the time that it is destroyed.

In our example account class, invariants might include:
- Account ID, Customer name and Balance must be assigned values
- Account ID and Customer Name do not change once assigned
- Balance >= 0

And in our bank class:
- No more than MAX_SIZE accounts at any time
- Each account ID is unique
- An account must be opened before any other operation is applied
- Operations can be applied to an account until it is closed

For each operation, we must specify a *precondition* and a *postcondition*.

A precondition is a statement that is required to be true at the point when an operation is called. If this condition is not true, there is no guarantee that the operation will complete successfully.

A postcondition is a statement that will be true when the operation has completed.

We can think of these conditions as a connection between programmer and designer, where given that the programmer ensures that the preconditions are met at time of operation, the postcondition specified by the designer will be true when the call ends.

A class invariant is implicit in the pre and postconditions of all operations associated with that class.

##### Program Testing

Before implementing the modules that we have designed, we design a plan that will be used to test them. As well as testing the software using “expected” data, we should also test the data with boundary values. So, for example, if we have a function that takes a single, positive integer as a parameter, our test for expected data would be value > 0, and our test for boundary data would be value = 0.

For our account class, we should test all public operations with expected and boundary data. We should also check to see that the class invariant is maintained at all times. For example, what happens if we try to withdraw an amount greater than the current balance?


# 2. Abstract Data Types and Classes
An **abstract data type** is a user-defined data type that is implemented in such a way that the data is hidden behind a public set of operations on that data.

The notion of *information hiding* is extremely common--most gadgets we interact with have well-designed interfaces that hide the inner workings of the object. For example, the buttons on the outside of a cell phone provide the user with all the operations necessary to use the phone effectively. Most cell phone users probably have no idea about the inner workings of the phone. The point is good design--it is not necessary for anyone to understand exactly how it works in order to use it. The data types we define should operate on the same principle: they should provide an easy to user interface that hides the inner workings.

In C++ abstract data types (ADTs) are implemented using classes. The public members comprise the interface while the private members and the implementation of the public member functions comprise the “inner workings of the class. In order to use a class effectively, a client programmer needs to be familiar only with the public interface.

An **object** is an **instance** of a class. Consider the following declaration:

`string firstName;`

Here `string` is a class, while `firstName` is an object of type `string`.

In the rest of this section we will review how to design and implement C++ classes by designing an `IntVector` class. This class represents a vector (dynamic array) of integers.

Our first attempt at producing an `IntVector` class will store the data in an array. We will see that this imposes certain restrictions which we will attempt to remove in future versions.

Our `Intvector` class has only four public member functions:

1. A **Constructor**: a function which is implicitly called when we declare an instance of the IntVector class. The constructor is used to perform any necessary initialization of member variables. 
Note:
- A constructor always has the same name as the class
- A constructor does not have a return type

```C++
IntVector();
// Default constructor
// POST: an IntVector with capacity MAX_SIZE
// has been created
```

2. An **Accessor**: a function that returns information about the state of an object but does not alter the state of the object. In this case, the accessor returns the integer that is stored at a particular index.

```C++
int at( int index ) const;
// Accessor
// POST: if 0 <= index < MAX_SIZE the element at
// index is returned.
// Otherwise, program is aborted.
```

3. An accessor that returns the maximum capacity of the vector:

```C++
int size( void ) const;
// Accessor
// POST: maximum capacity of vector has been
// returned.
```

4. A **Mutator**: a function that changes the state of the object in some way. In this case our mutator will change the value of an integer at a specified index.

```C++
int& at( int index );
// Mutator
// POST: If 0 <= index < MAX_SIZE the element at
// index is returned.
// Otherwise, the program is aborted.
```

Notice that the const keyword is not used here because this function will change the state of the object.

Also note that we now have two functions with the same name. This is called **function overloading** and occurs when two functions have the same name but different signatures:

```C++
at( int index ) const;
at( int index );
```

Now that we have designed the public interface, we can think about how a client might use an object of type IntVector:

```C++
IntVector myVector;
int capacity = myVector.size();
for( int index = 0; index < capacity; index++ )
	myVector.at( index ) = index + 1;
for( index = capacity - 1; index >= 0; index-- )
	cout << myVector.at( index ) << endl;
```

Declaring `IntVector myVector;` calls our constructor function. `cout << myVector.at(0);` uses the “at” member function we defined. Notice how we have been able to write this code using only our knowledge of the public interface.

We declare the state variables or member variables in the private section. By declaring these variables in the private section we hide the way that the class has been implemented from the client programmer. This is a good thing!

In general you should declare the minimum number of state variables necessary to represent the state of the object. In the case of the IntVector class, the private section looks as follows:
```C++
private:
	static const int MAX_SIZE = 100; // capacity of array
	int data[MAX_SIZE]; // data in vector
```

Finally we implement each of the four member functions:

```C++
IntVector::IntVector()
{
// Documentation
}
```
```C++
int IntVector::at( int index ) const
{
    if (0 <= index && index < max_size)
    	return data[index];
    else 
    {
    cout << “error: index not valid” << endl;
    exit(1);
    }
}
```
```C++
int IntVector::size( void ) const
{
    return max_size;
}
```
```C++
int& IntVector::at( int index )
{
    // exactly the same as the const version!
}
```

Given that the version of the `at` function above returns a reference to the integer stored at the given index and that this reference could be used not only to change that integer but also to retrieve its value, why do we need the `const` version of the `at` function?

Consider the following piece of client code:

```C++
void printVector( const IntVector& myVector )
{
    int capacity = myVector.size();
    
    for( int index = 0; index < capacity; index++ )
        cout << myVector.at( index ) << endl;
}
```
The face that the parameter is declared to be `const` means that the function `printVector` cannot do anything to change the state of the `myVector` object. Hence the only member functions of the `IntVector` class that can be invoked on that object are the `const` member functions. For this reason we need a `const` version of the `at` member function, otherwise we would not be able to access the data stored in the vector.

#### Dynamic Memory Allocation

Our first attempt at writing an `IntVector` class has left us with a new abstract data type that has some serious limitations. In the next few sections we will introduce some concepts that will allow us to develop a far more useful version.

One limitation is that the capacity of an `IntVector` object is fixed at `MAX_SIZE` elements. If we want to change the capacity, and hence the value of `MAX_SIZE`, we have to edit the header file and recompile the program--not pretty!

We can get around this problem by using *dynamic memory allocation*, that is, memory that is allocated to an object or collection of objects at run-time (i.e. while the program is running). Before we talk about dynamic memory allocation, we need to review some concepts on pointers.

A *pointer* is a data type whose value is the address of an object in memory rather than the object itself. Consider the following declarations:

```C++
char* charPtr;
char myChar = 'A';
```

How do we get `charPtr` to "point to" (i.e. contain the address of) `myChar`? We do this using the `&` or "address of" operator:

```C++
charPtr = &myChar;
```

Now that `charPtr` points to `myChar`, we can change the value of `myChar` indirectly by using the pointer. We do this using the * or "dereferencing" operator (meaning *value of this address*):

```C++
*charPtr = 'B'
```

##### Constant Pointers and Constant Objects

Consider the following declarations:

```C++
char char1 = 'A', char2 = 'B';
char* const charPtr = &char1;
```

The variable `charPtr` is declared to be a *constant* pointer to a `char`. In other words, the address stored in `charPtr` cannot be changed once it has been initialized, so the following is not allowed.

```C++
charPtr = &char2;
```

However, you *can* change the object pointed to by `charPtr`:

```C++
*charPtr = 'C';
```

Now suppose that you want the object pointed to by a pointer to be constant:

```C++
char char1 = 'A', char2 = 'B';
const char* charPtr = &char1;
```

You can now change only the address assigned to `charPtr` so that it points to a different object:

```C++
charPtr = &char2;
```

##### Reference Types

Consider the following declarations:

```C++
char myChar = 'A';
char& charRef = myChar;
```

Here, `charRef` is declared to be a *reference* to the character `myChar`. A reference variable is similar to a constant pointer in that:
- the reference variable stores the address of the variable that it is referencing
- this address cannot be changed once it is assigned

Unlike constant pointers, reference variables are automatically dereferenced--it is not necessary to use the * (dereferencing) operator:

```C++
charRef = 'B';
```

##### Reference Parameters

Reference variables are often used as function parameters so that the formal parameter becomes a reference to the actual parameter. Consider the following code segment:

```C++
int x = 2, y = 5;
swap( x, y );

void swap( int& num1, int& num2 )
{
    int temp;
    
    temp = num1;
    num1 = num2;
    num2 = temp;
}
```

C++ has three kind of variables:
- *automatic* variables for which memory is allocated and deallocated automatically
- *static* variables to which memory is allocated for the lifetime of the program
- *dynamic* variables for which memory is allocated and deallocated at a point during the program's execution that is specified by the programmer.

The ability to allocate memory dynamically and assign that memory to objects while the program is running allows the programmer to use memory efficiently. Rather than using arrays that have a capacity specified at compile time, we can use dynamic arrays that have a capacity specified at run time.

We allocate memory dynamically to an object using the `new` operator. This operator returns the address of the object in memory. Consider the following declarations:

```C++
int* intPtr;
intPtr = new int;
```

We can also allocate memory dynamically to an array of integers:

```C++
int capacity;
cout << "How many integers do you want to store?\n";
cin >> capacity;

int* data = new int[ capacity ];

for( int index = 0; index < capacity; index++ )
    cin >> data[ index ];
```

We release memory that has been allocated dynamically using the delete operator:

```C++
delete intPtr;
delete[] data;
```

Note that the delete operator does not delete the pointer! In fact, the value of the pointer is unchanged so the pointer still points to the block of memory that has just been released. A pointer that contains the address of a block of memory that has been freed is referred to as a *dangling pointer*.

In this case we can avoid the dangling pointer by assigning the value 0 to the pointer after performing the delete operation. A pointer that has the value 0 assigned to it is called a *null pointer*.

Dangling pointers can also be created when more than one pointer points to the same object:

```C++
int* intPtr1;
int* intPtr2;

intPtr1 = new int;
*intPtr1 = 7;
intPtr2 = intPtr1;
delete intPtr1;
intPtr1 = 0;
```

Rule of thumb: release memory allocated to an object only when you're sure that there is only one pointer pointing to it!

A **memory leak** occurs when:
- there is no pointer pointing to a dynamic object (making the object inaccessible)
- memory allocated to an object that is no longer needed is not released

```C++
int* intPtr;
intPtr = new int;
*intPtr = 7;
intPtr = new int;
*intPtr = 5;
```

The block of memory holding 7 is now inaccessible but has not been released.

##### Variable Lifetime and Scope

Every C++ variable has two characteristics: *lifetime* (or extent) and *scope* (or visibility).

C++ has four types of variable scope:
- *local scope*: variable is visible only inside the block of code { } in which it is defined, e.g. function parameters
- *global scope*: variable is declared outside of any block and is visible throughout the source file in which it is declard and potentially in other source files as well
- *file scope*: variable is visible only within the source file in which it is declared (ie. `static` variables)
- *class or struct scope*: variable is a data member of a `class` or `struct` and is visible only within that `class` or `struct`

C++ has three types of variable lifetime:
- *automatic*: memory is allocated to the variable whenever the block in which it is declared is entered, and destroyed when the block is exited
- *dynamic*: memory is allocated at run-time using the new operator and released using the delete operator
- *static*: memory is allocated when the program is loaded and is destroyed only when the program ends

##### Memory Organization

There are four segments in memory when a C++ program is running:
- *Code segment*: contains the machine language code produced by the compiler and any constant data (e.g. const int, const double)
- *Static data segment*: contains all data corresponding to variables that have been declared to be static.
- *Stack segment*: contains activation recrods (or stack frames) that are generated when a function is called. Activation records store:
  - function parameters
  - local, automatic variables
  - return address (i.e. address of next instruction to be executed when function call ends)
  - return value and other bookkeeping information
- *Heap segment*: the area of memory that is used for dynamic memory allocation. Data stored in the heap is accessed indirectly through pointers.

##### Memory Fragmentation

When a request is received for memory to be allocated dynamically, a portion of the heap big enough to store the data is allocated to that data assuming that memory is available. We know that as well as allocating memory dynamically, we can also de-allocate that memory using the `delete` operator. After a large number of allocations and de-allocations, the memory can become fragmented making it impossible to allocate memory to larger data.

Memory is allocated by the operating system using any of the following strategies:

- *First fit*: the first block of memory large enough to accommodate the request is used
  - Pros: speed
  - Cons: potentially causes more fragmentation/less efficient
- *Best fit*: the smallest suitable block of memory is used
  - Pros: less fragmentation/more efficient
  - Cons: takes more time

##### Copying Objects

Consider the following very simple class:

```C++
class CSample
{
public:
    CSample( void );
    
    int getData( void ) const; // const after function means it cannot change any member vals unless mutable
    void setData( int data );

private:
    int m_data;
};
```

There is one operator that can always be applied to any object regardless of whether or not it is explicitly declared in the class declaration--the assignment (`=`) operator. If we do not implement the assignment oeprator, the compiler provides a default version.

Hence, the following client code is valid:

```C++
Csample sample1, sample2;

sample1.setData( 7 );
sample2 = sample1;
cout << sample2.getData;
```

When the code on the third line is exectued, the default version of the assignment operator uses the data members in the object `sample1` to initialize the corresponding data members in `sample2`--this is a completely natural thing to do.

We say that the assignment operator has been *overloaded* so that it is defined for the objects of type `CSample`. We sill say more about operator overloading later.

In many situations, the default version of the assignment operator supplied by the compiler will perform adequately--but there are exceptions, as we will see.

A similar thing happens when we pass an object by value:

```C++
CSample sample1;
sample1.setData( 7 );
myFunc( sample1 );

void myFunc( CSample sample )
{ ...
}
```

When we pass an object by value, a special constructor called a *copy constructor* is implicitly invoked. If we do not implement a copy constructor for a class, the compiler implements one for us. The version provided by the compiler simply copies the data members from one object and uses them to initialize the data members in another. Again, this is a natural thing to do.

The copy constructor can in fact be invoked explicitly as follows:

```C++
CSample sample1;
sample1.setData( 7 );
CSample sample2( sample1 );
cout << sample2.getData();
```

Now that we have introduced the notion of an overloaded assignment operator and a copy constructor, we will attempt to create an improved version of our `IntVector` class.

Recall that our first version stored the data in an array. Hence the capacity of the vector had to be declared at compile time. We will now create an improved version that uses dynamically allocated memory to an array so that the capacity of our vector could potentially be specified at run-time.

We would like a client of our `IntVector` class to be able to do the following:

```C++
int capacity;
cout << "How many integers do you want to store? ";
cin >> capacity;
IntVector myVector( capacity );
```

In order to be able to specify the capacity of the vector, we need to provide a constructor that accepts the capacity as a parameter:

```C++
IntVector( int numInts );
// Pre: numInts > 0
// Post: vector has capacity numInts integers
```

We will also supply a default constructor that creates a vector having a default capacity of 100 integers. The declaration of our class now looks as follows:

```C++
class IntVector
{
public:                          // not yet complete!
    IntVector( void );
    // Post: vector has a capacity of MAX_SIZE integers
    IntVector( int numInts);
    // Pre: numInts > 0
    // Post: vector has capacity of numInts integers
    int at( index ) const;
    // Post: if 0 <= index < size() the element
    // at index is returned, otherwise program aborted
    int size( void ) const;
    // Post: capacity of vector has been returned
    int& at( index );
    // Post: if 0 <= index < size() the element
    // at index is returned, otherwise program aborted
private:
    static const int max_size = 100;
    int* data; // pointer to dynamic array
    int capacity; // max capacity of vector
}
```

Note that other than the additional constructor, the public interface for our `IntVector` class has not changed. This is a good thing. It means that any existing client code that is based on the previous version of our `IntVector` class will not need to be changed! All we have to do is compile the new `IntVector` class and link it to the existing client code. We say that the new version of our `IntVector` class is *backwards compatible* with the old version--all the operations that were available in the old version are supported in the new version.

We will now look at the implementation of the member functions:

```C++
IntVector::IntVector( void )
// Post: vector has a capacity of MAX_SIZE integers
{
    data = new int[ max_size ];
    capacity = max_size;
}

IntVector::IntVector( int numInts )
// Pre: numInts > 0
// Post: vector has capacity of numInts integers
{
    data = new int[ numInts ];
    capacity = numInts;
}

int IntVector::at( int index ) const
// Post: if 0 <= index < size() the element
// at index is returned, otherwise program aborted
{
    if( index >= 0 && index < capacity )
        return data[ index ];
    else
    {
        cout << "Index out of range" << endl;
        exit(1);
    }
}

int IntVector::size( void ) const
// Post: capacity of vector has been returned
{
    return capacity;
}

int& IntVector::at( int index )
// Post: if 0 <= index < size() the element
// at index is returned, otherwise program aborted
{
    // same as const version!
}
```

This new version of our `IntVector` class allows the client to specify the capacity of the `IntVector` without having to recompile the program--a significant improvement on the previous version.

However, we're about to discover that having made use of dynamic memory allocation, there are some extra considerations that msut be accommodated. Consider the collowing client code:

```C++
IntVector myVector( 4 );

myVector.at( 0 ) = 27;
myVector.at( 2 ) = -32;

IntVector myOtherVector( 4 );
myOtherVector = myVector;
myOtherVector.at( 0 ) = 12;

cout << myVector.at( 0 ) << " "
     << myOtherVector.at( 0 );
 ```
 
Output: `12 12`

Not exactly what we would expect! The problem lies on line 5 of the code where the assignment operator is invoked. Note that we did not explicitly overload the assignment operator and hence the default version supplied by the compiler has been used. Let's think about what this default version of the overloaded assignment operator has done.

We call this a *shallow copy* because we have copied only the pointers and not the objects to which the pointers are pointing. The vectors end up sharing the same array of data. What we want is a *deep copy*.

We run into a similar problem when the compiler-supplied version of the copy constructor is used. WE must therefore supply definitions of the overloaded assignment operator and copy constructor so that a copy of the object rather than the pointer is created. Then, modifying the data stored in `myOtherVector` will not result in changes to the data stored in `myVector`.

##### The Copy Constructor

The declaration of the copy constructor for our `IntVector` class looks as follows:

```C++
IntVector( const IntVector& otherVector );
// Post: this vector is a copy of otherVector
```

Note that, just like any other constructor, the copy constructor has the same name as the class and it does not specify a return type.

Also note that the parameter is passed by reference--this is essential, otherwise a copy of the parameter would have to be made which would require a recursive call to the copy constructor. No base case causes infinite recursion, which causes stack overflow.

Most compilers will generate an error if you attempt to use a value type parameter when declaring a copy constructor.

Note that in order to implement both the copy constructor and the overloaded assignment operator, we need to make a copy of the data stored in a dynamic array. We will therefore write a helper function to do this. This funtion will be declared in the private section of the class because it is not an operation that a client needs to know about.

```C++
void IntVector::copy( const IntVector& otherVector )
// Post: this vector is a copy of otherVector
{
    capacity = otherVector.capacity;
    data = new int[ capacity ];
    for( int index = 0; index < capacity; index ++ )
        data[ index ] = otherVector.data[ index ]
}
```

We can now use our `copy` helper function in our implementation of the copy constructor and overloaded assignment operator:

```C++
IntVector::IntVector( const IntVector& otherVector )
{
    copy( otherVector );
}
```

##### The Keyword `this`

The keyword `this` can be used in any member function of any class to provide a pointer to the object that invoked the member function. So, when implementing member functions, we can think of `*this` as referring to "this" object, in other words, the object whose data we are currently operating on.

##### The Assignment Operator

The declaration of the overloaded assignment operator for our `IntVector` class looks as follows:

```C++
IntVector& operator=( const IntVector& otherVector );
// Post: this vector is a copy of otherVector
```

Note again that the parameter is passed by reference. Note also that this operator returns a reference to an object of type `IntVector`.

To explain why we return a reference to an `IntVector` object, we need a bit of terminology. The `=` operator is a binary operator--it takes two operands, a left operand and a right operand. For example:

```
vector2 = vector1;
```

The assignment operator will return a reference to the left operand, in this case `vector2`. In the example above, we do not make use of this reference. However, consider the following, keeping in mind that the assignment operator is *right associative*:

```
vector3 = vector2 = vector1;
```

If the return type of the assignment operator were `void`, the assignment `vector3 = void` would result, which doesn't make sense.

Before we attempt to implement the assignment operator for our `IntVector` class, there is one more question we must ask. When an assignment such as `vector2 = vector1;` is made, which vector is considered to have invoked the assignment operator? In other words, when we implement the assignment operator, which vector does `this` point to? The question is easily answered when we consider an alternative syntax for invoking operators in C++. The above line is equivalent to:

```C++
vector2.operator=( vector1 );
```

We are now ready to attempt to implement the assignment operator for our `IntVector` class.

```C++
IntVector& IntVector::operator=(
            const IntVector& otherVector )
{
    if( this != &otherVector )
    {
        delete[] data;
        copy( otherVector );
    }
    return *this;
}
```

We have now covered two of the Big Three: the copy constructor and the overloaded assignment operator.

Consider the following client code:

```C++
void myFunc( int size )
{
    IntVector myVector( 75 );
    // do whatever with the vector
}
```

Note that the lifetime of `myVector` is automatic. Hence, memory is allocated to the data members of `myVector` when `myFunc` is called and it is released when the call to `myFunc` ends.

Unfortunately, the memory allocated to the pointer data is not automatically released. In order to release this memory we need to implement a special member function called a *destructor*--the last of the Big Three. A destructor is a member function of a class that is automatically invoked when an instance of that class goes out of scope (i.e. is destroyed).

We must include the declaration of the destructor in the public section of the class declaration:

```C++
~IntVector( void );
// Post: memory allocated to vector has been released
```

Note that destructors do not have a return type specified and that the name of a destructor is always the same as the name of the class preceded by a `~`. Destructors are used to perform any "clean-up" operations that might be necessary before an object is destroyed.

The implementation of the destructor for our `IntVector` class looks as follows:

```C++
IntVector::~InVector( void )
// Post: memory allocated to vector has been released
{
    delete[] data;
}
```

We have now covered the Big Three:
- copy constructor
- overloaded assignment operator
- destructor

Whenever you implement a class that has a data member that points to dynamically allocated memory, you must provide an implementation of the Big Three. You cannot rely on the default version of these member functions provided by the compiler.


