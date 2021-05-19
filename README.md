# C++ Snippets for DS and Algo 

## List 
+ [Disjoint set union](#disjoint-set-union)
+ [Dijkstra algorithm](#dijkstra-algorithm)

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
struct DJ{
	vector<int> dist;
	const int inf = 1e15;
	// @param v = adjacency list with edge weights as second in pair.
	// 		Eg : v[u] = [{v1, wt}, {v2, wt}.....]
	// @param n = number of total nodes
	// Note - The dist matrix is 1 based indexed.
	DJ(vector<pair<int,int>> v[], int source, int n){
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



