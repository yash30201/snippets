# Iterative segment tree

```cpp
struct seg_tree{
	int n;
	vector<int> t;
	seg_tree(vector<int> &a){
		n = a.size();
		t.assign(2*n, 0);
		for(int i = 0 ; i < n ; i++) t[i + n] = a[i];
		for(int i = n-1 ; i > 0 ; i--){
			t[i] = t[i << 1]	+ t[i << 1 | 1];
		}
	}
	void modify(int pos, int value){
		pos += n;
		for(t[pos] = value ; pos > 1 ; pos >>= 1){
			t[pos >> 1] = t[pos] + t[pos^1];
		}
	}
	int get(int l, int r){ // [l, r)
		int res = 0;
		for(l += n, r += n ; l < r ; l >>= 1, r >>= 1){
			if(l & 1) res += t[l++];
			if(r & 1) res += t[--r];
		}
		return res;
	}
};


// If we want
// 1) add a value to all elements in some interval;
// 2) compute an element at some position.

// void modify(int l, int r, int value) {
  // for (l += n, r += n; l < r; l >>= 1, r >>= 1) {
    // if (l&1) t[l++] += value;
    // if (r&1) t[--r] += value;
  // }
// }
// 
// int get(int p) {
  // int res = 0;
  // for (p += n; p > 0; p >>= 1) res += t[p];
  // return res;
// }
```