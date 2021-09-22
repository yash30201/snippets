# Tries


```cpp
struct Trie{
	Trie *children[26];
	bool is_leaf;
	Trie(){
		for(int i = 0 ;i < 26 ; i++) this->children[i] = NULL;
		this->is_leaf = false;
	}
	
	void insert(string &s){
		Trie *root = this;
		for(char c : s){
			if(root->children[c-'a'] == NULL) root->children[c - 'a'] = new Trie();
			root = root->children[c-'a'];
		}
		root->is_leaf = true;
	}
	
	bool search(string &s){
		Trie *root = this;
		for(char c : s){
			if(root->children[c-'a'] == NULL) return false;
			root = root->children[c-'a'];
		}
		return root->is_leaf;
	}
};

// Trie *root = new Trie();
// root->insert(str);
// root->search(str);
```