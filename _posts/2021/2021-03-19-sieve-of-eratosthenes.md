---
layout: post
title:  "素数列挙 (エラトステネスの篩)"
categories: algorithm
---

エラトステネスの篩とは、指定した数以下の素数を探すアルゴリズムである。

指定した数までの数列を用意し、素数を見つけたらその素数の倍数を篩い落としていくことで、素数のみの数列が残るという仕組みである。

### ソースコード

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
#define srep(i, s, t) for (int i = s; i < t; ++i)
using namespace std;
using vi = vector<int>;

vi sieve_of_eratosthenes(int n) {
  vector<bool> is_prime(n + 1);  // 添え字の数が素数か判定した結果を持つ配列
  // 0, 1 は素数ではないので false のまま初期化する
  srep(i, 2, n + 1) is_prime[i] = true;

  vi primes;  // 素数を格納する配列
  rep(i, n + 1) {
    if (is_prime[i]) {
      primes.push_back(i);
      // 素数iについて、i^2以上のiの倍数を篩い落とすため false にする
      for (int j = i * i; j <= n; j += i) is_prime[j] = false;
    }
  }

  return primes;
}

int main() {
  vi primes = sieve_of_eratosthenes(100);
  for (int p : primes) cout << p << " ";
  cout << endl;
  return 0;
}
```

100までの素数を出力した結果は以下の通り。

```
2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97
```

### 参考

- [エラトステネスの篩 - Wikipedia](https://ja.wikipedia.org/wiki/%E3%82%A8%E3%83%A9%E3%83%88%E3%82%B9%E3%83%86%E3%83%8D%E3%82%B9%E3%81%AE%E7%AF%A9)
