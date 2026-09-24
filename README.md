# DSA-Java-LeetCode
Java solution for LeetCode Two Sum problem


## Problem Summary
Given an array of integers `nums` and an integer `target`, we need to find the indices of the two numbers in the array that add up to `target`. Each input has exactly one valid solution, and the same element cannot be used twice. The answer can be returned in any order.

## Intuition
The most basic way to solve this is to check every possible pair of numbers in the array and see if they add up to the target. Since we need two distinct indices, we can fix one number and search through the rest of the array for its complement (`target - nums[i]`). This brute-force idea is simple to think of and easy to implement, even though it's not the most efficient approach.

## Approach
- Use two nested loops.
- The outer loop picks the first number (`nums[i]`).
- The inner loop starts right after the outer loop's index and checks every subsequent number (`nums[j]`).
- If `nums[j]` equals `target - nums[i]`, it means `nums[i] + nums[j] == target`, so we return `[i, j]`.
- If no pair is found after checking all combinations, return an empty array (this is just a safe fallback since the problem guarantees a solution exists).

## Algorithm
1. Loop through the array with index `i` from `0` to `nums.length - 1`.
2. For each `i`, loop through the array with index `j` from `i + 1` to `nums.length - 1`.
3. Check if `nums[j] == target - nums[i]`.
4. If true, return a new array `{i, j}`.
5. If the loops complete without finding a pair, return an empty array `{}`.

## Java Solution
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == target - nums[i]) {
                    return new int[] { i, j };
                }
            }
        }
        // If no valid pair is found, return an empty array instead of null
        return new int[] {};
    }
}
```

## Dry Run
**Input:** `nums = [2, 7, 11, 15]`, `target = 9`

| i | j | nums[i] | nums[j] | target - nums[i] | Match? |
|---|---|---------|---------|-------------------|--------|
| 0 | 1 | 2       | 7       | 7                 | Yes |

- `i = 0` → `nums[0] = 2`
- `j = 1` → `nums[1] = 7`
- Check: `nums[j] == target - nums[i]` → `7 == 9 - 2` → `7 == 7` → **true**
- Return `[0, 1]`
**Output:**`[0, 1]`

Explanation: `nums[0] + nums[1] = 2 + 7 = 9 = target`, so indices `0` and `1` are returned.

## Time Complexity
**O(n²)** — For each element at index `i`, the inner loop scans the remaining elements to find a match. In the worst case (no early match), this results in roughly `n * (n-1) / 2` comparisons, which simplifies to O(n²).

## Space Complexity
**O(1)** (excluding the output array) — No extra data structures are used to store intermediate results; only a constant amount of extra space is used for loop variables.

## Concepts Used
* Array traversal
* Nested loops (brute-force pair checking)
* Basic conditional logic

## Key Learning
This problem teaches the brute-force pattern of checking all pairs in an array, which is a common starting point before optimizing with better data structures like hash maps. It highlights the tradeoff between simplicity of implementation and time efficiency.

## Interview Notes
Common follow-up questions and optimizations interviewers may ask:
* **"Can you do better than O(n²)?"** — Yes, using a `HashMap` to store each number's complement while iterating once through the array, achieving O(n) time complexity.
* **"What if the array is sorted?"** — A two-pointer approach (one pointer at the start, one at the end) could achieve O(n) time and O(1) space.
* **"What if there are multiple valid pairs?"** — Clarify whether the problem wants all pairs or just one (this version assumes exactly one solution exists).
* **"What if no solution exists?"** — Discuss how to handle this gracefully (e.g., returning an empty array, throwing an exception, or returning `null`).

---

## Alternative Optimized Approach

The brute-force approach works but is inefficient for large inputs. We can optimize it to **O(n) time** using a `HashMap` to store numbers we've already seen along with their indices. This lets us check for the complement in constant time instead of looping again.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> seen = new HashMap<>(); // value -> index
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (seen.containsKey(complement)) {
                return new int[] { seen.get(complement), i };
            }
            seen.put(nums[i], i);
        }
        return new int[] {};
    }
}
```


**Time Complexity:* O(n) — We traverse the array once, and HashMap operations (`containsKey`, `get`, `put`) take O(1) on average.

**Space Complexity:* O(n) — In the worst case, we store all `n` elements in the HashMap before finding a match.

**Tradeoff:** This approach trades extra space (O(n)) for significantly better time complexity (O(n) instead of O(n²)), making it the preferred solution for large inputs.
