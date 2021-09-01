# C++ Snippets for DS and Algo 

## List 
+ [Disjoint set union](#disjoint-set-union)
+ [Dijkstra algorithm](#dijkstra-algorithm)
+ [Longest increasing subsequence](#Longest-increasing-subsequence)
+ [Next/Previous smaller/greater element](#NextPrevious-smallergreater-element)
+ [Longest common subsequence](#Longest-common-subsequence)
+ [Tries](#Tries)
+ [Primes](#Primes)
+ [Smallest Prime Factor](#Smallest-prime-factor)
+ [Binomial Coefficients](#Binomial-Coefficients)
+ [Lowest Common Ancestor(binary lifting)](#Lowest-common-ancestor)
+ [Coordinate Compressor](#Coordinate-Compressor)
+ [Iterative segment tree](#iterative-segment-tree)
+ [Lazy Segment Tree](#lazy-segment-tree)

---
### Disjoint set union
```cpp
// DSU , rank heuristic + path compression
struct Dsu{
	vector<int> par, rank;
	Dsu(int n){
		par = vector<int>(n + 1);
		rank = vector<int>(n + 1, 1);
		for(int i = 1 ; i <= n ; i++) par[i] = i;
	}
	inline int get(int a){
		return par[a] = (par[a] == a ? a : get(par[a]));
	}
	// return true if they already belong to same set
	bool join(int a, int b){
		a = get(a);
		b = get(b);
		if(a == b) return true;
		if(rank[a] == rank[b]) rank[a]++;
		(rank[a] >= rank[b] ? par[b] = a : par[a] = b);
		return false;
	}
};

// Size heuristic + path compression
// maintains sum, maximum and minimum of sets
// arr is 0-indexed based but all the operations are 1-indexed based

struct Dsu{
	vector<int> par, sz, maxi, mini, sum;
	Dsu(vector<int> &arr){
		int n = arr.size();
		par = vector<int>(n + 1);
		sz = vector<int>(n + 1, 1);
		maxi = vector<int>(n + 1);
		for(int i = 1 ; i <= n ; i++) par[i] = i, maxi[i] = arr[i-1];
		mini = sum = vector<int>(maxi.begin(), maxi.end());
	}
	inline int get(int a){
		return par[a] = (par[a] == a ? a : get(par[a]));
	}
	void join(int a, int b){
		a = get(a);
		b = get(b);
		if(a == b) return;
		if(sz[a] < sz[b]) swap(a, b);
		sz[a] += sz[b];
		sum[a] += sum[b];
		maxi[a] = max(maxi[a], maxi[b]);
		mini[a] = min(mini[a], mini[b]);
		par[b] = a;
	}
};
```
---
### Dijkstra algorithm
```cpp
struct Dj{
	vector<int> dist;
	const int inf = 1e15;
	// @param v = adjacency list with edge weights as second in pair.
	// 		Eg : v[u] = [{v1, wt}, {v2, wt}.....]
	// @param n = number of total nodes
	// Note - The dist matrix is 1 based indexed.
	Dj(vector<pair<int,int>> v[], int source, int n){
		dist = vector<int>(n + 1, inf);
		dist[source] = 0;
		priority_queue<
			pair<int,int>,
			vector<pair<int,int>>,
			greater<pair<int,int>>
		> pq;
		pq.push({(int)0, source});
		
		// Start algo
		while(!pq.empty()){
			auto [wt, x] = pq.top();
			pq.pop();
			if(dist[x] < wt) continue;
			for(auto [y, ewt] : v[x]){
				if(dist[y] > dist[x] + ewt){
					dist[y] = dist[x] + ewt;
					pq.push({dist[y], y});
				}
			}
		}
	}
};

```
---
### Longest increasing subsequence
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
---
### Next/Previous smaller/greater element
```cpp
struct Nextprev{
	// next and prev vectors show n and -1 respectively in case when
	// required element doesn't exists in vector.
	vector<int> nextgreater, prevgreater, nextsmaller, prevsmaller, a;
	int n;
	Nextprev(vector<int> &arr){
		a = vector<int>(arr.begin(), arr.end());
		n = a.size();
	}
	void calcNextGreater(){
		nextgreater = vector<int>(n, n);
		stack<int> st;
		for(int i = 0 ; i < n ; i++){
			while(!st.empty() && a[st.top()] < a[i]){
				nextgreater[st.top()] = i;
				st.pop();
			}
			st.push(i);
		}
		return;
	}
	void calcNextSmaller(){
		nextsmaller = vector<int>(n, n);
		stack<int> st;
		for(int i = 0 ; i < n ; i++){
			while(!st.empty() && a[st.top()] > a[i]){
				nextsmaller[st.top()] = i;
				st.pop();
			}
			st.push(i);
		}
		return;
	}
	void calcPrevSmaller(){
		prevsmaller = vector<int> (n, -1);
		stack<int> st;
		for(int i = n - 1 ; i >= 0 ; i--){
			while(!st.empty() && a[i] < a[st.top()]){
				prevsmaller[st.top()] = i;
				st.pop();
			}
			st.push(i);
		}
		return;
	}
	void calcPrevGreater(){
		prevgreater = vector<int>(n, -1);
		stack<int> st;
		for(int i = n - 1 ; i >= 0 ; i--){
			while(!st.empty() && a[i] > a[st.top()]){
				prevgreater[st.top()] = i;
				st.pop();
			}
			st.push(i);
		}
		return;
	}
};
```
---
### Longest common subsequence
```cpp
struct Lcs{
	int n, m, len;
	vector<vector<int>> dp;
	string lcs;
	Lcs(string &s, string &t){
		n = s.length();
		m = t.length();
		dp = vector<vector<int>>(n + 1, vector<int>(m + 1,0));
		for(int i = 1 ; i <= n ; i++){
			for(int j = 1 ; j <= m ; j++){
				if(s[i-1] == t[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
				else dp[i][j] = max(dp[i-1][j] ,dp[i][j-1]);
			}
		}
		len = dp[n][m];
		
		// Reconsturction of lcs
		int i = n, j = m;
		while(i > 0 && j > 0){
			if(s[i-1] == t[j-1]){
				lcs.push_back(s[i-1]);
				i--;j--;
			}
			else if(dp[i-1][j] >= dp[i][j-1]) i--;
			else j--;
		}
		reverse(lcs.begin(), lcs.end());
	}
};
```
---
### Tries
```cpp
struct Trie{
	Trie *children[26];
	bool is_leaf;
	Trie(){
		for(int i = 0 ;i < 26 ; i++) this->children[i] = NULL;
		this->is_leaf = false;
	}
	
	void insert(string &s){
		Trie *root = this;
		for(char c : s){
			if(root->children[c-'a'] == NULL) root->children[c - 'a'] = new Trie();
			root = root->children[c-'a'];
		}
		root->is_leaf = true;
	}
	
	bool search(string &s){
		Trie *root = this;
		for(char c : s){
			if(root->children[c-'a'] == NULL) return false;
			root = root->children[c-'a'];
		}
		return root->is_leaf;
	}
};

// Trie *root = new Trie();
// root->insert(str);
// root->search(str);
```
---
### Primes
```cpp
struct Prime{
	vector<bool> isPrime; // isPrime[i] Stores whether i is prime or not
	vector<int> arr; // arr stores all the prime numbers
	int cnt; // cnt = arr.size()
	
	Prime(int n){ // Process....Using Sieve and wheel factorisation
		isPrime = vector<bool>(n+1, true);
		isPrime[0] = isPrime[1] = false;
		arr.emplace_back(2);
		for(int i = 4 ; i <= n ; i+=2) isPrime[i] = false;
		for(int i = 3 ; i <= n ; i+=2){
			if(!isPrime[i]) continue;
			arr.emplace_back(i);
			for(int j = i*i ; j <= n ; j+=i) isPrime[j] = false;
		}
		cnt = arr.size();
	}
	
	int primePowerSum(int n){// sum of powers of primes in prime factorisation
		int res = 0;
		for(auto &i : arr){
			if(i * i > n) break;
			while(n % i == 0){
				res++;
				n /= i;
			}
		}
		if(n > 1) res++;
		return res;
	}
	
	map<int,int> factorise(int n){ // prime factoisation of number n
		map<int,int> mp;
		for(auto &i : arr){
			if(i * i > n) break;
			while(n % i == 0){
				mp[i]++;
				n /= i;
			}
		}
		if(n > 1) mp[n]++;
		return mp;
	}
};
```
---
### Smallest prime factor
```cpp
struct Spf{
	vector<int> arr;
	Spf(int n){
		arr = vector<int>(n + 1);
		iota(arr.begin(), arr.end(), 0);
		for(int i = 4 ; i <= n ; i+=2) arr[i] = 2;
		for(int i = 3 ; i <= n ; i+=2){
			if(arr[i] == i){
				for(int j = i*i ; j <= n ; j+=i) arr[j] = i;
			}
		}
		return;
	}
};
```
---
### Binomial Coefficients
```cpp
struct BinomialCoefficients{
    vector<int> fact, inverse;

    BinomialCoefficients(int n){
        fact = vector<int>(n+1, 1);
        inverse = vector<int>(n+1, 1);
        for(int i = 2 ; i <= n ; i++){
            fact[i] = 1LL * fact[i-1] * i % mod;
            inverse[i] = getPower(fact[i], mod-2);
        }
    }

    int getPower(int x, int y){
        int res = 1;
        while(y){
            if(y & 1) res = 1LL * res * x % mod;
            y >>= 1;
            x = 1LL * x * x % mod;
        }
        return res;
    }
    
    inline int getNcr(int n, int r){
        return (n >= r ? 1LL * fact[n] * inverse[r] % mod * inverse[n-r] % mod : 0); 
    }
};
```

---
### Lowest Common Ancestor
```cpp
struct Lca{ //  This is zero based
	vector<vector<int>> par;
	vector<int> time_in, time_out, dist;
	int timer;
	Lca(vector<int> g[], int n){
		par = vector<vector<int>>(n,vector<int>(20,-1));
		time_in = vector<int>(n);
		time_out = vector<int>(n);
		dist = vector<int>(n, 0);
		timer = 0;
		par[0][0] = 0;
		dfs(g,0,-1);
	}
	
	void dfs(vector<int> g[], int node, int parent){
		time_in[node] = timer++;
		for(int i = 1 ; i < 20 ; i++) par[node][i] = par[par[node][i-1]][i-1];
		for(int i : g[node]){
			if(i != parent){
				par[i][0] = node;
				dist[i] = dist[node] + 1;
				dfs(g, i, node);
			}
		}
		time_out[node] = timer++;
	}
	
	inline bool isAncestor(int x, int y){ // is x ancestor of y ? 
		return time_in[x] <= time_in[y] && time_out[x] >= time_out[y];
	}
	
	int getLca(int x, int y){
		if(isAncestor(x, y)) return x;
		if(isAncestor(y, x)) return y;
		for(int i = 19 ; i >= 0 ; i--){
			if(!isAncestor(par[x][i], y)) x = par[x][i];
		}
		return par[x][0];
	}
	
	int getDist(int x, int y){
		int lca = getLca(x, y);
		return dist[x] + dist[y] - (dist[lca]<<1);
	}
};
```
---
### Coordinate Compressor
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
```
---
### Iterative segment tree
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

---
### Lazy segment tree
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

