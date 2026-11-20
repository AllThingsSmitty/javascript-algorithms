# JavaScript Algorithms

A collection of JavaScript algorithms and data structures I've written up for interview prep and as a quick reference. Each one has a short explanation, its complexity, and an example you can paste into a Node (18+) or browser console.

Most functions validate their input so the examples fail loudly. In an interview you can usually skip that part and say you would add it.

## Algorithms

### String Manipulation

- [Reverse a String](#reverse-a-string)
- [Palindrome Check](#palindrome-check)
- [Character Frequency Counter](#character-frequency-counter)
- [Anagram Check](#anagram-check)
- [Valid Parentheses](#valid-parentheses)

### Math & Number Theory

- [Prime Number Check](#prime-number-check)
- [Fibonacci Sequence (Memoized)](#fibonacci-sequence-memoized)
- [Factorial of a Number](#factorial-of-a-number)
- [Find the GCD (Greatest Common Divisor)](#find-the-gcd-greatest-common-divisor)
- [Climbing Stairs (Dynamic Programming)](#climbing-stairs-dynamic-programming)

### Searching

- [Two Sum (Using Hash Map)](#two-sum-using-hash-map)
- [Binary Search](#binary-search)

### Sorting

- [Bubble Sort](#bubble-sort)
- [Merge Sort](#merge-sort)
- [Quick Sort](#quick-sort)
- [Merge Two Sorted Arrays](#merge-two-sorted-arrays)

### Array Utilities

- [Find Maximum in Array](#find-maximum-in-array)
- [Maximum Subarray (Kadane)](#maximum-subarray-kadane)
- [Flatten a Nested Array](#flatten-a-nested-array)

### Data Structures

- [Linked List](#linked-list)
- [Stack](#stack)
- [Queue](#queue)
- [Binary Search Tree](#binary-search-tree)

### Graph Algorithms

- [Depth-First Search (DFS)](#depth-first-search-dfs)
- [Breadth-First Search (BFS)](#breadth-first-search-bfs)

### Utility Functions

- [Debounce Function](#debounce-function)
- [Throttle Function](#throttle-function)

## String Manipulation

### Reverse a String

```js
/**
 * Reverses a string
 * @param {string} str - The string to reverse
 * @returns {string} The reversed string
 */
function reverseString(str) {
  if (typeof str !== "string") throw new TypeError("Input must be a string");
  return [...str].reverse().join("");
}

console.log(reverseString("hello")); // Output: "olleh"
```

**Explanation**: Spreads the string into an array, reverses it, and joins it back. I use the spread instead of `split("")` because it keeps emoji and other multi-unit characters intact.

**Complexity**: Time O(n), space O(n).

<sup>[Back to top](#algorithms)</sup>

### Palindrome Check

```js
/**
 * Checks if a string is a palindrome (ignores case, spaces, and punctuation)
 * @param {string} str - The string to check
 * @returns {boolean} True if palindrome, false otherwise
 */
function isPalindrome(str) {
  if (typeof str !== "string") throw new TypeError("Input must be a string");
  const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, "");
  return cleaned === [...cleaned].reverse().join("");
}

console.log(isPalindrome("racecar")); // Output: true
console.log(isPalindrome("A man, a plan, a canal: Panama")); // Output: true
console.log(isPalindrome("hello")); // Output: false
```

**Explanation**: Lowercases the string, strips everything that isn't a letter or digit, then compares it to its reverse. Note that this only handles English letters. Accented characters would be stripped.

**Complexity**: Time O(n), space O(n).

<sup>[Back to top](#algorithms)</sup>

### Character Frequency Counter

```js
/**
 * Counts the frequency of each character in a string
 * @param {string} str - The string to analyze
 * @returns {Object} An object with characters as keys and frequencies as values
 */
function charFrequency(str) {
  if (typeof str !== "string") throw new TypeError("Input must be a string");
  return [...str].reduce((freq, char) => {
    freq[char] = (freq[char] ?? 0) + 1;
    return freq;
  }, {});
}

console.log(charFrequency("hello")); // Output: { h: 1, e: 1, l: 2, o: 1 }
```

**Explanation**: Walks the characters with `reduce()` and uses `??` to start each count at zero. A `Map` would be the safer choice if the input could contain keys like `__proto__`.

**Complexity**: Time O(n), space O(k) where k is the number of distinct characters.

<sup>[Back to top](#algorithms)</sup>

### Anagram Check

```js
/**
 * Determines if two strings are anagrams (ignores case and spaces)
 * @param {string} str1 - First string
 * @param {string} str2 - Second string
 * @returns {boolean} True if anagrams, false otherwise
 */
function isAnagram(str1, str2) {
  if (typeof str1 !== "string" || typeof str2 !== "string") {
    throw new TypeError("Both inputs must be strings");
  }
  const normalize = (str) =>
    [...str.toLowerCase().replace(/\s+/g, "")].sort().join("");
  return normalize(str1) === normalize(str2);
}

console.log(isAnagram("listen", "silent")); // Output: true
console.log(isAnagram("The Eyes", "They See")); // Output: true
```

**Explanation**: Lowercases both strings, removes spaces, sorts the characters, and compares the results. If you need better than O(n log n), count characters with a `Map` instead of sorting.

**Complexity**: Time O(n log n), space O(n).

<sup>[Back to top](#algorithms)</sup>

### Valid Parentheses

```js
/**
 * Checks that every bracket in a string is closed in the right order
 * @param {string} str - String containing brackets
 * @returns {boolean} True if brackets are balanced
 */
function isValidParentheses(str) {
  if (typeof str !== "string") throw new TypeError("Input must be a string");
  const pairs = { ")": "(", "]": "[", "}": "{" };
  const stack = [];
  for (const char of str) {
    if (char === "(" || char === "[" || char === "{") {
      stack.push(char);
    } else if (char in pairs) {
      if (stack.pop() !== pairs[char]) return false;
    }
  }
  return stack.length === 0;
}

console.log(isValidParentheses("({[]})")); // Output: true
console.log(isValidParentheses("([)]")); // Output: false
console.log(isValidParentheses("((")); // Output: false
```

**Explanation**: Push every opening bracket onto a stack. When a closing bracket shows up, the top of the stack has to be its match. The string is valid only if the stack is empty at the end. Characters that aren't brackets are ignored.

**Complexity**: Time O(n), space O(n).

<sup>[Back to top](#algorithms)</sup>

## Math & Number Theory

### Prime Number Check

```js
/**
 * Checks if a number is prime
 * @param {number} num - The number to check
 * @returns {boolean} True if prime, false otherwise
 */
function isPrime(num) {
  if (!Number.isInteger(num)) throw new TypeError("Input must be an integer");
  if (num <= 1) return false;
  if (num <= 3) return true;
  if (num % 2 === 0 || num % 3 === 0) return false;
  for (let i = 5; i * i <= num; i += 6) {
    if (num % i === 0 || num % (i + 2) === 0) return false;
  }
  return true;
}

console.log(isPrime(7)); // Output: true
console.log(isPrime(1)); // Output: false
```

**Explanation**: Rules out 2 and 3 up front, then only tests divisors of the form 6k ± 1 up to the square root. Every prime above 3 has that form, so this skips two thirds of the candidates.

**Complexity**: Time O(√n), space O(1).

<sup>[Back to top](#algorithms)</sup>

### Fibonacci Sequence (Memoized)

```js
/**
 * Returns the nth Fibonacci number using memoization
 * @param {number} n - The position in the Fibonacci sequence
 * @param {Map} memo - Cache of results already computed
 * @returns {number} The nth Fibonacci number
 */
function fibonacci(n, memo = new Map()) {
  if (!Number.isInteger(n) || n < 0) {
    throw new TypeError("Input must be a non-negative integer");
  }
  if (n <= 1) return n;
  if (memo.has(n)) return memo.get(n);

  const result = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);
  memo.set(n, result);
  return result;
}

console.log(fibonacci(6)); // Output: 8
console.log(fibonacci(50)); // Output: 12586269025
```

**Explanation**: Plain recursion recomputes the same values over and over, which is O(2^n). Caching each result in a `Map` means every value is computed once. Past `fibonacci(78)` the result no longer fits in a safe integer, so use `BigInt` if you need bigger numbers.

**Complexity**: Time O(n), space O(n).

<sup>[Back to top](#algorithms)</sup>

### Factorial of a Number

```js
/**
 * Calculates the factorial of a number
 * @param {number} n - The number to calculate factorial for
 * @returns {number} The factorial of n
 */
function factorial(n) {
  if (!Number.isInteger(n)) throw new TypeError("Input must be an integer");
  if (n < 0) {
    throw new RangeError("Factorial is not defined for negative numbers");
  }
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // Output: 120
console.log(factorial(0)); // Output: 1
```

**Explanation**: The textbook recursive version. Results above `factorial(170)` overflow to `Infinity`, and very large inputs can blow the call stack.

**Complexity**: Time O(n), space O(n) for the call stack.

<sup>[Back to top](#algorithms)</sup>

### Find the GCD (Greatest Common Divisor)

```js
/**
 * Finds the greatest common divisor using the Euclidean algorithm
 * @param {number} a - First number
 * @param {number} b - Second number
 * @returns {number} The GCD of a and b
 */
function gcd(a, b) {
  if (!Number.isInteger(a) || !Number.isInteger(b)) {
    throw new TypeError("Both inputs must be integers");
  }
  a = Math.abs(a);
  b = Math.abs(b);
  return b === 0 ? a : gcd(b, a % b);
}

console.log(gcd(48, 18)); // Output: 6
console.log(gcd(-48, 18)); // Output: 6
```

**Explanation**: The GCD of `a` and `b` is the same as the GCD of `b` and `a % b`, so keep going until the remainder is zero. Negative inputs are converted to positive first.

**Complexity**: Time O(log(min(a, b))), space O(log(min(a, b))) for the call stack.

<sup>[Back to top](#algorithms)</sup>

### Climbing Stairs (Dynamic Programming)

```js
/**
 * Counts the distinct ways to climb n stairs taking 1 or 2 steps at a time
 * @param {number} n - Number of stairs
 * @returns {number} Number of distinct ways
 */
function climbStairs(n) {
  if (!Number.isInteger(n) || n < 0) {
    throw new TypeError("Input must be a non-negative integer");
  }
  if (n <= 1) return 1;
  let prev = 1;
  let curr = 1;
  for (let i = 2; i <= n; i++) {
    [prev, curr] = [curr, prev + curr];
  }
  return curr;
}

console.log(climbStairs(2)); // Output: 2
console.log(climbStairs(5)); // Output: 8
```

**Explanation**: The ways to reach step `i` are the ways to reach step `i - 1` plus the ways to reach step `i - 2`. That's the Fibonacci recurrence, and we only need the last two values, so there's no array to keep around. It's a good first dynamic programming problem to explain out loud.

**Complexity**: Time O(n), space O(1).

<sup>[Back to top](#algorithms)</sup>

## Searching

### Two Sum (Using Hash Map)

```js
/**
 * Finds two indices in an array whose values sum to the target
 * @param {number[]} nums - Array of numbers
 * @param {number} target - Target sum
 * @returns {number[]} Array of two indices, or empty array if not found
 */
function twoSum(nums, target) {
  if (!Array.isArray(nums) || typeof target !== "number") {
    throw new TypeError("Input must be an array and a number");
  }
  const seen = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (seen.has(complement)) return [seen.get(complement), i];
    seen.set(nums[i], i);
  }
  return [];
}

console.log(twoSum([2, 7, 11, 15], 9)); // Output: [0, 1]
console.log(twoSum([1, 2, 3], 10)); // Output: []
```

**Explanation**: For each number, check whether the value needed to reach the target has already been seen. If not, store the number and move on. The brute-force version compares every pair and is O(n²).

**Complexity**: Time O(n), space O(n).

<sup>[Back to top](#algorithms)</sup>

### Binary Search

```js
/**
 * Searches for a target in a sorted array
 * @param {number[]} arr - Sorted array to search in
 * @param {number} target - Value to search for
 * @returns {number} Index of target, or -1 if not found
 */
function binarySearch(arr, target) {
  if (!Array.isArray(arr) || typeof target !== "number") {
    throw new TypeError("Input must be an array and a number");
  }
  let left = 0;
  let right = arr.length - 1;
  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
}

console.log(binarySearch([1, 2, 3, 4, 5], 4)); // Output: 3
console.log(binarySearch([1, 2, 3, 4, 5], 9)); // Output: -1
```

**Explanation**: Compare the target to the middle element and throw away the half that can't contain it. The array has to be sorted. The `left + (right - left) / 2` form isn't needed in JavaScript, since numbers are doubles, but it's the habit interviewers in other languages look for.

**Complexity**: Time O(log n), space O(1).

<sup>[Back to top](#algorithms)</sup>

## Sorting

### Bubble Sort

```js
/**
 * Sorts an array using bubble sort with an early exit
 * @param {number[]} arr - Array to sort
 * @returns {number[]} A new sorted array
 */
function bubbleSort(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  const sorted = [...arr]; // Copy so the original isn't mutated
  for (let i = 0; i < sorted.length; i++) {
    let swapped = false;
    for (let j = 0; j < sorted.length - i - 1; j++) {
      if (sorted[j] > sorted[j + 1]) {
        [sorted[j], sorted[j + 1]] = [sorted[j + 1], sorted[j]];
        swapped = true;
      }
    }
    if (!swapped) break; // No swaps means it's already sorted
  }
  return sorted;
}

console.log(bubbleSort([5, 3, 8, 4, 2])); // Output: [2, 3, 4, 5, 8]
console.log(bubbleSort([1, 2, 3, 4, 5])); // Exits after one pass
```

**Explanation**: Repeatedly swaps adjacent elements that are out of order, so the largest remaining value drifts to the end on each pass. The `swapped` flag lets it stop early on sorted input. It's slow and you'd never ship it, but it's easy to explain.

**Complexity**: Time O(n²) average and worst case, O(n) best case. Space O(n) because of the copy (O(1) if you sort in place).

<sup>[Back to top](#algorithms)</sup>

### Merge Sort

```js
/**
 * Sorts an array using merge sort
 * @param {number[]} arr - Array to sort
 * @returns {number[]} A new sorted array
 */
function mergeSort(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  if (arr.length <= 1) return [...arr];

  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  const merged = [];
  let i = 0;
  let j = 0;
  while (i < left.length && j < right.length) {
    merged.push(left[i] <= right[j] ? left[i++] : right[j++]);
  }
  return [...merged, ...left.slice(i), ...right.slice(j)];
}

console.log(mergeSort([38, 27, 43, 3, 9, 82, 10])); // Output: [3, 9, 10, 27, 38, 43, 82]
```

**Explanation**: Split the array in half, sort each half recursively, then merge the two sorted halves. Unlike quick sort, the running time doesn't depend on the input, and it's stable because ties take from the left half first. The cost is the extra memory.

**Complexity**: Time O(n log n) in every case, space O(n).

<sup>[Back to top](#algorithms)</sup>

### Quick Sort

```js
/**
 * Sorts an array using quick sort
 * @param {number[]} arr - Array to sort
 * @returns {number[]} A new sorted array
 */
function quickSort(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  const sorted = [...arr]; // Work on a copy
  quickSortInPlace(sorted, 0, sorted.length - 1);
  return sorted;
}

function quickSortInPlace(arr, low, high) {
  if (low < high) {
    const pi = partition(arr, low, high);
    quickSortInPlace(arr, low, pi - 1);
    quickSortInPlace(arr, pi + 1, high);
  }
}

function partition(arr, low, high) {
  const pivot = arr[high];
  let i = low - 1;
  for (let j = low; j < high; j++) {
    if (arr[j] < pivot) {
      i++;
      [arr[i], arr[j]] = [arr[j], arr[i]];
    }
  }
  [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]];
  return i + 1;
}

console.log(quickSort([3, 6, 8, 10, 1, 2, 1])); // Output: [1, 1, 2, 3, 6, 8, 10]
```

**Explanation**: Picks the last element as the pivot, moves everything smaller to its left (the Lomuto partition scheme), and recurses on each side. It sorts in place, so it needs far less memory than merge sort. Always picking the last element means already-sorted input hits the O(n²) worst case. Picking a random or median-of-three pivot avoids that.

**Complexity**: Time O(n log n) average, O(n²) worst case. Space O(log n) average for the call stack, O(n) worst case.

<sup>[Back to top](#algorithms)</sup>

### Merge Two Sorted Arrays

```js
/**
 * Merges two sorted arrays into one sorted array
 * @param {number[]} arr1 - First sorted array
 * @param {number[]} arr2 - Second sorted array
 * @returns {number[]} Merged sorted array
 */
function mergeSortedArrays(arr1, arr2) {
  if (!Array.isArray(arr1) || !Array.isArray(arr2)) {
    throw new TypeError("Both inputs must be arrays");
  }
  const merged = [];
  let i = 0;
  let j = 0;
  while (i < arr1.length && j < arr2.length) {
    if (arr1[i] <= arr2[j]) {
      merged.push(arr1[i++]);
    } else {
      merged.push(arr2[j++]);
    }
  }
  return [...merged, ...arr1.slice(i), ...arr2.slice(j)];
}

console.log(mergeSortedArrays([1, 3, 5], [2, 4, 6])); // Output: [1, 2, 3, 4, 5, 6]
```

**Explanation**: Keeps one pointer in each array and always takes the smaller front element. When one array runs out, the rest of the other gets appended. This is the merge step from merge sort.

**Complexity**: Time O(n + m), space O(n + m).

<sup>[Back to top](#algorithms)</sup>

## Array Utilities

### Find Maximum in Array

```js
/**
 * Finds the maximum value in an array
 * @param {number[]} arr - Array to search
 * @returns {number} The maximum value
 */
function findMax(arr) {
  if (!Array.isArray(arr) || arr.length === 0) {
    throw new TypeError("Input must be a non-empty array");
  }
  return arr.reduce((max, current) => (current > max ? current : max));
}

console.log(findMax([1, 2, 3, 4, 5])); // Output: 5
console.log(Math.max(...[1, 2, 3, 4, 5])); // Output: 5
```

**Explanation**: Walks the array once and keeps the largest value seen so far. `Math.max(...arr)` is shorter, but spreading a very large array (roughly 100,000+ items) can throw a `RangeError` because of the argument limit. `reduce()` doesn't have that problem.

**Complexity**: Time O(n), space O(1).

<sup>[Back to top](#algorithms)</sup>

### Maximum Subarray (Kadane)

```js
/**
 * Finds the largest sum of any contiguous subarray
 * @param {number[]} nums - Non-empty array of numbers
 * @returns {number} The largest subarray sum
 */
function maxSubarray(nums) {
  if (!Array.isArray(nums) || nums.length === 0) {
    throw new TypeError("Input must be a non-empty array");
  }
  let current = nums[0];
  let best = nums[0];
  for (let i = 1; i < nums.length; i++) {
    current = Math.max(nums[i], current + nums[i]);
    best = Math.max(best, current);
  }
  return best;
}

console.log(maxSubarray([-2, 1, -3, 4, -1, 2, 1, -5, 4])); // Output: 6 ([4, -1, 2, 1])
console.log(maxSubarray([-3, -1, -2])); // Output: -1
```

**Explanation**: At each element, decide whether to extend the running subarray or start a new one at this element. Track the best sum seen along the way. Starting `current` and `best` at the first element, not zero, keeps it correct for all-negative arrays.

**Complexity**: Time O(n), space O(1).

<sup>[Back to top](#algorithms)</sup>

### Flatten a Nested Array

```js
/**
 * Flattens an array of any nesting depth
 * @param {Array} arr - Array that may contain nested arrays
 * @returns {Array} A new, flat array
 */
function flatten(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  const result = [];
  for (const item of arr) {
    if (Array.isArray(item)) {
      result.push(...flatten(item));
    } else {
      result.push(item);
    }
  }
  return result;
}

console.log(flatten([1, [2, [3, [4]], 5]])); // Output: [1, 2, 3, 4, 5]
console.log([1, [2, [3, [4]], 5]].flat(Infinity)); // Built-in equivalent
```

**Explanation**: Recurse into anything that's an array and push everything else. In real code, use `arr.flat(Infinity)`. Interviewers often ask for the manual version first.

**Complexity**: Time O(n) where n is the total number of elements, space O(n).

<sup>[Back to top](#algorithms)</sup>

## Data Structures

### Linked List

```js
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

/**
 * Singly linked list
 */
class LinkedList {
  constructor() {
    this.head = null;
  }

  /**
   * Inserts a value at the beginning. O(1).
   * @param {*} data - Value to insert
   */
  insertAtHead(data) {
    const newNode = new Node(data);
    newNode.next = this.head;
    this.head = newNode;
  }

  /**
   * Searches for a value. O(n).
   * @param {*} data - Value to search for
   * @returns {boolean} True if found
   */
  search(data) {
    let current = this.head;
    while (current) {
      if (current.data === data) return true;
      current = current.next;
    }
    return false;
  }

  /**
   * Deletes the first occurrence of a value. O(n).
   * @param {*} data - Value to delete
   */
  delete(data) {
    if (!this.head) return;
    if (this.head.data === data) {
      this.head = this.head.next;
      return;
    }
    let current = this.head;
    while (current.next) {
      if (current.next.data === data) {
        current.next = current.next.next;
        return;
      }
      current = current.next;
    }
  }

  /**
   * Reverses the list in place. O(n).
   */
  reverse() {
    let prev = null;
    let current = this.head;
    while (current) {
      const next = current.next;
      current.next = prev;
      prev = current;
      current = next;
    }
    this.head = prev;
  }

  /**
   * Converts the list to an array. O(n).
   * @returns {Array}
   */
  toArray() {
    const result = [];
    let current = this.head;
    while (current) {
      result.push(current.data);
      current = current.next;
    }
    return result;
  }
}

const list = new LinkedList();
list.insertAtHead(3);
list.insertAtHead(2);
list.insertAtHead(1);
console.log(list.toArray()); // Output: [1, 2, 3]
console.log(list.search(2)); // Output: true
list.reverse();
console.log(list.toArray()); // Output: [3, 2, 1]
list.delete(2);
console.log(list.toArray()); // Output: [3, 1]
```

**Explanation**: Each node points to the next one, so inserting at the head is cheap but finding anything means walking the list. Reversing is the classic pointer exercise. Keep track of `prev`, `current`, and `next` and rewire one node at a time.

<sup>[Back to top](#algorithms)</sup>

### Stack

```js
/**
 * Stack (last in, first out). Push, pop, and peek are all O(1).
 */
class Stack {
  constructor() {
    this.items = [];
  }

  /** Adds an element to the top */
  push(element) {
    this.items.push(element);
  }

  /** Removes and returns the top element, or undefined if empty */
  pop() {
    return this.items.pop();
  }

  /** Returns the top element without removing it, or undefined if empty */
  peek() {
    return this.items[this.items.length - 1];
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }

  clear() {
    this.items = [];
  }

  toArray() {
    return [...this.items];
  }
}

const stack = new Stack();
stack.push(10);
stack.push(20);
stack.push(30);
console.log(stack.peek()); // Output: 30
console.log(stack.pop()); // Output: 30
console.log(stack.size()); // Output: 2
```

**Explanation**: Last in, first out. It shows up in undo/redo, call stacks, and bracket matching (see [Valid Parentheses](#valid-parentheses)).

<sup>[Back to top](#algorithms)</sup>

### Queue

```js
/**
 * Queue (first in, first out). Enqueue, dequeue, and peek are all O(1).
 * Uses an object with head and tail indices so dequeue never has to
 * shift the remaining elements.
 */
class Queue {
  constructor() {
    this.items = {};
    this.head = 0;
    this.tail = 0;
  }

  /** Adds an element to the back */
  enqueue(element) {
    this.items[this.tail++] = element;
  }

  /** Removes and returns the front element, or undefined if empty */
  dequeue() {
    if (this.isEmpty()) return undefined;
    const element = this.items[this.head];
    delete this.items[this.head++];
    return element;
  }

  /** Returns the front element without removing it, or undefined if empty */
  peek() {
    return this.items[this.head];
  }

  isEmpty() {
    return this.tail === this.head;
  }

  size() {
    return this.tail - this.head;
  }

  clear() {
    this.items = {};
    this.head = 0;
    this.tail = 0;
  }

  toArray() {
    return Object.values(this.items);
  }
}

const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
console.log(queue.peek()); // Output: 1
console.log(queue.dequeue()); // Output: 1
console.log(queue.size()); // Output: 2
```

**Explanation**: First in, first out. The obvious version uses `array.shift()`, but that is O(n) because every remaining element moves down one slot. Tracking head and tail indices avoids that. For interview answers the `shift()` version is usually fine, as long as you mention the cost.

<sup>[Back to top](#algorithms)</sup>

### Binary Search Tree

```js
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

/**
 * Binary search tree. Smaller values go left, larger values go right.
 * Insert and search are O(log n) on a balanced tree and O(n) on a skewed one.
 */
class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  /** Inserts a value. Duplicates are ignored. */
  insert(value) {
    const node = new TreeNode(value);
    if (!this.root) {
      this.root = node;
      return;
    }
    let current = this.root;
    while (true) {
      if (value === current.value) return;
      if (value < current.value) {
        if (!current.left) {
          current.left = node;
          return;
        }
        current = current.left;
      } else {
        if (!current.right) {
          current.right = node;
          return;
        }
        current = current.right;
      }
    }
  }

  /** Returns true if the value is in the tree */
  contains(value) {
    let current = this.root;
    while (current) {
      if (value === current.value) return true;
      current = value < current.value ? current.left : current.right;
    }
    return false;
  }

  /** Returns values in sorted order (left, node, right) */
  inOrder() {
    const result = [];
    const visit = (node) => {
      if (!node) return;
      visit(node.left);
      result.push(node.value);
      visit(node.right);
    };
    visit(this.root);
    return result;
  }
}

const tree = new BinarySearchTree();
[8, 3, 10, 1, 6, 14].forEach((n) => tree.insert(n));
console.log(tree.contains(6)); // Output: true
console.log(tree.contains(7)); // Output: false
console.log(tree.inOrder()); // Output: [1, 3, 6, 8, 10, 14]
```

**Explanation**: Each node's left subtree holds smaller values and its right subtree holds larger ones, so search works like binary search. An in-order traversal returns the values sorted. Inserting already-sorted data produces a lopsided tree, which is why self-balancing trees exist.

<sup>[Back to top](#algorithms)</sup>

## Graph Algorithms

Both examples below use this adjacency list.

```js
const graph = {
  A: ["B", "C"],
  B: ["A", "D"],
  C: ["A", "D"],
  D: ["B", "C", "E"],
  E: ["D"],
};
```

### Depth-First Search (DFS)

```js
/**
 * Depth-first traversal, recursive
 * @param {Object} graph - Adjacency list
 * @param {string|number} start - Starting vertex
 * @returns {Array} Vertices in the order visited
 */
function dfs(graph, start) {
  if (!graph || !(start in graph)) {
    throw new TypeError("Invalid graph or start vertex");
  }
  const visited = new Set();
  const result = [];

  function explore(vertex) {
    if (visited.has(vertex)) return;
    visited.add(vertex);
    result.push(vertex);
    for (const neighbor of graph[vertex]) {
      explore(neighbor);
    }
  }

  explore(start);
  return result;
}

/**
 * Depth-first traversal, iterative with an explicit stack
 */
function dfsIterative(graph, start) {
  if (!graph || !(start in graph)) {
    throw new TypeError("Invalid graph or start vertex");
  }
  const visited = new Set();
  const result = [];
  const stack = [start];

  while (stack.length > 0) {
    const vertex = stack.pop();
    if (visited.has(vertex)) continue;
    visited.add(vertex);
    result.push(vertex);
    // Push in reverse so the first neighbor is popped first
    for (let i = graph[vertex].length - 1; i >= 0; i--) {
      if (!visited.has(graph[vertex][i])) stack.push(graph[vertex][i]);
    }
  }
  return result;
}

console.log(dfs(graph, "A")); // Output: ["A", "B", "D", "C", "E"]
console.log(dfsIterative(graph, "A")); // Output: ["A", "B", "D", "C", "E"]
```

**Explanation**: Follows one path as far as it goes, then backs up and tries the next. The recursive version is shorter. The iterative version avoids stack overflow on very deep graphs. It's the building block for cycle detection, topological sort, and finding connected components.

**Complexity**: Time O(V + E), space O(V).

<sup>[Back to top](#algorithms)</sup>

### Breadth-First Search (BFS)

```js
/**
 * Breadth-first traversal
 * @param {Object} graph - Adjacency list
 * @param {string|number} start - Starting vertex
 * @returns {Array} Vertices in the order visited, level by level
 */
function bfs(graph, start) {
  if (!graph || !(start in graph)) {
    throw new TypeError("Invalid graph or start vertex");
  }
  const visited = new Set([start]);
  const queue = [start];
  const result = [];

  // Walk the array with an index instead of shift(), which is O(n)
  for (let head = 0; head < queue.length; head++) {
    const vertex = queue[head];
    result.push(vertex);
    for (const neighbor of graph[vertex]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
  return result;
}

/**
 * Finds the shortest path (fewest edges) between two vertices
 * @returns {Array|null} The path, or null if there isn't one
 */
function bfsShortestPath(graph, start, end) {
  if (!graph || !(start in graph) || !(end in graph)) {
    throw new TypeError("Invalid graph or vertices");
  }
  if (start === end) return [start];

  const visited = new Set([start]);
  const queue = [[start]];

  for (let head = 0; head < queue.length; head++) {
    const path = queue[head];
    const vertex = path[path.length - 1];

    for (const neighbor of graph[vertex]) {
      if (neighbor === end) return [...path, neighbor];
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push([...path, neighbor]);
      }
    }
  }
  return null;
}

console.log(bfs(graph, "A")); // Output: ["A", "B", "C", "D", "E"]
console.log(bfsShortestPath(graph, "A", "E")); // Output: ["A", "B", "D", "E"]
```

**Explanation**: Visits every vertex one step away, then every vertex two steps away, and so on. That's why it finds shortest paths in unweighted graphs. It uses a queue where DFS uses a stack. The path-copying in `bfsShortestPath` is simple but costs extra memory. For big graphs, store a `parent` map and rebuild the path at the end.

**Complexity**: Time O(V + E), space O(V).

<sup>[Back to top](#algorithms)</sup>

## Utility Functions

### Debounce Function

```js
/**
 * Delays calling fn until `delay` ms have passed since the last call
 * @param {Function} fn - Function to debounce
 * @param {number} delay - Delay in milliseconds
 * @returns {Function} Debounced function
 */
function debounce(fn, delay) {
  if (typeof fn !== "function" || typeof delay !== "number") {
    throw new TypeError("First argument must be a function, second a number");
  }
  let timerId;
  return function (...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => fn.apply(this, args), delay);
  };
}

const log = debounce(() => console.log("Debounced!"), 300);
log();
log();
log(); // Logs once, 300ms after the last call
```

**Explanation**: Every call cancels the previous timer and starts a new one, so the function only runs once things go quiet. Typical uses are search-as-you-type inputs and window resize handlers.

<sup>[Back to top](#algorithms)</sup>

### Throttle Function

```js
/**
 * Lets fn run at most once every `limit` ms
 * @param {Function} fn - Function to throttle
 * @param {number} limit - Minimum time between calls in milliseconds
 * @returns {Function} Throttled function
 */
function throttle(fn, limit) {
  if (typeof fn !== "function" || typeof limit !== "number") {
    throw new TypeError("First argument must be a function, second a number");
  }
  let lastCall = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastCall >= limit) {
      lastCall = now;
      return fn.apply(this, args);
    }
  };
}

const onScroll = throttle(() => console.log("Scrolled!"), 200);
onScroll(); // Runs immediately
onScroll(); // Ignored, still inside the 200ms window
```

**Explanation**: Debounce waits for calls to stop. Throttle lets the first call through and ignores the rest until the time window passes. Use it for things that fire constantly, like scroll and mousemove handlers. This version only runs on the leading edge, so a final call inside the window gets dropped.

<sup>[Back to top](#algorithms)</sup>
