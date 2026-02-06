# JavaScript Algorithms (Grouped by Type)

This repository contains a curated list of JavaScript algorithms, organized by category. These range from simple string manipulation to advanced searching and sorting techniques — perfect for interviews and foundational learning.

> [!Note]
> Popularity is based on common interview topics, educational materials, and developer community usage.

## Algorithms

### String Manipulation

- [Reverse a String](#reverse-a-string)
- [Palindrome Check](#palindrome-check)
- [Character Frequency Counter](#character-frequency-counter)
- [Anagram Check](#anagram-check)

### Math & Number Theory

- [Prime Number Check](#prime-number-check)
- [Fibonacci Sequence (Recursive)](#fibonacci-sequence-recursive)
- [Factorial of a Number](#factorial-of-a-number)
- [Find the GCD (Greatest Common Divisor)](#find-the-gcd-greatest-common-divisor)

### Searching

- [Two Sum (Using Hash Map)](#two-sum-using-hash-map)
- [Binary Search](#binary-search)

### Sorting

- [Bubble Sort](#bubble-sort)
- [Quick Sort](#quick-sort)
- [Merge Two Sorted Arrays](#merge-two-sorted-arrays)

### Array Utilities

- [Find Maximum in Array](#find-maximum-in-array)

### Data Structures

- [Linked List](#linked-list)
- [Stack](#stack)
- [Queue](#queue)

### Graph Algorithms

- [Depth-First Search (DFS)](#depth-first-search-dfs)
- [Breadth-First Search (BFS)](#breadth-first-search-bfs)

### Utility Functions

- [Debounce Function](#debounce-function)

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

**Explanation**: Reverses the characters in a string using the spread operator (modern alternative to split) and reverse. Includes type validation.

<sup>[Back to top](#algorithms)</sup>

### Palindrome Check

```js
/**
 * Checks if a string is a palindrome (case-insensitive, ignores spaces)
 * @param {string} str - The string to check
 * @returns {boolean} True if palindrome, false otherwise
 */
function isPalindrome(str) {
  if (typeof str !== "string") throw new TypeError("Input must be a string");
  const cleaned = str.toLowerCase().replace(/\s+/g, "");
  return cleaned === [...cleaned].reverse().join("");
}

console.log(isPalindrome("racecar")); // Output: true
console.log(isPalindrome("A man a plan a canal Panama")); // Output: true
```

**Explanation**: Determines if a string reads the same backward as forward. Now handles case-insensitivity and ignores spaces for real-world usage.

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

**Explanation**: Counts how often each character appears in a string using modern `reduce()` and nullish coalescing (`??`). More functional approach.

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

**Explanation**: Determines if two strings are anagrams by normalizing case/spaces and comparing sorted characters. Handles real-world edge cases.

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
```

**Explanation**: Checks if a number is prime using an optimized approach with integer validation. Eliminates multiples of 2 and 3 for efficiency.

<sup>[Back to top](#algorithms)</sup>

### Fibonacci Sequence (Recursive)

```js
/**
 * Generates the nth Fibonacci number using memoization
 * @param {number} n - The position in Fibonacci sequence
 * @param {Map} memo - Cache for memoization
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
console.log(fibonacci(50)); // Output: 12586269025 (efficient with memoization)
```

**Explanation**: Generates the nth Fibonacci number recursively with memoization for efficiency. Solves the exponential time complexity problem of naive recursion. Time complexity: `O(n)` instead of `O(2^n)`.

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
  if (n < 0)
    throw new RangeError("Factorial is not defined for negative numbers");
  if (n === 0 || n === 1) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // Output: 120
```

**Explanation**: Calculates the factorial of a number recursively with comprehensive input validation using modern type-checking.

<sup>[Back to top](#algorithms)</sup>

### Find the GCD (Greatest Common Divisor)

```js
/**
 * Finds the greatest common divisor using Euclidean algorithm
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

**Explanation**: Uses the Euclidean algorithm with support for negative numbers and type validation.

<sup>[Back to top](#algorithms)</sup>

## Searching

### Two Sum (Using Hash Map)

```js
/**
 * Finds two indices in array whose values sum to target
 * @param {number[]} nums - Array of numbers
 * @param {number} target - Target sum
 * @returns {number[]} Array of two indices, or empty array if not found
 */
function twoSum(nums, target) {
  if (!Array.isArray(nums) || typeof target !== "number") {
    throw new TypeError("Input must be an array and a number");
  }
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (map.has(complement)) return [map.get(complement), i];
    map.set(nums[i], i);
  }
  return [];
}

console.log(twoSum([2, 7, 11, 15], 9)); // Output: [0, 1]
```

**Explanation**: Finds two indices whose values sum to target using a Map (hash map) for O(n) time complexity. Includes input validation.

<sup>[Back to top](#algorithms)</sup>

### Binary Search

```js
/**
 * Searches for target in a sorted array using binary search
 * @param {number[]} arr - Sorted array to search in
 * @param {number} target - Value to search for
 * @returns {number} Index of target, or -1 if not found
 */
function binarySearch(arr, target) {
  if (!Array.isArray(arr) || typeof target !== "number") {
    throw new TypeError("Input must be an array and a number");
  }
  let left = 0,
    right = arr.length - 1;
  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
}

console.log(binarySearch([1, 2, 3, 4, 5], 4)); // Output: 3
```

**Explanation**: Searches for target in sorted array using divide-and-conquer. Uses `left + Math.floor((right - left) / 2)` to avoid overflow issues.

<sup>[Back to top](#algorithms)</sup>

## Sorting

### Bubble Sort

```js
/**
 * Sorts an array using bubble sort algorithm with early exit optimization
 * @param {number[]} arr - Array to sort
 * @returns {number[]} Sorted array
 * Time Complexity: O(n²) worst/average case, O(n) best case
 * Space Complexity: O(n)
 */
function bubbleSort(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  const sorted = [...arr]; // Create copy to avoid mutating original
  for (let i = 0; i < sorted.length; i++) {
    let swapped = false; // Optimization: early exit if already sorted
    for (let j = 0; j < sorted.length - i - 1; j++) {
      if (sorted[j] > sorted[j + 1]) {
        [sorted[j], sorted[j + 1]] = [sorted[j + 1], sorted[j]];
        swapped = true;
      }
    }
    if (!swapped) break; // Array is already sorted
  }
  return sorted;
}

console.log(bubbleSort([5, 3, 8, 4, 2])); // Output: [2, 3, 4, 5, 8]
console.log(bubbleSort([1, 2, 3, 4, 5])); // Exits early (already sorted)
```

**Explanation**: Sorts by repeatedly swapping adjacent elements. Optimized with a `swapped` flag to exit early when the array is already sorted, improving best-case complexity to O(n).

<sup>[Back to top](#algorithms)</sup>

### Quick Sort

```js
/**
 * Sorts an array using in-place quick sort algorithm
 * @param {number[]} arr - Array to sort
 * @returns {number[]} Sorted array
 * Time Complexity: O(n log n) average, O(n²) worst case
 * Space Complexity: O(log n) due to recursion stack
 */
function quickSort(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  const sorted = [...arr]; // Work on copy to avoid mutation
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

**Explanation**: Divide-and-conquer in-place sorting with Hoare partition scheme. Uses O(log n) space instead of O(n), making it more memory-efficient. Better for interview discussions about space optimization.

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
  let i = 0,
    j = 0;
  while (i < arr1.length && j < arr2.length) {
    if (arr1[i] < arr2[j]) {
      merged.push(arr1[i++]);
    } else {
      merged.push(arr2[j++]);
    }
  }
  return [...merged, ...arr1.slice(i), ...arr2.slice(j)];
}

console.log(mergeSortedArrays([1, 3, 5], [2, 4, 6])); // Output: [1, 2, 3, 4, 5, 6]
```

**Explanation**: Merges two sorted arrays efficiently in O(n + m) time. Modern syntax with spread operator.

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
  return Math.max(...arr);
}

// Alternative for very large arrays (avoids stack overflow):
function findMaxAlternative(arr) {
  if (!Array.isArray(arr) || arr.length === 0) {
    throw new TypeError("Input must be a non-empty array");
  }
  return arr.reduce((max, current) => (current > max ? current : max));
}

console.log(findMax([1, 2, 3, 4, 5])); // Output: 5
```

**Explanation**: Finds the largest number. Now includes validation and an alternative using `reduce()` for very large arrays to avoid stack overflow from spread operator.

<sup>[Back to top](#algorithms)</sup>

## Utility Functions

### Debounce Function

```js
/**
 * Creates a debounced function that delays execution
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

// Modern async version with promises
async function debounceAsync(fn, delay) {
  if (typeof fn !== "function" || typeof delay !== "number") {
    throw new TypeError("First argument must be a function, second a number");
  }
  let timerId;
  return function (...args) {
    return new Promise((resolve) => {
      clearTimeout(timerId);
      timerId = setTimeout(() => {
        resolve(fn.apply(this, args));
      }, delay);
    });
  };
}

const log = debounce(() => console.log("Debounced!"), 300);
log();
log();
log(); // Logs once after 300ms of inactivity
```

**Explanation**: Limits rate at which a function fires. Classic callback version and modern async/Promise version for modern use cases. Includes parameter validation.

<sup>[Back to top](#algorithms)</sup>

## Data Structures

### Linked List

```js
/**
 * Node class for LinkedList
 */
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

/**
 * Singly Linked List implementation
 */
class LinkedList {
  constructor() {
    this.head = null;
  }

  /**
   * Inserts a value at the beginning
   * @param {*} data - Value to insert
   * Time Complexity: O(1)
   */
  insertAtHead(data) {
    const newNode = new Node(data);
    newNode.next = this.head;
    this.head = newNode;
  }

  /**
   * Searches for a value in the list
   * @param {*} data - Value to search for
   * @returns {boolean} True if found
   * Time Complexity: O(n)
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
   * Deletes first occurrence of a value
   * @param {*} data - Value to delete
   * Time Complexity: O(n)
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
   * Reverses the linked list in-place
   * Time Complexity: O(n)
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
   * Converts list to array for easy viewing
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

// Usage
const list = new LinkedList();
list.insertAtHead(3);
list.insertAtHead(2);
list.insertAtHead(1);
console.log(list.toArray()); // Output: [1, 2, 3]
console.log(list.search(2)); // Output: true
list.reverse();
console.log(list.toArray()); // Output: [3, 2, 1]
```

**Explanation**: Fundamental data structure with insert, search, delete, and reverse operations. Essential for interviews to understand pointers and node traversal.

<sup>[Back to top](#algorithms)</sup>

### Stack

```js
/**
 * Stack implementation (Last-In-First-Out)
 * Time Complexity: O(1) for push/pop/peek
 * Space Complexity: O(n)
 */
class Stack {
  constructor() {
    this.items = [];
  }

  /**
   * Adds element to top of stack
   * @param {*} element - Value to push
   */
  push(element) {
    this.items.push(element);
  }

  /**
   * Removes and returns element from top
   * @returns {*} Removed element or undefined
   */
  pop() {
    return this.items.length === 0 ? undefined : this.items.pop();
  }

  /**
   * Views top element without removing
   * @returns {*} Top element or undefined
   */
  peek() {
    return this.items.length === 0
      ? undefined
      : this.items[this.items.length - 1];
  }

  /**
   * Checks if stack is empty
   * @returns {boolean}
   */
  isEmpty() {
    return this.items.length === 0;
  }

  /**
   * Returns size of stack
   * @returns {number}
   */
  size() {
    return this.items.length;
  }

  /**
   * Clears the stack
   */
  clear() {
    this.items = [];
  }

  /**
   * Converts stack to array
   * @returns {Array}
   */
  toArray() {
    return [...this.items];
  }
}

// Usage
const stack = new Stack();
stack.push(10);
stack.push(20);
stack.push(30);
console.log(stack.peek()); // Output: 30
console.log(stack.pop()); // Output: 30
console.log(stack.size()); // Output: 2
```

**Explanation**: LIFO data structure critical for parsing, undo/redo, and function call management. Classic interview topic with applications in parenthesis matching and expression evaluation.

<sup>[Back to top](#algorithms)</sup>

### Queue

```js
/**
 * Queue implementation (First-In-First-Out)
 * Time Complexity: O(1) for enqueue/dequeue/peek
 * Space Complexity: O(n)
 */
class Queue {
  constructor() {
    this.items = [];
  }

  /**
   * Adds element to back of queue
   * @param {*} element - Value to enqueue
   */
  enqueue(element) {
    this.items.push(element);
  }

  /**
   * Removes and returns element from front
   * @returns {*} Removed element or undefined
   */
  dequeue() {
    return this.items.length === 0 ? undefined : this.items.shift();
  }

  /**
   * Views front element without removing
   * @returns {*} Front element or undefined
   */
  peek() {
    return this.items.length === 0 ? undefined : this.items[0];
  }

  /**
   * Checks if queue is empty
   * @returns {boolean}
   */
  isEmpty() {
    return this.items.length === 0;
  }

  /**
   * Returns size of queue
   * @returns {number}
   */
  size() {
    return this.items.length;
  }

  /**
   * Clears the queue
   */
  clear() {
    this.items = [];
  }

  /**
   * Converts queue to array
   * @returns {Array}
   */
  toArray() {
    return [...this.items];
  }
}

// Usage
const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
console.log(queue.peek()); // Output: 1
console.log(queue.dequeue()); // Output: 1
console.log(queue.size()); // Output: 2
```

**Explanation**: FIFO data structure essential for BFS, task scheduling, and buffering. Note: JavaScript arrays' `shift()` is O(n), so for production use a circular array or linked-list implementation.

<sup>[Back to top](#algorithms)</sup>

## Graph Algorithms

### Depth-First Search (DFS)

```js
/**
 * Depth-First Search traversal
 * @param {Object} graph - Adjacency list representation
 * @param {string|number} start - Starting vertex
 * @returns {Array} Order of visited vertices
 * Time Complexity: O(V + E) where V = vertices, E = edges
 * Space Complexity: O(V) for recursion stack
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

// Iterative DFS with explicit stack
function dfsIterative(graph, start) {
  if (!graph || !(start in graph)) {
    throw new TypeError("Invalid graph or start vertex");
  }
  const visited = new Set();
  const result = [];
  const stack = [start];

  while (stack.length > 0) {
    const vertex = stack.pop();
    if (!visited.has(vertex)) {
      visited.add(vertex);
      result.push(vertex);
      // Add neighbors in reverse for left-to-right traversal
      for (let i = graph[vertex].length - 1; i >= 0; i--) {
        if (!visited.has(graph[vertex][i])) {
          stack.push(graph[vertex][i]);
        }
      }
    }
  }
  return result;
}

// Usage
const graph = {
  A: ["B", "C"],
  B: ["A", "D"],
  C: ["A", "D"],
  D: ["B", "C", "E"],
  E: ["D"],
};

console.log(dfs(graph, "A")); // Output: ["A", "B", "D", "C", "E"]
console.log(dfsIterative(graph, "A")); // Output: ["A", "C", "D", "E", "B"]
```

**Explanation**: Explores graph deeply before backtracking. Recursive version is elegant; iterative version avoids stack overflow on large graphs. Used for cycle detection, topological sorting, and connected components.

<sup>[Back to top](#algorithms)</sup>

### Breadth-First Search (BFS)

```js
/**
 * Breadth-First Search traversal
 * @param {Object} graph - Adjacency list representation
 * @param {string|number} start - Starting vertex
 * @returns {Array} Order of visited vertices (level-by-level)
 * Time Complexity: O(V + E) where V = vertices, E = edges
 * Space Complexity: O(V) for queue
 */
function bfs(graph, start) {
  if (!graph || !(start in graph)) {
    throw new TypeError("Invalid graph or start vertex");
  }
  const visited = new Set([start]);
  const queue = [start];
  const result = [];

  while (queue.length > 0) {
    const vertex = queue.shift();
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

// BFS to find shortest path
function bfsShortestPath(graph, start, end) {
  if (!graph || !(start in graph) || !(end in graph)) {
    throw new TypeError("Invalid graph or vertices");
  }
  if (start === end) return [start];

  const visited = new Set([start]);
  const queue = [[start]];

  while (queue.length > 0) {
    const path = queue.shift();
    const vertex = path[path.length - 1];

    for (const neighbor of graph[vertex]) {
      if (neighbor === end) {
        return [...path, neighbor];
      }
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push([...path, neighbor]);
      }
    }
  }
  return null; // No path found
}

// Usage
const graph = {
  A: ["B", "C"],
  B: ["A", "D"],
  C: ["A", "D"],
  D: ["B", "C", "E"],
  E: ["D"],
};

console.log(bfs(graph, "A")); // Output: ["A", "B", "C", "D", "E"]
console.log(bfsShortestPath(graph, "A", "E")); // Output: ["A", "C", "D", "E"]
```

**Explanation**: Explores graph level-by-level, ideal for finding shortest paths and closest elements. Uses queue (FIFO). Less common than DFS for most problems but essential for shortest-path and connectivity queries.

<sup>[Back to top](#algorithms)</sup>