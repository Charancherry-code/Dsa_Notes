# 🏗️ C++ Basics for DSA

> *Striver A2Z DSA Sheet — One Shot Prerequisite*
> 💡 **"Don't dig too deep at the start. Learn basics → solve problems → deepen later."**

---

## 🧠 Program Structure

```cpp
#include <bits/stdc++.h>  // ⚡ includes EVERYTHING
using namespace std;       // 🚫 no need to write std:: everywhere

int main() {
    // your code here
    return 0;
}
```

| Part | Why? |
|------|------|
| `#include <iostream>` | for cin/cout |
| `#include <bits/stdc++.h>` | 🔥 shortcut — includes ALL headers |
| `using namespace std` | skip writing `std::` every time |
| `int main()` | entry point of program |

---

## 📦 Data Types

| Type | Size | Use Case | Example |
|------|------|----------|---------|
| `int` | 4 bytes | numbers up to ~2×10⁹ | `int x = 10;` |
| `long long` | 8 bytes | numbers > 10⁹ | `long long n = 1e18;` |
| `double` | 8 bytes | decimals | `double pi = 3.14;` |
| `char` | 1 byte | single character | `char c = 'A';` |
| `string` | varies | text | `string s = "hello";` |

⚡ **When to use what:**
- **int** → default for most problems
- **long long** → when constraints say N ≤ 10¹⁸ or multiplication overflow possible
- **double** → geometry, precision problems

---

## 🔀 Control Flow

### If-Else

```cpp
if (age >= 18) {
    cout << "Adult";
} else if (age >= 13) {
    cout << "Teen";
} else {
    cout << "Child";
}
```

🎯 **Pattern:** condition → action → else fallback

### Switch Statement

```cpp
switch (day) {
    case 1: cout << "Mon"; break;  // ⚠️ break is CRUCIAL
    case 2: cout << "Tue"; break;
    default: cout << "Other";      // 🛡️ safety net
}
```

❌ **Common Mistake:** forgetting `break` → falls through to next case!

---

## 📦 Arrays

### 1D Array
```cpp
int arr[5] = {10, 20, 30, 40, 50};
//            [0] [1] [2] [3] [4]  ← ZERO-BASED indexing!

cout << arr[0];  // 10 (first element)
cout << arr[4];  // 50 (last element)
```

### 2D Array
```cpp
int mat[3][3] = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
cout << mat[1][2];  // 6 → row 1, col 2
```

⚡ **Key Points:**
- Zero-based indexing → first element is `arr[0]`
- Stored in **consecutive memory** locations
- Arrays are **passed by reference** to functions (implicitly!)

---

## 🔤 Strings

```cpp
string name = "Striver";

cout << name[0];       // 'S' → access by index
cout << name.length(); // 7

// input
string s;
cin >> s;             // reads one word
getline(cin, s);      // reads full line (with spaces)
```

---

## 🔁 Loops

### For Loop (most common in DSA)
```cpp
for (int i = 0; i < n; i++) {
    cout << arr[i] << " ";
}
```

### While Loop
```cpp
int i = 0;
while (i < n) {
    cout << i;
    i++;
}
```

### Do-While Loop
```cpp
int i = 0;
do {
    cout << i;
    i++;
} while (i < n);  // ⚡ runs AT LEAST once
```

🎯 **When to use:**
| Loop | Best for |
|------|----------|
| `for` | known number of iterations |
| `while` | unknown iterations, condition-based |
| `do-while` | must run at least once (rare in DSA) |

---

## ⚙️ Functions

### Why?
- 📌 **Modularity** — break code into chunks
- 📌 **Reusability** — write once, call many times
- 📌 **Readability** — clean, understandable code

### Basic Function
```cpp
int add(int a, int b) {
    return a + b;
}

int main() {
    cout << add(3, 5);  // 8
}
```

### 🔥 Pass by Value vs Pass by Reference

```
┌─────────────────────────────────────────────────┐
│  PASS BY VALUE          PASS BY REFERENCE        │
│                                                   │
│  void f(int x)         void f(int &x)           │
│       ↓                      ↓                   │
│  gets a COPY            gets the ORIGINAL        │
│  original UNCHANGED     original MODIFIED        │
└─────────────────────────────────────────────────┘
```

```cpp
// ❌ Pass by Value — original NOT changed
void increment(int x) {
    x++;  // only local copy changes
}

// ✅ Pass by Reference — original IS changed
void increment(int &x) {
    x++;  // actual variable changes
}

int main() {
    int a = 5;
    increment(a);  // with &: a becomes 6
                   // without &: a stays 5
}
```

⚡ **Arrays are ALWAYS passed by reference** (no & needed)
```cpp
void modify(int arr[], int n) {
    arr[0] = 99;  // ✅ this modifies original array!
}
```

---

## 📋 Quick Revision Cheat Sheet

```
┌────────────────────────────────────────────┐
│           C++ BASICS CHECKLIST              │
├────────────────────────────────────────────┤
│ ✅ bits/stdc++.h → includes everything     │
│ ✅ int for normal, long long for big       │
│ ✅ if/else → conditions                    │
│ ✅ switch → multiple fixed values          │
│ ✅ arrays → 0-indexed, contiguous memory   │
│ ✅ for loop → known iterations             │
│ ✅ while → condition-based                 │
│ ✅ functions → modular, reusable           │
│ ✅ & → pass by reference (modifies orig)   │
│ ✅ arrays always passed by reference       │
└────────────────────────────────────────────┘
```

---

## 🔗 Related
- [Arrays & Hashing →](./arrays.md)
- [Strings →](./strings.md)
- [Sorting & Searching →](./sorting-searching.md)
