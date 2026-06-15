# 🔃 Sorting Part 1 — Selection, Bubble, Insertion Sort

> *Striver A2Z DSA Course — Step 2.1*
> 💡 **"Know these 3 by heart. Interviewers WILL ask you to code them on the spot."**

---

## 1️⃣ Selection Sort

> **Select the minimum, swap it to the front.** Repeat for remaining.

### Algorithm:
1. Find minimum in `[0, n-1]` → swap with index 0
2. Find minimum in `[1, n-1]` → swap with index 1
3. Find minimum in `[2, n-1]` → swap with index 2
4. ...continue till `n-2`

### Code:
```cpp
void selectionSort(int arr[], int n) {
    for(int i = 0; i < n - 1; i++) {
        int mini = i;
        for(int j = i + 1; j < n; j++) {
            if(arr[j] < arr[mini])
                mini = j;
        }
        swap(arr[mini], arr[i]);
    }
}
```

### Dry Run:
```
[13, 46, 24, 52, 20, 9]
Step 1: min=9(idx5)  → swap with idx0 → [9, 46, 24, 52, 20, 13]
Step 2: min=13(idx5) → swap with idx1 → [9, 13, 24, 52, 20, 46]
Step 3: min=20(idx4) → swap with idx2 → [9, 13, 20, 52, 24, 46]
Step 4: min=24(idx4) → swap with idx3 → [9, 13, 20, 24, 52, 46]
Step 5: min=46(idx5) → swap with idx4 → [9, 13, 20, 24, 46, 52] ✅
```

### Complexity:
| Case | Time | Space |
|------|------|-------|
| Best | O(N²) | O(1) |
| Average | O(N²) | O(1) |
| Worst | O(N²) | O(1) |

> Always O(N²) — no optimization possible. N + (N-1) + (N-2) + ... + 1 = N(N+1)/2 ≈ N²

---

## 2️⃣ Bubble Sort

> **Push the maximum to the end** via adjacent swaps. Biggest bubbles up!

### Algorithm:
1. Compare adjacent pairs, swap if wrong order → max reaches end
2. Repeat for remaining (exclude last sorted portion)
3. **Optimization:** If no swap in a pass → already sorted, BREAK

### Code:
```cpp
void bubbleSort(int arr[], int n) {
    for(int i = n - 1; i >= 1; i--) {
        bool swapped = false;
        for(int j = 0; j < i; j++) {
            if(arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if(!swapped) break;  // ⚡ optimization: already sorted!
    }
}
```

### Dry Run:
```
[13, 46, 24, 52, 20, 9]
Pass 1: 13,46 ✓ | 46,24 swap | 46,52 ✓ | 52,20 swap | 52,9 swap
        → [13, 24, 46, 20, 9, 52]  (52 at end)
Pass 2: → [13, 24, 20, 9, 46, 52]  (46 at place)
Pass 3: → [13, 20, 9, 24, 46, 52]  (24 at place)
Pass 4: → [13, 9, 20, 24, 46, 52]  (20 at place)
Pass 5: → [9, 13, 20, 24, 46, 52]  ✅
```

### Complexity:
| Case | Time | Space |
|------|------|-------|
| **Best** | **O(N)** ← sorted array, no swaps, breaks early | O(1) |
| Average | O(N²) | O(1) |
| Worst | O(N²) | O(1) |

⚡ **Best case O(N) only with the `swapped` optimization!** Without it = always O(N²).

---

## 3️⃣ Insertion Sort

> **Take element, place it in its correct position** in the sorted left portion.

### Algorithm:
1. Start from index 1 (index 0 is "sorted" by itself)
2. Take element, compare with left neighbors
3. Shift bigger elements right, insert at correct spot
4. Repeat for all elements

### Code:
```cpp
void insertionSort(int arr[], int n) {
    for(int i = 1; i < n; i++) {
        int j = i;
        while(j > 0 && arr[j - 1] > arr[j]) {
            swap(arr[j - 1], arr[j]);
            j--;
        }
    }
}
```

### Dry Run:
```
[14, 9, 15, 12, 6, 8, 13]
i=1: 9 < 14 → swap → [9, 14, 15, 12, 6, 8, 13]
i=2: 15 > 14 → no swap → stays
i=3: 12 < 15 → swap, 12 < 14 → swap → [9, 12, 14, 15, 6, 8, 13]
i=4: 6 goes all the way left → [6, 9, 12, 14, 15, 8, 13]
i=5: 8 goes left till after 6 → [6, 8, 9, 12, 14, 15, 13]
i=6: 13 goes left till after 12 → [6, 8, 9, 12, 13, 14, 15] ✅
```

### Complexity:
| Case | Time | Space |
|------|------|-------|
| **Best** | **O(N)** ← already sorted, inner loop never runs | O(1) |
| Average | O(N²) | O(1) |
| Worst | O(N²) ← reverse sorted, every element goes to start | O(1) |

---

## 📊 Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable? | Key Idea |
|-----------|------|---------|-------|-------|---------|----------|
| **Selection** | O(N²) | O(N²) | O(N²) | O(1) | ❌ No | Select min, swap to front |
| **Bubble** | **O(N)** | O(N²) | O(N²) | O(1) | ✅ Yes | Adjacent swap, max bubbles up |
| **Insertion** | **O(N)** | O(N²) | O(N²) | O(1) | ✅ Yes | Place element in correct position |

### When to use which?
- **Nearly sorted array** → Insertion sort (best = O(N))
- **Need stable sort** → Bubble or Insertion (not Selection)
- **Small array** → Any of the three (N² is fine for small N)
- **Large array** → None of these! Use Merge Sort / Quick Sort (N log N)

---

## 🎯 Patterns

| Interview asks... | Answer |
|------------------|--------|
| "Sort with O(1) space" | Any of these 3 (all in-place) |
| "Best case O(N)?" | Bubble (with optimization) or Insertion |
| "Stable sort?" | Bubble or Insertion (Selection is unstable) |
| "Code selection sort" | Find min in [i, n-1], swap with i |
| "Why is selection always N²?" | No way to skip — must check all for min |

---

## ❌ Common Mistakes

1. **Bubble sort without `swapped` flag** → misses the O(N) best case. Always add it.

2. **Insertion sort: `j >= 0` instead of `j > 0`** → accessing `arr[-1]` = runtime error!

3. **Selection sort: swapping inside inner loop** → wrong! Find min first, swap ONCE after inner loop.

4. **Off-by-one in bubble sort** → comparing `arr[j]` with `arr[j+1]` means loop till `j < i`, NOT `j <= i` (would access out of bounds).

5. **Saying selection sort is stable** → it's NOT. Swap can change relative order of equal elements.

6. **Not knowing best cases** → Bubble and Insertion have O(N) best. Selection doesn't. Know this.

---

## 🏢 Interview Questions (FAANG)

### Q1: "Code insertion sort" (Amazon, Microsoft — asked directly)
```cpp
void insertionSort(int arr[], int n) {
    for(int i = 1; i < n; i++) {
        int j = i;
        while(j > 0 && arr[j-1] > arr[j]) {
            swap(arr[j-1], arr[j]);
            j--;
        }
    }
}
```

### Q2: "Which sorting algo is best for nearly sorted data?"
> **Insertion sort** — O(N) best case. Each element is close to its final position, so inner loop barely runs.

### Q3: "What is a stable sort? Which of these are stable?"
> Stable = equal elements maintain their relative order after sorting.
> - Bubble Sort: ✅ stable (only swaps adjacent if strictly greater)
> - Insertion Sort: ✅ stable (only shifts if strictly greater)
> - Selection Sort: ❌ unstable (swap can jump over equal elements)

### Q4: "Why don't we use these in production?"
> All are O(N²) worst/average. For large data, use O(N log N) algorithms:
> - Merge Sort (stable, guaranteed N log N)
> - Quick Sort (avg N log N, worst N² but rare with random pivot)
> - Tim Sort (Python/Java default — hybrid of merge + insertion)

### Q5: "Sort an array where each element is at most K positions away from its sorted position"
> **Insertion sort** — inner loop runs at most K times per element → O(N×K). If K is small, this is nearly O(N).

---

## 📋 Quick Revision

```
┌──────────────────────────────────────────────────────────┐
│              SORTING PART 1 CHEAT SHEET                   │
├──────────────────────────────────────────────────────────┤
│ SELECTION: find min → swap to front → repeat             │
│            Always O(N²). Unstable. Simple.               │
│                                                           │
│ BUBBLE: adjacent swap → max bubbles to end               │
│         Best O(N) with swapped flag. Stable.             │
│                                                           │
│ INSERTION: take element → place in correct position      │
│            Best O(N) for sorted. Stable. Best for small. │
│                                                           │
│ ALL: O(1) space (in-place), O(N²) worst/avg             │
│ FOR LARGE DATA: use Merge Sort / Quick Sort (N log N)    │
└──────────────────────────────────────────────────────────┘
```

---

## 🔗 Related
- [Time & Space Complexity →](./time-space-complexity.md)
- [C++ STL sort() →](./cpp-stl.md)
- [Recursion (for Merge/Quick Sort) →](./recursion-intro.md)
