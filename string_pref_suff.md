# String : Prefix and suffix function

> Prefix => prefix[i] = max_len suffix match ending at i

> Suffix => suffix[i] = max_len suffix match starting at i

```cpp
struct string_functions{
	vector<int> prefix, suffix;

	string_functions(string &s)	{
		int n = s.length();
		prefix.assign(n, 0);
		suffix.assign(n, 0);
		process_prefix(s, n);
		process_suffix(s, n);
	}
	
	void process_prefix(string &s, int n){
		for(int i = 1 ; i < n ; i++){
			prefix[i] = prefix[i - 1];
			while(prefix[i] > 0 && s[prefix[i]] != s[i]) prefix[i] = prefix[prefix[i] - 1];
			if(s[prefix[i]] == s[i]) prefix[i]++;
		}
	}
	
	void process_suffix(string &s, int n){
		for(int i = 1, l = 0, r = 0 ; i < n ; i++){
			if(i <= r) suffix[i] = min(r - i + 1, suffix[i - l]);
			while(i + suffix[i] < n && s[i + suffix[i]] == s[suffix[i]]) suffix[i]++;
			if(i + suffix[i] - 1 > r){
				l = i;
				r = i + suffix[i] - 1;
			}
		}
	}
};
```

---