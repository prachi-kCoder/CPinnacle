Binary Lifting Principle :
Any number can be expressed in forms of powers of 2 : 
    NUM    BINARY
Ex : 9 ->  1001   -> 2^3 + 2^0 -> 8 + 1 
   : 10 ->  1010   -> 2^3 + 2^1 -> 8 + 2
   : 13 ->  1101   -> 2^3 + 2^2 + 2^1 -> 8 + 4 + 1
   
## KTH PARENT
In Generic Tree , if the Kth Parent asked -> it the Parent of a node K level up !
nstead of traversing the tree manually for k steps, we precompute ancestors at powers of 2 (like 1st, 2nd, 4th, 8th parent...) using Binary Lifting. 

### Key idea is:

Each node stores its ancestor at distance 2^j (stored in up[][]).

To find the k-th ancestor (getKthParent), we decompose k into powers of 2 (binary representation).
Example : -
Finding 5th ancestor of node u
5 = 101₂ (binary) → 2⁰ + 2²

This means, move 1 step (2⁰) and then 4 steps (2²) : {Ask par of your parent !}
```
Code :#include<iostream>
#include<vector>
using namespace std;

const int MAXN = 1e5 + 5, LOG = 17;
vector<int> adj[MAXN];
int up[MAXN][LOG]; // Binary lifting table
int depth[MAXN];

void dfs(int u, int p) {
    up[u][0] = p;  // Direct parent
    depth[u] = (p != -1) ? depth[p] + 1 : 0;

    for (int v : adj[u]) {
        if (v != p) dfs(v, u);
    }
}

// Precompute Binary Lifting Table
void preprocess(int n) {
    for (int j = 1; j < LOG; j++) {
        for (int i = 0; i < n; i++) {
            if (up[i][j - 1] != -1)
                up[i][j] = up[up[i][j - 1]][j - 1];
        }
    }
}

// Get K-th Parent using Binary Lifting
int getKthParent(int u, int k) {
    for (int j = 0; j < LOG; j++) {
        if (k & (1 << j)) { // Check if `j-th` bit is set
            u = up[u][j];
            if (u == -1) return -1; // If we reach the root
        }
    }
    return u;
}

int main() {
    vector<vector<int>> edges = {{0,1},{1,5},{5,6},{5,7},{0,2},{2,3},{2,4},{4,8},{8,9}};
    int n = edges.size() + 1;

    for (auto& e : edges) {
        adj[e[0]].push_back(e[1]);
        adj[e[1]].push_back(e[0]);
    }

    dfs(0, -1);
    preprocess(n);

    // Queries: Get Kth Parent
    cout << "2nd parent of node 7: " << getKthParent(7, 2) << endl;
    cout << "3rd parent of node 9: " << getKthParent(9, 3) << endl;

    return 0;
}
```


# Q2 ) GET-LCA of any 2 nodes in a Generic Tree 

## How Binary Lifting Works for LCA
1️⃣ Precompute Ancestors at Powers of 2

We store up[u][j], where up[u][j] is the 2^j-th ancestor of node u.

This allows us to jump efficiently instead of moving one step at a time.

2️⃣ Finding LCA Efficiently (O(log N))

First, lift the deeper node (u) to the same level as v.
Then, 
### lift both nodes up until they meet at their LCA.


CODE :
```
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1e5 + 5, LOG = 17;
vector<int> adj[MAXN];
int up[MAXN][LOG], depth[MAXN];

void dfs(int u, int p) {
    up[u][0] = p;
    depth[u] = (p != -1) ? depth[p] + 1 : 0;

    for (int v : adj[u]) {
        if (v != p) dfs(v, u);
    }
}

void preprocess(int n) {
    for (int j = 1; j < LOG; j++) {
        for (int i = 0; i < MAXN; i++) { // Ensure all nodes are covered
            if (up[i][j - 1] != -1)
                up[i][j] = up[up[i][j - 1]][j - 1];
        }
    }
}

int getLCA(int u, int v) {
    if (depth[u] < depth[v]) swap(u, v);

    // Lift `u` to the same level as `v`
    for (int j = LOG - 1; j >= 0; j--) {
        if (depth[u] - (1 << j) >= depth[v]) {
            u = up[u][j];
        }
    }

    if (u == v) return u; // Found LCA

    // Lift both nodes up until they meet
    for (int j = LOG - 1; j >= 0; j--) {
        if (up[u][j] != up[v][j]) {
            u = up[u][j];
            v = up[v][j];
        }
    }

    return up[u][0]; // Return LCA
}

int main() {
    vector<vector<int>> edges = {{0,1},{1,5},{5,6},{5,7},{2,3},{2,4},{4,8},{8,9}};
    int n = edges.size() + 1;

    for (auto& e : edges) {
        adj[e[0]].push_back(e[1]);
        adj[e[1]].push_back(e[0]);
    }

    dfs(0, -1);
    preprocess(n);

    cout << "LCA of 3 & 8: " << getLCA(3, 8) << endl;
    cout << "LCA of 4 & 9: " << getLCA(4, 9) << endl;

    return 0;
}

```
