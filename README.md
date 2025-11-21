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
 * Sorts an array using bubble sort algorithm
 * @param {number[]} arr - Array to sort
 * @returns {number[]} Sorted array
 */
function bubbleSort(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  const sorted = [...arr]; // Create copy to avoid mutating original
  for (let i = 0; i < sorted.length; i++) {
    for (let j = 0; j < sorted.length - i - 1; j++) {
      if (sorted[j] > sorted[j + 1]) {
        [sorted[j], sorted[j + 1]] = [sorted[j + 1], sorted[j]];
      }
    }
  }
  return sorted;
}

console.log(bubbleSort([5, 3, 8, 4, 2])); // Output: [2, 3, 4, 5, 8]
```

**Explanation**: Sorts an array by repeatedly swapping adjacent elements. Modern version uses array spread to avoid mutating the original array.

<sup>[Back to top](#algorithms)</sup>

### Quick Sort

```js
/**
 * Sorts an array using quick sort algorithm
 * @param {number[]} arr - Array to sort
 * @returns {number[]} Sorted array
 */
function quickSort(arr) {
  if (!Array.isArray(arr)) throw new TypeError("Input must be an array");
  if (arr.length <= 1) return arr;
  const pivot = arr[Math.floor(arr.length / 2)];
  const left = arr.filter((x) => x < pivot);
  const middle = arr.filter((x) => x === pivot);
  const right = arr.filter((x) => x > pivot);
  return [...quickSort(left), ...middle, ...quickSort(right)];
}

console.log(quickSort([3, 6, 8, 10, 1, 2, 1])); // Output: [1, 1, 2, 3, 6, 8, 10]
```

**Explanation**: A divide-and-conquer sorting algorithm with `O(n log n)` average-case complexity. Uses middle pivot to handle duplicates better, and maintains immutability.

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