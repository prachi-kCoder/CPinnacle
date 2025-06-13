# KADANE ALGORITHM 
Kadane's Algorithm is a powerful and efficient method to solve the Maximum Subarray Sum Problem. It helps find the contiguous subarray within a given array (containing positive, negative, or zero values) that has the largest sum.
The algorithm works by iterating through the array while maintaining two variables:
### currSum: Tracks the sum of the current subarray.
### maxSum: Stores the maximum sum encountered so far.
- The key idea is to reset currSum to 0 whenever it becomes negative, as a negative sum would only reduce the overall contribution of the subarray.
## Real-World Applications of Kadane's Algorithm :
1) Stock Market Analysis:
2) Gaming and Performance Analysis:
3) Website Traffic or Sales Analytics:

BUT APPLICATION FOR US :  TO GET THE MAXSUBARRAY SUM 
![image](https://github.com/user-attachments/assets/135b9f85-64ac-4e91-929a-304c5ecd6e29)
### RESET negative currSum = 0 .
![image](https://github.com/user-attachments/assets/779a7d1f-e45a-4687-a64b-cb872592b7ad)
### RESET negative currSum = 0 .
![image](https://github.com/user-attachments/assets/d2c50bbe-1805-42cc-98ab-6f22b970236f)
### MAXSUM UPDATE 
![image](https://github.com/user-attachments/assets/e4fe38db-cf41-4e2b-af6d-e76812ac291b)
![image](https://github.com/user-attachments/assets/2ad290e1-a00c-4afd-9c06-8ca96d3f2ebd)
![image](https://github.com/user-attachments/assets/9a4de2de-23c5-4e81-81e5-43ef03e400ed)
### MAXSUM UPDATE 
![image](https://github.com/user-attachments/assets/234e4105-9c6d-4fd2-bdac-8327762a79c3)
![image](https://github.com/user-attachments/assets/be8ef862-e457-4598-bc19-6ac725adcfcb)

### GOT maxSum = 7 

### UNDERSTAND : HOW IT THE BEST OPTMISED 
### OPTMISING FROM BRUTE FORCE -> KADANE
![image](https://github.com/user-attachments/assets/0d392aef-5ea9-4942-a80f-5570babf5593)

## BRUTE FORCE APPROACH : 
```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std ;
typedef long long ll ;
// BurteForce : Get all subarrays and calulate
ll maxSubSum(vector<int>& v) {
    int n = v.size() ;
    ll mx = LLONG_MIN ;
    for (int st = 0; st< n ; st++) {
        for (int e = st ; e<n; e++) {
            ll currSum = 0LL ;
            for (int k = st ; k<=e ; k++) {
               currSum += v[k] ; 
            }
            mx = max(mx , currSum);
        }
    }
    return mx ;
}

int main() {
    vector<int> v = {-2,-3,4,-1-2,1,5,-3};
    cout << maxSubSum(v) << endl ;
    return 0;
}
```

## OPTMISED BRUTE FORCE :
```cpp
ll maxSubSum2(vector<int>& v) {
    int n = v.size() ;
    ll mx = LLONG_MIN ;
    for (int st = 0; st < n; st++) {
        ll currSum = 0 ;
        for (int e = st ; e < n ; e++) {
            currSum += v[e] ;
            mx = max(mx , currSum);
        }
    }
    return mx ;
}
int main() {
    vector<int> v = {-2,-3,4,-1-2,1,5,-3};
    cout << maxSubSum2(v) << endl ;
    return 0;
}
```

## KADANE'S APPROACH 
```cpp 
ll kadaneApproach(vector<int>& v) {
    int n = v.size() ;
    ll currSum = 0LL ; ll mx = LLONG_MIN ;
    for (int i = 0; i <n ; i++) {
        currSum += v[i] ;
        mx = max(mx , currSum) ;
        if (currSum < 0) {
            // ie Effectively no +ve contribution
            currSum = 0 ;
            // reset to 0 ie startSubArray from here
        }
    }
    return mx;
}
int main() {
    vector<int> v = {-2,-3,4,-1-2,1,5,-3};
    cout << kadaneApproach(v) << endl ;
    return 0;
}
```

## Try these now : 
### LeetCode
1) Maximum Subarray (LC #53) .
2) Maximum Sum Circular Subarray (LC #918) .
3) Maximum Subarray Sum with One Deletion (LC #1186) .
4) Maximum Product Subarray (LC #152) (Kadane variant for product) .
5) Best Time to Buy and Sell Stock (LC #121) (Kadane-style greedy).

### GeeksforGeeks
1) Largest Sum Contiguous Subarray.
2) Maximum Subarray Sum in O(n) .
3) Maximum Sum Increasing Subsequence (Kadane + DP hybrid) .
4) Maximum Sum Rectangle in a 2D Matrix (Kadane on rows) .

### Codeforces
1) Problem 474B – Worms (Kadane-style prefix logic) .
2) Problem 1385C – Make It Good (Kadane intuition for suffixes).

### CodeChef
1) SUBINC – Count Increasing Subsequences .
2) MAXSUM – Maximum Sum Subarray .
3) SUMTRAIN – Sum of Train Routes (Kadane on prefix sums).

### AtCoder / HackerRank / SPOJ
1) Maximum Subarray Sum (HackerRank)
2) SUBSUMS – SPOJ
