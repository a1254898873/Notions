# fundamentals 

## Data Types

### (1) int

Its size is usually 4 bytes. Meaning, it can store values from -2147483648 to 2147483647.

```c++
int salary = 85000;
```



### (2) float and double

The size of float is 4 bytes and the size of double is 8 bytes. Hence, double has two times the precision of float

```c++
double distance = 45E12    // 45E12 is equal to 45*10^12
```



### (3) char

Characters in C++ are enclosed inside single quotes ' '.

```c++
char test = 'h';
```



### (4) bool

```c++
bool cond = false;
```







## IO

### (1) Output

The cout object is defined inside the std namespace. To use the std namespace, we used the using namespace std; statement.

 If we don't include the using namespace std; statement, we need to use std::cout instead of cout

```javascript
#include <iostream>
using namespace std;

int main() {
    // prints the string enclosed in double quotes
    cout << "This is C++ Programming";
    return 0;
}

```



### (2) Input

```c++
#include <iostream>
using namespace std;

int main() {
    int num;
    cout << "Enter an integer: ";
    cin >> num;   // Taking input
    cout << "The number is: " << num;
    return 0;
}

```







## Type Conversion

C-style Type Casting:

```c++
// initializing int variable
int num_int = 26;

// declaring double variable
double num_double;

// converting from int to double
num_double = (double)num_int;
```

Function-style Casting:

```c++
// initializing int variable
int num_int = 26;

// declaring double variable
double num_double;

// converting from int to double
num_double = double(num_int);
```







## Functions

### (1) function prototype

In C++, the code of function declaration should be before the function call. However, if we want to define a function after the function call, we need to use the function prototype.

```c++
// function prototype
void add(int, int);

int main() {
    // calling the function before declaration.
    add(5, 3);
    return 0;
}

// function definition
void add(int a, int b) {
    cout << (a + b);
}
```

The syntax of a function prototype is:

```c++
returnType functionName(dataType1, dataType2, ...);
```



### (2) Default Arguments

```c++
#include <iostream>
using namespace std;

// defining the default arguments
void display(char c = '*', int count = 3) {
    for(int i = 1; i <= count; ++i) {
        cout << c;
    }
    cout << endl;
}

int main() {
    int count = 5;

    cout << "No argument passed: ";
    // *, 3 will be parameters
    display(); 
    
    cout << "First argument passed: ";
     // #, 3 will be parameters
    display('#'); 
    
    cout << "Both argument passed: ";
    // $, 5 will be parameters
    display('$', count); 

    return 0;
}
```

Once we provide a default value for a parameter, all subsequent parameters must also have default values. 

```c++
// Invalid
void add(int a, int b = 3, int c, int d);

// Invalid
void add(int a, int b = 3, int c, int d = 4);

// Valid
void add(int a, int c, int b = 3, int d = 4);
```

If we are defining the default arguments in the function definition instead of the function prototype, then the function must be defined before the function call.

```c++
// Invalid code

int main() {
    // function call
    display();
}

void display(char c = '*', int count = 5) {
    // code
}
```







## Arrays

### (1) Array Declaration

```c++
dataType arrayName[arraySize];
```

```c++
int x[6];
```





### (2) Ranged for Loop

```c++
// initialize an int array
int num[3] = {1, 2, 3};

// use of ranged for loop
for (int var : num) {
    // code
}
```

```c++
#include <iostream>
#include <vector>
using namespace std;

int main() {

    // declare and initialize vector  
    vector<int> num_vector = {1, 2, 3, 4, 5};

    // print vector elements  
    for (int n : num_vector) {
        cout << n << " ";
    }
  
    return 0;
}
```

it's better to write the ranged based for loop like this:

```c++
// access memory location of elements of num
for (int &var : num) {
    // code
}
```

Notice the use of `&` before var. Here,

- `int var : num` - Copies each element of num to the var variable in each iteration. This is not good for computer memory.
- `int &var : num` - Does not copy each element of num to var. Instead, accesses the elements of num directly from num itself. This is more efficient.

 If we are not modifying the array/vector/collection within the loop, it is better to use the const keyword in range declaration.

```c++
// collection is not modified in the loop
for (const int &var : num) {
    // code
}
```





### (3) Pass Arrays to Functions

C++ does not allow to pass an entire array as an argument to a function. However, You can pass a pointer to an array by specifying the array's name without an index.

If you want to pass a single-dimension array as an argument in a function, you would have to declare function formal parameter in one of following three ways and all three declaration methods produce similar results because each tells the compiler that an integer pointer is going to be received.

```c++
void myFunction(int *param) {
   .
   .
   .
}
```

```c++
void myFunction(int param[10]) {
   .
   .
   .
}
```

```c++
void myFunction(int param[]) {
   .
   .
   .
}
```





## String

In C++, you can also create a string object for holding strings.

Unlike using char arrays, string objects has no fixed length, and can be extended as per your requirement.

```c++
#include <iostream>
using namespace std;

int main()
{
    // Declaring a string object
    string str;
    cout << "Enter a string: ";
    getline(cin, str);

    cout << "You entered: " << str << endl;
    return 0;
}
```

Instead of using cin>> or cin.get() function, you can get the entered line of text using getline().







## Structures

In C++, a structure is the same as a class except for a few differences. The most important of them is security. A Structure is not secure and cannot hide its implementation details from the end-user while a class is secure and can hide its programming and designing details. Following are some differences between a class and a structure.

| Class                                       | Structure                                     |
| :------------------------------------------ | :-------------------------------------------- |
| Members of a class are private by default.  | Members of a structure are public by default. |
| Memory allocation happens on the heap.      | Memory allocation happens on a stack.         |
| It is a reference type data type.           | It is a value type data type.                 |
| It is declared using the **class** keyword. | It is declared using the **struct** keyword.  |





## Class

### (1) Create a Class

```c++
class className {
   // some data
   // some functions
};
```

```c++
class Room {
    public:
        double length;
        double breadth;
        double height;   

        double calculateArea(){   
            return length * breadth;
        }

        double calculateVolume(){   
            return length * breadth * height;
        }

};
```



### (2) Define Object

```c++
className objectVariableName
```

```c++
// Program to illustrate the working of
// objects and class in C++ Programming

#include <iostream>
using namespace std;

// create a class
class Room {

   public:
    double length;
    double breadth;
    double height;

    double calculateArea() {
        return length * breadth;
    }

    double calculateVolume() {
        return length * breadth * height;
    }
};

int main() {

    // create object of Room class
    Room room1;

    // assign values to data members
    room1.length = 42.5;
    room1.breadth = 30.8;
    room1.height = 19.2;

    // calculate and display the area and volume of the room
    cout << "Area of Room =  " << room1.calculateArea() << endl;
    cout << "Volume of Room =  " << room1.calculateVolume() << endl;

    return 0;
}
```



### (3) Constructors

```c++
class  Wall {
  public:
    // create a constructor
    Wall() {
      // code
    }
};
```

Here, the function `Wall()` is a constructor of the class `Wall`. Notice that the constructor

- has the same name as the class,
- does not have a return type, and
- is `public`



```c++
// C++ program to calculate the area of a wall

#include <iostream>
using namespace std;

// declare a class
class Wall {
  private:
    double length;
    double height;

  public:
    // parameterized constructor to initialize variables
    Wall(double len, double hgt) {
      length = len;
      height = hgt;
    }

    double calculateArea() {
      return length * height;
    }
};

int main() {
  // create object and initialize data members
  Wall wall1(10.5, 8.6);
  Wall wall2(8.5, 6.3);

  cout << "Area of Wall 1: " << wall1.calculateArea() << endl;
  cout << "Area of Wall 2: " << wall2.calculateArea();

  return 0;
}
```



### (4) Pass objects to a function

```c++
// C++ program to calculate the average marks of two students

#include <iostream>
using namespace std;

class Student {

   public:
    double marks;

    // constructor to initialize marks
    Student(double m) {
        marks = m;
    }
};

// function that has objects as parameters
void calculateAverage(Student s1, Student s2) {

    // calculate the average of marks of s1 and s2 
    double average = (s1.marks + s2.marks) / 2;

   cout << "Average Marks = " << average << endl;

}

int main() {
    Student student1(88.0), student2(56.0);

  // pass the objects as arguments
   calculateAverage(student1, student2);

    return 0;
}
```



### (5) Return Object from a Function

```c++
#include <iostream>
using namespace std;

class Student {
   public:
    double marks1, marks2;
};

// function that returns object of Student
Student createStudent() {
    Student student;

    // Initialize member variables of Student
    student.marks1 = 96.5;
    student.marks2 = 75.0;

    // print member variables of Student
    cout << "Marks 1 = " << student.marks1 << endl;
    cout << "Marks 2 = " << student.marks2 << endl;

    return student;
}

int main() {
    Student student1;

    // Call function
    student1 = createStudent();

    return 0;
}
```





### (6) Operator Overloading

```c++
class className {
    ... .. ...
    public
       returnType operator symbol (arguments) {
           ... .. ...
       } 
    ... .. ...
};
```

```c++
// Overload ++ when used as prefix

#include <iostream>
using namespace std;

class Count {
   private:
    int value;

   public:

    // Constructor to initialize count to 5
    Count() : value(5) {}

    // Overload ++ when used as prefix
    void operator ++ () {
        ++value;
    }

    void display() {
        cout << "Count: " << value << endl;
    }
};

int main() {
    Count count1;

    // Call the "void operator ++ ()" function
    ++count1;

    count1.display();
    return 0;
}
```

The above example works only when ++ is used as a prefix. To make ++ work as a postfix we use this syntax.

```c++
void operator ++ (int) {
    // code
}
```

Notice the int inside the parentheses. It's the syntax used for using unary operators as postfix; it's not a function parameter.

```c++
#include <iostream>
using namespace std;

class Count {
   private:
    int value;

   public
       :
    // Constructor to initialize count to 5
    Count() : value(5) {}

    // Overload ++ when used as prefix
    Count operator ++ () {
        Count temp;

        // Here, value is the value attribute of the calling object
        temp.value = ++value;

        return temp;
    }

    // Overload ++ when used as postfix
    Count operator ++ (int) {
        Count temp;

        // Here, value is the value attribute of the calling object
        temp.value = value++;

        return temp;
    }

    void display() {
        cout << "Count: " << value << endl;
    }
};

int main() {
    Count count1, result;

    // Call the "Count operator ++ ()" function
    result = ++count1;
    result.display();

    // Call the "Count operator ++ (int)" function
    result = count1++;
    result.display();

    return 0;
}
```

Binary Operator Overloading:

```c++
// C++ program to overload the binary operator +
// This program adds two complex numbers

#include <iostream>
using namespace std;

class Complex {
   private:
    float real;
    float imag;

   public:
    // Constructor to initialize real and imag to 0
    Complex() : real(0), imag(0) {}

    void input() {
        cout << "Enter real and imaginary parts respectively: ";
        cin >> real;
        cin >> imag;
    }

    // Overload the + operator
    Complex operator + (const Complex& obj) {
        Complex temp;
        temp.real = real + obj.real;
        temp.imag = imag + obj.imag;
        return temp;
    }

    void output() {
        if (imag < 0)
            cout << "Output Complex number: " << real << imag << "i";
        else
            cout << "Output Complex number: " << real << "+" << imag << "i";
    }
};

int main() {
    Complex complex1, complex2, result;

    cout << "Enter first complex number:\n";
    complex1.input();

    cout << "Enter second complex number:\n";
    complex2.input();

   // complex1 calls the operator function
   // complex2 is passed as an argument to the function
    result = complex1 + complex2;
    result.output();

    return 0;
}
```







## Pointers

### (1) declare pointers

Here is how we can declare pointers.

```c++
int *pointVar;
```

We can also declare pointers in the following way.

```c++
int* pointVar; // preferred syntax
```



### (2) Assigning Addresses to Pointers

```c++
int* pointVar, var;
var = 5;

// assign address of var to pointVar pointer
pointVar = &var;
```

Here, 5 is assigned to the variable var. And, the address of var is assigned to the pointVar pointer with the code pointVar = &var





### (3) Get the Value from the Address Using Pointers

```c++
int* pointVar, var;
var = 5;

// assign address of var to pointVar
pointVar = &var;

// access value pointed by pointVar
cout << *pointVar << endl;   // Output: 5
```

In the above code, the address of var is assigned to pointVar. We have used the *pointVar to get the value stored in that address.

When * is used with pointers, it's called the dereference operator. It operates on a pointer and gives the value pointed by the address stored in the pointer. That is, *pointVar = var.

Note: In C++, pointVar and *pointVar is completely different. We cannot do something like *pointVar = &var;





### (4) Common mistakes

```c++
int var, *varPoint;

// Wrong! 
// varPoint is an address but var is not
varPoint = var;

// Wrong!
// &var is an address
// *varPoint is the value stored in &var
*varPoint = &var;

// Correct! 
// varPoint is an address and so is &var
varPoint = &var;

 // Correct!
// both *varPoint and var are values
*varPoint = var;
```





### (5) Pointers and Arrays

```c++
int *ptr;
int arr[5];

// store the address of the first
// element of arr in ptr
ptr = arr;
```

Here, ptr is a pointer variable while arr is an int array. The code ptr = arr; stores the address of the first element of the array in variable ptr.

Notice that we have used arr instead of &arr[0]. This is because both are the same. So, the code below is the same as the code above.

```c++
int *ptr;
int arr[5];
ptr = &arr[0];
```





### (6) Pointers and Functions

```c++
#include <iostream>
using namespace std;

// function definition to swap values
void swap(int &n1, int &n2) {
    int temp;
    temp = n1;
    n1 = n2;
    n2 = temp;
}

int main()
{

    // initialize variables
    int a = 1, b = 2;

    cout << "Before swapping" << endl;
    cout << "a = " << a << endl;
    cout << "b = " << b << endl;

    // call function to swap numbers
    swap(a, b);

    cout << "\nAfter swapping" << endl;
    cout << "a = " << a << endl;
    cout << "b = " << b << endl;

    return 0;
}
```

```c++
#include <iostream>
using namespace std;

// function prototype with pointer as parameters
void swap(int*, int*);

int main()
{

    // initialize variables
    int a = 1, b = 2;

    cout << "Before swapping" << endl;
    cout << "a = " << a << endl;
    cout << "b = " << b << endl;

    // call function by passing variable addresses
    swap(&a, &b);

    cout << "\nAfter swapping" << endl;
    cout << "a = " << a << endl;
    cout << "b = " << b << endl;
    return 0;
}

// function definition to swap numbers
void swap(int* n1, int* n2) {
    int temp;
    temp = *n1;
    *n1 = *n2;
    *n2 = temp;
}
```



### (7) memory management 

new Operator:

```c++
// declare an int pointer
int* pointVar;

// dynamically allocate memory
// using the new keyword 
pointVar = new int;

// assign value to allocated memory
*pointVar = 45;
```



delete Operator:

```c++
// declare an int pointer
int* pointVar;

// dynamically allocate memory
// for an int variable 
pointVar = new int;

// assign value to the variable memory
*pointVar = 45;

// print the value stored in memory
cout << *pointVar; // Output: 45

// deallocate the memory
delete pointVar;
```









## References

### (1) introduction of reference 

When a variable is declared as a reference, it becomes an alternative name for an existing variable. A variable can be declared as a reference by putting ‘&’ in the declaration. 

```c++
#include <iostream>
using namespace std;
 
int main()
{
  int x = 10;
 
  // ref is a reference to x.
  int& ref = x;
 
  // Value of x is now changed to 20
  ref = 20;
  cout << "x = " << x << '\n';
 
  // Value of x is now changed to 30
  x = 30;
  cout << "ref = " << ref << '\n';
 
  return 0;
}
```

```
x = 20
ref = 30
```





### (2) Applications

(a) Modify the passed parameters in a function

If a function receives a reference to a variable, it can modify the value of the variable. For example, the following program variables are swapped using references. 

```c++
#include <iostream>
using namespace std;
 
void swap (int& first, int& second)
{
    int temp = first;
    first = second;
    second = temp;
}
 
int main()
{
    int a = 2, b = 3;
    swap( a, b );
    cout << a << " " << b;
    return 0;
}
```



(b) Avoiding a copy of large structures

Imagine a function that has to receive a large object. If we pass it without reference, a new copy of it is created which causes wastage of CPU time and memory. We can use references to avoid this. 

```c++
struct Student {
   string name;
   string address;
   int rollNo;
}
 
// If we remove & in below function, a new
// copy of the student object is created.
// We use const to avoid accidental updates
// in the function as the purpose of the function
// is to print s only.
void print(const Student &s)
{
    cout << s.name << "  " << s.address << "  " << s.rollNo << '\n';
}
```



(c) For Each Loop to avoid the copy of objects

 We can use references in each loop to avoid a copy of individual objects when objects are large.  

```c++

#include <bits/stdc++.h>
using namespace std;
 
int main()
{
    vector<string> vect{"geeksforgeeks practice",
                     "geeksforgeeks write",
                     "geeksforgeeks ide"};
 
    // We avoid copy of the whole string
    // object by using reference.
    for (const auto& x : vect)
    {
        cout << x << '\n';
    }
 
    return 0;
}
```





### (3) References vs Pointers

(a) A pointer can be declared as void but a reference can never be void

```c++
int a = 10;
void* aa = &a; // it is valid
void& ar = a;  // it is not valid
```



(b) The pointer variable has n-levels/multiple levels of indirection i.e. single-pointer, double-pointer, triple-pointer. Whereas, the reference variable has only one/single level of indirection



(c) Reference variable cannot be updated.



(d) Reference variable is an internal pointer .



(e) Declaration of Reference variable is preceded with ‘&’ symbol ( but do not read it as “address of”).



```c++
#include <iostream>
using namespace std;
 
int main() {
    int i = 10; // simple or ordinary variable.
    int *p = &i; // single pointer
    int **pt = &p; // double pointer
    int ***ptr = &pt; // triple pointer
    // All the above pointers differ in the value they store or point to.
    cout << "i = " << i << "\t" << "p = " << p << "\t"
         << "pt = " << pt << "\t" << "ptr = " << ptr << '\n';
    int a = 5; // simple or ordinary variable
    int &S = a;
    int &S0 = S;
    int &S1 = S0;
    cout << "a = " << a << "\t" << "S = " << S << "\t"
         << "S0 = " << S0 << "\t" << "S1 = " << S1 << '\n';
    // All the above references do not differ in their values
    // as they all refer to the same variable.
 }
```







## Inheritance

### (1) Access Modes in Inheritance

The various ways we can derive classes are known as **access modes**. These access modes have the following effect:

1. **public:** If a derived class is declared in `public` mode, then the members of the base class are inherited by the derived class just as they are.
2. **private:** In this case, all the members of the base class become `private` members in the derived class.
3. **protected:** The `public` members of the base class become `protected` members in the derived class.

```c++
class Base {
    public:
        int x;
    protected:
        int y;
    private:
        int z;
};

class PublicDerived: public Base {
    // x is public
    // y is protected
    // z is not accessible from PublicDerived
};

class ProtectedDerived: protected Base {
    // x is protected
    // y is protected
    // z is not accessible from ProtectedDerived
};

class PrivateDerived: private Base {
    // x is private
    // y is private
    // z is not accessible from PrivateDerived
}
```

Accessibility in public Inheritance

| Accessibility | private members | protected members | public members |
| :------------ | :-------------- | :---------------- | :------------- |
| Base Class    | Yes             | Yes               | Yes            |
| Derived Class | No              | Yes               | Yes            |

Accessibility in protected Inheritance

| Accessibility | private members | protected members | public members                         |
| :------------ | :-------------- | :---------------- | :------------------------------------- |
| Base Class    | Yes             | Yes               | Yes                                    |
| Derived Class | No              | Yes               | Yes (inherited as protected variables) |

Accessibility in private Inheritance

| Accessibility | private members | protected members                    | public members                       |
| :------------ | :-------------- | :----------------------------------- | :----------------------------------- |
| Base Class    | Yes             | Yes                                  | Yes                                  |
| Derived Class | No              | Yes (inherited as private variables) | Yes (inherited as private variables) |







### (2) Function Overriding

```c++
// C++ program to demonstrate function overriding

#include <iostream>
using namespace std;

class Base {
   public:
    void print() {
        cout << "Base Function" << endl;
    }
};

class Derived : public Base {
   public:
    void print() {
        cout << "Derived Function" << endl;
    }
};

int main() {
    Derived derived1;
    derived1.print();
    return 0;
}
```

```
Derived Function
```

To access the overridden function of the base class, we use the scope resolution operator ::

```c++
// C++ program to access overridden function
// in main() using the scope resolution operator ::

#include <iostream>
using namespace std;

class Base {
   public:
    void print() {
        cout << "Base Function" << endl;
    }
};

class Derived : public Base {
   public:
    void print() {
        cout << "Derived Function" << endl;
    }
};

int main() {
    Derived derived1, derived2;
    derived1.print();

    // access print() function of the Base class
    derived2.Base::print();

    return 0;
}
```

```c++
// C++ program to call the overridden function
// from a member function of the derived class

#include <iostream>
using namespace std;

class Base {
   public:
    void print() {
        cout << "Base Function" << endl;
    }
};

class Derived : public Base {
   public:
    void print() {
        cout << "Derived Function" << endl;

        // call overridden function
        Base::print();
    }
};

int main() {
    Derived derived1;
    derived1.print();
    return 0;
}
```





### (3) Friend Function

A friend function can access the private and protected data of a class. We declare a friend function using the friend keyword inside the body of the class.

```c++
class className {
    ... .. ...
    friend returnType functionName(arguments);
    ... .. ...
}
```

```c++
// C++ program to demonstrate the working of friend function

#include <iostream>
using namespace std;

class Distance {
    private:
        int meter;
        
        // friend function
        friend int addFive(Distance);

    public:
        Distance() : meter(0) {}
        
};

// friend function definition
int addFive(Distance d) {

    //accessing private members from the friend function
    d.meter += 5;
    return d.meter;
}

int main() {
    Distance D;
    cout << "Distance: " << addFive(D);
    return 0;
}
```

Here, addFive() is a friend function that can access both private and public data members.

Though this example gives us an idea about the concept of a friend function, it doesn't show any meaningful use.

A more meaningful use would be operating on objects of two different classes. That's when the friend function can be very helpful.

```c++
// Add members of two different classes using friend functions

#include <iostream>
using namespace std;

// forward declaration
class ClassB;

class ClassA {
    
    public:
        // constructor to initialize numA to 12
        ClassA() : numA(12) {}
        
    private:
        int numA;
        
         // friend function declaration
         friend int add(ClassA, ClassB);
};

class ClassB {

    public:
        // constructor to initialize numB to 1
        ClassB() : numB(1) {}
    
    private:
        int numB;
 
        // friend function declaration
        friend int add(ClassA, ClassB);
};

// access members of both classes
int add(ClassA objectA, ClassB objectB) {
    return (objectA.numA + objectB.numB);
}

int main() {
    ClassA objectA;
    ClassB objectB;
    cout << "Sum: " << add(objectA, objectB);
    return 0;
}
```





### (4) friend Classes

We can also use a friend Class in C++ using the friend keyword. 

```c++
class ClassB;

class ClassA {
   // ClassB is a friend class of ClassA
   friend class ClassB;
   ... .. ...
}

class ClassB {
   ... .. ...
}
```

When a class is declared a friend class, all the member functions of the friend class become friend functions.

Since ClassB is a friend class, we can access all members of ClassA from inside ClassB.

However, we cannot access members of ClassB from inside ClassA. It is because friend relation in C++ is only granted, not taken.

```c++
// C++ program to demonstrate the working of friend class

#include <iostream>
using namespace std;

// forward declaration
class ClassB;

class ClassA {
    private:
        int numA;

        // friend class declaration
        friend class ClassB;

    public:
        // constructor to initialize numA to 12
        ClassA() : numA(12) {}
};

class ClassB {
    private:
        int numB;

    public:
        // constructor to initialize numB to 1
        ClassB() : numB(1) {}
    
    // member function to add numA
    // from ClassA and numB from ClassB
    int add() {
        ClassA objectA;
        return objectA.numA + numB;
    }
};

int main() {
    ClassB objectB;
    cout << "Sum: " << objectB.add();
    return 0;
}
```





### (5) Virtual Function

A virtual function is a member function which is declared within a base class and is re-defined (overridden) by a derived class. When you refer to a derived class object using a pointer or a reference to the base class, you can call a virtual function for that object and execute the derived class’s version of the function. 

- Virtual functions ensure that the correct function is called for an object, regardless of the type of reference (or pointer) used for function call.
- They are mainly used to achieve Runtime polymorphism
- Functions are declared with a virtual keyword in base class.
- The resolving of function call is done at runtime.

```c++
// CPP program to illustrate
// working of Virtual Functions
#include<iostream>
using namespace std;
 
class base {
public:
    void fun_1() { cout << "base-1\n"; }
    virtual void fun_2() { cout << "base-2\n"; }
    virtual void fun_3() { cout << "base-3\n"; }
    virtual void fun_4() { cout << "base-4\n"; }
};
 
class derived : public base {
public:
    void fun_1() { cout << "derived-1\n"; }
    void fun_2() { cout << "derived-2\n"; }
    void fun_4(int x) { cout << "derived-4\n"; }
};
 
int main()
{
    base *p;
    derived obj1;
    p = &obj1;
 
    // Early binding because fun1() is non-virtual
    // in base
    p->fun_1();
 
    // Late binding (RTP)
    p->fun_2();
 
    // Late binding (RTP)
    p->fun_3();
 
    // Late binding (RTP)
    p->fun_4();
 
    // Early binding but this function call is
    // illegal (produces error) because pointer
    // is of base type and function is of
    // derived class
    // p->fun_4(5);
   
    return 0;
}
```

```
base-1
derived-2
base-3
base-4
```





## Templates

### (1) Class Template Declaration

```c++
template <class T>
class className {
  private:
    T var;
    ... .. ...
  public:
    T functionName(T arg);
    ... .. ...
};
```

In the above declaration, T is the template argument which is a placeholder for the data type used, and class is a keyword.

Inside the class body, a member variable var and a member function functionName() are both of type T.



### (2) Creating a Class Template Object

```c++
className<dataType> classObject;
```

```c++
className<int> classObject;
className<float> classObject;
className<string> classObject;
```



### (3) Defining a Class Member Outside the Class Template

```c++
template <class T>
class ClassName {
    ... .. ...
    // Function prototype
    returnType functionName();
};

// Function definition
template <class T>
returnType ClassName<T>::functionName() {
    // code
}
```









# Standard Template Library

## Containers

### (1) Pair 

Although Pair and Tuple are not actually the part of container library but we'll still discuss them as they are very commonly required in programming competitions and they make certain things very easy to implement.

```c++
pair<T1,T2>  pair1, pair2 ;
```

- Operator `=` : assign values to a pair.
- swap : swaps the contents of the pair.
- make_pair() : create and returns a pair having objects defined by parameter list.
- Operators( == , != , > , < , <= , >= ) : lexicographically compares two pairs.

```c++
#include <iostream>

using namespace std;    
    
int main ()
{
   pair<int,int> pair1, pair3;    //creats pair of integers
   pair<int,string> pair2;    // creates pair of an integer an a string
    
   pair1 = make_pair(1, 2);     // insert 1 and 2 to the pair1
   pair2 = make_pair(1, "Studytonight") // insert 1 and "Studytonight" in pair2
   pair3 = make_pair(2, 4)
   cout<< pair1.first << endl;  // prints 1, 1 being 1st element of pair1
   cout<< pair2.second << endl; // prints Studytonight

   if(pair1 == pair3)
        cout<< "Pairs are equal" << endl;
   else
        cout<< "Pairs are not equal" << endl;
   
   return 0;
}

```





### (2) Tuple

tuple and pair are very similar in their structure. Just like in pair we can pair two heterogeneous object, in tuple we can pair three heterogeneous objects.

```c++
// creates tuple of three object of type T1, T2 and T3
tuple<T1, T2, T3> tuple1;  
```

- A Constructor to construct a new tuple
- Operator `= `: to assign value to a tuple
- swap : to swap value of two tuples
- make_tuple() : creates and return a tuple having elements described by the parameter list.
- Operators( == , != , > , < , <= , >= ) : lexicographically compares two pairs.
- Tuple_element : returns the type of tuple element
- Tie : Tie values of a tuple to its refrences.

```c++
#include <iostream>
        
int main ()
{
   tuple<int, int, int> tuple1;   //creates tuple of integers
   tuple<int, string, string> tuple2;    // creates pair of an integer an 2 string
    
   tuple1 = make_tuple(1,2,3);  // insert 1, 2 and 3 to the tuple1
   tuple2 = make_pair(1,"Studytonight", "Loves You");
   /* insert 1, "Studytonight" and "Loves You" in tuple2  */

   int id;
   string first_name, last_name;

   tie(id,first_name,last_name) = tuple2;
   /* ties id, first_name, last_name to 
   first, second and third element of tuple2 */

   cout << id <<" "<< first_name <<" "<< last_name;
   /* prints 1 Studytonight Loves You  */

   return 0;
}

```



### (3) Array

The introduction of array class from C++11 has offered a better alternative for C-style arrays. The advantages of array class over C-style array are :

- Array classes knows its own size, whereas C-style arrays lack this property. So when passing to functions, we don’t need to pass size of Array as a separate parameter.
- With C-style array there is more risk of array being decayed into a pointer. Array classes don’t decay into pointers
- Array classes are generally more efficient, light-weight and reliable than C-style arrays.



```c++
#include <iostream>
#include <array>

using namespace std;

int main ()
{
    array<int,10> array1 = {1,2,3,4,5,6,7,8,9};
   
    cout << array1.at(2)     // prints 3
    cout << array1.at(4)     // prints 5
  
}
```



```c++

#include <array>
int main()
{
    array<int,8> myarray;
    myarray.fill(1);
}

```









### (4) vector

Vectors are same as dynamic arrays with the ability to resize itself automatically when an element is inserted or deleted, with their storage being handled automatically by the container. Vector elements are placed in contiguous storage so that they can be accessed and traversed using iterators. In vectors, data is inserted at the end. Inserting at the end takes differential time, as sometimes there may be a need of extending the array. Removing the last element takes only constant time because no resizing happens. Inserting and erasing at the beginning or in the middle is linear in time.



push_back function

```c++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int>  v;
    v.push_back(1);  //insert 1 at the back of v
    v.push_back(2);  //insert 2 at the back of v
    v.push_back(4);  //insert 3 at the back of v
    
    for(vector<int>::iterator i = v.begin(); i != v.end(); i++) 
    {
          cout << *i <<" ";   // for printing the vector
    }
}

```



pop_back function

```c++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    vector<int> v1 {10,20,30,40};
    
    v1.pop_back();  
    
    vector<int>::iterator it;
    
    for(it = v.begin(); it != v.end(); it++) 
    {
        cout << *it <<" ";   // for printing the vector
    }
}

```







### (5) list

Lists are sequence containers that allow non-contiguous memory allocation. As compared to vector, the list has slow traversal, but once a position has been found, insertion and deletion are quick. 



insert function

```c++
#include <iostream>
#include <list>

using namespace std;

int main()
{
    list<int> l = {1,2,3,4,5};
    list<int>::iterator it = l.begin();           
    
    l.insert (it+1, 100);    // insert 100 before 2 position
    /* now the list is 1 100 2 3 4 5 */
    
    list<int> new_l = {10,20,30,40};   // new list
    
    new_l.insert (new_l.begin(), l.begin(), l.end());
    /* 
        insert elements from beginning of list l to end of list l 
        before 1 position in list new_l */
    
    /* now the list new_l is 1 100 2 3 4 5 10 20 30 40 */
    
    l.insert(l.begin(), 5, 10);  // insert 10 before beginning 5 times
    /* now l is 10 10 10 10 10 1 100 2 3 4 5 */
    
    return 0;
}
```



push_back and push_front functions

```c++
#include <iostream>
#include <list>

using namespace std;

int main()
{
    list<int> l{1,2,3,4,5};
    
    l.push_back(6);
    l.push_back(7);
    /* now the list becomes 1,2,3,4,5,6,7 */
    
    l.push_front(8);
    l.push_front(9);
    /* now the list becomes 9,8,1,2,3,4,5,6,7 */

}
```



pop_back and pop_front functions

```c++
#include <iostream>
#include <list>

using namespace std;

int main()
{
    list<int> l{1,2,3,4,5};
    
    l.pop_back()();
    /* now the list becomes 1,2,3,4 */
    
    l.pop_front()();
    /* now the list becomes 2,3,4 */
}

```



reverse function

```c++
#include <iostream>
#include <list>

using namespace std;

int main()
{
    list<int> l{1,2,3,4,5};
    
    l.reverse();
    /* now the list becomes 5,4,3,2,1 */
}

```



sort function

```c++
#include <iostream>
#include <list>

using namespace std;

bool compare_function( string& s1 , string& s2 )
{
    return ( s1.length() > s2.length() );
}

int main()
{
    list<int> list1 = {2,4,5,6,1,3};
    list<string> list2 = {"h", "hhh", "hh"};
    
    list1.sort();
    /* list1 is now 1 2 3 4 5 6 */
    
    list2.sort(compare_function);
    /* list2 is now h hh hhh */ 
}
```





### (6) stack

Stacks are a type of container adaptors with LIFO(Last In First Out) type of working, where a new element is added at one end (top) and an element is removed from that end only.  Stack uses an encapsulated object of either vector or deque (by default) or list (sequential container class) as its underlying container, providing a specific set of member functions to access its elements.



```c++
#include <iostream>
#include <stack>
using namespace std;
int main() {
    stack<int> stack;
    stack.push(21);
    stack.push(22);
    stack.push(24);
    stack.push(25);
     
    stack.pop();
    stack.pop();
 
    while (!stack.empty()) {
        cout << ' ' << stack.top();
        stack.pop();
    }
}
```





### (7) Queue

The queue container is used to replicate queue in C++, insertion always takes place at the back of the queue and deletion is always performed at the front of the queue.

```c++

// CPP code to illustrate Queue in
// Standard Template Library (STL)
#include <iostream>
#include <queue>
 
using namespace std;
 
// Print the queue
void showq(queue<int> gq)
{
    queue<int> g = gq;
    while (!g.empty()) {
        cout << '\t' << g.front();
        g.pop();
    }
    cout << '\n';
}
 
// Driver Code
int main()
{
    queue<int> gquiz;
    gquiz.push(10);
    gquiz.push(20);
    gquiz.push(30);
 
    cout << "The queue gquiz is : ";
    showq(gquiz);
 
    cout << "\ngquiz.size() : " << gquiz.size();
    cout << "\ngquiz.front() : " << gquiz.front();
    cout << "\ngquiz.back() : " << gquiz.back();
 
    cout << "\ngquiz.pop() : ";
    gquiz.pop();
    showq(gquiz);
 
    return 0;
}
```



### (8) Deque

Double Ended Queues are basically an implementation of the data structure double-ended queue. A queue data structure allows insertion only at the end and deletion from the front. 

```c++
// CPP Program to implement Deque in STL
#include <deque>
#include <iostream>

using namespace std;

void showdq(deque<int> g)
{
	deque<int>::iterator it;
	for (it = g.begin(); it != g.end(); ++it)
		cout << '\t' << *it;
	cout << '\n';
}

int main()
{
	deque<int> gquiz;
	gquiz.push_back(10);
	gquiz.push_front(20);
	gquiz.push_back(30);
	gquiz.push_front(15);
	cout << "The deque gquiz is : ";
	showdq(gquiz);

	cout << "\ngquiz.size() : " << gquiz.size();
	cout << "\ngquiz.max_size() : " << gquiz.max_size();

	cout << "\ngquiz.at(2) : " << gquiz.at(2);
	cout << "\ngquiz.front() : " << gquiz.front();
	cout << "\ngquiz.back() : " << gquiz.back();

	cout << "\ngquiz.pop_front() : ";
	gquiz.pop_front();
	showdq(gquiz);

	cout << "\ngquiz.pop_back() : ";
	gquiz.pop_back();
	showdq(gquiz);

	return 0;
}

```





### (9) Priority Queue

Priority queues are a type of container adapters, specifically designed such that the first element of the queue is the greatest of all elements in the queue and elements are in nonincreasing order.

```c++
// CPP Program to demonstrate Priority Queue
#include <iostream>
#include <queue>
using namespace std;

void showpq(priority_queue<int> gq)
{
	priority_queue<int> g = gq;
	while (!g.empty()) {
		cout << '\t' << g.top();
		g.pop();
	}
	cout << '\n';
}

// Driver Code
int main()
{
	priority_queue<int> gquiz;
	gquiz.push(10);
	gquiz.push(30);
	gquiz.push(20);
	gquiz.push(5);
	gquiz.push(1);

	cout << "The priority queue gquiz is : ";
	showpq(gquiz);

	cout << "\ngquiz.size() : " << gquiz.size();
	cout << "\ngquiz.top() : " << gquiz.top();

	cout << "\ngquiz.pop() : ";
	gquiz.pop();
	showpq(gquiz);

	return 0;
}

```



### (10) Set

Sets are a type of associative containers in which each element has to be unique because the value of the element identifies it. The values are stored in a specific order. 

```c++
// CPP program to demonstrate various functions of
// Set in C++ STL
#include <iostream>
#include <iterator>
#include <set>

using namespace std;

int main()
{
	// empty set container
	set<int, greater<int> > s1;

	// insert elements in random order
	s1.insert(40);
	s1.insert(30);
	s1.insert(60);
	s1.insert(20);
	s1.insert(50);

	// only one 50 will be added to the set
	s1.insert(50);
	s1.insert(10);

	// printing set s1
	set<int, greater<int> >::iterator itr;
	cout << "\nThe set s1 is : \n";
	for (itr = s1.begin(); itr != s1.end(); itr++) {
		cout << *itr << " ";
	}
	cout << endl;

	// assigning the elements from s1 to s2
	set<int> s2(s1.begin(), s1.end());

	// print all elements of the set s2
	cout << "\nThe set s2 after assign from s1 is : \n";
	for (itr = s2.begin(); itr != s2.end(); itr++) {
		cout << *itr << " ";
	}
	cout << endl;

	// remove all elements up to 30 in s2
	cout << "\ns2 after removal of elements less than 30 "
			":\n";
	s2.erase(s2.begin(), s2.find(30));
	for (itr = s2.begin(); itr != s2.end(); itr++) {
		cout << *itr << " ";
	}

	// remove element with value 50 in s2
	int num;
	num = s2.erase(50);
	cout << "\ns2.erase(50) : ";
	cout << num << " removed\n";
	for (itr = s2.begin(); itr != s2.end(); itr++) {
		cout << *itr << " ";
	}

	cout << endl;

	// lower bound and upper bound for set s1
	cout << "s1.lower_bound(40) : \n"
		<< *s1.lower_bound(40) << endl;
	cout << "s1.upper_bound(40) : \n"
		<< *s1.upper_bound(40) << endl;

	// lower bound and upper bound for set s2
	cout << "s2.lower_bound(40) :\n"
		<< *s2.lower_bound(40) << endl;
	cout << "s2.upper_bound(40) : \n"
		<< *s2.upper_bound(40) << endl;

	return 0;
}

```





### (11) Map

Maps are associative containers that store elements in a mapped fashion. Each element has a key value and a mapped value. No two mapped values can have the same key values.

```c++
// CPP Program to demonstrate the implementation in Map
#include <iostream>
#include <iterator>
#include <map>
using namespace std;

int main()
{

	// empty map container
	map<int, int> gquiz1;

	// insert elements in random order
	gquiz1.insert(pair<int, int>(1, 40));
	gquiz1.insert(pair<int, int>(2, 30));
	gquiz1.insert(pair<int, int>(3, 60));
	gquiz1.insert(pair<int, int>(4, 20));
	gquiz1.insert(pair<int, int>(5, 50));
	gquiz1.insert(pair<int, int>(6, 50));
	gquiz1.insert(pair<int, int>(7, 10));

	// printing map gquiz1
	map<int, int>::iterator itr;
	cout << "\nThe map gquiz1 is : \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz1.begin(); itr != gquiz1.end(); ++itr) {
		cout << '\t' << itr->first << '\t' << itr->second
			<< '\n';
	}
	cout << endl;

	// assigning the elements from gquiz1 to gquiz2
	map<int, int> gquiz2(gquiz1.begin(), gquiz1.end());

	// print all elements of the map gquiz2
	cout << "\nThe map gquiz2 after"
		<< " assign from gquiz1 is : \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first << '\t' << itr->second
			<< '\n';
	}
	cout << endl;

	// remove all elements up to
	// element with key=3 in gquiz2
	cout << "\ngquiz2 after removal of"
			" elements less than key=3 : \n";
	cout << "\tKEY\tELEMENT\n";
	gquiz2.erase(gquiz2.begin(), gquiz2.find(3));
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first << '\t' << itr->second
			<< '\n';
	}

	// remove all elements with key = 4
	int num;
	num = gquiz2.erase(4);
	cout << "\ngquiz2.erase(4) : ";
	cout << num << " removed \n";
	cout << "\tKEY\tELEMENT\n";
	for (itr = gquiz2.begin(); itr != gquiz2.end(); ++itr) {
		cout << '\t' << itr->first << '\t' << itr->second
			<< '\n';
	}

	cout << endl;

	// lower bound and upper bound for map gquiz1 key = 5
	cout << "gquiz1.lower_bound(5) : "
		<< "\tKEY = ";
	cout << gquiz1.lower_bound(5)->first << '\t';
	cout << "\tELEMENT = " << gquiz1.lower_bound(5)->second
		<< endl;
	cout << "gquiz1.upper_bound(5) : "
		<< "\tKEY = ";
	cout << gquiz1.upper_bound(5)->first << '\t';
	cout << "\tELEMENT = " << gquiz1.upper_bound(5)->second
		<< endl;

	return 0;
}

```







## Iterators

As we have discussed earlier, Iterators are used to point to the containers in STL, because of iterators it is possible for an algorithm to manipulate different types of data structures/Containers.

```c++
container_type <parameter_list>::iterator iterator_name;
```

Iterators can be used to traverse the container, and we can de-reference the iterator to get the value of the element it is pointing to. Here is an example:

```c++

#include<iostream>
#include<vector>
int main()
{
    vector<int> v(10);
    /* creates an vector v : 0,0,0,0,0,0,0,0,0,0  */
    
    vector<int>::iterator i;
    
    for(i = v.begin(); i! = v.end(); i++)
    cout << *i <<"  ";
    /* in the above for loop iterator I iterates though the 
    vector v and *operator is used of printing the element 
    pointed by it.  */

return 0;
}

```



advance() Operation

```c++

#include<iostream>
#include<vector>

int main()
{
    vector<int>  v(10) ;    // create a vector of 10 0's
    vector<int>::iterator i;  // defines an iterator i to the vector of integers
    
    i = v.begin();
    /* i now points to the beginning of the vector v */
    
    advance(i,5);
    /* i now points to the fifth element form the 
    beginning of the vector v */
    
    advance(i,-1);
    /* i  now points to the fourth element from the 
    beginning of the vector */ 
}

```







## Algorithms

### (1) Sorting Algorithms

sort method

```c++

#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

bool compare_function(int i, int j)
{
   return i > j;    // return 1 if i>j else 0
}
bool compare_string(string i, string j)
{ 
  return (i.size() < j.size()); 
}

int main()
{
    int arr[5] = {1,5,8,4,2};
    
    sort(arr , arr+5);    // sorts arr[0] to arr[4] in ascending order
    /* now the arr is 1,2,4,5,8  */
    
    vector<int> v1;
    
    v1.push_back(8);
    v1.push_back(4);
    v1.push_back(5);
    v1.push_back(1);
    
    /* now the vector v1 is 8,4,5,1 */
    vector<int>::iterator i, j;
    
    i = v1.begin();   // i now points to beginning of the vector v1
    j = v1.end();     // j now points to end of the vector v1
    
    sort(i,j);      //sort(v1.begin() , v1.end() ) can also be used
    /* now the vector v1 is 1,4,5,8 */
    
    
    /* use of compare_function */
    int a2[] = { 4,3,6,5,6,8,4,3,6 };
    
    sort(a2,a2+9,compare_function);  // sorts a2 in descending order 
    /* here we have used compare_function which uses operator(>), 
    that result into sorting in descending order */
    
    /* compare_function is also used to sort non-numeric elements such as*/
    
    string s[]={"a" , "abc", "ab" , "abcde"};
    
    sort(s,s+4,compare_string);
    /* now s is "a","ab","abc","abcde"  */
}

```



is_sorted method

```c++

#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main()
{
  int a[5] = {1,5,8,4,2}; 
  cout<<is_sorted(a, a+5);
  /* prints 0 , Boolean false  */
  
  vector<int> v1;
  
  v1.push_back(8);
  v1.push_back(4);
  v1.push_back(5);
  v1.push_back(1);

  /* now the vector v1 is 8,4,5,1 */
  cout<<is_sorted(v1.begin() , v1.end() );
  /* prints 0 */
  sort(v1.begin() , v1.end() );
  /* sorts the vector v1 */
  cout<<is_sorted(v1.begin() , v1.end());
  /*  prints 1 , as vector v1 is sorted */    
}

```





### (2) Binary Search

```
binary_search(startaddress, 
              endaddress, valuetofind)
Parameters :
startaddress: the address of the first 
              element of the array.
endaddress: the address of the next contiguous 
            location of the last element of the array.
valuetofind: the target value which we have 
             to search for.
Returns :
true if an element equal to valuetofind is found, else false.
```

```c++
int a[] = { 1, 5, 8, 9, 6, 7, 3, 4, 2, 0 };
binary_search(a, a + 10, 2)
```

```cassandra
int inputs[] = {7,8,4,1,6,5,9,4};
vector<int> v(inputs, inputs+8);
binary_search(v.begin() , v.end() , 7 );  
```





### (3) Upper Bound and Lower Bound

upper_bound() returns an iterator to the elements in the given range which does not compare greater than the given value. The range given should be already sorted for upper_bound() to work properly. In other words it returns an iterator to the upper bound of the given element in the given sorted range.

```c++
#include <iostream>    
#include <algorithm> 
#include <vector>
using namespace std;

int main ()
{
    int input[] = {1,2,2,3,4,4,5,6,7,8,10,45};
    vector<int> v(input, input+12);
    
    vector<int>::iterator it1 , it2;
    
    it1 = upper_bound(v.begin(), v.end(), 6); 
    /* points to eight element in v */ 
    
    it2 = upper_bound(v.begin(), v.end(), 4);
    /* points to seventh element in v */
}

```

lower_bound() returns an iterator to the elements in the given range which does no compare less than the given value. The range given should be already sorted for lower_bound() to work properly. In other words it returns an iterator to the lower bound of the given element in the given sorted range.

```c++
#include <iostream>     
#include <algorithm>   
#include <vector>
using namespace std;

int main ()
{
    int input[] = {1,2,2,3,4,4,5,6,7,8,10,45};
    vector<int> v(input,input+12);
    
    vector<int>::iterator it1 , it2;
    
    it1 = lower_bound(v.begin(), v.end(), 4); 
    /* points to fifth element in v */ 
    
    it2 = lower_bound (v.begin(), v.end(), 10);
    /* points to second last element in v */
}

```



### (4) count

count() returns the number of elements in the given range that are equal to given value.

```c++
#include <iostream>   
#include <algorithm>
#include <vector>
using namespace std;

int main ()
{
    int values[] = {5,1,6,9,10,1,12,5,5,5,1,8,9,7,46};
    
    int count_5 = count(values, values+15, 5);
    /* now count_5 is equal to 4 */
    
    vector<int> v(values, values+15);
    
    int count_1 = count(v.begin(), v.end(), 1);
    /* now count_1 is equal to  */
    
    return 0;
}

```



### (5) reverse

reverse(iterator first, iterator last) is used to reverse the order of the elements in the range [first, last].

```c++
#include <iostream>     
#include <algorithm>
#include <vector>

using namespace std;

int main () 
{
    int a[] = {1,5,4,9,8,6,1,3,5,4};
    
    reverse(a, a+10);
    
    /* reverse all the elements of the array a*/ 
    /* now a is : 4,5,3,1,6,8,9,4,5,1 */
    
    reverse(a, a+5);
    
    /* reverse first 5 elements of the array a */
    /* now a is : 6,1,3,5,4,8,9,4 */ 
    
    vector<int> v(a, a+10);
    
    reverse(v.begin(), v.end());
    
    /* reverse the elements of the vector v */
    /* vector is now 4,9,8,4,5,3,1,6 */
}

```



### (6) Max and Min

```c++
max(object_type a, object_type b, compare_function)
```

```c++
min(object_type a, object_type b, compare_function)
```

```c++
#include<iostream>
#include<algorithm>

using namespace std;

/*compare function for strings*/
bool myMaxCompare(string a, string b)
{
    return (a.size() > b.size());
}

bool myMinCompare(string a, string b)
{
    return (a.size() > b.size());
}

int main()
{
    int x=4, y=5;
    
    cout << max(x,y);   // prints 5
    cout << min(x,y);   // prints 4
    
    cout << max(2.312, 5.434);     // prints 5.434
    cout << min(2.312, 5.434);     // prints 2.312
    
    string s = "smaller srting";
    string t = "longer string---";
    
    string s1 = max(s, t, myMaxCompare);
    cout<< s1 <<endl;  // prints longer string---
    
    string s1 = min(s, t, myMinCompare);
    cout<< s1 <<endl;  // prints smaller string
}

```

