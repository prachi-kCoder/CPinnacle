# BITWISE OPERATIONS :
KEEP IN MIND : 

- Left Shift {<<}	Shifts bits left (multiplies by 2^n)
- Right Shift {<<}	Shifts bits right (divides by 2^n )

## Important Bitwise Identities
a) Relationship Between XOR, OR, and AND :-
### (A⊕B)+(A∣B)+(A&B) = 2×(A∣B)
Proof : A⊕B = A+B−2×(A&B)    {As only pair of opposite bits are to be taken 1-0 or 0-1}
        A∣B = A+B−(A&B)   {Eliminating the common part once as it may have been considered twice while adding A - B} 
        (A⊕B)+(A∣B)=(A+B−2(A&B))+(A+B−(A&B)) =  2A+2B−3(A&B)
        Add (A&B) :
            2A+2B−2(A&B) = 2(A+B−(A&B)) = 2(A∣B) = RHS 

b) XOR and Addition :-
### A+B = (A⊕B) + 2×(A&B)

c) Identity:   A⊕0=A   #XOR WITH 0 have no impact 

d) Self-Inverse: A⊕A=0  # All bits equal in both A , A 

e) Swap Trick:   (Swaps A and B without a temporary variable)
### A⊕=B  ; B⊕=A ;   A⊕=B 

f)  OR and AND Properties :
  ### OR is monotonic: A∣B ≥ max(A,B)
  ### AND is monotonic: A&B ≤ min(A,B)
  ### Distributive Laws: A&(B∣C) = (A&B)∣(A&C)      , A∣(B&C) = (A∣B)&(A∣C)


  Example problem : (ICPC ALGO QUIZ) :-
  Q) You are given a positive integers array A1,A2,...,AN of length N.
Find the number of unordered pairs (i,j) such that i<j and the expression (Ai⊕Aj) + (Ai|Aj) + (Ai&Aj) is divisible by 4.
Here, ⊕ denotes the Bitwise XOR operation, | denotes the Bitwise OR operation and & denotes the Bitwise AND operation.

## INTUITION : 
  Here (Ai⊕Aj) + (Ai|Aj) + (Ai&Aj) = 2(Ai|Aj)   -> is divisible by 4.
      2(Ai|Aj) / 4
  That is (Ai|Aj)  -> Divisible by 2
   Only possible if LSB of both Ai and Aj is 0  otherwise the OR gives 1 on LSB and that will be odd number
  Then Ai -> Even , Aj -> Even
  
  Problem boils down to : Get the count of pairs of Ai , Aj with both are evens
  That is get the count of all even numbers and forms all possible pairs 

```cpp
#include <bits/stdc++.h>  // Includes all standard libraries (efficient for CP)
using namespace std;

int main() {
    ios_base::sync_with_stdio(false); // Improves input/output efficiency
    cin.tie(NULL); // Prevents unnecessary flushing of output
    
    int t; // Number of test cases
    cin >> t;
    while (t--) { // Loop through all test cases
        int n; // Number of elements in the array
        cin >> n;

        int even_count = 0; // Count of even numbers in the array

        // Reading the array and counting even numbers
        for (int i = 0; i < n; i++) {
            int num;
            cin >> num;
            if (num % 2 == 0) { // Check if the number is even
                even_count++;
            }
        }

        // Calculate the number of pairs that can be formed using even numbers
        long long pairs = (long long)even_count * (even_count - 1) / 2;
        
        cout << pairs << '\n'; // Output result
    }
    
    return 0;
}
```

  
