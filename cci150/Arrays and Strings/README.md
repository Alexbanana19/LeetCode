# Arrays and Strings

## Hash Tables

A hash table is a data structure that maps keys to values for highly efficient lookup.

In this simple implementation, we use an **array of linked lists** and a **hash code** function.

### Insert A Key

1. Compute the key's **hash code**
2. **Map** the hash code to an index in the array. This could be done with something like hash (key)
   % array_length.
3. Solve **Collision**

*If the number of collisions is very high, the worst case runtime is `O(N)`, where N is the number of keys



### Alternative

- We can implement the hash table with a `balanced binary search tree`. This gives us an `O(log N)` lookup time. The advantage of this is potentially using **less space**, since we no longer allocate a large array. We can also iterate through the keys in order, which can be useful sometimes.



## Resizable Arrays

A typical implementation is that when the array is full, the array doubles in size. Each doubling takes `O(n)` time, but happens so rarely that its **amortized** insertion time is still `O(1)`.

### Why?

Suppose you have an array of size `N`. We can work backwards to compute how many elements we copied
at each capacity increase. Observe that when we increase the array to `K` elements, the array was previously
half that size. Therefore, we needed to copy `K/2` elements. 

- final capacity increase `n/2` elements to copy
- previous capacity increase: `n/4` elements to copy
- previous capacity increase: `n/8` elements to copy
- previous capacity increase:` n/16` elements to copy	
- ...
- second capacity increase `2` elements to copy
- first capacity increase `1` element to copy

Therefore, the total number of copies to insert `N` elements is roughly `N/2` + `N/4` + `N/8` + . . . + `2` +
`1`, which is just less than `N`.



## StringBuilder

Imagine you were concatenating a list of strings, as shown below. What would the running time of this code be? For simplicity, assume that the strings are all the same length (call this `x`) and that there are `n` strings.

````java
String joinWords(String[] words) {
	String sentence = "";
	for (String w: words) {
		sentence = sentence + w;
	}
	return sentence;
}
````

The first iteration requires us to copy `x` characters. The second iteration requires copying `2x` characters. The third iteration requires `3x`, and so on. The total time therefore is `O(x + 2x + . . . + nx)`. This reduces to `O(xn^2)`.



StringBuilder can help you avoid this problem. StringBuilder simply creates a resizable array of all the strings, copying them back to a string only when necessary.