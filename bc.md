# Binomial Coefficients

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