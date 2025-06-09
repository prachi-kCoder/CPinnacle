# TRIE (Used to check for prefixes of words and numbers) 
## TRIE have multiple children nodes (Binary Tree is a special case have 2 children at max)
## All children nodes are kept in vector<TrieNode*>  and bool numEnd -> to mark whether andy number (in case we're working with number), eow(end of word) (in case we're working with words) have ended at that TrieNode
## Trie all storing the common prefix of multiple nums/words to be stored once and then division happens deep Trie to make up different nums 
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct TrieNode {
    int val;
    vector<TrieNode*> children;
    bool numEnd;

    TrieNode(int val) {
        this->val = val;
        this->children.resize(10, nullptr);
        this->numEnd = false;
    }
};

class Trie {
public:
    vector<TrieNode*> roots;

    Trie() {
        roots.resize(10, nullptr); // Support numbers 0-9 as roots
    }

    void add(int val) {
        string s = to_string(val);
        int firstDigit = s[0] - '0';

        if (!roots[firstDigit]) {
            roots[firstDigit] = new TrieNode(firstDigit);
        }

        TrieNode* root = roots[firstDigit];
        for (int i = 1; i < s.length(); i++) {
            int d = s[i] - '0';
            if (!root->children[d]) {
                root->children[d] = new TrieNode(d);
            }
            root = root->children[d];
        }
        root->numEnd = true;
    }

    bool search(int val) {
        string s = to_string(val);
        int firstDigit = s[0] - '0';

        if (!roots[firstDigit]) return false;

        TrieNode* root = roots[firstDigit];
        for (int i = 1; i < s.length(); i++) {
            int d = s[i] - '0';
            if (!root->children[d]) return false;
            root = root->children[d];
        }
        return root->numEnd;
    }
};

int main() {
    Trie trie;
    vector<int> nums = {112576899, 1111567, 234589};

    for (int num : nums) {
        trie.add(num);
    }

    cout << "Searching for 234589: " << (trie.search(234589) ? "Found" : "Not Found") << endl;
    cout << "Searching for 99999: " << (trie.search(99999) ? "Found" : "Not Found") << endl;

    return 0;
}
```

## Example Ques : Given two integers n and k, return the kth lexicographically smallest integer in the range [1, n].
### (TRIE-Based Approach may not be the best optmised but will give you a good understanding of it !)
### Traversing trie sequentially will give us lexographically ordered numbers eg [1,10,11,12,13,14 ....2,20,21,22,23,,,,3,4,,,] 

```cpp
#include <iostream>
#include <vector>
using namespace std;

struct TrieNode {
    string num; // Store full number representation
    bool numEnd;
    vector<TrieNode*> children;

    TrieNode(string num) {
        this->num = num;
        this->numEnd = false;
        this->children.resize(10, nullptr);
    }
};

class Trie {
public:
    TrieNode* root;

    Trie() {
        root = new TrieNode(""); // Root node starts empty
    }

    void add(int val) {
        string s = to_string(val);
        TrieNode* node = root;

        for (char ch : s) {
            int d = ch - '0';
            if (!node->children[d]) {
                node->children[d] = new TrieNode(node->num + ch);
            }
            node = node->children[d];
        }
        node->numEnd = true;
    }

    int cnt = 0;
    string ans = "";

    void search(TrieNode* node, int k) {
        if (!node) return;

        if (node->numEnd) cnt++; // Count only completed numbers
        if (cnt == k) {
            ans = node->num; 
            return;
        }

        for (int d = 0; d <= 9; d++) {
            if (!node->children[d]) continue;
            search(node->children[d], k);
            if (!ans.empty()) return; // Stop early when found
        }
    }

    int getKth(int k) {
        cnt = 0; ans = "";
        search(root, k);
        return ans.empty() ? -1 : stoi(ans);
    }
};

class Solution {
public:
    int findKthNumber(int n, int k) {
        Trie trie;
        for (int num = 1; num <= n; num++) {
            trie.add(num);
        }
        return trie.getKth(k);
    }
};

int main() {
    Solution sol;
    int n = 13, k = 2;
    cout << "K-th Lexicographically Smallest Number: " << sol.findKthNumber(n, k) << endl;
    return 0;
}

```
