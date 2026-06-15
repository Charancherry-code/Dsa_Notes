# 🔢 Basic Maths for DSA

> *Striver A2Z DSA Course — Step 1.4*
> 💡 **"If you know how to extract digits, you can solve most basic maths problems."**

---

## 🧠 Digit Extraction (Core Concept)

> **Get every digit from a number using % 10 and / 10**

```cpp
int n = 7789;
while(n > 0) {
    int lastDigit = n % 10;  // extracts last digit
    n = n / 10;              // removes last digit
}
```

### Dry Run:
```
n = 7789 → lastDigit = 9, n becomes 778
n = 778  → lastDigit = 8, n becomes 77
n = 77   → lastDigit = 7, n becomes 7
n = 7    → lastDigit = 7, n becomes 0
→ STOP (n == 0)
```

⚡ **Extraction happens in REVERSE order** (9, 8, 7, 7)

### ⏱️ Time Complexity: O(log₁₀ N)
> Number of digits = number of times divisible by 10

---

## 1️⃣ Count Digits

> Given N, return number of digits.

```cpp
int countDigits(int n) {
    int count = 0;
    while(n > 0) {
        count++;
        n = n / 10;
    }
    return count;
}
```

**One-liner approach:**
```cpp
int count = (int)log10(n) + 1;
```

⏱️ **TC: O(log₁₀ N)** | SC: O(1)

---

## 2️⃣ Reverse a Number

> Given 7789 → return 9877

**Key formula:**
```
reverseNum = reverseNum × 10 + lastDigit
```

```cpp
int reverse(int n) {
    int rev = 0;
    while(n > 0) {
        int lastDigit = n % 10;
        rev = rev * 10 + lastDigit;
        n = n / 10;
    }
    return rev;
}
```

### Dry Run:
```
n=7789, rev=0   → rev = 0*10 + 9  = 9,    n=778
n=778,  rev=9   → rev = 9*10 + 8  = 98,   n=77
n=77,   rev=98  → rev = 98*10 + 7 = 987,  n=7
n=7,    rev=987 → rev = 987*10 + 7= 9877, n=0
```

⏱️ **TC: O(log₁₀ N)** | SC: O(1)

---

## 3️⃣ Check Palindrome

> A number whose reverse == itself. (121, 1331, 7, 11)

```cpp
bool isPalindrome(int n) {
    int duplicate = n;  // ⚠️ store copy! n becomes 0 after loop
    int rev = 0;
    while(n > 0) {
        rev = rev * 10 + (n % 10);
        n /= 10;
    }
    return duplicate == rev;
}
```

⚠️ **Must store copy of N** — original N becomes 0 after extraction!

⏱️ **TC: O(log₁₀ N)** | SC: O(1)

---

## 4️⃣ Armstrong Number

> Sum of cubes of digits == number itself.
> 371 → 3³ + 7³ + 1³ = 27 + 343 + 1 = 371 ✅

```cpp
bool isArmstrong(int n) {
    int duplicate = n;
    int sum = 0;
    while(n > 0) {
        int lastDigit = n % 10;
        sum += lastDigit * lastDigit * lastDigit;
        n /= 10;
    }
    return sum == duplicate;
}
```

⏱️ **TC: O(log₁₀ N)** | SC: O(1)

---

## 5️⃣ Print All Divisors

> Given 36, print: 1 2 3 4 6 9 12 18 36

### Brute Force — O(N)
```cpp
for(int i = 1; i <= n; i++) {
    if(n % i == 0) cout << i << " ";
}
```

### Optimized — O(√N)
> If `i` is a factor, then `n/i` is also a factor.
> All factor pairs exist within √N.

```cpp
vector<int> divisors;
for(int i = 1; i * i <= n; i++) {
    if(n % i == 0) {
        divisors.push_back(i);
        if(n / i != i)           // avoid duplicate (e.g., 6×6=36)
            divisors.push_back(n / i);
    }
}
sort(divisors.begin(), divisors.end());
```

⏱️ **TC: O(√N) + O(k log k)** where k = number of factors

---

## 6️⃣ Check Prime

> A number with **exactly 2 factors** (1 and itself).
> ❌ Wrong definition: "divisible by 1 and itself" (makes 1 prime)
> ✅ Right: "has EXACTLY two factors"

### Brute Force — O(N)
```cpp
int count = 0;
for(int i = 1; i <= n; i++)
    if(n % i == 0) count++;
return count == 2;
```

### Optimized — O(√N)
```cpp
bool isPrime(int n) {
    if(n <= 1) return false;
    int count = 0;
    for(int i = 1; i * i <= n; i++) {
        if(n % i == 0) {
            count++;
            if(n / i != i) count++;
        }
    }
    return count == 2;
}
```

⏱️ **TC: O(√N)**

---

## 7️⃣ GCD / HCF (Euclidean Algorithm)

> **Greatest Common Divisor** — largest number that divides both.
> GCD(9, 12) = 3 | GCD(20, 40) = 20 | GCD(11, 13) = 1

### Brute Force — O(min(a, b))
```cpp
int gcd = 1;
for(int i = 1; i <= min(a, b); i++) {
    if(a % i == 0 && b % i == 0)
        gcd = i;
}
```

### Euclidean Algorithm — O(log(min(a,b)))

**Concept:**
```
GCD(a, b) = GCD(a % b, b)    where a > b
Keep going until one becomes 0.
The other number = GCD.
```

**Example:**
```
GCD(52, 10)
→ GCD(52%10, 10) = GCD(2, 10)
→ GCD(10%2, 2)  = GCD(0, 2)
→ one is 0, other is 2
→ GCD = 2 ✅
```

```cpp
int gcd(int a, int b) {
    while(a > 0 && b > 0) {
        if(a > b) a = a % b;
        else      b = b % a;
    }
    return a == 0 ? b : a;
}
```

⏱️ **TC: O(log_φ(min(a,b)))** — φ (phi) ≈ 1.618

💡 **Key insight:** Division → logarithmic time complexity. Always.

---

## 📋 Quick Revision

```
┌──────────────────────────────────────────────────────────┐
│              BASIC MATHS CHEAT SHEET                     │
├──────────────────────────────────────────────────────────┤
│ ✅ n % 10 → last digit                                  │
│ ✅ n / 10 → remove last digit                           │
│ ✅ Extraction = reverse order                            │
│ ✅ Reverse: rev = rev*10 + lastDigit                    │
│ ✅ Palindrome: reverse == original (store copy!)         │
│ ✅ Armstrong: sum of digit³ == number                    │
│ ✅ Divisors: loop till √N, pair = n/i                    │
│ ✅ Prime: exactly 2 factors (check till √N)             │
│ ✅ GCD: Euclidean — a%b, b%a till one is 0              │
│ ✅ Division in loop → log time complexity                │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [Time & Space Complexity →](./time-space-complexity.md)
- [C++ STL →](./cpp-stl.md)
- [Recursion & Backtracking →](../dsa-notes/recursion-backtracking.md)
