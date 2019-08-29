# Sorting and Searching

## Common Sorting Algorithms

### Bubble Sort | Runtime: O(n^2) average and worst case. Memory: O(1).

In bubble sort, we start at the beginning of the array and swap the first two elements if the first is greater than the second. Then, we go to the next pair, and so on, continuously making sweeps of the array until it is sorted. In doing so, the smaller items slowly"bubble" up to the beginning of the list.

### Selection Sort I Runtime: O(n^2) average and worst case. Memory: O(1).

Find the smallest element using a linear scan and move it to the front (swapping it with the front element). Then, find the second smallest and move it, again doing a linear scan. Continue doing this until all the elements are in place.

### Merge Sort I Runtime: O(nlog (n)) average and worst case. Memory: O(n)

Merge sort divides the array in half, sorts each of those halves, and then merges them back together. Each of those halves has the same sorting algorithm applied to it. Eventually, you are merging just two single element arrays. It is the "merge" part that does all the heavy lifting.

```java
void mergesort(int[] array) {
	int[] helper = new int[array.length];
	mergesort(array, helper, 0, array.length - 1);
}

void mergesort(int[] array, int[] helper, int low, int high) {
	if (low< high) {
		int middle = (low + high)/ 2;
	}
	mergesort(array, helper, low, middle); // Sort left half
	mergesort(array, helper, middle+l, high); // Sort right half
	merge(array, helper, low, middle, high); // Merge them
}

void merge(int[] array, int[] helper, int low, int middle, int high) {
/* Copy both halves into a helper array*/
	for (int i= low; i <= high; i++) {
		helper[i] = array[i];
	}
	int helperleft = low;
	int helperRight = middle + l;
	int current = low;
/* Iterate through helper array. Compare the left and right half, copying back
* the smaller element from the two halves into the original array. */
	while (helperLeft <= middle && helperRight <= high) {
		if (helper[helperleft] <= helper[helperRight]) {
			array[current] = helper[helperleft];
			helperleft++;
		} else {//If right element is smaller than left element
			array[current] = helper[helperRight];
			helperRight++;
		}
		current++;
	}
/* Copy the rest of the left side of the array into the target array*/
	int remaining= middle - helperleft;
	for (int i= 0; i <= remaining; i++) {
		array[current + i] = helper[helperleft + i];
	}// we don't need to copy the right because they are already there
}
```



### Quick Sort I Runtime: O(nlog( n)) average, O(n^2) worst case. Memory: O(log(n)).

In quick sort we pick a random element and partition the array, such that all numbers that are less than the partitioning element come before all elements that are greater than it.

If we repeatedly partition the array (and its sub-arrays) around an element, the array will eventually become sorted. However, as the partitioned element is not guaranteed to be the median (or anywhere near the median), our sorting could be very slow. This is the reason for the 0( n2) worst case runtime.

```java
void quickSort(int[] arr, int left, int right) {
	int index = partition(arr, left, right);
	if (left< index - 1) { // Sort left half
		quickSort(arr, left, index - 1);
	}
	if (index< right) { // Sort right half
		quickSort(arr, index, right);
	}
}

int partition(int[] arr, int left, int right) {
	int pivot = arr[(left + right) / 2]; // Pick pivot point
	while (left<= right) {
		// Find element on left that should be on right
		while (arr[left] < pivot) left++;
		// Find element on right that should be on left
		while (arr[right] > pivot) right--;
		//Swap elements, and move left and right indices
		if (left<= right) {
			swap(arr, left, right); // swaps elements
			left++;
			right--;
		}
	}
	return left;
}
```



### Radix Sort I Runtime: O(kn)

Radix sort is a sorting algorithm for integers (and some other data types) that takes advantage of the fact that integers have a finite number of bits. In radix sort, we iterate through each digit of the number, grouping numbers by each digit. For example, if we have an array of integers, we might first sort by the
first digit, so that the `0s` are grouped together. Then, we sort each of these groupings by the next digit. We repeat this process sorting by each subsequent digit, until finally the whole array is sorted.

Unlike comparison sorting algorithms, which cannot perform better than `O(n log(n))` in the average case, radix sort has a runtime of `O(kn)`, where `n` is the number of elements and `k` is the number of passes of the sorting algorithm.



## Searching Algorithms (Binary Search)

### Iterative Solution

```java
int binarySearch(int[] a, int x) {
	int low = 0;
	int high = a.length - 1;
	int mid;
	while (low <= high) {
		mid = (low + high)/ 2;
		if (a[mid] < x) {
			low = mid + 1;
		} else if (a[mid] > x) {
			high= mid - 1;
		} else {
			return mid;
		}
	}
	return -1; // Error
}
```



### Recursive Solution

```java
int binarySearchRecursive(int[] a, int x, int low, int high) {
	if (low > high) return -1; // Error
	
    int mid (low + high) / 2;
	if (a[mid] < x) {
		return binarySearchRecursive(a, x, mid + 1, high);
	} else if (a[mid] > x) {
		return binarySearchRecursive(a, x, low, mid - 1);
	} else {
		return mid;
	}
}
```

