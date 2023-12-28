convert class from to another using reinterpret_cast<>
mutable can be used to change the data in a const object The keyword mutable is mainly used to allow a particular data member of const object to be modified. When we declare a function as const, the this pointer passed to function becomes const. Adding mutable to a variable allows a const pointer to change members.

https://stackoverflow.com/questions/16456366/why-does-the-size-of-a-class-depends-on-the-order-of-the-member-declaration-and

cout << x returns cout for chaining multiple
RBV does optimization to avoid copy
explicit prevents type promotions
Class& operator=(const Class&)
std::addressof() if someone does something naughty by overloading &
inline recursive doesnt go very far
lvalue,rvalue,xvalue
emplace_back is more efficient
subclass exceptions
std::move converts an lvalue to rvalue for the move constructor class(class&& obj)
make_unique to upgrade a regular pointer ot a unique pointer
can do std::move to unique_ptr, but dont dream about doing =
{} is uniform initialization
    * std::atomic (uncopyable objects) needs {} or () cant do =
    * checks for breakages if you try to do int a = {x+y+z} where x,y,z are doubles
    * does weird fuckery with initializer list
IntArray(std::initializer_list<int> list) : // allow IntArray to be initialized via list initialization is like a variadic constructor
nth_element vs partial_sort
remove vs erase for vector
v.erase(std::remove(v.begin(), v.end(), val), v.end());
valarray
https://en.cppreference.com/w/cpp/container/vector/operator_cmp
priority_queue<int> is a max heap
priority_queue <int, vector<int>, greater<int>> g = gq; is a minheap
priority_queue<pair<int, int> > pq key value style for maxheap
typedef pair<int, int> pi; 
priority_queue<pi, vector<pi>, greater<pi> > pq; is for minheap
lower_bound/upper_bound need array to be presorted

stack.top()/push()/pop()
bool next_permutation (BidirectionalIterator first,
                       BidirectionalIterator last);
rbegin()/rend()
do map[key]++ if map<key,int>. By default it\'s 0
iterate through set whilst deleting
for (auto it = numbers.begin(); it != numbers.end(); ) {
    if (*it % 2 == 0) {
        it = numbers.erase(it);
    }
    else {
        ++it;
    }
}

no decrease_key of priority queue,use lazy deletion using a visited array instead   

auto is good! use it for iterators. map<const key,int>
std::vector<bool> is a weird type - uses 1 bit per type
use deque<bool> for regular behavior
T * p = & c[0]; is illegal if c is vector<bool>

using= >>>> typedef

do enum class {} instead of enum{} need to do name::enumtype
enum Color: std::uint8_t {ALLA,HU,AK,BAR}

makes these functions impossible to create
basic_ios(const basic_ios& ) = delete;
basic_ios& operator=(const basic_ios&) = delete;

Widget& v //lvalue
Widget&& v//rvalue so you can use it as such in a function:
DataType& data() &
{ return values; }
// for lvalue Widgets,
// return lvalue
DataType data() &&
{ return std::move(values); }
...
// for rvalue Widgets,
// return rvalue


reverse iterator rbegin() -> points to end of vector and also goes the wrong way when you do ++ 
for (; rit!= myvector.rend(); ++rit)
*rit = ++i;

int f(int x) noexcept;
 // no exceptions from f: C++11 style, optimizes compilation

constexpr is stronger cost than const eg when 
int s;
const auto x = s; // okay
constexpr auto x = s; //bad X
constexpr functions can be calculated at compile time if its args are known then

Mutex style
// you should really be using std::atomic<int> x for single assignments, but mutex for multiple
But, you can do mutex.lock()/mutex.unlock() or better std::lock_guard<std::mutex> g(m) where the mutex is m. Will automatically lock/unlcok when it goes out of scope

template<typename T>
* explicit - dont change me!!!

## Ptrs
unique_ptr makes src nullptr (make_unique)!!
// custom deleters
std::unique_ptr<
Widget, decltype(loggingDel)> upw(new Widget, loggingDel);
good for factory pattern
unique_ptr<T> and unique_ptr<T[]> for arrays, though inferior
shared_ptr == 2 ptrs - one for actual ptr and one for the ref count
    * uses atomic under the hood
    * once shared - cant downgrade
    * std::shared_ptr<int> p(new int);  // or \'=shared_ptr<int>(new int)\' if you insist
    * auto p = std::make_shared<int>(); // or \'std::shared_ptr<int> p\' if you insist
weak_ptr - catch dangles
    * shared_ptr which doesnt affect reference count
    * std::weak_ptr<Widget> wpw(spw);
    * auto spw = wpw.lock() for testing or getting ptr

## COnstructor/MOves
copy ctr - Point(const Point &p2) {x = p2.x; y = p2.y; } 
called when  Point p2 = p1;
rule_of_five(const rule_of_five& other) // copy constructor
: rule_of_five(other.cstring)
{}

rule_of_five(rule_of_five&& other) noexcept // move constructor
: cstring(std::exchange(other.cstring, nullptr))
{}

rule_of_five& operator=(const rule_of_five& other) // copy assignment
{
        return *this = rule_of_five(other);
}

rule_of_five& operator=(rule_of_five&& other) noexcept // move assignment
{
    std::swap(cstring, other.cstring);
    return *this;
}

Dafaq is std::Exchange? exchange(x,y) doesnt update y with x, but returns the value of y

a RetByValue() {
    a obj;
    return obj; // Might call move ctor, or no ctor.
}

void TakeByValue(a);

int main() {
    a a1;
    a a2 = a1; // copy ctor
    a a3 = std::move(a1); // move ctor

    TakeByValue(std::movea RetByValue() {
    a obj;
    return obj; // Might call move ctor, or no ctor.
} 
* // RETURN BY VALUE MIGHT CALL MOVE CTR

void TakeByValue(a);

int main() {
    a a1;
    a a2 = a1; // copy ctor
    a a3 = std::move(a1); // move ctor

    TakeByValue(std::move(a2)); // Might call move ctor, or no ctor.

    a a4 = RetByValue(); // Might call move ctor, or no ctor.

    a1 = RetByValue(); // Calls move assignment, a::operator=(a&&)}(a2)); // Might call move ctor, or no ctor.

    a a4 = RetByValue(); // Might call move ctor, or no ctor.

    a1 = RetByValue(); // Calls move assignment, a::operator=(a&&)
}

## PIMPL

need to have 
class foo {
public:
    move assmt,ctr
private:
    class impl;
    std::unique_ptr<impl> pimpl;
}

pointer casts dont require recompilation

## lvalue,rvalue
http://thbecker.net/articles/rvalue_references/section_01.html
int i;
int &r = i;//lvalue ref
but
const int &r = 7 //can cos it wont change

this->x=5 // rvalue expression
int &&x = 5; // rvalue ref
Conceptually, you can tame this type-zoo by grouping the five value categories into supersets, where glvalues include lvalues and xvalues, and rvalues include xvalues and prvalues // fuckme

cannot distinguish between X const& and X&&

cool!
http://www.cplusplus.com/reference/valarray/mask_array/

## QQ\'s
* wtf is RVO return value opti
*R AII
Rvalue

PIMPL
https://cpppatterns.com/patterns/pimpl.html
class Widget {
public:
Widget();
~Widget();
...
// still in header "widget.h"
// dtor is needed—see below
private:
struct Impl;
Impl *pImpl;
};
 