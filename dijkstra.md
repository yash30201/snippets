# Dijkstra algorithm

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