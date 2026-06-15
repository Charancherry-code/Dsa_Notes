# ⚡ Quick Sort — Divide & Conquer (In-Place)

> *Striver A2Z DSA Course — Step 2.2*
> 💡 **"Pick a pivot, place it at its correct position, smaller on left, larger on right. Repeat."**

---

## 🧠 Core Idea

```
1. Pick a PIVOT (first element, last, random — any works)
2. Place pivot at its CORRECT position in sorted array
3. Everything SMALLER goes LEFT, everything LARGER goes RIGHT
4. Recursively do the same for left and right portions
```

If every element reaches its correct position → array is sorted!

---

## 💻 Code

```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[low];
    int i = low;
    int j = high;

    while(i < j) {
        // Find first element GREATER than pivot from left
        while(arr[i] <= pivot && i < high)
            i++;

        // Find first element SMALLER than pivot from right
        while(arr[j] > pivot && j > low)
            j--;

        // If they haven't crossed, swap
        if(i < j)
            swap(arr[i], arr[j]);
    }

    // Place pivot at its correct position (where j stopped)
    swap(arr[low], arr[j]);
    return j;  // partition index
}

void quickSort(vector<int>& arr, int low, int high) {
    if(low < high) {
        int pIndex = partition(arr, low, high);
        quickSort(arr, low, pIndex - 1);   // sort left of pivot
        quickSort(arr, pIndex + 1, high);  // sort right of pivot
    }
}

// Call: quickSort(arr, 0, n - 1);
```

---

## 🔍 How Partition Works (Dry Run)

```
arr = [4, 6, 2, 5, 7, 9, 1, 3]
pivot = 4 (first element)
i = low (0), j = high (7)

Step 1: i finds first > pivot → i stops at 6 (idx 1)
        j finds first < pivot → j stops at 3 (idx 7)
        i < j → swap(6, 3) → [4, 3, 2, 5, 7, 9, 1, 6]

Step 2: i finds > pivot → i stops at 5 (idx 3)
        j finds < pivot → j stops at 1 (idx 6)
        i < j → swap(5, 1) → [4, 3, 2, 1, 7, 9, 5, 6]

Step 3: i finds > pivot → i stops at 7 (idx 4)
        j finds < pivot → j stops at 1 (idx 3)
        i > j → CROSSED! Stop. Don't swap.

Final: swap pivot (arr[low]) with arr[j]
       swap(4, 1) → [1, 3, 2, 4, 7, 9, 5, 6]
                            ↑ pivot at correct position (idx 3)

Result: [1, 3, 2] | 4 | [7, 9, 5, 6]
         smaller   pivot   larger
```

Now recursively sort `[1, 3, 2]` and `[7, 9, 5, 6]`.

---

## 🌳 Recursion Tree

```
quickSort([4,6,2,5,7,9,1,3], 0, 7)
├── partition → pivot 4 at idx 3
├── quickSort([1,3,2], 0, 2)        ← left of pivot
│   ├── partition → pivot 1 at idx 0
│   ├── quickSort([], empty)         ← nothing left
│   └── quickSort([3,2], 1, 2)
│       ├── partition → pivot 2 at idx 1 (after swap with 3)
│       └── done
└── quickSort([7,9,5,6], 4, 7)      ← right of pivot
    ├── partition → pivot 7 at idx 6
    ├── quickSort([5,6], 4, 5)
    └── quickSort([9], 7, 7) → single element, return
```

---

## ⏱️ Complexity

| Case | Time | Space |
|------|------|-------|
| **Best** | O(N log N) | O(1) + O(log N) stack |
| **Average** | O(N log N) | O(1) + O(log N) stack |
| **Worst** | O(N²) ← sorted/reverse sorted array | O(1) + O(N) stack |

**Why O(N log N) average?**
- log N levels of recursion (splitting roughly in half)
- O(N) partition work at each level

**Why O(N²) worst case?**
- Already sorted array + first element as pivot
- Pivot always at extreme → splits into (0, N-1) instead of (N/2, N/2)
- Fix: use **random pivot** or **median-of-three**

**Space: O(1)** — no temp array! Only recursion stack (log N avg, N worst).

---

## 🆚 Quick Sort vs Merge Sort

| | Quick Sort | Merge Sort |
|--|-----------|------------|
| **Avg TC** | O(N log N) | O(N log N) |
| **Worst TC** | O(N²) ← rare with random pivot | O(N log N) ✅ always |
| **Space** | **O(1)** ✅ (in-place) | O(N) temp array |
| **Stable?** | ❌ No | ✅ Yes |
| **Approach** | Partition then recurse | Recurse then merge |
| **Best for** | Arrays (cache friendly) | Linked lists, stability needed |
| **In practice** | Faster (less overhead) | More predictable |

💡 **Quick sort = faster in practice, Merge sort = guaranteed O(N log N)**

---

## 🎯 Patterns

| Situation | Use Quick Sort when... |
|-----------|----------------------|
| In-place sorting needed | No extra O(N) space |
| Average case matters more | Random pivot makes worst case rare |
| Cache performance matters | Contiguous memory access |
| Don't need stability | Equal elements may reorder |

---

## ❌ Common Mistakes

1. **Using first element as pivot on sorted array** → O(N²) worst case. Use random pivot.

2. **Boundary overflow** — `i` moving right can exceed array. Use `i < high` guard. Same for `j > low`.

3. **Equal elements handling** — decide: `<=` on left means equals go left. `>` on right. Be consistent.

4. **Forgetting `i < j` check before swap** — if crossed, don't swap! Just proceed to final pivot swap.

5. **Swapping pivot with `arr[i]` instead of `arr[j]`** — ALWAYS swap with `j` (last element of left partition).

6. **Saying quick sort is stable** — it's NOT. Partition swaps can change relative order of equal elements.

---

## 🏢 Interview Questions (FAANG)

### Q1: "Code quick sort" (Amazon, Google — asked directly)
> Write the code above. Explain: pick pivot, partition, recurse left/right.

### Q2: "What's the worst case and when does it happen?"
> O(N²) when pivot always selects min/max (sorted array + first element pivot). Fix: randomized pivot.

### Q3: "Quick sort vs Merge sort — which to use?"
> Quick: in-place, faster in practice, O(1) space. Merge: stable, guaranteed N log N, needs O(N) space.
> Most standard libraries use Quick Sort variant (introsort) because it's faster on average.

### Q4: "Is quick sort stable? Why does it matter?"
> No. Partition swaps non-adjacent elements. Use merge sort when stability needed (sorting objects by multiple keys).

### Q5: "How to handle worst case?"
> 1. **Random pivot** — pick random element, swap with first, then partition normally
> 2. **Median-of-three** — pick median of first, middle, last as pivot
> 3. **Introsort** — switch to heap sort if recursion depth exceeds 2×log N (C++ std::sort uses this)

### Q6: Kth Smallest Element using Partition (Quick Select) — Google, Amazon
```cpp
int quickSelect(vector<int>& arr, int low, int high, int k) {
    int pIndex = partition(arr, low, high);
    if(pIndex == k - 1) return arr[pIndex];      // found!
    else if(pIndex > k - 1) return quickSelect(arr, low, pIndex - 1, k);
    else return quickSelect(arr, pIndex + 1, high, k);
}
// Average TC: O(N) — only goes one side each time!
// This is Quick Select — finds kth element without fully sorting
```

---

## 📋 Quick Revision

```
┌──────────────────────────────────────────────────────────┐
│              QUICK SORT CHEAT SHEET                       │
├──────────────────────────────────────────────────────────┤
│ ✅ Pick pivot (first/last/random)                        │
│ ✅ Partition: smaller left, larger right                  │
│ ✅ i → finds first greater than pivot from left          │
│ ✅ j → finds first smaller than pivot from right         │
│ ✅ If i < j: swap. If crossed: stop, swap pivot with j   │
│ ✅ Return j as partition index                           │
│ ✅ Recurse: quickSort(low, pIdx-1), quickSort(pIdx+1, high) │
│ ✅ TC: O(N log N) avg, O(N²) worst                      │
│ ✅ SC: O(1) — IN-PLACE! (no temp array)                 │
│ ✅ Not stable. Use random pivot to avoid worst case.     │
│ ✅ Quick Select: find Kth element in O(N) avg            │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [Merge Sort →](./sorting-2-merge.md)
- [Sorting Part 1 →](./sorting-1.md)
- [Recursion →](./recursion-intro.md)
