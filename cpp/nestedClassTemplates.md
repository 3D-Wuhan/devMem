# Nested class templates

<https://docs.microsoft.com/en-us/cpp/cpp/class-templates?view=msvc-160>

**********

Templates can be defined within classes or class templates, in which case they 
are referred to as **member templates**. Member templates that are classes are 
referred to as nested class templates. Member templates that are functions are 
discussed in [Member Function Templates](https://docs.microsoft.com/en-us/cpp/cpp/member-function-templates?view=msvc-160).

Nested class templates are declared as class templates inside the scope of the 
outer class. They can be defined inside or outside of the enclosing class.

The following code demonstrates a nested class template inside an ordinary class.
```cpp
// nested_class_template1.cpp
// compile with: /EHsc
#include <iostream>
using namespace std;

class X
{
   template <class T>
   struct Y
   {
      T m_t;
      Y(T t): m_t(t) { }
   };

   Y<int> yInt;
   Y<char> yChar;

public:
   X(int i, char c) : yInt(i), yChar(c) { }
   void print()
   {
      cout << yInt.m_t << " " << yChar.m_t << endl;
   }
};

int main()
{
   X x(1, 'a');
   x.print();
}
```

```cpp
// nested_class_template2.cpp
// compile with: /EHsc
#include <iostream>
using namespace std;

template <class T>
class X
{
   template <class U> class Y
   {
      U* u;
   public:
      Y();
      U& Value();
      void print();
      ~Y();
   };

   Y<int> y;
public:
   X(T t) { y.Value() = t; }
   void print() { y.print(); }
};

template <class T>
template <class U>
X<T>::Y<U>::Y()
{
   cout << "X<T>::Y<U>::Y()" << endl;
   u = new U();
}

template <class T>
template <class U>
U& X<T>::Y<U>::Value()
{
   return *u;
}

template <class T>
template <class U>
void X<T>::Y<U>::print()
{
   cout << this->Value() << endl;
}

template <class T>
template <class U>
X<T>::Y<U>::~Y()
{
   cout << "X<T>::Y<U>::~Y()" << endl;
   delete u;
}

int main()
{
   X<int>* xi = new X<int>(10);
   X<char>* xc = new X<char>('c');
   xi->print();
   xc->print();
   delete xi;
   delete xc;
}

//Output:
X<T>::Y<U>::Y()
X<T>::Y<U>::Y()
10
99
X<T>::Y<U>::~Y()
X<T>::Y<U>::~Y()
```

Local classes are not allowed to have member templates.

