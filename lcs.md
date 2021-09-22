### Longest common subsequence with reconstruction

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