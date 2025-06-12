# Counting SET-BIT
It seems very simple but usually its used in bit-manipulation hard questions (in combination)and we need to use the best suited for different scenarios .
![image](https://github.com/user-attachments/assets/b20fceb0-f3b4-4e97-b5a9-1f93583825c6)

🔹 1. Naive Bit-by-Bit Method :Check each bit by doing (num & 1) and right-shift until the number is zero.
TIME COMPLEXIC
```cpp
int cnt(int num) {
    int c = 0;
    while (num > 0) {
        c += (num & 1);
        num >>= 1;
    }
    return c;
}
```
🔹 2. Brian Kernighan’s Algorithm
INTUITION :
Subtracting 1 flips the rightmost set bit and all the bits to its right .
### num&(num-1) :- Remove the lowest set bit 
Eg num = 13 -> {1101}
Step 1 : 13&12  ie  {1101}&{1100} = {1100} = 12
Step 2 : 13&12  ie  {1100}&{1011} = {1000} = 8
Step 3 : 8&7  ie  {1000}&{111} = {0} = 0 
 Ans : 3 (bits) 

```cpp
int cnt(int num) {
    int c = 0;
    while (num) {
        num &= (num - 1);
        ++c;
    }
    return c;
}
```
TIME COMPLEXICITY : O(set bits) — much faster for sparse numbers (e.g. powers of two).
Keep in mind that is why we use (num&(num-1) == 0) to check the powers of 2 .

🔹 3. Built-in Popcount (GCC/Clang)  
TIME COMPLEXICITY : O(1) — hardware-accelerated

### 32 - BIT :-
Use : __builtin_popcount(num) .
### FOR LONG LONG  :-
Use : __builtin_popcountll(num) 

```cpp
cout << __builtin_popcount(num) << endl;
cout << __builtin_popcountll(num) << endl;
```
