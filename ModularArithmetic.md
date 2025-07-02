# MODULAR ARITHMETIC 
## 🧮 Problem: Efficient Computation of 𝑛𝐶𝑟 mod m
- When 𝑛 and 𝑟 are large (up to 10^6 or more), naive factorial and division become infeasible because:
- Factorials grow fast → risk overflow
- Division in modular arithmetic isn't straightforward

###  🧠 Key Mathematical Concepts Involved: 
## 1. Modular Arithmetic :
  ###  nCr mod m  = n! /(r! (n-r)!) mod m
- But since direct division doesn't exist under modulo, we replace division with multiplicative inverse:
  ### a/b mod m => a*modInv(b) mod m
  
## 2. Fermat’s Little Theorem :
  When 𝑚 is prime (which 10^9+7 is), we get:
  ### a-1 mod m = a^(m-2) mod m 
🏗️ Code Structure Breakdown
```cpp
ll modInv(ll num) {
    return modExpo(num, mod - 2); // because mod is prime
}
```
## 3. Modular Exponentiation
Used to compute 𝑎^(b) mod m efficiently in 𝑂 (log b) using exponentiation by squaring:
```cpp
ll modExpo(ll a, ll b) {
    ll res = 1;
    a %= mod;
    while (b > 0) {
        if (b & 1) res = (res * a) % mod; // odd power handling
        a = (a * a) % mod; // square the base {split num^4 -> num^2 * num^2}
        b >>= 1;
    }
    return res;
}
```

## 4. Precomputation using Factorials
Builds factorials up to 𝑛! mod 𝑚
```cpp
invFact[n] = modInv(fact[n]);
invFact[i] = ((i + 1) * invFact[i + 1]) % mod;  // Recursive backward
```
### 1/i!  =  1/ (i+1)! .(i+1) 

## 5. 🧩 nCr(n, r)
 nCr(n,r) = n! / r!*(n-r)! mod m  = fact[n].invFact[r].invFact[n-r] mod m 

 ### COMPLETE CODE :- 
 ```cpp
#include <bits/stdc++.h>
#include <iostream>
#define ll long long 
using namespace std ;
const int mod = 1e9 + 7 ;
vector<ll> fact , invFact ;
ll modExpo(ll a, ll b) {
    ll res = 1 ;
    a = a%mod ;
    while (b > 0) {
        if (b&1) res = (res*a)%mod ;
        a = (a*a)%mod ;
        b >>= 1 ;
    }
    return res ;
}
ll modInv(ll num) {
    return modExpo(num  , mod-2) ;
}
void preCompute(int n ){
    fact.resize(n+1) ; invFact.resize(n+1) ;
    fact[0] = 1LL ;
    for (int i = 1 ; i<=n ; i++) {
        fact[i] = (i*fact[i-1])%mod ;
    }
    invFact[n] = modInv(fact[n]) ;
    for (int i = n-1 ; i>= 0 ; i--) {
        invFact[i] = ((i+1)*invFact[i+1])%mod ;
    }
}

ll nCr(ll n , ll r) {
    return (((fact[n] * invFact[r])%mod)*invFact[n-r])%mod ;
}
int main() {
    ll n , r;
    cin >> n >> r ;
    if (n >= r) {
        preCompute(n) ;
        cout << nCr(n,r) << endl ;
    }else {
        cout << "Invalid entries !" << endl ;
    }
    return 0;
}
```
  
