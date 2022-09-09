# STL

[TOC]

## Container

### Interface of All Containers

#### Container opeator

| Type                        | Example                                |
| --------------------------- | -------------------------------------- |
| Default                     | std::vector<int> vec1                  |
| Range std::vector<int>      | vec2(vec1.begin(), vec1.end())         |
| Copy std::vector<int>       | vec3(vec2)                             |
| Copy std::vector<int>       | vec3= vec2                             |
| Move std::vector<int>       | vec4(std::move(vec3))                  |
| Move std::vector<int>       | vec4= std::move(vec3)                  |
| Sequence (Initializer list) | std::vector<int> vec5 {1, 2, 3, 4, 5}  |
| Sequence (Initializer list) | std::vector<int> vec5= {1, 2, 3, 4, 5} |
| Destructor                  | vec5.∼vector()                         |
| Delete elements             | vec5.clear()                           |

##### Assignment and Swap

If you assign containers, you copy all elements of the source container and remove all old elements in the destination container. Thus, assignment of containers is relatively expensive.

``` c++
copy (1) vector& operator= (const vector& x);
move (2) vector& operator= (vector&& x);
initializer list (3) vector& operator= (initializer_list<value_type> il);
```

swap is Exchanges the contents of the container with those of `other`. Does not invoke any move, copy, or swap operations on individual elements. All iterators and references remain valid. The past-the-end iterator is invalidated.

##### Content Type

| Type            | Req | Effect                                               |
| --------------- | --- | ---------------------------------------------------- |
| size_type       | Yes | Unsigned integral type for size values               |
| difference_type | Yes | Signed integral type for difference values           |
| value_type      | Yes | Type of the elements                                 |
| reference       | Yes | Type of element references                           |
| const_reference | Yes | Type of constant element references                  |
| iterator        | Yes | Type of iterators                                    |
| const_iterator  | Yes | Type of iterators to read-only elements              |
| pointer         |     | Type of pointers to elements (since C++11)           |
| const_pointer   |     | Type of pointers to read-only elements (since C++11) |

### Sequential Containers

Sequence containers are ordered collections in which every element has a certain position. This
position depends on the time and place of the insertion, but it is independent of the value of
the element. F

| Criteria                           | array                                             | vector                                                | deque                                                  | list                                            | forward_list                                             |
| ---------------------------------- | ------------------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------ | ----------------------------------------------- | -------------------------------------------------------- |
| size                               | static                                            | dynamic                                               | dynamic                                                | dynamic                                         | dynamic                                                  |
| Implementation                     | static array                                      | dynamic array                                         | sequence of arrays                                     | doubled linked list                             | single linked list                                       |
| Access                             | random                                            | random                                                | random                                                 | forward and backward                            | single linked list                                       |
| Optimized for insert and delete at |                                                   | end: O(1)                                             | begin and end: O(1)                                    | begin and end: O(1)                             | begin(1)                                                 |
| Memory reservation                 |                                                   | yes                                                   | no                                                     | no                                              | no                                                       |
| Release of memory                  |                                                   | shirnk_to_fit                                         | shirnk_to_fit                                          | always                                          | always                                                   |
| Strength                           | no memory allocation; minimal memory requirements | 95% solution                                          | insertion and deletion at the begin and end            | insertion and deletion at an arbitrary position | fast insertion and deletion; minimal memory requirements |
| Weakness                           | no dynamic memory & memory allocation             | Insertion and deletion at an arbitrary position: O(n) | Insertion and deletion at an arbitrary postition: O(n) | no random access                                | no random access                                         |

#### Array

#### Vector

| Method           | Description                                   |
| ---------------- | --------------------------------------------- |
| .size()          |                                               |
| .capcity()       |                                               |
| .resize(n)       |                                               |
| .reserve(n)      | reserves storage                              |
| .shrink_to_fit() | reduces memory usage by freeing unused memory |

**reserve**

Increase the capacity of the vector (the total number of elements that the vector can hold without requiring reallocation) to a value that's greater or equal to new_cap. If new_cap is greater than the current capacity(), new storage is allocated, otherwise the function does nothing.

reserve() does not change the size of the vector.

If new_cap is greater than capacity(), all iterators, including the past-the-end iterator, and all references to the elements are invalidated. Otherwise, no iterators or references are invalidated.

**shrink_to_fit**

Requests the removal of unused capacity.

It is a non-binding request to reduce capacity() to size(). It depends on the implementation whether the request is fulfilled.

If reallocation occurs, all iterators, including the past the end iterator, and all references to the elements are invalidated. If no reallocation takes place, no iterators or references are invalidated.

`vector<bool>`: behaves similarly to `std::vector`, but in order to be space efficient, it:

* Does not necessarily store its elements as a contiguous array.
* Exposes class `std::vector<bool>::reference` as a method of accessing individual bits. In particular, objects of this class are returned by `operator[]` by value.
* Does not use **std**::**allocator_traits**::**construct** to construct bit values.
* Does not guarantee that different elements in the same container can be modified concurrently by different threads.

#### Deque

#### List

#### Forward List

### Associative Container

Associative containers are sorted collections in which the position of an element depends on its
value (or key, if it’s a key/value pair) due to a certain sorting criterion.

| Associative container       | Sorted | Associated value | More identical keys | Access time |
| --------------------------- | ------ | ---------------- | ------------------- | ----------- |
| set                         | yes    | no               | no                  | logarithmic |
| unordered_set(unorder)      | no     | no               | no                  | constant    |
| map                         | yews   | yes              | no                  | logarithmic |
| unordered_map(unorder)      | no     | yes              | no                  | constant    |
| multiset                    | yes    | no               | yes                 | logarithmic |
| unordered_multiset(unorder) | no     | no               | yes                 | constant    |
| multimap                    | yes    | yes              | yes                 | logarithmic |
| unordered_multimap(unorder) | no     | yes              | yes                 | constant    |

#### Set and Multisets

``` c++
template <typename T,
         typename Compare = less<T>,
         typename Allocator = allocator<T> >
class set;
```

The optional second template argument defines the sorting criterion. If a special sorting criterion is not passed, the default criterion `less` is used.
The sorting criterion must define strict weak ordering, which is defined by the following four properties:

1. It has to be **antisymmetric**.
This means that for operator <: If x < y is true, then y < x is false.
This means that for a predicate op(): If op(x,y) is true, then op(y,x) is false.
2. It has to be **transitive**.
This means that for operator <: If x < y is true and y < z is true, then x < z is true.
This means that for a predicate op(): If op(x,y) is true and op(y,z) is true, then op(x,z)
is true.
3. It has to be **irreflexive**.
This means that for operator <: x < x is always false.
This means that for a predicate op(): op(x,x) is always false.
4. It has to have **transitivity of equivalence**, which means roughly: If a is equivalent to b and b is
equivalent to c, then a is equivalent to c.
This means that for operator <: If !(a<b) && !(b<a) is true and !(b<c) && !(c<b) is true
then !(a<c) && !(c<a) is true.
This means that for a predicate op(): If op(a,b), op(b,a), op(b,c), and op(c,b) all yield
false, then op(a,c) and op(c,a) yield false.

#### Maps and Multimaps

``` c++
template <typename Key, typename T,
         typename Compare = less<Key>,
         typename Allocator = allocator<pair<const Key,T> > >
class map;
```

The elements of a map or a multimap may have any types Key and T that meet the following two requirements:

1. Both key and value must be copyable or movable.
2. The key must be comparable with the sorting criterion.

**element type (value_type) is a pair `<const Key, T>`**

The elem has type `pair<>`. The pair first `elem.first` yields the key of the actual element, whereas the expression `elem.second` yields the value of the actual element.

Since C++11, the most convenient way to insert elements is to pass them as an initializer list, where the first entry is the key and the second entry is the value:

1. Use value_type
   `coll.insert(std::map<std::string,float>::value_type("otto", 22.3));`
2. Use pair<>
   `coll.insert(std::pair<const std::string,float>("otto",22.3));`
3. Use make_pair()
   `coll.insert(std::make_pair("otto",22.3));`

### Unordered Containers

Unordered (associative) containers are unordered collections in which the position of an element doesn’t matter.

**All standardized unordered container classes are implemented as hash tables**, which nonetheless still have a variety of implementation options.
**The hash tables use the “chaining” approach**, whereby a hash code is associated with a linked list. (This technique, also called “open hashing” or “closed addressing,” should not be confused with “open addressing” or “closed hashing.”)

``` c++
template <typename Key, typename T,
         typename Hash = hash<T>,
         typename EqPred = equal_to<T>,
         typename Allocator = allocator<pair<const Key, T> > >
class unordered_map;
```

### Adaptors for Containers

#### Stack

#### Queue

#### Priority Queue

## Iterators

* operator `*` returns the element of the current position. If the elements have members, you can use operator `->` to access those members directly from the iterator.
* operator `++` lets the iterator step forward to the next element. Most iterators also allow stepping backward by using operator `--`
* operator `==` `!=` return whether two iterators represent the same position.
* opearor `=` assigns an iterator (the position of the element to which it refers)
* `begin()` returns an iterator that represents the **beginning of the elements in the container**. The
  beginning is the position of the first element, if any.
* `end()` returns an iterator that represents the end of the elements in the container. **The end is the
  position behind the last element**. Such an iterator is also called a past-the-end iterator

In fact, every container defines two iterator types:

1. `container::iterator` is provided to iterate over elements in read/write mode.
2. `container::const_iterator` is provided to iterate over elements in read-only mode.

### Iterator Categories

1. `Forward iterators` are able to iterate only forward, using the increment operator. The iterators of the class forward_list are forward iterators. The iterators of the container classes
   unordered_set, unordered_multiset, unordered_map, and unordered_multimap are
   “at least” forward iterators
2. `Bidirectional iterators` are able to iterate in two directions: forward, by using the increment
   operator, and backward, by using the decrement operator. The iterators of the container classes
   list, set, multiset, map, and multimap are bidirectional iterators.
3. `Random-access iterators` have all the properties of bidirectional iterators. In addition, they
   can perform random access. In particular, they provide operators for iterator arithmetic (in
   accordance with pointer arithmetic of an ordinary pointer). You can add and subtract offsets,
   process differences, and compare iterators by using relational operators, such as < and >. The
   iterators of the container classes vector, deque, array, and iterators of strings are randomaccess iterators.
4. In addition, two other iterator categories are defined:
   • `Input iterators` are able to read/process some values while iterating forward. Input stream iterators are an example of such iterators.
   • `Output iterators` are able to write some values while iterating forward.

### Auxiliary Iterator Functions

`advance()`
Lets the input iterator pos step n elements forward (or backward).

`next()`

`prev()`

`distance()`
Returns the distance between the input iterators pos1 and pos2.

`iter_swap()`
• Swaps the values to which iterators pos1 and pos2 refer.
• The iterators don’t need to have the same type. However, the values must be assignable.

### Iterator Traits

For each iterator category, the C++ standard library provides an iterator tag that can be used as a
“label” for iterators:

```c++
namespace std {
   struct output_iterator_tag {
   };
   struct input_iterator_tag {
   };
   struct forward_iterator_tag
      : public input_iterator_tag {
   };
   struct bidirectional_iterator_tag
      : public forward_iterator_tag {
   };
   struct random_access_iterator_tag
      : public bidirectional_iterator_tag {
   };
}
```

the C++ standard library provides a special template structure to define the iterator traits. This structure contains all relevant information about an iterator and is used as a common interface for all the type definitions an iterator should have (the category, the type of the elements, and so on):

```cpp
namespace std {
template<typename T>
struct iterator_traits {
  typedef typename T::iterator_category iterator_category;
  typedef typename T::value_type
      value_type;
  typedef typename T::difference_type
      difference_type;
  typedef typename T::pointer
      pointer;
  typedef typename T::reference
      reference;
};
}
```

In this template, T stands for the type of the iterator. Thus, you can write code that, for any iterator, uses its category, the type of its elements, and so on. For example, the following expression yields the value type of iterator type T: `typename std::iterator_traits<T>::value_type`
This structure has two advantages:

1. It ensures that an iterator provides all type deﬁnitions.
2. It can be (partially) specialized for (sets of) special iterators.

```cpp
namespace std {
  template<typename T>
  struct iterator_traits<T *> {
    typedef T value_type;
    typedef ptrdiff_t difference_type;
    typedef random_access_iterator_tag iterator_category;
    typedef T *pointer;
    typedef T &reference;
  };
}
```

### Writing Generic Functions for Iterators

```cpp
#include <iterator>
#include<iostream>
#include<unordered_set>
#include<vector>
#include<algorithm>
using namespace std;

// class template for insert iterator for associative and unordered containers
template<typename Container>
class asso_insert_iterator
    : public std::iterator<std::output_iterator_tag,
                           typename Container::value_type> {
 protected:
  Container &container;
// container in which elements are inserted
 public:
// constructor
  explicit asso_insert_iterator(Container &c) : container(c) {
  }
  // assignment operator
// - inserts a value into the container
  asso_insert_iterator<Container> &
  operator=(const typename Container::value_type &value) {
    container.insert(value);
    return *this;
  }
// dereferencing is a no-op that returns the iterator itself
  asso_insert_iterator<Container> &operator*() {
    return *this;
  }
// increment operation is a no-op that returns the iterator itself
  asso_insert_iterator<Container> &operator++() {
    return *this;
  }
  asso_insert_iterator<Container> &operator++(int) {
    return *this;
  }
};
// convenience function to create the inserter
template<typename Container>
inline asso_insert_iterator<Container> asso_inserter(Container &c) {
  return asso_insert_iterator<Container>(c);
}

template<typename T>
void PRINT_ELEMENTS(T iter){
  for(auto i : iter)
    cout<<i<<" ";
  cout<<endl;
}

int main()
{
  std::unordered_set<int> coll;
// create inserter for coll
// - inconvenient way
  asso_insert_iterator<decltype(coll)> iter(coll);
// insert elements with the usual iterator interface
  *iter = 1;
  iter++;
  *iter = 2;
  iter++;
  *iter = 3;
  PRINT_ELEMENTS(coll);
// create inserter for coll and insert elements
// - convenient way
  asso_inserter(coll) = 44;
  asso_inserter(coll) = 55;
  PRINT_ELEMENTS(coll);
// use inserter with an algorithm
  std::vector<int> vals = { 33, 67, -4, 13, 5, 2 };
  std::copy (vals.begin(), vals.end(),
// source
             asso_inserter(coll));
// destination
  PRINT_ELEMENTS(coll);
}


//output
3 2 1 
44 55 3 2 1 
13 -4 33 5 44 55 3 67 2 1 
```

## STL Function Objects and Using Lambdas

### The Concept of Function Object

``` c++
class FunctionObjectType {
public:
   void operator() () {
      statements
   }
};
```

#### Function Object as Sorting Criteria

``` c++
#include <iostream>
#include <string>
#include <set>
#include <algorithm>
using namespace std;

class Person {
public:
   string firstname() const;
   string lastname() const;
...
};

// class for function predicate  
// - operator () returns whether a person is less than another person
class PersonSortCriterion {
public:
   bool operator() (const Person& p1, const Person& p2) const {
   // a person is less than another person
   // - if the last name is less
   // - if the last name is equal and the first name is less
      return p1.lastname()<p2.lastname() ||
            (p1.lastname()==p2.lastname() &&
            p1.firstname()<p2.firstname());
   }
};

int main()
{
   // create a set with special sorting criterion
   set<Person,PersonSortCriterion> coll;
   ...
   // do something with the elements
   for (auto pos = coll.begin(); pos != coll.end(); ++pos) {
   ...
   }
   ...
}
```

#### Function Object with Internal State

``` c++
#include <iostream>
#include <list>
#include <algorithm>
#include <iterator>
#include "print.hpp"
using namespace std;

class IntSequence {
private:
   int value;
public:
   // constructor
   IntSequence (int initialValue): value(initialValue) {
   }
   // ‘‘function call’’
   int operator() () {
      return ++value;
   }
};
int main()
{
   list<int> coll;
   IntSequence seq(1); // integral sequence starting with 1
   // insert values from 1 to 4
   // - pass function object by reference
   // so that it will continue with 5
   generate_n<back_insert_iterator<list<int>>,
            int, IntSequence&>(back_inserter(coll), // start
            4, // number of elements
            seq); // generates values
   PRINT_ELEMENTS(coll);

   // insert values from 42 to 45
   generate_n (back_inserter(coll), // start
               4, // number of elements
                IntSequence(42)); // generates values
   PRINT_ELEMENTS(coll);

   // continue with first sequence
   // - pass function object by value
   // so that it will continue with 5 again
   generate_n (back_inserter(coll), // start
               4, // number of elements
               seq); // generates values
   PRINT_ELEMENTS(coll);

   // continue with first sequence again
   generate_n (back_inserter(coll), // start
               4, // number of elements
               seq); // generates values
   PRINT_ELEMENTS(coll);
}

output:
2 3 4 5
2 3 4 5 43 44 45 46
2 3 4 5 43 44 45 46 6 7 8 9
2 3 4 5 43 44 45 46 6 7 8 9 6 7 8 9
```

#### The return value for `for_each()`

return value

``` c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// function object to process the mean value
class MeanValue {
private:
   long num; // number of elements
   long sum; // sum of all element values
public:
   // constructor
   MeanValue () : num(0), sum(0) {
   }
   // ‘‘function call’’
   // - process one more element of the sequence
   void operator() (int elem) {
      ++num; // increment count
      sum += elem; // add value
   }
   // return mean value
   double value () {
      return static_cast<double>(sum) / static_cast<double>(num);
   }
};

int main()
{
   vector<int> coll = { 1, 2, 3, 4, 5, 6, 7, 8 };
   // process and print mean value
   MeanValue mv = for_each (coll.begin(), coll.end(), // range
                              MeanValue()); // operation
   cout << "mean value: " << mv.value() << endl;
}

output:
mean value: 4.5
```

#### Predicates are functions or function objects

return a Boolean value

``` c++
//Error example
#include <iostream>
#include <list>
#include <algorithm>
using namespace std;

class Nth { // function object that returns true for the nth call
 private:
  int nth; // call for which to return true
  int count; // call counter
 public:
  Nth(int n) : nth(n), count(0) {
  }
  bool operator()(int) {
    return ++count == nth;
  }
};

void print(list<int> &a) {
  for (auto i : a)
    cout << i << " ";
  cout << endl;
}

int main() {
  list<int> coll = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
// remove third element
  list<int>::iterator pos;
  print(coll);
  pos = remove_if(coll.begin(), coll.end(), // range
                  Nth(3)); // remove criterion
  coll.erase(pos, coll.end());
  print(coll);

  return 0;
}
output:
1 2 3 4 5 6 7 8 9 10 
1 2 4 5 7 8 9 10 
```

For result, two element are removed. This happens because the usual implementation of the algorithm copies the predicate internally during the algorithm:

``` c++
template <typename ForwIter, typename Predicate>
ForwIter std::remove_if(ForwIter beg, ForwIter end, Predicate op)
{
   beg = find_if(beg, end, op);
   if (beg == end) {
      return beg;
   }
   else {
      ForwIter next = beg;
      return remove_copy_if(++next, end, beg, op);
   }
}
```

This behavior is not a bug. The standard does not specify how often a predicate might be copied internally by an algorithm. Thus, to get the guaranteed behavior of the C++ standard library, you should not pass a function object for which the behavior depends on how often it is copied or called. Thus, **if you call a unary predicate for two arguments and both arguments are equal, the predicate should always yield the same result.**
In other words: **A predicate should always be stateless**. That is, a predicate should not change its state due to a call, and a copy of a predicate should have the same state as the original. To ensure that you can’t change the state of a predicate due to a function call, you should declare `operator ()` as a constant member function.

It is possible to avoid this surprising behavior and to guarantee that this algorithm works as expected even for a function object such as Nth, without any performance penalties. You could implement `remove_if()` in such a way that the call of `find_if()` is replaced by its contents:

``` c++
template<typename ForwIter, typename Predicate>
ForwIter std::remove_if(ForwIter beg, ForwIter end,
                        Predicate op) {
  while (beg != end && !op(*beg)) {
    ++beg;
  }
  if (beg == end) {
    return beg;
  } else {
    ForwIter next = beg;
    return remove_copy_if(++next, end, beg, op);
  }
}
```

### Predefined Function Objects and Binders

#### Predefined Function Objects

| Expression        | Effect          |
| ----------------- | --------------- |
| `negate<type>()`  | -param          |
| `plus<type>()`    | param1 + param2 |
| `greater<type>()` | param1 > param2 |
| ....              | .....           |

#### Function Adapters and Binders

| Expression          | Effect                                                               |
| ------------------- | -------------------------------------------------------------------- |
| `bind(op, args...)` | Binds args to op                                                     |
| `mem_fn(op)`        | Calls `op()` as a member function for an object or pointer to object |
| `not1(op)`          | Unary negation: `!op(param)`                                         |
| `not2(op)`          | Binary negation: `!op(param1,param2)`                                |

`bind()`:

* Adapt and compose new function objects out of existing or predefined function objects
* Call global functions
* Call member functions for objects, pointers to objects, and smart pointers to objects

##### bind adapter

`bind()` binds parameters for callable objects. Thus, if a function, member function ,function object, or lambda requires some parameters, you can bind them to specific or passed arguments. Specific arguments you simply name. For passed arguments, you can use the redefined placeholders _1,_2, ... defined in namespace std::placeholders.
  
``` c++
#include <functional>
#include <iostream>

int main() {
  auto plus10 = std::bind(std::plus<int>(),
                          std::placeholders::_1,
                          10);
  std::cout << "+10 " << plus10(7) << std::endl;
  auto plus10times2 = std::bind(std::multiplies<int>(),
                                std::bind(std::plus<int>(),
                                          std::placeholders::_1,
                                          10),
                                2);
  std::cout << "+10 *2: " << plus10times2(7) << std::endl;
  auto pow3 = std::bind(std::multiplies<int>(),
                        std::bind(std::multiplies<int>(),
                                  std::placeholders::_1,
                                  std::placeholders::_1),
                        std::placeholders::_1);
  std::cout << "x*x*x: " << pow3(7) << std::endl;
  auto inversDivide = std::bind(std::divides<double>(),
                                std::placeholders::_1,
                                std::placeholders::_2);
  std::cout << "invdiv: " << inversDivide(49, 7) << std::endl;
}

// OUTPUT
+10 17
+10 *2: 34
x*x*x: 343
invdiv: 7

```

###### Calling global Function

```cpp
#include <iostream>
#include <algorithm>
#include <functional>
#include <locale>
#include <string>

using namespace std;
using namespace std::placeholders;

char myToupper(char c) {
  std::locale loc;
  return std::use_facet<std::ctype<char> >(loc).toupper(c);
}

int main() {
  string s("Internationalization");
  string sub("Nation");
// search substring case insensitive
  string::iterator pos;
  pos = search(s.begin(), s.end(),
// string to search in
               sub.begin(), sub.end(),
// substring to search
               bind(equal_to<char>(),
// compar. criterion
                    bind(myToupper, _1),
                    bind(myToupper, _2)));
  if (pos != s.end()) {
    cout << "\"" << sub << "\" is part of \"" << s << "\""
         << endl;
  }
}

//output
"Nation" is part of "Internationalization"
```

###### Calling member functions

```cpp
#include <functional>
#include <algorithm>
#include <vector>
#include <iostream>
#include <string>

using namespace std;
using namespace std::placeholders;

class Person {
 private:
  string name;
 public:
  Person(const string &n) : name(n) {
  }
  void print() const {
    cout << name << endl;
  }
  void print2(const string &prefix) const {
    cout << prefix << name << endl;
  }
};

int main() {
  vector<Person> coll
      = {Person("Tick"), Person("Trick"), Person("Track")};
// call member function print() for each person
  for_each(coll.begin(), coll.end(),
           bind(&Person::print, _1));
  cout << endl;
// call member function print2() with additional argument for each person
  for_each(coll.begin(), coll.end(),
           bind(&Person::print2, _1, "Person: "));
  cout << endl;
// call print2() for temporary Person
  bind(&Person::print2, _1, "This is: ")(Person("nico"));
}

//output
Tick
Trick
Track

Person: Tick
Person: Trick
Person: Track

This is: nico
```

`bind(&Person::print, _1)`, for member function, it hidden the first argument is `this`.

##### The mem_fn adapter

For member functions, you can also use the `mem_fn()` adapter, whereby you can skip the placeholder for the object the member function is call for:

```cpp
std::for_each (coll.begin(), coll.end(), std::mem_fn(&Person::print));
```

Thus, `mem_fn()` simply calls an initialized member function for a passed argument while additional
arguments are passed as parameters to the member function:

```cpp
std::mem_fn(&Person::print)(n);
// calls n.print()
std::mem_fn(&Person::print2)(n,"Person: "); // calls n.print2("Person: ")
```

However, to bind an additional argument to the function object, you again have to use bind():

```cpp
std::for_each (coll.begin(), coll.end(),
std::bind(std::mem_fn(&Person::print2),
            std::placeholders::_1,
            "Person: "));
```

##### User-Defined Function Objects for Function Adapters

```cpp
#include <cmath>
#include <iostream>
#include <vector>
#include <iterator>
#include <functional>

using namespace std;
using namespace std::placeholders;

template<typename T1, typename T2>
struct fopow {
  T1 operator()(T1 base, T2 exp) const {
    return std::pow(base, exp);
  }
};


int main() {
  vector<int> coll = {1, 2, 3, 4, 5, 6, 7, 8, 9};
// print 3 raised to the power of all elements
  transform(coll.begin(), coll.end(),
            ostream_iterator<float>(cout, " "),
            bind(fopow<float, int>(), 3, _1));
  cout << endl; // source
// destination
// operation
// print all elements raised to the power of 3
  transform(coll.begin(), coll.end(),
            ostream_iterator<float>(cout, " "),
            bind(fopow<float, int>(), _1, 3));
  cout << endl; // source
// destination
// operation
}


The program has the following output:
3 9 27 81 243 729 2187 6561 19683
1 8 27 64 125 216 343 512 729
```

##### Deprecated Function Adapters

|Expression | Effect|
|----------|-------|
|bind1st(op, arg)| Calls op(arg, param)|
|bind2nd(op, arg)| Calls op(param, arg)|
|ptr_fun(op)     | calls *op(param) or*op(param1, param2)|
|mem_fun(op)   | calls op() as a member function for a pointer to an object|
|mem_fun_ref(op)|Calls op() as a member function for an object|
|not1(op)        | Unary negation: !op(param) |
|not2(op)        | Binary negation: !op(param1,param2)|

### Lambda

using lambda for stateful function objects

```cpp
#include <iostream>
#include <list>
#include <algorithm>
using namespace std;

int main()
{
  list<int> coll = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
  PRINT_ELEMENTS(coll,  "coll");
// remove third element
  list<int>::iterator pos;
  int count=0;
// call counter
  pos = remove_if(coll.begin(),coll.end(),
                  [count] (int) mutable {
                    return ++count == 3;
                  });
  coll.erase(pos,coll.end());
// range
// remove criterion
  PRINT_ELEMENTS(coll,"3rd removed: ");
}


//output
coll: 1 2 3 4 5 6 7 8 9 10
3rd removed: 1 2 4 5 7 8 9 10
```

It requires mutable because by default, a function object should produce the same result every time it's called. This is the difference between an object orientated function and a function using a global variable, effectively.

## Algorithms

### Nonmodifying algorithms

`for_each()`

`count_if()`

`find()`

### Modifying algorithms

`for_each()`

`transform()`

### Removing algorithms

`remove()`

### Mutating algorithms

change the order of elements (and not their values) by assigning and swapping their values.

`reverse()`

`rotate()`

### Sorting algorithms

`sort()`
`stable_sort()`
`partial_sort()`
`partial_sort_copy()`

### Sorted-range algorithms

Sorted-range algorithms require that the ranges on which they operate be sorted according to their
sorting criterion.

`binary_search()`
`includes()`
`lower_bound()`
`upper_bound()`
`equal_range()`
`merge()`
`set_union()`

### Numeric algorithms

`accumulate()`
`inner_product()`
`adjacent_difference()`
`partial_sum()`

## Iterator Adapters

### Insert iterators

**Back inserters**: insert the elements at the back of their container (appends them) by calling push_back().

``` c++
vector<int> coll2;
copy (coll1.cbegin(), coll1.cend(), back_inserter(coll2));
```

**Front inserters**: insert the elements at the front of their container by calling push_front()

``` c++
 deque<int> coll3;
 copy (coll1.cbegin(), coll1.cend(), // source
          front_inserter(coll3)); // destination
```

**General inserters**, or simply inserters, insert elements directly in front of the position that is
passed as the second argument of its initialization

``` c++
  set<int> coll4;
  copy(coll1.cbegin(), coll1.cend(), // source
       inserter(coll4, coll4.begin())); // destination
```

| Expression                | Kind of Inserter                                               |
| ------------------------- | -------------------------------------------------------------- |
| back_inserter(container)  | Appends in the same order by using push_back(val)              |
| front_inserter(container) | Inserts at the front in reverse order by using push_front(val) |
| inserter(container,pos)   | Inserts at pos (in the same order) by using insert(pos,val)    |

### Stream iterators

`istream_iterator()`

### Reverse iterators

### Move iterators (since C++11)
