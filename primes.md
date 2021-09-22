# Primes

```cpp
struct Prime{
	vector<bool> isPrime; // isPrime[i] Stores whether i is prime or not
	vector<int> arr; // arr stores all the prime numbers
	int cnt; // cnt = arr.size()
	
	Prime(int n){ // Process....Using Sieve and wheel factorisation
		isPrime = vector<bool>(n+1, true);
		isPrime[0] = isPrime[1] = false;
		for(int i = 4 ; i <= n ; i+=2) isPrime[i] = false;
		for(int i = 3 ; i*i <= n ; i+=2){
			if(!isPrime[i]) continue;
			for(int j = i*i ; j <= n ; j+=i) isPrime[j] = false;
		}
		for(int i = 2 ; i <= n ; i++){
			if(isPrime[i]) arr.push_back(i);
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
	
	vector<int> factorise(int n){ // prime factorisation of number n
		vector<int> a;
		for(auto &i : arr){
			if(i * i > n){
				break;
			}
			while(n % i == 0){
				a.eb(i);
				n /= i;
			}
		}
		if(n > 1) a.eb(n);
		return a;
	}
};
```