# Lazy segment tree (Range assignment, Range sum)

```cpp
struct lazy_seg_tree_range_assign_range_sum_query{
	vector<int> t, lazy;
	int n, inf;
	lazy_seg_tree_range_assign_range_sum_query(int N){
		n = N;
		inf = -1e9;
		t.assign(4*n, 0);
		lazy.assign(4*n, inf);
	}
	void push(int ix, int lx, int rx){ // [lx, rx)
		if(lazy[ix] != inf){
			int m = (lx + rx) >> 1;
			lazy[ix << 1]  = lazy[ix << 1 | 1] = lazy[ix];
			t[ix << 1] = (m - lx) * lazy[ix]; 		
			t[ix << 1 | 1] = (rx - m) * lazy[ix];
			lazy[ix] = inf;
		}
	}
	void assignRange(int l, int r, int val, int ix, int lx, int rx){
		if(lx >= l && rx <= r){
			if(rx - lx > 1) push(ix, lx, rx);
			lazy[ix] = val;
			t[ix] = (rx - lx) * val;
			return;
		}
		if(lx >= r || rx <= l) return;
		if(rx - lx > 1) push(ix, lx, rx);
		int m = (lx + rx) >> 1;
		assignRange(l, r, val, ix << 1, lx, m);
		assignRange(l, r, val, ix << 1 | 1, m, rx);
		t[ix] = t[ix << 1] + t[ix << 1 | 1];
	}
	void assignRange(int l, int r, int val){ // add in [l, r)
		assignRange(l, r, val, 1, 0, n);
	}
	int getRangeSum(int l, int r, int ix, int lx, int rx){
		if(lx >= l && rx <= r){
			if(rx - lx > 1) push(ix, lx, rx);
			return t[ix];
		}
		if(lx >= r || rx <= l) return 0;
		if(rx - lx > 1) push(ix, lx, rx);
		int m = (lx + rx) >> 1;
		return getRangeSum(l, r, ix << 1, lx, m) + getRangeSum(l, r, ix << 1 | 1, m, rx);
	}
	int getRangeSum(int l, int r){ // get sum of [l, r)
		return getRangeSum(l, r, 1, 0, n);
	}
};
```