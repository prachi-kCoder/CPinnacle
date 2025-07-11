# WILDCARD LETTERS + COUNTING THE NUMBERS DIVISIBLE BY 15 
### This was asked in Adobe Hackathon Coding Round 
```cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long 

ll dfs(int pos, int digitSum, bool tight, int dollarVal, const string& s, vector<vector<vector<vector<ll>>>>& dp) {
    int n = s.length();
    if (pos == n) {
        return (digitSum % 3 == 0); // Last digit check already handled in solve()
    }
    if (dp[pos][digitSum][tight][dollarVal + 1] != -1) {
        return dp[pos][digitSum][tight][dollarVal + 1];
    }

    ll res = 0;
    char c = s[pos];

    if (c != '*' && c != '$') { // Fixed digit
        int d = c - '0';
        bool newTight = tight && (d == (s[pos] - '0'));
        res = dfs(pos + 1, digitSum + d, newTight, dollarVal, s, dp);
    } 
    else if (c == '*') { // * can be any digit (independent)
        int limit = tight ? 9 : 9; // Full range since '*' is wildcard
        for (int d = 0; d <= limit; d++) {
            bool newTight = false; // Since '*' is wildcard, tight constraint is always relaxed
            if (pos == 0 && d == 0) continue ;
            if (pos == n - 1 && (d != 0 && d != 5)) continue; // Last digit must be 0/5
            res += dfs(pos + 1, digitSum + d, newTight, dollarVal, s, dp);
        }
    } 
    else if (c == '$') { // $ must be same across all occurrences
        if (dollarVal == -1) { // First occurrence of $
            int limit = tight ? 9 : 9; // Full range since '$' is wildcard
            for (int d = 0; d <= limit; d++) {
                bool newTight = false; // Since '$' is wildcard, tight constraint is always relaxed
                if (pos == n - 1 && (d != 0 && d != 5)) continue; // Last digit must be 0/5
                res += dfs(pos + 1, digitSum + d, newTight, d, s, dp);
            }
        } 
        else { // Repeating $
            int d = dollarVal;
            bool newTight = false; // Since '$' is wildcard, tight constraint is always relaxed
            if (!(pos == n - 1 && (d != 0 && d != 5))) {
                res += dfs(pos + 1, digitSum + d, newTight, dollarVal, s, dp);
            } // Last digit must be 0/5
        }
    }

    return dp[pos][digitSum][tight][dollarVal + 1] = res;
}

ll solve(const string& s) {
    int n = s.length();
    // Early exit if last digit cannot be 0/5
    if (s[n-1] != '*' && s[n-1] != '$' && s[n-1] != '0' && s[n-1] != '5') {
        return 0;
    }

    // DP[pos][digitSum][tight][dollarVal] (dollarVal: -1 to 9)
    vector<vector<vector<vector<ll>>>> dp(
        n + 1,
        vector<vector<vector<ll>>>(
            n * 9 + 1,
            vector<vector<ll>>(
                2,
                vector<ll>(11, -1) // dollarVal ∈ [-1,9] → +1 offset
            )
        )
    );

    return dfs(0, 0, true, -1, s, dp);
}

int main() {
    string s = "*00";
    cout << solve(s) << endl; // Output: 3 (300, 600, 900)
    return 0;
}
```

| Metric  | COMPLEXICITY  |  HOW ??  | 
|----------|----------------------------|--------------------------|
|  TIME    |  O( n * (9*n) * 2 * 11 * 10) |  n = length of string num , digitSum (at max all n digits = 9) , tight (0/1) , dollarVal [ 0-9 + (-1) -> 11 Values ] , {For each state, we may iterate over 0 to 9 (for '*' or '$'), but due to memoization, each state is computed only once.}  |
| SPACE    |  O (n * (9*n) * 2 * 11 )  | Dp table  |
