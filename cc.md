# Co-ordinate compressor

```cpp
struct CoordinateCompressor {
	int sz;
    vector<int> master;
    CoordinateCompressor() { master.clear(); }
    void add(vector<int>& a) { // Update coordinate list
        for (int i : a) master.emplace_back(i);
    }
    void process() { // processes the final coordinate list
        sort(master.begin(), master.end());
        master.resize(unique(master.begin(), master.end()) - master.begin());
        sz = master.size();
    }
    void set(vector<int>& a) { // Convert older points into new points
        for (int& i : a) {
            i = lower_bound(master.begin(), master.end(), i) - master.begin();
        }
    }
    
/*
	Guide to use :
	CoordinateCompressor cc;
	For all vectors v = a, b, c, ....
		cc.add(v);
	cc.process();
	For all vectors v = a, b, c, ....
		cc.set(v);
*/
};