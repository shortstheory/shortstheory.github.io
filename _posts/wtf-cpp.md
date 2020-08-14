convert class from to another using reinterpret_cast<>
mutable can be used to change the data in a const object The keyword mutable is mainly used to allow a particular data member of const object to be modified. When we declare a function as const, the this pointer passed to function becomes const. Adding mutable to a variable allows a const pointer to change members.


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
do map[key]++ if map<key,int>. By default it's 0
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

basic_ios(const basic_ios& ) = delete;
basic_ios& operator=(const basic_ios&) = delete;

