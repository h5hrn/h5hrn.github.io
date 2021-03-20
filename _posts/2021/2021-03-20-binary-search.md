---
layout: post
title:  "二分探索 (binary search)"
categories: algorithm
---


二分探索とは、ソート済みの配列について、探索範囲を半分に絞り込む操作を繰り返す検索アルゴリズムである。

たとえば、素数の配列が与えられているとき、指定した数よりも小さい数のうち最大となる数を調べる。

### ソースコード

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
#define srep(i, s, t) for (int i = s; i < t; ++i)
using namespace std;
using vi = vector<int>;

vi sieve_of_eratosthenes(int n) {
  vector<bool> is_prime(n + 1);
  srep(i, 2, n + 1) is_prime[i] = true;

  vi primes;
  rep(i, n + 1) {
    if (is_prime[i]) {
      primes.push_back(i);
      for (int j = i * i; j <= n; j += i) is_prime[j] = false;
    }
  }

  return primes;
}

bool solve(int a, int x) {
  // 判定条件
  return a < x;
}

int binary_search(vi list, int x) {
  int ok = -1;           // 条件を満たす最大の位置
  int ng = list.size();  // 条件を満たさない最小の位置
  while (abs(ok - ng) > 1) {
    int mid = (ok + ng) / 2;
    if (solve(list[mid], x)) {
      // 条件を満たす場合 ok の位置を拡張する
      ok = mid;
    } else {
      // 条件を満たさない場合 ng の位置を拡張する
      ng = mid;
    }
  }
  return ok;
}

void print_info(vi list, int i) {
  cout << "index: " << i << endl;
  cout << "number: ";
  if (i == -1) {
    cout << "not found" << endl;
  } else {
    cout << list[i] << endl;
  }
}

int main() {
  // 100までの素数の配列
  // 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97
  vi primes = sieve_of_eratosthenes(100);

  cout << "29より小さい数のうち最大となる素数" << endl;
  print_info(primes, binary_search(primes, 29));

  cout << "30より小さい数のうち最大となる素数" << endl;
  print_info(primes, binary_search(primes, 30));

  cout << "2より小さい数のうち最大となる素数" << endl;
  print_info(primes, binary_search(primes, 2))

  return 0;
}
```

出力結果は以下の通り。

```
29より小さい数のうち最大となる素数
index: 8
number: 23
30より小さい数のうち最大となる素数
index: 9
number: 29
2より小さい数のうち最大となる素数
index: -1
number: not found
```

### 解説

探索する配列はソート済みである必要がある。

1. ok と ng は配列の添え字の下限-1と上限+1で初期化する。
    - ok: 条件を満たす最大の位置（初期値-1を除く）
    - ng: 条件を満たさない最小の位置
2. 中央値 mid が条件を満たすかを判定する。
    - true: ok の位置を mid まで拡張
    - false: ng の位置を mid まで拡張
3. ok と ng が隣同士になったとき、探索を終了する。
4. 探索終了時点の ok の位置が、条件を満たす最大の値となる。
    - 条件を満たす値が1つもない場合、ok の値は初期値のままで -1 となることに注意

n 個のデータがある場合の計算量は O(log n) となる。

### 参考

- [https://twitter.com/meguru_comp/status/697008509376835584](https://twitter.com/meguru_comp/status/697008509376835584)
