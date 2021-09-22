# Longest increasing subsequence

```cpp
struct Lis {
    int len;
	vector<int> dp;
    Lis(vector<int> &a) {
        for(int i = 0 ; i < (int)a.size() ; i++) {
            auto it = lower_bound(dp.begin(), dp.end(), a[i]);
            if (it == dp.end()) dp.pb(a[i]);
            else *it = a[i];
        }
        len = dp.size();
    }
};
```