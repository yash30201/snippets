# Co-ordinate compressor

```cpp
struct coordinate_compression {
    int sz;
    vector<int> master;
    map<int, int> original;// stores actual numbers corresponding to indices
    coordinate_compression() {
        master.clear();
        original.clear();
    }
    void add(vector<int>& a) {  // Update coordinate list
        for (int i : a) master.emplace_back(i);
    }
    void process() {  // processes the final coordinate list
        sort(master.begin(), master.end());
        master.resize(unique(master.begin(), master.end()) - master.begin());
        sz = master.size();
    }
    void set(vector<int>& a) {  // Convert older points into new points
    	int idx;
        for (int& i : a) {
            idx = lower_bound(master.begin(), master.end(), i) - master.begin();
            original[idx] = i;
            i = idx;
        }
    }
};
```

> **Upd1** : Now it also stores the original numbers according to new indices