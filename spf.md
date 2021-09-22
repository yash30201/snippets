# Smallest prime factor

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