# Bit Manipulation

Some tricks:

- 0110 + 0110 is equivalent to 0110 * 2, which is equivalent to shifting 0110 left by 1. Using the same rationale, multiplying by 0100 is left shifting by 2.
- If you XOR a bit with its own negated value, you will always get 1.
- ~0 is a sequence of 1 s, so ~0 < < 2 is 1 s followed by two 0s. AND that with another value will clear the last two bits of the value.



## Bit Fact and Tricks

In the following expressions, x stands for a sequence

| x ^ 0s = x      | x & 0s = 0s     | x \| 0s = x      |
| --------------- | --------------- | ---------------- |
| **x ^ 1s = ~x** | **x & 1s = 1s** | **x \| 1s = 1s** |
| **x ^ x = 0**   | **x & x = x**   | **x \| x = x**   |



### Get Bit

```java
boolean get(int num, int i){
	return ((num & (1<<i))!=0);
}
```



### Set Bit

```java
int setBit(int num, int i){
	return num | (1<<i);
}
```



### Clear Bit

To clear a single bit

```java
int clearBit(int num, int i){
	int mask = ~(1<<i);
	return num & mask;
}
```



To clear all bits from the most significant bit through `i` (inclusive)

```java
int clearBitsMSBthroughI(int num, int i){
	int mask = (1<<i)-1;
	return num & mask;
}
```



To clear all bits from `i` through 0 (inclusive)

```java
int clearBitsIthough0(int num, int i){
	int mask = (-1 << (i+1));
	return num & mask;
}
```



### Update Bit

```java
int updateBit(int num, int i, boolean bitIs1){
	int value = bitIs1 ? 1 : 0;
	int mask = ~(1<<i);
	return (num & mask) | (value << i);
}
```

