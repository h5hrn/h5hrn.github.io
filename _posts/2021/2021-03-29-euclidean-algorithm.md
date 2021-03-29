---
layout: post
title:  "最大公約数 (ユークリッドの互除法)"
categories: algorithm
---

ユークリッドの互除法とは、2つの自然数の最大公約数を求めるアルゴリズムである。

### ソースコード

```cpp
#include <bits/stdc++.h>
using namespace std;

int gcd(int a, int b) {
  if (b == 0) return a;
  return gcd(b, a % b);
}

int main() {
  cout << gcd(1071, 1029) << endl;
  cout << gcd(1029, 1071) << endl;
  return 0;
}
```

出力結果は以下の通り。

```
21
21
```

### 参考

- [ユークリッドの互除法 - Wikipedia - Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%A6%E3%83%BC%E3%82%AF%E3%83%AA%E3%83%83%E3%83%89%E3%81%AE%E4%BA%92%E9%99%A4%E6%B3%95)
