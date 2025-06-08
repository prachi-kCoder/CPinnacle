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

## CONCEPT OF PREFIX XOR :-
Since XOR exhibits properties similar to addition in certain cases (specifically associativity and invertibility over cumulative computations), prefix XOR can be leveraged to retrieve XOR over any subarray in constant time. 

Precompute prefixXor[i] as the XOR of all elements from A[1] to A[i]  :
### prefixXor[𝑖]=𝐴[1]⊕𝐴[2]⊕...⊕𝐴[𝑖]  
For any subarray [L, R], the XOR can be computed in O(1) as:
        XOR[𝐿,𝑅] = prefixXor[𝑅]⊕prefixXor[𝐿−1]  
WHY ?? 
prefixXOR[R] -> xor of range [1,R]   ; prefixXOR[l-1] -> xor of range [1,L-1]
Further if prefixXor[R]^prefixXor[L-1] is done then [0,L-1] range is undergoing xor twice is Nullifies each other hence we get the xor of range [L,R] 
Eg [1,3,1,5,6,7]  for range[2,5] 
        prefixXor[5] = (1⊕3⊕1⊕5⊕6⊕7)
        prefixXor[2] = (1⊕3⊕1)
        prefixXor[2]⊕prefixXor[5] = (1⊕3⊕1⊕5⊕6⊕7) ⊕ (1⊕3⊕1) => (5⊕6⊕7)  
- 1,3,1 nullified !  

```cpp
#include <iostream>
#include <vector>
using namespace std ;
int main() {
    // RangeQuery Over xor 
    vector<int> v = {3,1,7,8,9,2,5,7,0};
    int n = v.size();
    vector<vector<int>> q = {{2,4},{6,9},{1,5},{0,6}};
    vector<int> prefixXor(n , 0) ;
    
    prefixXor[0] = v[0] ;
    for (int  i = 1 ; i< n ; i++) {
        prefixXor[i] = (prefixXor[i-1]^v[i]) ;
    }
    for (int j= 0; j < q.size() ;j++) {
        int l = q[j][0] ; int r = q[j][1] ;
        cout << "For [" <<l<<","<<r<<"] :" << (l == 0 ? prefixXor[r] : prefixXor[r] ^ prefixXor[l-1]) << endl ;
    }
    return 0;
}
```
