## CONCEPT FOR THE RELATION SHIP BETWEEN THE EVEN AND ODD DIVISOR FOR A POSITIVE INTEGER 
#### For any positive integer N = 2^k * m ;  where 2^k -> Even part & m -> Odd part 

### FOR ANY POSITIVE INTEGER N , Number of odd divisor is Divisible by the No. of Even divisor
or say NO. of even divisor is multiple of No. of odd divisor
- A {No. of Odd divisor}
- B {No. of Even divisor}

- Look if we take Odd divisor = A = No. divisor of m {Odd part of N}
- Let m = (p1^a1) * (p2^a2) * (p3^a3) * (p4^a4) ....   Prime factorisation expanision

- Look for Even divisor = B  we can take any power of 2 and any odd divisor of m to form an even divisor
- Lets take d {odd divisor of m {odd part of N}}  and powers of 2 {1 to k}
- We get even divisors with d = d*(2^1)  , d*(2^2) , d*(2^3) .........d*(2^k)
- This if true for all d odd divisors of m
- Since for each odd divisor k even divisor can be formed , hence for A type of Odd divisor we get A*k type of even divisor
- Hence TOTAL ODD DIVISORS = K * (EVEN DIVISORS)
### A = K * (B)

Hence Proved ! 
```cpp
#include <bits/stdc++.h>
#define FASTIO ios_base::sync_with_stdio(0) , cin.tie(0) ,cout.tie(0)
using namespace std;

int main() {
	FASTIO ;
	int t ;
	cin >> t ;
	while (t-- > 0) {
	    long long a, b; cin >> a >> b;
	
	    if (a == 0) { // No odd divisor :(NOT possible atleast 1)
	        cout << "No" << endl ;
	        continue ;
	    }
	   // no of even div / no of odd 
	    if (b%a == 0) {
	        cout << "Yes" << endl ;
	    }else {
	        cout << "No" << endl ;
	        
	    }
	}

}
```
