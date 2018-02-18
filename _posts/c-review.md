title: c++review
date: 2016-09-18 08:54:38
tags: C++

---

# C++ review
Modern object-oriented (OO) languages provide 3 capabilities:

- encapsulation (combining the data and its functions into one single name)
- inheritance
- polymorphism

<!--more-->
## 多态和继承
``多态``：
- upcast+dynamic/static binding，
- 靠指向vtable的vptr实现

``virtual``
- 纯虚函数=>抽象类（不存在对象）
- virtual的析构函数，delete时执行子类的destructor，子类的destructor调用父类的

``override/overload/name hiding``
- override an overloaded function, you must override all of the variants. otherwise some will be hidden.
- If a function is declared in base class as virtual and is defined in the derived class with same name and signature, then it is called as function overriding.
- ``override``: different scope(base/derived)+virtual+same parameters
- ``overload``: same scope+different parameters
- ``name hiding``: different parameters/same parameters+not virtual

``upcast/downcast``
- upcast: C++ allows that a derived class pointer (or reference) to be treated as base class pointer
- downcast:  converting base class pointer (or reference) to derived class pointer
- If you want to use dynamic cast for downcasting, base class should be polymorphic - it must have at least one virtual function

``inheritance``
Single, Multilevel, Multiple, Hierarchical and Hybrid inheritance
- 子类的初始化列表不能初始化父类成员

不在initialising list传参给父类的话，调用父类的默认构造
```c++
class animal  
　{  
　public:  
　　　animal(int height, int weight)  
　　　{  
　　　　　cout<<"animal construct"<<endl;  
　　　}
　　　…  
　};  
　class fish:public animal  
　{  
　public:  
　　　fish():animal(400,300)  
　　　{  
　　　　　cout<<"fish construct"<<endl;  
　　　}
　　　…  
　};
```

## ctor
copy ctor:不调用默认构造函数，而是A(const A &){}
- member-wise copy(shallow copy)

[shalow vs deep copy](http://www.learncpp.com/cpp-tutorial/915-shallow-vs-deep-copying/)

尽量用初始化列表，在构造之前完成初始化；否则如果在构造函数里初始化，需要调用默认的构造函数，然后再赋值，要求要有默认构造函数并且浪费。

## template
- 类模板里面每一个函数都要是函数模板

## const
const member function: this是const的，不能修改成员变量，不能调用非const的函数

## inline
- to eliminate overhead of function call
- inline keyword must be repeated  both in declaration and definition. put the body in .h.
- 成员函数自动是inline的

## static

|||
|---|---|
|static local var|persistent storage|
|static member var|shared by all instances|
|static member func|shared by all instances, can only access static member var|

## reference
- cannot be null
- no ref to ref, no array of ref, no pointer to ref
- initialisation is required
- binding don’t change

## 运算符重载
- unary should be members
- = () [] -> * must be members
- other binary: free
- 类型转换: members

const Integer operator+(const Integer& that)
- implicit first argument
- no type conversion on receiver

const Integer operator+(const Integer &left, const Integer &right)
- explicit first argument
- type conversion on both arguments

赋值
```c++
T& T::operator=(const T& rhs) {
  if (this!=&rhs) {

  }
  return *this;
}
```


类型转换：
X::operator T()
没有返回类型，没有explicit argument，operator name is any type descriptor
编译器当做X->T的类型转换

## STL
### map
```c++
#include <iostream>
#include <string>
#include <map>

using namespace std;

int main(){

    //declaration container and iterator
    map<string, string> mapStudent;
    map<string, string>::iterator iter;
    map<string, string>::reverse_iterator iter_r;

    //insert element
    mapStudent.insert(pair<string, string>("r000", "student_zero"));

    mapStudent["r123"] = "student_first";
    mapStudent["r456"] = "student_second";

    //traversal
    for(iter = mapStudent.begin(); iter != mapStudent.end(); iter++)
                cout<<iter->first<<" "<<iter->second<<endl;
    for(iter_r = mapStudent.rbegin(); iter_r != mapStudent.rend(); iter_r++)
                cout<<iter_r->first<<" "<<iter_r->second<<endl;

    //find and erase the element
    iter = mapStudent.find("r123");
    mapStudent.erase(iter);

    iter = mapStudent.find("r123");

    if(iter != mapStudent.end())
       cout<<"Find, the value is "<<iter->second<<endl;
    else
       cout<<"Do not Find"<<endl;

    return 0;
}
```
## 其他
vector a(N,-1)
三目运算符同时计算，不存在短路

auto的使用：
- 比如vector的遍历，std::vector<int>::iterator可以用auto类型代替，变好懒
- 如果变量的类型依赖于模板参数，使用auto关键字使得在编译期确定这些类型

sort&cmp:
```c++
auto cmp = [](std::pair<K,V> const & a, std::pair<K,V> const & b)
{
     return a.second != b.second?  a.second < b.second : a.first < b.first;
};
std::sort(items.begin(), items.end(), cmp);
```


``lambda``函数：
```c++
[](int x, int y) { return x + y; } // 從return語句中隱式獲得的返回值類型
[](int& x) { ++x; }   // 沒有return語句 -> lambda函數的返回值為void
[]() { ++global_x; }  // 沒有參數，僅僅是訪問一個全局變量
[]{ ++global_x; }     // 與前者相同，()可以被省略
```

Q&A:
1. What is the difference between actual parameters and formal parameters?
The parameters passed to the function at calling end are called as actual parameters and the parameters at the receiving of the function definition called as formal parameters.
2. What is the difference between variable declaration and variable definition?
When we declare a variable, the compiler will get know about its data type, whereas variable definition tells the value to the variable.
3. What is ‘cin’
cin is the object of istream class. The object ‘cin’ is connected to console input device by default.
4. What is ‘std’?
It is the default namespace defined by C++.
5. Can we create and empty class? What would be the size of such object.
Yes, we can create an empty class. Size of the object of the empty class will be one.
6. What happens if an exception is thrown outside a try block?
The program will end abruptly.
7. What is the difference between delete and delete []?
delete will release the memory allocated to single object whereas delete [] will release the memory allocated to array of objects (all the memory assigned to the array of objects will be  released).
8. Is it legal to assign a base class object to a derived class pointer?
It is not legal to assign base class object to a derived class pointer. The compiler will fail to do this, and throw an error.
9. What are valid operations on pointers?
The valid operations on pointers are
- Comparison of two pointers
- Addition/Subtraction of two pointers (excluding void pointers)
10. Explain the purpose of the keyword volatile.
When we use volatile on the variable, it tells the compiler that it can be changed externally. Therefore compiler will not optimize the code while referencing the variables.
11. What is an inline function?

When we prefix a keyword inline before any normal function, the function becomes inline function. These types of functions are considered as macros and they execute faster than the normal functions.
12. What is a storage class?

Storage classes are used to indicate the scope of the variables and functions in a program.
There are 5 types of storage classes: auto, static, extern, register and mutable
13. What is the role of mutable storage class specifier?

Member variable of a constant class object can be altered by declaring it using mutable storage class specifier. This is applicable only for non-static and non-constant member variable of the class.
14. Distinguish between shallow copy and deep copy?

Shallow copy does memory allocation bit-by-bit from one object to another. Deep copy copies field by field from one object to another object. Deep copy is achieved by using copy constructor and or overloading ‘=’ (assignment operator).
15. Explain the static member function.

A static member function can be called using the class name since static member exists before class objects comes into existence. It can access only static members of the class.
16. What is role of static keyword on class member variable?

Static keyword is used for class member variables to share the common memory. That means, when objects are created using the class which has static member variable, then all the object’s static member variable will address same memory location. Hence this member variable will have same value in all the objects.

17. Which data type which can be used to store wide characters in C++?

Wide characters are stored using wchar_t data type.
18. Name the default standard streams in C++.

There are four default standard streams in C++: cin, cout, cerr and clog.
19.  What is the scope resolution operator?

The scope resolution operator is used to

- Determine the scope of global variables.
- Links the function definition to the class if the function is defined outside the class.
20. What is the difference between struct and class in C++?

The main difference between struct and class is: in struct are members are public by default whereas in class, members are private by default.
21. Explain this pointer?

this, is the pointer variable which always points to the current active object’s address.
22. What is the role of the file opening mode ios::trunk?

If the file already exists, this command truncates the file before opening it.
23. What is a namespace?

A namespace is the logical portion of the code which can be used to resolve the name conflict among the identifiers by placing them under different name space.
24. What is a class template?

A template class is a generic class. The keyword template is used to define a class template.
25. What is the purpose of extern storage specifier?

It is used to resolve the scope of global symbol
26. What is a preprocessor?

Preprocessor is a directive which directs the compiler to perform certain functions before compiling the actual code.
27. What are the different ways of passing parameters to the functions? Which to use when?

Call by value: In this method, only the values are passed to the function as arguments. In this case, the values passed will be manipulated within the program, but its actual value in the variable will not be modified.
Call by address: Here address of the actual parameters is passed instead of values. One can opt for this method if one needs to modify both actual parameters and formal parameters.
Call by reference: The actual parameters are receives new reference variables as formal parameters. One can opt this method if they want both actual parameters and formal parameters to be modified.
The functions are called by value by default.
28.  When should we use the register storage specifier?

If a variable is used most frequently, then retrieving such variable from memory needs frequent access. Therefore it should be declared using register storage specifier, so that the compiler gives CPU register for its storage which speeds up the variable retrieval.
29.
