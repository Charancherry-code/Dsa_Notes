# 🔄 Introduction to Recursion

> *Striver A2Z DSA Course — Step 2.1*
> 💡 **"A function that calls itself until a specified condition (base case) is met."**

---

## 🧠 What is Recursion?

> A function **calls itself** repeatedly until a **base condition** stops it.

```cpp
void f() {
    // ... do something
    f();  // calls itself again
}
```

**Three things to always identify:**
1. **Base case** — when to STOP
2. **Recursive call** — function calling itself
3. **Work done per call** — what happens each time

---

## 🌳 Recursion Tree

> Visual representation of how function calls branch out.

```
main() → f() → f() → f() → f() [base hit → RETURN]
                                      ↩️
                               ↩️
                        ↩️
                 ↩️
```

Instead of writing full function each time, represent as:
```
f() ──→ f() ──→ f() ──→ f() ──→ RETURN
                                    ↩️ back
                             ↩️ back
                      ↩️ back
               ↩️ back
```

---

## 📚 Stack Space (Call Stack)

> Every function call that hasn't completed yet **waits in memory** (stack).

```
┌───────────────┐
│   f() call 4  │ ← currently executing (hits base, returns)
├───────────────┤
│   f() call 3  │ ← waiting (line not complete)
├───────────────┤
│   f() call 2  │ ← waiting
├───────────────┤
│   f() call 1  │ ← waiting
├───────────────┤
│     main()    │ ← waiting
└───────────────┘
     STACK
```

- Each recursive call **pushes** onto stack
- When base case hit → function **returns** → pops off stack
- Previous calls resume and complete one by one

---

## 💥 Stack Overflow

> When there's **no base case** → infinite recursion → stack fills up → **CRASH**

```cpp
void f() {
    cout << 1;
    f();  // ❌ no stopping condition!
}
// Prints 1 forever until SEGMENTATION FAULT
```

⚠️ **Stack overflow** = too many function calls waiting in memory without returning.

---

## 🛑 Base Condition

> The condition where recursion **STOPS** — no more self-calls.

```cpp
int count = 0;

void f() {
    if(count == 3) return;  // 🛑 BASE CASE — stop here!
    cout << count << endl;
    count++;
    f();  // recursive call
}

// Output: 0 1 2
```

### How it works:
```
f() → count=0, print 0, count++, call f()
  f() → count=1, print 1, count++, call f()
    f() → count=2, print 2, count++, call f()
      f() → count=3, BASE HIT → return ↩️
    ↩️ return
  ↩️ return
↩️ return
```

---

## ⚙️ Better Way — Pass Parameters

> Instead of global variables, pass state as arguments.

```cpp
void f(int count) {
    if(count == 3) return;    // base case
    cout << count << endl;    // work
    f(count + 1);             // recursive call with updated state
}

// Call: f(0)
// Output: 0 1 2
```

💡 **Pattern:** `f(i)` does work, then calls `f(i+1)`.

---

## 📐 Anatomy of Every Recursive Function

```cpp
returnType f(parameters) {
    // 1. BASE CASE — when to stop
    if(condition) return something;
    
    // 2. WORK — what to do this call
    // ... some logic ...
    
    // 3. RECURSIVE CALL — call self with smaller/changed input
    f(modified_parameters);
}
```

**Always ask yourself:**
1. What's the **smallest** version of this problem? → Base case
2. What do I do **each step**? → Work
3. How do I **reduce** the problem? → Recursive call

---

## ⏱️ Time & Space Complexity of Recursion

| Aspect | Value |
|--------|-------|
| **Time** | O(number of calls) |
| **Space** | O(depth of recursion) — stack space |

For printing 0 to N:
- Time: O(N) — N calls made
- Space: O(N) — N calls in stack simultaneously

---

## 🎯 Patterns — When to Use Recursion

| You see this... | Think recursion |
|----------------|-----------------|
| "Do something N times" | f(i) → f(i+1) till base |
| "Break problem into smaller subproblems" | Classic recursion |
| "Tree/Graph traversal" | DFS = recursion |
| "All subsets / permutations / combinations" | Backtracking (recursion) |
| "Try all possibilities" | Recursion + pruning |
| "Divide and conquer" | Merge sort, quick sort |
| "Dynamic Programming" | Recursion + memoization |

---

## ❌ Common Mistakes

1. **Forgetting base case** → infinite recursion → stack overflow → crash.

2. **Base case never reached** — make sure parameters actually change toward the base case each call.

3. **Modifying global variables carelessly** — prefer passing parameters instead of relying on globals.

4. **Not understanding stack space = O(depth)** — recursion uses memory! N calls = O(N) extra space.

5. **Confusing iteration with recursion** — anything recursive can be iterative, but recursion is cleaner for trees, graphs, backtracking.

6. **Not trusting the recursion** — beginners try to trace every single call. Trust that f(smaller) gives correct answer, build on it.

---

## 🏢 Interview Questions (FAANG)

### Q1: What is recursion? (Theory — asked everywhere)
> A function that calls itself with a smaller input until a base condition is met. Uses call stack for memory.

### Q2: What happens if there's no base case?
> Infinite recursion → stack overflow → program crashes (segmentation fault).

### Q3: What is stack overflow vs stack space?
> **Stack space** = memory where pending function calls wait.
> **Stack overflow** = stack runs out of memory due to too many calls.

### Q4: Print 1 to N using recursion (Amazon, Microsoft)
```cpp
void print(int i, int n) {
    if(i > n) return;       // base case
    cout << i << " ";       // work
    print(i + 1, n);        // recursive call
}
// Call: print(1, 5) → Output: 1 2 3 4 5
```

### Q5: Print N to 1 using recursion
```cpp
void print(int n) {
    if(n == 0) return;      // base case
    cout << n << " ";       // work BEFORE recursive call
    print(n - 1);           // go smaller
}
// Call: print(5) → Output: 5 4 3 2 1
```

### Q6: Sum of first N numbers (Google)
```cpp
int sum(int n) {
    if(n == 0) return 0;        // base
    return n + sum(n - 1);      // n + rest
}
// sum(5) = 5 + 4 + 3 + 2 + 1 + 0 = 15
```

### Q7: Factorial of N
```cpp
int factorial(int n) {
    if(n <= 1) return 1;        // base: 0! = 1! = 1
    return n * factorial(n - 1);
}
// factorial(5) = 5 * 4 * 3 * 2 * 1 = 120
```

### Q8: Fibonacci Number (Amazon, Meta)
```cpp
int fib(int n) {
    if(n <= 1) return n;    // base: fib(0)=0, fib(1)=1
    return fib(n-1) + fib(n-2);
}
// ⚠️ TC: O(2^N) — exponential! Use DP for optimization.
```

### Q9: Explain recursion vs iteration tradeoff
> **Recursion:** cleaner code, uses stack space O(N), risk of overflow.
> **Iteration:** more verbose, O(1) space, no overflow risk.
> Use recursion for trees/graphs/backtracking. Use iteration for simple loops.

---

## 📋 Quick Revision

```
┌──────────────────────────────────────────────────────────┐
│            RECURSION INTRO CHEAT SHEET                   │
├──────────────────────────────────────────────────────────┤
│ ✅ Recursion = function calls itself                     │
│ ✅ Base case = STOP condition (prevents infinite)        │
│ ✅ Stack space = where pending calls wait (O(depth))     │
│ ✅ Stack overflow = no base case → crash                 │
│ ✅ Recursion tree = visual of call chain                 │
│ ✅ Every call: base case → work → recursive call         │
│ ✅ Trust the recursion — don't trace every call          │
│ ✅ TC = O(num calls) | SC = O(depth)                    │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [Basic Maths →](./basic-maths.md)
- [C++ STL →](./cpp-stl.md)
- [Time & Space Complexity →](./time-space-complexity.md)


---

## 🔥 Recursion Problems (Lecture 2)

### Problem 1: Print Name N Times

```cpp
void f(int i, int n) {
    if(i > n) return;       // base case
    cout << "Raj" << endl;  // work
    f(i + 1, n);            // move forward
}
// Call: f(1, 3) → prints Raj 3 times
```

**TC: O(N)** | **SC: O(N)** stack

---

### Problem 2: Print 1 to N

```cpp
void f(int i, int n) {
    if(i > n) return;
    cout << i << " ";   // print BEFORE call
    f(i + 1, n);
}
// Call: f(1, 4) → Output: 1 2 3 4
```

---

### Problem 3: Print N to 1

```cpp
void f(int i, int n) {
    if(i < 1) return;
    cout << i << " ";   // print BEFORE call
    f(i - 1, n);        // go backwards
}
// Call: f(3, 3) → Output: 3 2 1
```

---

### Problem 4: Print 1 to N using BACKTRACKING (no +1)

> 💡 **Key Insight:** Print AFTER the recursive call — the deepest call prints first!

```cpp
void f(int i, int n) {
    if(i < 1) return;
    f(i - 1, n);        // go deep first
    cout << i << " ";   // print AFTER call (backtracking)
}
// Call: f(3, 3) → Output: 1 2 3
```

**How it works:**
```
f(3) → calls f(2) → calls f(1) → calls f(0) → BASE, return
       ↩️ f(1) prints 1
  ↩️ f(2) prints 2
↩️ f(3) prints 3

Output: 1 2 3 (even though we started from 3!)
```

---

### Problem 5: Print N to 1 using BACKTRACKING (no -1)

> Same idea — print AFTER call, but go forward.

```cpp
void f(int i, int n) {
    if(i > n) return;
    f(i + 1, n);        // go deep first
    cout << i << " ";   // print AFTER call
}
// Call: f(1, 3) → Output: 3 2 1
```

---

## 🧩 Backtracking Concept

```
┌─────────────────────────────────────────────────┐
│  PRINT BEFORE CALL        PRINT AFTER CALL      │
│  (normal)                 (backtracking)         │
│                                                   │
│  f(i) {                   f(i) {                 │
│    print(i);  ← HERE        f(i-1);             │
│    f(i+1);                   print(i); ← HERE   │
│  }                        }                      │
│                                                   │
│  Prints: 1 2 3            Prints: 1 2 3         │
│  (going forward)          (coming BACK)          │
└─────────────────────────────────────────────────┘
```

**Rule:**
- Code **before** recursive call → executes while **going deeper**
- Code **after** recursive call → executes while **coming back** (backtracking)

---

## 🎯 Pattern Recognition

| Want to... | Approach |
|-----------|----------|
| Print 1 to N | start i=1, print, call f(i+1) |
| Print N to 1 | start i=N, print, call f(i-1) |
| Print 1 to N (backtrack) | start i=N, call f(i-1), THEN print |
| Print N to 1 (backtrack) | start i=1, call f(i+1), THEN print |

💡 **Backtracking = do work AFTER the recursive call returns**

---

## ❌ Common Mistakes (Problems)

1. **Wrong base case direction** — if going i+1, check `i > n`. If going i-1, check `i < 1`.

2. **Printing before vs after call** — this completely changes output order. Understand WHY.

3. **Not passing parameters** — use `f(i+1, n)` instead of relying on global variables. Cleaner and safer.

4. **Forgetting TC/SC** — always state: TC = O(N), SC = O(N) for linear recursion.

---

## 🏢 Interview Follow-ups

### "Print 1 to N without loop or +1 in recursion"
> Use backtracking: go from N to 0, print AFTER call.

### "What's the difference between work before vs after recursive call?"
> Before = executes going DOWN the tree. After = executes coming BACK UP (backtracking).

### "Can you convert this recursion to iteration?"
> Yes, any linear recursion → simple for loop. But backtracking pattern shows how recursion handles "coming back" naturally.

### "What if N = 10⁶? Will recursion work?"
> Stack overflow risk! Each call uses stack memory. For large N, prefer iteration or increase stack size.


---

## 🎭 Parameterised vs Functional Recursion (Lecture 3)

> Two ways to solve the same problem. Learn BOTH patterns — they appear everywhere in DP later.

---

### 🅰️ Parameterised Recursion

> **Carry the answer in a parameter.** Print/use it at base case.

```cpp
void f(int i, int sum) {
    if(i < 1) {
        cout << sum;    // answer built up in parameter
        return;
    }
    f(i - 1, sum + i);  // carry sum forward
}
// Call: f(3, 0)
```

**Dry Run:**
```
f(3, 0) → f(2, 0+3=3) → f(1, 3+2=5) → f(0, 5+1=6)
                                           ↑ base hit! print 6
```

💡 **Pattern:** answer accumulates in parameter, print at base.

---

### 🅱️ Functional Recursion

> **Function RETURNS the answer.** Build answer from return values.

```cpp
int f(int n) {
    if(n == 0) return 0;       // base: sum of 0 numbers = 0
    return n + f(n - 1);       // n + sum of rest
}
// Call: f(3) → returns 6
```

**Dry Run:**
```
f(3) = 3 + f(2)
           f(2) = 2 + f(1)
                      f(1) = 1 + f(0)
                                 f(0) = 0 ← base
                             = 1 + 0 = 1
                  = 2 + 1 = 3
       = 3 + 3 = 6 ✅
```

💡 **Pattern:** break into `current + f(smaller)`. Base returns identity.

---

### Factorial using Functional Recursion

```cpp
int factorial(int n) {
    if(n == 0) return 1;       // ⚠️ NOT 0! (0! = 1)
    return n * factorial(n - 1);
}
// factorial(4) = 4 * 3 * 2 * 1 * 1 = 24
```

⚠️ **Base case for multiplication = return 1** (not 0, or everything becomes 0!)

---

### 🆚 Comparison

```
┌──────────────────────────────────────────────────────────┐
│  PARAMETERISED              FUNCTIONAL                    │
│                                                           │
│  void f(i, sum)            int f(n)                      │
│  carry answer in param     function RETURNS answer       │
│  print at base case        build from return values      │
│  f(i-1, sum+i)             return n + f(n-1)            │
│                                                           │
│  Use when: printing        Use when: need return value   │
│  answer at the end         (DP, memoization)             │
└──────────────────────────────────────────────────────────┘
```

---

### 🎯 Pattern: Choosing Base Case for Return

| Operation | Base case returns |
|-----------|------------------|
| **Sum** (addition) | 0 (identity for +) |
| **Product** (multiplication) | 1 (identity for ×) |
| **Min** | INT_MAX |
| **Max** | INT_MIN |
| **Count** | 0 |
| **Boolean OR** | false |
| **Boolean AND** | true |

💡 **Return the IDENTITY element** of the operation at base case!

---

### ❌ Common Mistakes

1. **Returning 0 for factorial** — 0! = 1, not 0. If base returns 0, everything × 0 = 0.

2. **Confusing when to use which** — Parameterised: for printing. Functional: when you need return value (DP uses functional).

3. **Not reducing problem** — `f(n-1)` must move toward base. If you forget `n-1`, infinite loop.

4. **Both have same TC/SC** — O(N) time, O(N) stack space. Functional is NOT slower.

---

### 🏢 Interview: "Sum of N without loop, formula, or + in recursion"

```cpp
// Using subtraction + backtracking concept
int sum(int n) {
    if(n == 0) return 0;
    return n + sum(n-1);  // functional way
}
// Interviewer follow-up: "What's TC?" → O(N)
// "Can you do O(1)?" → n*(n+1)/2 formula
```

**TC: O(N)** | **SC: O(N)** for both approaches.


---

## 🔁 Functional Recursion Problems (Lecture 4)

---

### Reverse an Array (Two Pointer Recursion)

> Swap left and right, then recurse on the inner portion.

#### Approach 1: Two Pointers (l, r)

```cpp
void f(int l, int r, int arr[]) {
    if(l >= r) return;           // base: crossed or met
    swap(arr[l], arr[r]);        // task: swap ends
    f(l + 1, r - 1, arr);       // recurse: move inward
}
// Call: f(0, n-1, arr)
```

#### Approach 2: Single Pointer (i only)

> Corresponding index = `n - i - 1`

```cpp
void f(int i, int arr[], int n) {
    if(i >= n / 2) return;               // base: crossed middle
    swap(arr[i], arr[n - i - 1]);        // task: swap mirror elements
    f(i + 1, arr, n);                    // recurse: next index
}
// Call: f(0, arr, n)
```

**Dry Run (arr = {1, 3, 5, 4, 2}):**
```
f(0): swap arr[0] & arr[4] → {2, 3, 5, 4, 1}
f(1): swap arr[1] & arr[3] → {2, 4, 5, 3, 1}
f(2): 2 >= 5/2 → return (middle, no swap needed)
Result: {2, 4, 5, 3, 1}
```

**TC: O(N/2) ≈ O(N)** | **SC: O(N/2)** stack

---

### Check Palindrome (Functional Recursion)

> Instead of swapping, CHECK if mirror elements are equal.

```cpp
bool f(int i, string &s) {
    if(i >= s.size() / 2) return true;        // checked all — palindrome!
    if(s[i] != s[s.size() - i - 1]) return false;  // mismatch → NOT palindrome
    return f(i + 1, s);                       // check next pair
}
// Call: f(0, s)
```

**How it works:**
```
"madam" → f(0): m==m ✅ → f(1): a==a ✅ → f(2): 2>=5/2 → return true ✅

"match" → f(0): m==h ❌ → return false immediately!
```

💡 **Key insight:** Same logic as reverse, but **check equality** instead of swap. If any pair fails → immediately return false. If all pass → true.

**TC: O(N/2)** | **SC: O(N/2)** stack

---

### 🎯 Pattern: Recursion on Two Ends

| Problem | Task per call | Base case |
|---------|---------------|-----------|
| Reverse array | swap(arr[l], arr[r]) | l >= r |
| Check palindrome | compare s[i] vs s[n-i-1] | i >= n/2 |
| Two-pointer problems | depends on problem | pointers cross |

**Template:**
```cpp
returnType f(int i, ...) {
    if(i >= n/2) return baseValue;         // done
    // do task with arr[i] and arr[n-i-1]
    return f(i + 1, ...);                  // next pair
}
```

---

### ❌ Common Mistakes

1. **Off-by-one in mirror index** — it's `n - i - 1`, NOT `n - i`. Easy to mess up.

2. **Base case: >= not just >** — for even length, pointers meet. For odd, they cross. `>=` handles both.

3. **Not passing array by reference** — arrays in C++ pass by reference implicitly, but strings need `&`.

4. **Returning wrong thing** — palindrome must return `f(i+1, s)` not just `true`. "All remaining must also be palindrome."

---

### 🏢 Interview: Reverse String In-Place (LeetCode 344) — Amazon, Google

```cpp
void reverseString(vector<char>& s) {
    // Can do iteratively, but recursive shows concept:
    reverse(0, s.size() - 1, s);
}

void reverse(int l, int r, vector<char>& s) {
    if(l >= r) return;
    swap(s[l], s[r]);
    reverse(l + 1, r - 1, s);
}
```

### 🏢 Interview: Valid Palindrome (LeetCode 125) — Meta, Microsoft

```cpp
bool isPalindrome(string s) {
    // clean string first (remove non-alphanumeric, lowercase)
    string clean = "";
    for(char c : s)
        if(isalnum(c)) clean += tolower(c);
    return check(0, clean);
}

bool check(int i, string &s) {
    if(i >= s.size() / 2) return true;
    if(s[i] != s[s.size() - i - 1]) return false;
    return check(i + 1, s);
}
// ⚠️ In interview: mention iterative two-pointer is O(1) space vs O(N) for recursion
```


---

## 🌿 Multiple Recursion Calls (Lecture 5)

> Till now: ONE recursive call per function. Now: MULTIPLE calls. This is how trees branch.

---

### Fibonacci — The Classic Multiple Call Example

```
Fibonacci: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34...
           f(0) f(1) f(2) f(3) f(4) f(5) ...

Rule: f(n) = f(n-1) + f(n-2)
Base: f(0) = 0, f(1) = 1
```

```cpp
int fib(int n) {
    if(n <= 1) return n;                   // base: f(0)=0, f(1)=1
    int last = fib(n - 1);                 // first call goes FULLY
    int secondLast = fib(n - 2);           // then second call goes
    return last + secondLast;
}
```

---

### 🌳 Recursion Tree for fib(4)

```
                    fib(4)
                   /      \
              fib(3)      fib(2)
             /     \       /    \
         fib(2)  fib(1)  fib(1) fib(0)
         /    \     |       |      |
     fib(1) fib(0)  1       1      0
        |      |
        1      0
```

**Execution order (left goes FIRST, fully completes, then right):**
```
fib(4) → calls fib(3) first
  fib(3) → calls fib(2) first
    fib(2) → calls fib(1) → returns 1
           → calls fib(0) → returns 0
           → returns 1+0 = 1
  fib(3) → now calls fib(1) → returns 1
         → returns 1+1 = 2
fib(4) → now calls fib(2)
  fib(2) → calls fib(1) → returns 1
         → calls fib(0) → returns 0
         → returns 1+0 = 1
fib(4) → returns 2+1 = 3 ✅
```

---

### ⚡ Key Rule: Multiple Calls Execution Order

```
┌──────────────────────────────────────────────────────────┐
│  f() {                                                    │
│      x = f(n-1);   ← goes FULLY first (all the way down)│
│      y = f(n-2);   ← WAITS until first call returns      │
│      return x + y;                                        │
│  }                                                        │
│                                                           │
│  FIRST call completes entirely → THEN second call starts │
└──────────────────────────────────────────────────────────┘
```

💡 **NOT simultaneous!** First call goes all the way down to base, returns, THEN second call starts.

---

### ⏱️ Time Complexity: O(2^N) — Exponential!

**Why?**
- Each call makes 2 more calls
- Tree branches: 2 × 2 × 2 × ... ≈ 2^N nodes

```
Level 0:  1 call
Level 1:  2 calls
Level 2:  4 calls
Level 3:  8 calls
...
Level N:  2^N calls
```

**Space: O(N)** — max depth of recursion tree (stack holds one branch at a time)

⚠️ **fib(40) = ~1 billion calls. WAY too slow!** → Use DP to fix this.

---

### 🆚 Single vs Multiple Recursion Calls

| | Single Call | Multiple Calls |
|--|------------|----------------|
| **Pattern** | f() calls f() once | f() calls f() twice+ |
| **Tree shape** | linear chain | branching tree |
| **TC** | O(N) | O(2^N) exponential |
| **SC** | O(N) | O(N) (depth of tree) |
| **Examples** | factorial, print 1-N | fibonacci, merge sort, trees |

---

### 🎯 Pattern: When You'll See Multiple Calls

| Problem Type | # of Calls | Example |
|-------------|------------|---------|
| Fibonacci | 2 calls | f(n-1) + f(n-2) |
| Binary tree traversal | 2 calls | left + right |
| Merge sort | 2 calls | sort left + sort right |
| N-Queens | N calls | try each column |
| Subsets/Subsequences | 2 calls | pick or not pick |
| Permutations | N calls | try each position |

💡 **Multiple calls = branching = exponential unless optimized (DP/pruning)**

---

### ❌ Common Mistakes

1. **Thinking both calls go simultaneously** — NO. First call completes FULLY, then second starts.

2. **Not realizing it's exponential** — fib(50) has ~10^15 calls. Always mention TC in interviews.

3. **Forgetting this can be optimized** — naive fibonacci = O(2^N). With memoization = O(N). Interviewers expect you to know both.

4. **Wrong base case** — fib needs TWO base cases: f(0)=0 AND f(1)=1. Missing one causes infinite recursion.

5. **Confusing SC** — even though 2^N total calls happen, stack only holds ONE branch at a time = O(N) depth.

---

### 🏢 Interview: Fibonacci (LeetCode 509) — Everyone asks this

```cpp
// ❌ Naive — O(2^N) — too slow for large N
int fib(int n) {
    if(n <= 1) return n;
    return fib(n-1) + fib(n-2);
}

// ✅ Memoized — O(N) — what interviewer wants to see
int fib(int n, vector<int>& dp) {
    if(n <= 1) return n;
    if(dp[n] != -1) return dp[n];         // already computed!
    return dp[n] = fib(n-1, dp) + fib(n-2, dp);
}

// ✅ Iterative — O(N) time, O(1) space — best
int fib(int n) {
    if(n <= 1) return n;
    int prev2 = 0, prev1 = 1;
    for(int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

**Interview flow:**
1. Start with recursive → show understanding
2. Mention "this is O(2^N), too slow"
3. Optimize with memoization → O(N) time, O(N) space
4. Further optimize iterative → O(N) time, O(1) space
