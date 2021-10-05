# String Splitter

```cpp
vector<string> split(string &s, string delim = " ") {
    size_t prev = 0, next, ld = delim.length();
    string token;
    vector<string> res;

    while ((next = s.find (delim, prev)) != string::npos) {
        token = s.substr (prev, next - prev);
        prev = next + ld;
        res.push_back (token);
    }

    res.push_back (s.substr (prev));
    return res;
}
```