# Lazy segment tree(Range add, Range Maximum)

```cpp
struct lazy_segtree{
	vector<int> Tree, lazy;
	int n;
	lazy_segtree(int m){
		n = m+1;
		Tree.assign(4*n, 0);
		lazy.assign(4*n, 0);
	}
	void push(int ix){
		if(lazy[ix] != 0){
			lazy[ix<<1] += lazy[ix];
			lazy[ix<<1|1] += lazy[ix];
			Tree[ix<<1] += lazy[ix];
			Tree[ix<<1|1] += lazy[ix];
			lazy[ix] = 0;
		}
	}
	void calc(int ix){
		Tree[ix] = max(Tree[ix << 1], Tree[ix << 1|1]);
	}
	int calc(int val1, int val2){
		return max(val1, val2);
	}
	void update(int l, int r, int val, int ix, int lx, int rx){
		if(lx >= r || rx <= l) return;
		if(rx - lx != 1) push(ix);
		if(lx >= l && rx <= r){
			lazy[ix] += val;
			Tree[ix] += val;
			return;
		}
		int m = (lx + rx) >> 1;
		update(l, r, val, ix << 1, lx, m);
		update(l, r, val, ix << 1|1, m, rx);
		calc(ix);
	}
	void update(int l, int r, int val){
		update(l, r, val, 1, 0, n);
	}
	int get(int l, int r, int ix, int lx, int rx){
		if(lx >= r || rx <= l) return 0;
		if(rx - lx != 1)push(ix);
		if(lx >= l && rx <= r) return Tree[ix];
		int m = (lx + rx) >> 1;
		return calc(get(l, r, ix<<1, lx, m) , get(l, r, ix<<1|1, m, rx));
	}
	inline int get(int l, int r){
		return get(l, r, 1, 0, n);
	}
};
```