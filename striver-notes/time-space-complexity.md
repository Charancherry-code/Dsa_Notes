# ⏱️ Time & Space Complexity

> *Striver A2Z DSA Course — Step 1: Learn the Basics*
> 💡 **"Don't memorize complexities. Understand WHY a loop running N times = O(N)."**

---

## 🧠 What is Time Complexity?

> **Rate at which time taken increases with respect to input size.**

- It's NOT the actual time (seconds) — it's the **growth pattern**
- We count the number of **operations** relative to input size `N`
- We care about **worst case** (Big O) most of the time

```
Input doubles → How much more work?
O(1)     → same work          (constant)
O(log N) → slightly more      (logarithmic)
O(N)     → double the work    (linear)
O(N²)    → 4x the work       (quadratic)
O(2^N)   → explodes           (exponential)
```

---

## 📊 Big O Notation — The Ranking

```
BEST ←————————————————————————————→ WORST

O(1) < O(log N) < O(N) < O(N log N) < O(N²) < O(N³) < O(2^N) < O(N!)
 ↑        ↑         ↑        ↑            ↑        ↑        ↑        ↑
const  binary    single   merge/quick   nested   triple  recursive  perms
       search    loop     sort          loops    loops   subsets
```

---

## 🔥 Rules for Calculating Time Complexity

### Rule 1: Drop Constants
```cpp
for(int i = 0; i < 5*n; i++) { }  // O(5N) → O(N)
```
> Constants don't matter at scale. 5N and N grow the same way.

### Rule 2: Drop Lower Order Terms
```cpp
// O(N² + N + 1) → O(N²)
for(int i=0; i<n; i++)
    for(int j=0; j<n; j++) { }  // N²
for(int k=0; k<n; k++) { }      // + N → ignored
```

### Rule 3: Different Inputs = Different Variables
```cpp
// O(N * M) — NOT O(N²)
for(int i=0; i<n; i++)
    for(int j=0; j<m; j++) { }
```

---

## 💻 Common Patterns & Their Complexities

### O(1) — Constant
```cpp
int x = arr[0];          // direct access
int sum = n * (n+1) / 2; // formula
```

### O(log N) — Logarithmic
```cpp
// Binary Search — halves search space each time
while(lo <= hi) {
    int mid = (lo + hi) / 2;
    if(arr[mid] == target) return mid;
    else if(arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
}
```
> N → N/2 → N/4 → ... → 1 = log₂(N) steps

### O(N) — Linear
```cpp
for(int i = 0; i < n; i++) {
    cout << arr[i];  // visits each element once
}
```

### O(N log N) — Linearithmic
```cpp
sort(arr, arr + n);  // merge sort, quick sort (avg)
```

### O(N²) — Quadratic
```cpp
for(int i = 0; i < n; i++)
    for(int j = 0; j < n; j++)
        cout << i << j;  // nested loop = N × N
```

### O(2^N) — Exponential
```cpp
// Recursive subsets / fibonacci without memo
int fib(int n) {
    if(n <= 1) return n;
    return fib(n-1) + fib(n-2);  // branches into 2 each call
}
```

---

## 📱 The 10⁸ Rule (Constraint Trick)

> **1 second ≈ 10⁸ operations**

| Constraint (N) | Max Allowed Complexity | Approach Hint |
|----------------|----------------------|---------------|
| N ≤ 10 | O(N!) or O(2^N) | brute force / backtracking |
| N ≤ 20 | O(2^N) | bitmask / recursion |
| N ≤ 100 | O(N³) | triple nested loops |
| N ≤ 1000 | O(N²) | nested loops |
| N ≤ 10⁵ | O(N log N) | sorting / binary search |
| N ≤ 10⁶ | O(N) | single pass / hashing |
| N ≤ 10⁸ | O(log N) or O(1) | math / binary search |

⚡ **This table = your first hint when reading constraints!**

---

## 🗄️ Space Complexity

> **Extra memory your algorithm uses** (excluding the input itself)

### Auxiliary Space vs Total Space
- **Auxiliary Space** = extra space used (what we usually care about)
- **Total Space** = input space + auxiliary space

### Examples:
```cpp
// O(1) space — no extra memory
int sum = 0;
for(int i = 0; i < n; i++) sum += arr[i];

// O(N) space — creating new array
int temp[n];
for(int i = 0; i < n; i++) temp[i] = arr[n-1-i];

// O(N) space — recursion call stack
void print(int n) {
    if(n == 0) return;
    print(n-1);  // N calls stacked in memory
}
```

---

## 🎯 Quick Complexity Identification Cheat

| Pattern | Time | Space |
|---------|------|-------|
| Simple formula / math | O(1) | O(1) |
| Single loop (0 to N) | O(N) | O(1) |
| Two nested loops | O(N²) | O(1) |
| Loop halving each time | O(log N) | O(1) |
| Sorting | O(N log N) | O(N) or O(1) |
| Recursion with 2 branches | O(2^N) | O(N) stack |
| Visiting all subsets | O(2^N) | O(N) |
| Visiting all permutations | O(N!) | O(N) |

---

## ⚡ Best/Average/Worst Case

```
         Best Case    Average Case    Worst Case
         (Omega Ω)      (Theta Θ)       (Big O)
              ↓              ↓              ↓
         minimum ops    expected ops    maximum ops

Example: Linear Search for element X in array
  Best:    O(1)    → found at index 0
  Average: O(N/2)  → found somewhere in middle → O(N)
  Worst:   O(N)    → found at last index or not found
```

> 🎯 **In interviews, always analyze WORST CASE unless asked otherwise.**

---

## ❌ Common Mistakes

1. **Saying O(2N) instead of O(N)** → drop the constant!
2. **Confusing O(N²) with O(N+M)** → different inputs = different variables
3. **Ignoring recursion stack space** → each recursive call uses memory
4. **Thinking sorting is O(N)** → comparison-based sorting minimum is O(N log N)

---

## 📋 Quick Revision

```
┌─────────────────────────────────────────────────────────┐
│         TIME & SPACE CHEAT SHEET                        │
├─────────────────────────────────────────────────────────┤
│ ✅ Time = how operations grow with input                │
│ ✅ Space = extra memory used                            │
│ ✅ Drop constants: O(3N) → O(N)                        │
│ ✅ Drop lower terms: O(N² + N) → O(N²)                 │
│ ✅ Different inputs → different variables               │
│ ✅ 10⁸ operations ≈ 1 second                           │
│ ✅ Always think WORST CASE                              │
│ ✅ Recursion uses stack space = O(depth)                │
│ ✅ Binary search = O(log N) because halving             │
│ ✅ Nested loop = multiply: O(N × M)                    │
└─────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [C++ Basics →](../dsa-notes/cpp-basics.md)
- [Arrays & Hashing →](../dsa-notes/arrays.md)
- [Sorting & Searching →](../dsa-notes/sorting-searching.md)
