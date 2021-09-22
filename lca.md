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