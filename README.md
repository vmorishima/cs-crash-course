Useful links:

- [Problem Solving with Algorithms and Data Structures](http://interactivepython.org/runestone/static/pythonds/index.html)
- [Introduction to Algorithms](http://bayanbox.ir/view/4177858657730907268/introduction-to-algorithms-3rd-edition.pdf)
- [Cracking the Coding Interview](http://www.eenadupratibha.net/Engineering-Colleges/Engineering-Jobs/Documents/crackingthecodinginterview.pdf)

#### CS 252 Table of Contents

1. [Software Engineering Principles](#1-software-engineering-principles)
  - Software life cycle
  - Modules and modular design
  - Top down and object oriented design

2. [Data Abstraction and Classes](#2-data-abstraction-and-classes)
  - Abstract data types and classes in C++
  - Constructors, destructors, friends, and operator overloading

3. [Concrete Data Structures](#3-concrete-data-structures)
  - Data structure overview
  - Concrete vs abstract data structures, linear vs nonlinear data structures
  - Fundamental data structures in C++: arrays and pointers, structs and unions, linked lists

4. [Recursion](#4-recursion)
  - Recursive functions
  - Recursion vs iteration
  - Recursive data structures: binary trees

5. [Basic Abstract Data Types](#5-basic-abstract-data-types)
  - General concepts, templates
  - Basic abstract data types: vectors, sets, bags, lists, stacks, queues, tables

6. [Object Oriented Design](#6-object-oriented-design)
  - Inheritance
  - Virtual functions
  - Inheritance and program design

7. [Search Structures and Searching Algorithms](#7-search-structures-and-searching-algorithms)
  - Complexity analysis and big-O notation
  - Linear and binary search
  - Hashing, linear-probed hash tables, chained hashing
  - Binary search trees, AVL trees
  - Heaps - priority queues

8. [Sorting Algorithms](#8-sorting-algorithms)
  - Insertion, selection and bubble sort
  - Quicksort, average and worst case analysis
  - Tree sort, heap sort, finding nth largest

9. [External Searching and Sorting](#9-external-searching-and-sorting)
  - Searching on disk: m-way search trees, b-trees
  - Sorting on disk - mergesort

10. [Graphs](#10-graphs)
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


# 2. Data Abstraction and Classes
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

##### Operator Overloading

When we want to assign an element to a vector or retrieve an element stored in a vector, we can do so using the indexing operator [] rather than the at() member function:

```C++
vector<int> myVector( 4 );
myVector[0] = 34;
cout << myVector[0] << endl;
```

We are now going to consider how to add this functionality to our `IntVector` class.

For the same reason that we have two versions of the `at()` member function, we need to supply two versions of the overloaded indexing operator:

```C++
int operator[]( int index ) const;
// Pre: if 0 <= index < size() the element at
// index is returned, otherwise program aborted

int& operator[]( int index );
// Pre: if 0 <= index < size() the element at
// index is returned, otherwise program aborted
```

The implementation of these operators is exactly the same as the corresponding `at()` function. We can simply call the `at()` function from the `operator[]` function:

```C++
int IntVector::operator[]( int index ) const
{
    return at( index );
}
```

This has the advantage of being concise, but it is a little inefficient because of the cost of the function call. We have to remember that this operator may be called many times by a client program.

A more efficient implementation can be had by duplicating the code that is found in the `at()` function. The disadvantage here is that the code becomes "bloated" and we see blocks of duplicate code. We can still use the concise version of the `operator[]` defined previously and possibly avoid the overhead of a function call by declaring the `at()` functions to be `inline`. The `inline` keyword *suggests* that the compiler replace each call to the `inline` function with the body of that function, thereby avoiding the function call. Note the use of the word *suggests*--the compiler can choose not to `inline` a function if it sees fit!

We declare a member function to be inline by adding the `inline` keyword at the beginning of the function definition:

```C++
inline int IntVector::at( int index ) const
{
...
}
```

# 3. Concrete Data Structures

There are two kinds of data types:
- *Simple* (or *atomic*) -- represents a single data item: int, char
- *Structured* -- represents a collection of items and has:
  - domain: description of item associated with the structure
  - structure: description of how items are organized/arranged within structure
  - operations: operations that can be performed on the collection

Data structures can be classified in the following way:
- *Linear* -- there is a unique first and unique last element and every other element has a unique predecessor and a unique successor
- *Non-linear* -- a data structure that is not linear

Linear data structures can be further classified as:
- *Direct-access*: elements can be accessed at random, in any order
- *Sequential-access*: elements must be accessed in some specified order

Data structures can further be classified as:
- *Homogeneous*: all the elements in the structure are of the same data type
- *Heterogenous (non-homogeneous)*: the elements in the structure are of different types, for example: a record (`struct` in C++)

A `struct` is just like a class with the exception that, by default, the member are public. For example:

```C++
struct date
{
    int day;
    string month;
    int year;
};
```

##### Concrete vs. Abstract Data Types

*Concrete Data Types* (CDTs) are direct implementations of fairly simple structures. Abstraction and information hiding are not employed when designing CDTs.

*Abstract Data Types* (ADTs) offer a high-level, easy-to-use interface that is independent of the ADT's implementation.

Example: We can implement a student record either as a CDT or an ADT:

CDT: implement it as a `struct` where all the data is public:

```C++
struct student
{
    string name;
    int idNum;
};
```

ADT: Implement it as a class where the data is private but a set of public operations is defined that allow the client to operate on the data.

We will now examine some basic concrete data types.

##### Records

Records allow us to group together data of potentially differing data types into a single unit. In C++ records are implemented using `struct`s.
- *Domain*: a collection of items of potentially different data types and a set of names for the items in the record
- *Structure*: objects are stored in consecutive memory locations and each object has a unique name
- *Operations*: the '.' operator provides access to each element in the record.

Example:

```C++
struct Course
{
    string dept;
    int number;
};
// client code below
Course compCourse;
compCourse.dept = "CPSC";
compCourse.number = 252;
```

Note that we have access to the individual data members using the '.' operator. No attempt is made to "hide" the data behind a set of public operations.

##### Arrays

An array is a linear, direct access, homogeneous, concrete data structure.
- *Domain*: a collection of *n* items of the same data type and a set of indexes in the range [ 0, *n* ]
- *Structure*: items are stored in consecutive locations each having a unique index
- *Operations*: the indexing operator [] gives read and write access to the item at the specified index.

Given that we are classifying an array as a CDT, we should have a good understanding of how it is implemented. It turns out that there is a strong connection between arrays and pointers. Consider the following declaration:

```C++
int data[ 3 ];
```

This declaration allocates memory for three integers and assigns the address of the first byte of this memory to `data` (which can be thought of as a constant pointer).

##### Pointer Arithmetic

The algebraic operators +, -, ++ and -- are overloaded to operate on pointers. So, `(data + 2)` is a pointer to the third integer in the array. In other words, `(data + 2)` points to the integer at index 2! Hence, we can assign a value to the integer at index 2 as follows:

```C++
data[ 2 ] = 7;
// or
*(data + 2) = 7; // value at (data + 2) is 7
```

Notice that there is something rather subtle going on here. By adding the integer 2 to data, we do not produce an address that is 2 bytes higher than the one stored in data. We in fact produce an address that is 8 bytes higher, assuming that each integer occupies 4 bytes:
|     pointer     |  array   |  memory      |
|----------|:-----:|---------|
|   data   | ___ | 4 bytes |
| data + 1 | ___ | 4 bytes |
| data + 2 | ___ | 4 bytes |

The following code again illustrates the connection between indexing into an array and pointer arithmetic:

```C++
int data[ 4 ] = { 4, 3, 2, 1 };

for( int index = 0; index < 4; index++ )
    cout << *( data + index ) << " ";

// output: 4 3 2 1
// equivalent to data[ index ]
```

Arrays can be passed as parameters to functions. A function `printArray` can be declared to accept an array of integers as a parameter in either of the following ways:

```C++
void printArray( int data[], int size );
void printArray( int* data, int size );
```

Regardless of the way in which the function is declared, it can be implemented in either of the following ways:

```C++
{
    for( int index = 0; index < size; index++ )
        cout << data[ index ] << " ";
}
// or
{
    for( int index = 0; index < size; index++ )
        cout << *(data + index) << " ";
}
```

Finally, let's recall how we allocate an array dynamically:

```C++
int* data = new int[ 3 ];
```

This appears no different from the previous array, however the array is stored in different segments of memory. With the previous array, the data is stored in the stack segment, whereas the data in the dynamically allocated array is stored in the heap segment. We can access elements in a dynamically allocated array using either the indexing operator or pointer arithmetic.

##### Multidimensional Arrays

Suppose we want to store the average rainfall during each hour for a period of one week. We can do so conveniently using a two-dimensional array:

```C++
float rainfall[ 7 ][ 24 ];
```

We can think of the two-dimensional array as a table having 7 rows (one for each day) and 24 columns (one for each hour). To set the rainfall on the first day for the hour between noon and 1pm:

```C++
rainfall[ 0 ][ 12 ] = 1.2;
```

What if we want to use dynamic memory allocation? One way to proceed is to declare an array of pointers to each row in the table and then dynamically allocate memory to each row:

```C++
int* data[ 3 ]; // array of 3 pointers
data[ 0 ] = new int[ 3 ];
data[ 1 ] = new int[ 2 ];
data[ 2 ] = new int[ 4 ];
```

We can index into the array just as before. Notice that this has given us the flexibility of having a two dimensional array where the rows are of different lengths! But we are still faced with the restriction that if we want to change the number of rows, we have to re-compile the program because the dimension of the array of pointers must be a constant.

We can gain the most flexibility by declaring data to be a pointer to a pointer to an integer. We can then declare the array of pointers dynamically as well as dynamically allocating memory to each row:

```C++
int** data; // ** creates pointer to a pointer
data = new int*[ 2 ];
data[ 0 ] = new int[ 4 ];
data[ 1 ] = new int[ 3 ];
```

Again, we can index into the array just as before.

##### Linked Lists

*Motivation*: We now have a version of the `IntVector` class that uses dynamic memory allocation. This allows us to specify the dimension of an object of type `IntVector` at run-time. However, if we specify the dimension of an `IntVector` to be, say 100, but then determine that we in fact want to store 110 integers, it is an expensive operation to increase the capacity of the vector:
- allocate memory to a larger array
- copy the data from the original array to the new array
- release the memory allocated to the original array

It is not possible to increase the capacity of the original array.

A linked list will allow us to store data in a structure that can grow or shrink dynamically as needed. A (singly) linked list is a linear, sequential access, homogeneous data structure.
- *Domain*: a pointer to a node at the head of the list; a collection of nodes each of which contains a data element
- *Structure*: there is a pointer to the first node in the list and each node contains a pointer that points to the next node in the list with the exception of the last.
- *Operations*: insertFirst, insertLast, insertAfter, find, deleteItem, printNode, etc.

We will now implement a linked list toolkit--a module that contains the implementation of the operations that we want to support. In doing so, we are implementing the linked list as a concrete data type.

First we must determine how to represent a node. This is a simple data structure so we will implement it as a CDT using a `struct`. We will assume that a `typedef` statement is used to define the data type of the elements to be stored in the list:

```C++
typedef int Item_type;

struct Node
{
    Item_type item; // data stored in node
    Node* next; // points to next node in list
};
```

Now let's suppose that we declare a pointer to a `Node` object and dynamically allocate memory to a new node:

```C++
Node* nodePtr;
notePtr = new Node;
```

When we insert a new element into a linked list, we need to create a new node. We will start by writing a helper function to do this:

```C++
Node* makeNode( const Itemtype& theItem,
                        Node* nextNode = 0 )
// Post: a new node is created containing theItem; the
// new node is linked to nextNode (or is null); a
// pointer to the new node is returned
{
    Node* newNodePtr = new Node;
    newNodePtr->item = theItem;
    newNodePtr->next = nextNode;
    return newNodePtr;
}
```

Note that in our documentation, we do not specify what will happen if our request for a new node fails due to insufficient memory. We will address this problem later in the course.

We will now write functions to implement each of the other operations. When working with linked lists, *draw a picture* of what it is you are trying to do. You will then find the implementation a lot easier!

```C++
void insertFirst( Node*& head,
                  const Item_type& item )
// Pre: head points to the head of a list or is null
// Post: item is inserted at the front of the list
{
    Node* newNode = makeNode(item, head);
    head = newNode;
}

void insertLast( Node*& head,
                  const Item_type& item )
// Pre: head points to the head of a list or is null
// Post: item is inserted at the end of the list
{
    Node* newNode = makeNode( item );
    if ( head == null)
        head = newNode;
    else
    {
        node *cursor = head;
        while (cursor->next != null)
            cursor = cursor->next;
            cursor->next = newNode;
    }
}

Node* find( Node* head,
                  const Item_type& item )
// Pre: head points to the head of a list or is null
// Post: if the item is in the list, a pointer to the node
// containing it is returned; otherwise a null pointer
// is returned
{
    Node* cursor;
    while ( cursor != null && cursor->item != item )
        cursor = cursor->next;
    return cursor;
}

bool deleteItem( Node* head,
                  const Item_type& item )
// Pre: head points to the head of a list or is null
// Post: if item is in the list it has been deleted
// and true returned; otherwise false returned
{
    Node* cursor = head;
    Node* previousNode = null;
    while ( cursor != null && cursor->item != item )
    {
        previousNode = cursor;
        cursor = cursor->next;
    }
    if ( cursor == null ) // item not found
        return false;
    else // item found
    {
        if ( previousNode == null ) // item is the first one
            head = head->next;
        else
        {
            previousNode->next = cursor->next;
            delete cursor;
        }
        return true;
    }
}
```

Let's now consider the `printNode` function:

```C++
void printNode( Node* thisNode )
// Pre: thisNode points to a node, the << operator is
// overloaded for objects of type Item_type
// Post: the item in the node pointed to by thisNode has
// been printed on screen
{
    cout << thisNode->item << endl;
}
```

Now let's consider the problem of traversing an entire linked list and operating on the data in each node. For example, we may want to print out the data in each node. We will write a function `traverse` that receives a pointer to a linked list and a pointer to a function as parameters. We will assume that the function pointed to has a `void` return type and takes a pointer to a `Node` as a parameter--our `printNode` function, for example!

```C++
void traverse( Node* head, void ( *visit ) ( Node * ) )
// Pre: head points to the head of a list or is null;
// visit points to a function that takes a pointer to
// a node as a parameter and returns void
// Post: the function pointed to by visit is applied to each node in the list
{
    Node* cursor;
    for( cursor = head; cursor != null; cursor = cursor->next )
        ( *visit )( cursor );
}
```

We should now give some consideration to how the traverse function will be called. Let's suppose that we want to print every node in a list pointed to by `head`:
```C++
traverse( head, printNode );
```

Now let's suppose that we want to traverse a linked list and set all the data elements to zero. We will assume that Item_type is such that the operation of assigning a value of zero to such an item is defined.

Given that we already have the traverse function, all we have to do is write a function to assign a value of zero to the item in a node. If we are to pass this function as a parameter to traverse we must make sure that it has a `void` return type and that it takes a single parameter of type `Node*`:

```C++

void makeZero( Node* thisNode )
// Pre: thisNode is a pointer to a node; a value of 0
// can be assigned to an Item_type
// Post: the item in the node pointed to by thisNode
// has been set to 0
{
    thisNode->item = 0;
}
```

If we now have a linked list pointed to by head, we can set every item in the list to zero by calling the traverse function:

```C++
traverse( head, makeZero );
```

The traverse function provides us with a reasonable amount of flexibility in that we specify how each node is to be processed by passing a pointer to a function that will process the node. However, we are restricted by the fact that the function processing each node must have a `void` return type and must take a single parameter of type `Node*`.

#### Variations on Singly Linked Lists

##### Head Nodes / Dummy Nodes

Recall that the algorithm for inserting a node at the front of a singly linked list is different from that for inserting a node at any other point in the list. The same can be said for deleting a node. The problem is that when we insert or delete at the front of the list, there is no "previous node". Hence, insertion or deletion at the front of a list is a special case that needs to be checked--this leads to a certain amount of inefficiency when coding these operations. We can avoid this situation by using a head node or dummy node. A head node is not used to store data. It is created when the list is created. If we now want to insert an item at the head of the list, we insert it *after* the head node.

The following code segment can now be used to insert a node into the list no matter where the insertion occurs. We assume that `previousNode` points to the node after which the new node is to be inserted:
```C++
Node* newNode = new Node;
newNode->next = previousNode->next;
previousNode->next = newNode;
```
The use of a head node therefore simplifies our algorithms for insertion and deletion, However, we must realize that we are paying the price of allocating memory to a node that is not used to store data.

##### Circular Linked Lists

A circular linked list is such that the node at the end of the list contains a link to the node at the head of the list.

Suppose we have a computer system that supports multiple users. Users can log in or log off from the system at any time so it is useful to maintain a list of users using a linked list--the list can then grow or shrink as needed and we can delete users from the middle of the list without having to do any shifting (as would be the case if we used a circular array).

The users are kept in a list so that they each get access to the system's resources for some fixed period of time. Once all of the users have had a turn at accessing the system's resources, we then go back to the beginning of the list and give each of the users another turn and so on. Rather than having to check for the end of the list and then resetting the cursor to point to the head of the list, it is more convenient to have the last node point to the first so we can simply advance the cursor, thereby creating a circular linked list.

##### Doubly Linked Lists

A doubly linked list is similar to a singly linked list except that each node has a pointer to the previous node as well as a pointer to the next node.

Such lists are useful when we want to be able to traverse the list in either direction. Having a doubly linked list can also make the operation of deleting a node easier. Let's suppose we have a singly linked list and a pointer to a node in that list that we want to delete.

In order to complete this operation, we need a pointer to the previous node. The only way to get this pointer is to start at the head of the list and traverse the list until we locate the previous node. However, if we have a doubly linked list, getting a pointer to the previous node is trivial since every node contains a pointer to both the previous node and the next node (assuming these nodes exist).

Another use for a doubly linked list is to maintain a list of data in sorted order based on two different criteria. Suppose, for example, we have a list of students each having a last name and a student number. We can sort by last name or by student number. We can maintain a list of students sorted using both criteria by using a doubly linked list with two head pointers.

Building a list in this way is useful when we have a large number of users accessing the same data--some want the data sorted one way, other another. Rather than re-sorting the data as necessary, we link the data together using both sorting orders.

However, building such a list is expensive (and algorithmically challenging). Each node requires two pointers. When we insert a new node, we have to search to find an appropriate insertion point twice--once to maintain the order by name and again to maintain the order by number.

#### Binary Trees

*Motivation*: Suppose we want to search for data stored in a structure. We know that if the data is stored in an array, we can use a simple linear search (O(n)) or we can use binary search (O(log n)). Now let's suppose the data is stored in a linked list.

Again, we can use a simple linear search algorithm (as we did in our `find` function in the singly linked list toolkit). However, the use of a binary search algorithm requires us to be able to directly access the item in the middle of the list--as a linked list is a sequential access structure, this is not possible--we have to traverse the list to find the midpoint and this degrades the performance of the algorithm.

In a binary tree (a binary search tree, a special case of binary tree), at each node in the tree, the data to the left is smaller and the data to the right is larger (alphabetically). Suppose we are searching for the letter J. We start at the top of the tree (called the root), knowing that J is greater than G, we can discard the left half of the tree and search only the right half. We have therefore cut down the amount of data to be searched by half--just as we do when searching an array using a binary search algorithm! Similarly, when we arrive at the node containing K, we know that J is smaller than K, so we discard the right half of the remaining tree and search the left.

Definition: A binary tree is a structure that is either empty or consists of a node called a *root* and two binary trees called the *left sub-tree* and *right sub-tree*. Note that this definition is *recursive*--we define a binary tree as a structure that consists of two other (sub) trees.
- *Domain*: a set of nodes containing one data item and two pointers to other nodes (the set could be empty)
- *Structure*: there is a unique root node (that has no parent node) having zero, one, or two child nodes; every other node has exactly one parent node and either zero, one or two child nodes
- *Operations*: insertLeft, insertRight, find, findParent, deleteItem, print, etc.

Some terminology:

The *path* from node N1 to node Nk is a sequence of nodes N1, N2, ..., Nk where Ni is a parent of Ni+1. The *length* of the path is the number of nodes in the path (some texts use the number of edges rather than number of nodes).

The *depth* or *level* of a node N is the length of the path from the foor to N. The level of the root is 1.

The *height* of a node N is the length of the longest path from N to a leaf node. The height of a leaf node is 1. The *height of a tree* is the height of the root node.

The number of nodes in a tree of height h is at least h and no more than (2^h)-1.

