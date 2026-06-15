# 🔀 Merge Sort — Divide & Merge

> *Striver A2Z DSA Course — Step 2.2*
> 💡 **"Divide the array into halves recursively, then merge sorted halves back together."**

---

## 🧠 Core Idea

```
DIVIDE: split array into 2 halves → keep splitting till single elements
MERGE:  combine 2 sorted halves into 1 sorted array → build back up
```

```
         [3, 1, 2, 4, 1, 5, 2, 6, 4]
                    ↓ divide
        [3,1,2,4,1]     [5,2,6,4]
            ↓                ↓
      [3,1,2]  [4,1]    [5,2]  [6,4]
        ↓        ↓        ↓      ↓
    [3,1] [2]  [4] [1]  [5][2] [6][4]
      ↓                    ↓      ↓
   [3] [1]              [2,5]  [4,6]
      ↓ merge               ↓ merge
    [1,3]                  [2,4,5,6]
        ↓ merge
      [1,2,3]  [1,4]
           ↓ merge
       [1,1,2,3,4]  [2,4,5,6]
                ↓ final merge
         [1,1,2,2,3,4,4,5,6] ✅
```

---

## 💻 Code

```cpp
void merge(vector<int>& arr, int low, int mid, int high) {
    vector<int> temp;
    int left = low;       // start of left half
    int right = mid + 1;  // start of right half

    // Compare and fill temp
    while(left <= mid && right <= high) {
        if(arr[left] <= arr[right]) {
            temp.push_back(arr[left]);
            left++;
        } else {
            temp.push_back(arr[right]);
            right++;
        }
    }

    // Copy remaining left
    while(left <= mid) {
        temp.push_back(arr[left]);
        left++;
    }

    // Copy remaining right
    while(right <= high) {
        temp.push_back(arr[right]);
        right++;
    }

    // Put back into original array
    for(int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }
}

void mergeSort(vector<int>& arr, int low, int high) {
    if(low >= high) return;  // base: single element
    int mid = (low + high) / 2;
    mergeSort(arr, low, mid);       // sort left half
    mergeSort(arr, mid + 1, high);  // sort right half
    merge(arr, low, mid, high);     // merge both sorted halves
}

// Call: mergeSort(arr, 0, n - 1);
```

---

## 🔍 How Merge Works (Two Pointer)

> Both halves are already sorted. Use two pointers to combine.

```
Left:  [1, 3, 4]     Right: [2, 5, 6]
        ↑ left               ↑ right

Step 1: 1 < 2 → take 1, left++    temp = [1]
Step 2: 3 > 2 → take 2, right++   temp = [1, 2]
Step 3: 3 < 5 → take 3, left++    temp = [1, 2, 3]
Step 4: 4 < 5 → take 4, left++    temp = [1, 2, 3, 4]
Step 5: left exhausted → copy remaining right
                                    temp = [1, 2, 3, 4, 5, 6] ✅
```

---

## 🌳 Recursion Flow (Dry Run for arr = [3, 2, 4, 1, 3])

```
mergeSort(0, 4)
├── mergeSort(0, 2)         ← left half
│   ├── mergeSort(0, 1)
│   │   ├── mergeSort(0, 0) → return (single element: 3)
│   │   ├── mergeSort(1, 1) → return (single element: 2)
│   │   └── merge(0, 0, 1)  → [2, 3]
│   ├── mergeSort(2, 2) → return (single element: 4)
│   └── merge(0, 1, 2)  → [2, 3, 4]
├── mergeSort(3, 4)         ← right half
│   ├── mergeSort(3, 3) → return (single element: 1)
│   ├── mergeSort(4, 4) → return (single element: 3)
│   └── merge(3, 3, 4)  → [1, 3]
└── merge(0, 2, 4)          ← final merge
    [2, 3, 4] + [1, 3] → [1, 2, 3, 3, 4] ✅
```

**Key:** Left recursion completes FULLY first → then right → then merge.

---

## ⏱️ Complexity

| Case | Time | Space |
|------|------|-------|
| Best | O(N log N) | O(N) |
| Average | O(N log N) | O(N) |
| Worst | O(N log N) | O(N) |

**Why O(N log N)?**
- **log N levels** of division (halving each time)
- **O(N) work** at each level (merging)
- Total: N × log N

**Why O(N) space?**
- Temporary array used during merge (worst case = full array size)

---

## 🆚 Merge Sort vs Previous Sorts

| | Selection/Bubble/Insertion | Merge Sort |
|--|---------------------------|------------|
| **Time (worst)** | O(N²) | O(N log N) ✅ |
| **Time (best)** | O(N) for bubble/insertion | O(N log N) |
| **Space** | O(1) ✅ | O(N) |
| **Stable?** | Bubble/Insertion: yes | ✅ Yes |
| **Approach** | Iterative | Recursive (divide & conquer) |

**Trade-off:** Merge sort is faster but uses extra space.

---

## 🎯 Patterns — When to Use Merge Sort

| Situation | Why merge sort |
|-----------|---------------|
| Need guaranteed O(N log N) | Unlike quick sort, no worst case N² |
| Need stable sort | Preserves relative order of equal elements |
| Sorting linked lists | Merge sort is ideal for linked lists (no random access needed) |
| Count inversions in array | Classic merge sort application |
| External sorting (files) | Can merge sorted chunks from disk |

---

## ❌ Common Mistakes

1. **Forgetting `low >= high` base case** — causes infinite recursion.

2. **Wrong mid calculation** — use `(low + high) / 2`. For large numbers: `low + (high - low) / 2` to avoid overflow.

3. **Not passing array by reference** — merge modifies array, must be reference/pointer.

4. **Off-by-one in merge** — left goes `low to mid`, right goes `mid+1 to high`. Not `mid` to `high`!

5. **Forgetting to copy temp back to original** — the `arr[i] = temp[i - low]` step is crucial.

6. **Using `<` instead of `<=` in merge comparison** — use `<=` for stability (equal elements maintain order).

---

## 🏢 Interview Questions (FAANG)

### Q1: "Code merge sort" (asked directly — Amazon, Google, Microsoft)
> Write the code above. Key: explain divide-merge-combine clearly.

### Q2: "What's the time complexity? Why?"
> O(N log N). log N levels of recursion (halving), O(N) merge at each level.

### Q3: "Is merge sort stable? Why does it matter?"
> Yes — `<=` in comparison ensures equal elements maintain original order. Matters when sorting objects by multiple keys.

### Q4: Count Inversions in Array (Google, Amazon — classic)
> Inversion = pair (i,j) where i < j but arr[i] > arr[j].

```cpp
int merge(vector<int>& arr, int low, int mid, int high) {
    int count = 0;
    vector<int> temp;
    int left = low, right = mid + 1;
    while(left <= mid && right <= high) {
        if(arr[left] <= arr[right]) {
            temp.push_back(arr[left++]);
        } else {
            count += (mid - left + 1);  // all remaining on left are inversions!
            temp.push_back(arr[right++]);
        }
    }
    while(left <= mid) temp.push_back(arr[left++]);
    while(right <= high) temp.push_back(arr[right++]);
    for(int i = low; i <= high; i++) arr[i] = temp[i - low];
    return count;
}
// TC: O(N log N) — same as merge sort
```

### Q5: "Merge sort vs Quick sort?"
> - Merge: O(N log N) always, stable, O(N) space
> - Quick: O(N log N) avg, O(N²) worst, O(1) space, unstable
> - Use merge for stability/guarantees. Use quick for space efficiency.

### Q6: "Can merge sort be done in-place (O(1) space)?"
> Theoretically yes, but extremely complex and slower in practice. Not expected in interviews. Just mention it exists.

---

## 📋 Quick Revision

```
┌──────────────────────────────────────────────────────────┐
│              MERGE SORT CHEAT SHEET                       │
├──────────────────────────────────────────────────────────┤
│ ✅ Divide array into halves recursively                  │
│ ✅ Base case: single element (low >= high)               │
│ ✅ Merge: two-pointer on sorted halves → temp array      │
│ ✅ Copy temp back to original array                      │
│ ✅ TC: O(N log N) — all cases                           │
│ ✅ SC: O(N) — temp array during merge                   │
│ ✅ Stable sort: yes (use <= in comparison)               │
│ ✅ Left recursion → Right recursion → Merge              │
│ ✅ Application: count inversions, external sort          │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [Sorting Part 1 (Selection, Bubble, Insertion) →](./sorting-1.md)
- [Recursion →](./recursion-intro.md)
- [Time & Space Complexity →](./time-space-complexity.md)
