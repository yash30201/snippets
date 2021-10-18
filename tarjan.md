# Tarjan's bridge finder algorithm

```cpp
class bridgeFinder {
    vector<bool> visited;
    vector<int> entry, minTime;
    int counter;

    void whatToDo(int x, int y) { ans.push_back({x, y}); }

    void dfs_this(vector<int> v[], int node, int parent) {
        visited[node] = true;
        entry[node] = minTime[node] = counter++;
        for (int i : v[node]) {
            if (i == parent) continue;
            if (visited[i]) {
                minTime[node] = min(minTime[node], entry[i]);
            } else {
                dfs_this(v, i, node);
                minTime[node] = min(minTime[node], minTime[i]);
                if (minTime[i] > entry[node]) {
                    whatToDo(i, node);
                }
            }
        }
        return;
    }

   public:
    vector<pair<int, int>> ans;
    bridgeFinder(vector<int> v[], int N) {  // 1 based graph
        visited.assign(N + 1, false);
        entry.assign(N + 1, -1);
        minTime.assign(N + 1, -1);
        ans.clear();
        counter = 0;
        dfs_this(v, 1, -1);
    }
};
```