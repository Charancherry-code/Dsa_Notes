# #️⃣ Hashing — Maps, Collisions & Frequency Counting

> *Striver A2Z DSA Course — Step 1.6*
> 💡 **"Hashing = Pre-store & Fetch. Instead of searching every time, store once, get in O(1)."**

---

## 🧠 What is Hashing?

> **Pre-storing** data so you can **fetch** it instantly when needed.

**Problem:** Given array, answer Q queries — "how many times does X appear?"

```
Brute Force: For each query, loop entire array → O(Q × N) ❌
Hashing:     Pre-compute frequencies, answer each query in O(1) → O(N + Q) ✅
```

---

## 📦 Number Hashing (Using Arrays)

> Create a frequency array — index = number, value = count.

```cpp
int arr[] = {1, 2, 1, 3, 2};
int hash[13] = {0};  // size = max_element + 1

// Pre-compute
for(int i = 0; i < n; i++)
    hash[arr[i]]++;   // go to that index, increment

// Fetch
cout << hash[1];  // → 2 (one appears twice)
cout << hash[3];  // → 1
cout << hash[4];  // → 0 (doesn't exist)
```

### How it works:
```
arr = {1, 2, 1, 3, 2}

hash[1]++ → hash[1] = 1
hash[2]++ → hash[2] = 1
hash[1]++ → hash[1] = 2
hash[3]++ → hash[3] = 1
hash[2]++ → hash[2] = 2

Result: hash = [0, 2, 2, 1, 0, 0, ...]
                    ↑  ↑  ↑
                    1  2  3 appear this many times
```

### ⚠️ Array Size Limits

| Declared Where | Max Size (int) | Max Size (bool) |
|---------------|----------------|-----------------|
| **Inside main** | 10⁶ | 10⁷ |
| **Globally** | 10⁷ | 10⁸ |

❌ Beyond this → Segmentation fault! Use **map** instead.

---

## 🔤 Character Hashing

> For strings: index = character's position (using ASCII).

### Lowercase only (26 chars)
```cpp
int hash[26] = {0};
string s = "abcdabef";

for(int i = 0; i < s.size(); i++)
    hash[s[i] - 'a']++;    // 'a'-'a'=0, 'b'-'a'=1, etc.

// Fetch
cout << hash['a' - 'a'];  // hash[0] → 2
cout << hash['c' - 'a'];  // hash[2] → 1
cout << hash['z' - 'a'];  // hash[25] → 0
```

### All characters (256 ASCII)
```cpp
int hash[256] = {0};

for(int i = 0; i < s.size(); i++)
    hash[s[i]]++;          // auto-casts char to ASCII int

// Fetch
cout << hash['a'];  // → count of 'a'
```

💡 **Character hashing has NO size issues** — max 256 characters exist.

---

## 🗺️ Map (For Large Numbers)

> When numbers exceed 10⁷ → can't use arrays. Use **map** or **unordered_map**.

```cpp
map<int, int> mp;

// Pre-compute
for(int i = 0; i < n; i++)
    mp[arr[i]]++;

// Fetch
cout << mp[1];   // → 2
cout << mp[4];   // → 0 (not stored, returns 0)
```

### Map stores only what's needed:
```
Array hashing: declare size 13 even if only 4 numbers exist
Map:           stores only {1:2, 2:2, 3:1} — just 3 entries
```

### Map vs Unordered Map

| | `map` | `unordered_map` |
|--|-------|-----------------|
| **Order** | Sorted by key | Random order |
| **Best/Avg TC** | O(log N) | **O(1)** ✅ |
| **Worst TC** | O(log N) | O(N) (collision) |
| **Key types** | ANY (pair, vector, etc.) | Only basic types (int, string, char) |
| **Use when** | Need sorted order, or complex keys | Default choice — fastest |

### Strategy:
1. **First try `unordered_map`** — O(1) average
2. **If TLE** (worst case collision) → switch to `map`

---

## 💥 Collisions (Why Worst Case Happens)

> When multiple keys hash to the **same index** → chaining → slow lookup.

### Division Method (How hashing works internally):
```
hash_index = number % table_size

Example (table_size = 10):
  2  % 10 = 2
  5  % 10 = 5
  16 % 10 = 6
  28 % 10 = 8
  38 % 10 = 8  ← COLLISION with 28!
  48 % 10 = 8  ← COLLISION again!
```

### Linear Chaining:
```
Index 8: [28] → [38] → [48]  (chained together)
```

### When collision is BAD:
- All elements hash to SAME index
- Entire chain searched linearly → O(N)
- **Extremely rare** in practice — "once in a blue moon"

💡 You don't implement this. Just understand WHY worst case = O(N).

---

## 📊 Time Complexity Summary

| Method | Pre-compute | Fetch (per query) | Total for Q queries |
|--------|------------|-------------------|---------------------|
| **Brute force** | — | O(N) | O(Q × N) |
| **Array hash** | O(N) | O(1) | O(N + Q) |
| **map** | O(N log N) | O(log N) | O(N log N + Q log N) |
| **unordered_map** | O(N) avg | O(1) avg | O(N + Q) avg |

---

## 🎯 Patterns — When to Use Hashing

| Problem says... | Use |
|----------------|-----|
| "count frequency / occurrences" | hash map |
| "find duplicates" | hash set/map |
| "two sum / pair with given sum" | hash map (store complement) |
| "first non-repeating character" | frequency array |
| "subarray with given sum" | prefix sum + hash map |
| "group anagrams" | sorted string as key in map |
| "intersection/union of arrays" | hash set |

---

## ❌ Common Mistakes

1. **Declaring huge array inside main** — max 10⁶ inside main! Declare globally or use map.

2. **Using map when array works** — if max element ≤ 10⁶, array hash is faster than map. Don't over-engineer.

3. **Forgetting unordered_map can't have pair/vector as key** — use `map` for complex keys.

4. **Not mentioning TC in interviews** — always say: "unordered_map is O(1) average, O(N) worst case due to collisions."

5. **Using map when unordered_map is fine** — map is O(log N), unordered is O(1). Default to unordered.

6. **Forgetting global arrays auto-initialize to 0** — inside main they have garbage values! Always initialize.

7. **Off-by-one in array hash size** — to store index 12, you need size 13 (not 12).

---

## 🏢 Interview Questions (FAANG)

### Q1: Two Sum (LeetCode 1) — EVERYONE asks this
> Find two numbers that add up to target. Return indices.

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> mp;  // value → index
    for(int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];
        if(mp.find(complement) != mp.end())
            return {mp[complement], i};
        mp[nums[i]] = i;
    }
    return {};
}
// TC: O(N) | SC: O(N)
// Pattern: store what you've seen, check if complement exists
```

### Q2: Frequency of Elements (asked everywhere)
```cpp
unordered_map<int, int> mp;
for(int x : arr) mp[x]++;
// mp[key] = frequency
```

### Q3: First Non-Repeating Character (Amazon, Google)
```cpp
char firstUnique(string s) {
    int freq[26] = {0};
    for(char c : s) freq[c - 'a']++;
    for(char c : s)
        if(freq[c - 'a'] == 1) return c;
    return '_';
}
```

### Q4: Check if two strings are anagrams (Meta, Microsoft)
```cpp
bool isAnagram(string s, string t) {
    if(s.size() != t.size()) return false;
    int freq[26] = {0};
    for(int i = 0; i < s.size(); i++) {
        freq[s[i] - 'a']++;
        freq[t[i] - 'a']--;
    }
    for(int i = 0; i < 26; i++)
        if(freq[i] != 0) return false;
    return true;
}
// TC: O(N) | SC: O(1) — fixed 26 chars
```

### Q5: Subarray with Sum K (Google, Amazon)
```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> prefixCount;
    prefixCount[0] = 1;
    int sum = 0, count = 0;
    for(int x : nums) {
        sum += x;
        if(prefixCount.find(sum - k) != prefixCount.end())
            count += prefixCount[sum - k];
        prefixCount[sum]++;
    }
    return count;
}
// Pattern: prefix sum + hash map
// TC: O(N) | SC: O(N)
```

### Q6: "Explain map vs unordered_map" (Theory — asked often)
> - map: sorted keys, O(log N), any key type (BST internally)
> - unordered_map: O(1) avg, O(N) worst (hash table), basic keys only
> - Default: unordered_map. Switch to map if collision TLE or need sorted.

---

## 📋 Quick Revision

```
┌──────────────────────────────────────────────────────────┐
│              HASHING CHEAT SHEET                          │
├──────────────────────────────────────────────────────────┤
│ ✅ Hashing = pre-store + fetch in O(1)                   │
│ ✅ Array hash: index = number, value = frequency         │
│ ✅ Char hash: index = char - 'a' (26 size)              │
│ ✅ Array limit: 10⁶ in main, 10⁷ global                 │
│ ✅ Beyond limits → use map/unordered_map                 │
│ ✅ unordered_map: O(1) avg, O(N) worst (collision)       │
│ ✅ map: O(log N) always, sorted, any key type            │
│ ✅ Default choice: unordered_map. If TLE → map           │
│ ✅ Collision = all keys same hash → linear search        │
│ ✅ Division method: hash_index = key % table_size        │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [C++ STL →](./cpp-stl.md)
- [Time & Space Complexity →](./time-space-complexity.md)
- [Arrays & Hashing Problems →](../dsa-notes/arrays.md)
