# 🧰 C++ STL — Complete Guide

> *Striver A2Z DSA Course — Standard Template Library*
> 💡 **"STL = pre-built containers + algorithms. Don't reinvent the wheel."**

---

## 🧠 What is STL?

> **Standard Template Library** — a compilation of containers, algorithms, iterators & functions so you don't write lengthy code.

```
STL = Containers + Algorithms + Iterators + Functions
         ↓              ↓            ↓           ↓
    vector/map/set    sort()     begin/end    min/max
```

---

## 👫 Pairs

> Store **two values** together. Part of `<utility>` library.

```cpp
pair<int, int> p = {1, 3};

p.first   // → 1
p.second  // → 3
```

### Nested Pairs (store 3+ values)
```cpp
pair<int, pair<int, int>> p = {1, {3, 4}};

p.first          // → 1
p.second.first   // → 3
p.second.second  // → 4
```

### Pair Arrays
```cpp
pair<int, int> arr[] = {{1,2}, {3,4}, {5,6}};

arr[1].second  // → 4
```

---

## 📦 Vectors

> **Dynamic array** — size can grow/shrink anytime.

### Declaration & Insert
```cpp
vector<int> v;          // empty
v.push_back(1);         // adds 1 at back
v.emplace_back(2);      // faster than push_back

vector<int> v(5, 100);  // {100,100,100,100,100}
vector<int> v(5);       // {0,0,0,0,0}
vector<int> v2(v);      // copy of v
```

⚡ `emplace_back` > `push_back` (faster, constructs in-place)

### Access
```cpp
v[0]        // first element (like array)
v.back()    // last element
v.front()   // first element
```

### Iterators
```cpp
vector<int>::iterator it = v.begin();  // points to first
// or just:
auto it = v.begin();

*it          // value at iterator
it++         // move to next
v.begin()    // → first element
v.end()      // → AFTER last element (not last!)
v.rbegin()   // → last element (reverse)
v.rend()     // → before first (reverse)
```

### Print Vector (3 ways)
```cpp
// Way 1: index
for(int i = 0; i < v.size(); i++)
    cout << v[i] << " ";

// Way 2: iterator
for(auto it = v.begin(); it != v.end(); it++)
    cout << *it << " ";

// Way 3: for-each (cleanest)
for(auto x : v)
    cout << x << " ";
```

### Erase
```cpp
v.erase(v.begin() + 1);              // delete index 1
v.erase(v.begin() + 2, v.begin() + 4); // delete [2, 4) range
//                                       start included, end NOT
```

### Insert
```cpp
v.insert(v.begin(), 300);             // insert 300 at front
v.insert(v.begin() + 1, 5);          // insert 5 at index 1
v.insert(v.begin() + 1, 2, 5);       // insert two 5's at index 1
```

### Other Functions
```cpp
v.size()      // number of elements
v.pop_back()  // remove last element
v.clear()     // remove ALL elements
v.empty()     // true if empty
v1.swap(v2)   // swap two vectors
```

---

## 📋 List

> Like vector but **doubly linked list** internally. Has `push_front`.

```cpp
list<int> ls;
ls.push_back(2);
ls.emplace_back(4);
ls.push_front(5);      // O(1) — cheap!
ls.emplace_front(3);

// All vector functions work: begin, end, size, clear, etc.
```

⚡ **push_front is O(1) in list** vs O(N) insert at front in vector!

---

## 🔄 Deque

> Double-ended queue — push/pop from both ends.

```cpp
deque<int> dq;
dq.push_back(1);
dq.push_front(2);
dq.pop_back();
dq.pop_front();
dq.front();
dq.back();
```

---

## 📚 Stack (LIFO)

> **Last In, First Out** — think stack of plates.

```cpp
stack<int> st;
st.push(1);      // insert
st.push(2);
st.push(3);
st.emplace(5);   // same as push

st.top();        // → 5 (peek, doesn't remove)
st.pop();        // removes top (now top = 3)
st.size();       // → 3
st.empty();      // false
```

⚡ **All operations O(1)**
❌ **No indexing!** Can't do `st[0]` — only top/push/pop

---

## 🚶 Queue (FIFO)

> **First In, First Out** — think ticket line.

```cpp
queue<int> q;
q.push(1);       // {1}
q.push(2);       // {1, 2}
q.push(4);       // {1, 2, 4}

q.front();       // → 1
q.back();        // → 4
q.pop();         // removes front (1)
q.front();       // → 2 now
q.size();
q.empty();
```

⚡ **All operations O(1)**

---

## ⛰️ Priority Queue (Heap)

> Largest element always on top (Max Heap by default).

### Max Heap (default)
```cpp
priority_queue<int> pq;
pq.push(5);
pq.push(2);
pq.push(8);
pq.push(10);

pq.top();   // → 10 (largest)
pq.pop();   // removes 10
pq.top();   // → 8 now
```

### Min Heap
```cpp
priority_queue<int, vector<int>, greater<int>> pq;
pq.push(5);
pq.push(2);
pq.push(8);

pq.top();   // → 2 (smallest)
```

### ⏱️ Complexity
| Operation | Time |
|-----------|------|
| push | O(log N) |
| top | O(1) |
| pop | O(log N) |

---

## 🎯 Set

> **Sorted + Unique** elements only. Tree internally.

```cpp
set<int> s;
s.insert(1);      // {1}
s.emplace(2);     // {1, 2}
s.insert(2);      // {1, 2} — duplicate ignored!
s.insert(4);      // {1, 2, 4}
s.insert(3);      // {1, 2, 3, 4} — auto sorted!
```

### Find & Count
```cpp
s.find(3)         // → iterator to 3
s.find(6)         // → s.end() (not found)
s.count(1)        // → 1 (exists)
s.count(6)        // → 0 (not exists)
```

### Erase
```cpp
s.erase(5);                  // erase element 5
s.erase(s.find(3));          // erase by iterator
s.erase(s.find(2), s.find(4)); // erase range [2, 4)
```

### ⏱️ All operations: O(log N)

---

## 🎯 Multiset

> **Sorted** but allows **duplicates**.

```cpp
multiset<int> ms;
ms.insert(1);
ms.insert(1);
ms.insert(1);   // {1, 1, 1} — all stored!

ms.count(1);    // → 3

ms.erase(1);    // ❌ erases ALL 1's!
ms.erase(ms.find(1));  // ✅ erases only ONE 1
```

---

## 🎯 Unordered Set

> **Unique** but **NOT sorted**. Random order.

```cpp
unordered_set<int> us;
us.insert(1);
us.insert(5);
us.insert(3);
// stored in random order, but unique
```

### ⏱️ Complexity
| Case | Time |
|------|------|
| Average | O(1) ← most cases |
| Worst | O(N) ← rare |

❌ **No lower_bound / upper_bound** in unordered_set!

---

## 🗺️ Map

> **Key-Value pairs**. Keys are **sorted & unique**.

```cpp
map<int, int> mp;
mp[1] = 2;                   // key:1, value:2
mp.emplace(3, 1);            // key:3, value:1
mp.insert({2, 4});           // key:2, value:4

// stored as: {1:2}, {2:4}, {3:1} — sorted by KEY
```

### Access
```cpp
mp[1]           // → 2 (value at key 1)
mp[5]           // → 0 (key doesn't exist, returns 0)
```

### Find
```cpp
mp.find(3)      // → iterator to {3, 1}
mp.find(5)      // → mp.end() (not found)

auto it = mp.find(3);
(*it).second    // → 1 (the value)
```

### Iterate
```cpp
for(auto it : mp) {
    cout << it.first << " " << it.second << endl;
}
// prints: 1 2 \n 2 4 \n 3 1
```

### ⏱️ All operations: O(log N)

---

## 🗺️ Multimap & Unordered Map

| | Sorted | Unique Keys | Time |
|---|---|---|---|
| **map** | ✅ | ✅ | O(log N) |
| **multimap** | ✅ | ❌ duplicates OK | O(log N) |
| **unordered_map** | ❌ random | ✅ | O(1) avg |

---

## ⚡ Algorithms

### Sort
```cpp
sort(a, a + n);                    // sort array
sort(v.begin(), v.end());          // sort vector
sort(a + 2, a + 4);               // sort only [2,4)
sort(a, a + n, greater<int>());    // descending
```

### Custom Comparator
```cpp
bool comp(pair<int,int> p1, pair<int,int> p2) {
    if(p1.second < p2.second) return true;
    if(p1.second > p2.second) return false;
    // if second same → sort by first descending
    if(p1.first > p2.first) return true;
    return false;
}

sort(arr, arr + n, comp);
```

💡 **Comparator logic:** "Is p1 before p2 correct? Return true/false."

### __builtin_popcount
```cpp
__builtin_popcount(7);    // → 3 (binary: 111)
__builtin_popcount(6);    // → 2 (binary: 110)
__builtin_popcountll(n);  // for long long
```

### next_permutation
```cpp
string s = "123";
sort(s.begin(), s.end()); // MUST start sorted!

do {
    cout << s << endl;
} while(next_permutation(s.begin(), s.end()));

// prints: 123, 132, 213, 231, 312, 321
```

⚠️ **Start from sorted string to get ALL permutations!**

### max_element / min_element
```cpp
*max_element(a, a + n);   // returns max value
*min_element(a, a + n);   // returns min value
```

---

## 📋 Master Cheat Sheet

```
┌─────────────────────────────────────────────────────────────┐
│                    C++ STL CHEAT SHEET                       │
├─────────────────────────────────────────────────────────────┤
│ CONTAINER        │ KEY PROPERTY           │ TIME            │
├──────────────────┼────────────────────────┼─────────────────┤
│ vector           │ dynamic array          │ O(1) access     │
│ list             │ doubly linked          │ O(1) push_front │
│ stack            │ LIFO                   │ O(1) all ops    │
│ queue            │ FIFO                   │ O(1) all ops    │
│ priority_queue   │ max on top             │ O(log N) push   │
│ set              │ sorted + unique        │ O(log N) all    │
│ multiset         │ sorted + duplicates    │ O(log N) all    │
│ unordered_set    │ unique + O(1)          │ O(1) avg        │
│ map              │ sorted keys + unique   │ O(log N) all    │
│ unordered_map    │ unique keys + O(1)     │ O(1) avg        │
├──────────────────┼────────────────────────┼─────────────────┤
│ ALGORITHM        │ USE                    │ TIME            │
├──────────────────┼────────────────────────┼─────────────────┤
│ sort()           │ sort anything          │ O(N log N)      │
│ next_permutation │ all permutations       │ O(N)            │
│ __builtin_popcount│ count set bits        │ O(1)            │
│ max/min_element  │ find max/min           │ O(N)            │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [C++ Basics →](../dsa-notes/cpp-basics.md)
- [Time & Space Complexity →](./time-space-complexity.md)
- [Arrays & Hashing →](../dsa-notes/arrays.md)
