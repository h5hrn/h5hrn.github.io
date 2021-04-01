---
layout: post
title:  "バブルソート (Bubble sort)"
categories: algorithm
---

### ソースコード

バブルソートとは、順番が逆になっている隣接要素がなくなるまで大小を比較し交換するソートアルゴリズムである。

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
using namespace std;
using vi = vector<int>;

void print_list(vi A, int n) {
  rep(i, n) cout << (i > 0 ? " " : "") << A[i];
  cout << endl;
}

void bubble_sort(vi &A, int n) {
  // 外側のループが1週すると、配列の最後からi番目の数が確定する
  rep(i, n - 1) {
    // 未確定部分について、隣同士を比較して入れ替えていく
    rep(j, n - 1 - i) {
      if (A[j] > A[j + 1]) swap(A[j], A[j + 1]);
    }
    print_list(A, n);
  }
}

int main() {
  int n;
  cin >> n;
  vi A(n);
  rep(i, n) cin >> A[i];

  bubble_sort(A, n);

  return 0;
}
```

実行した結果は以下のとおり。

```
// 入力
8
8 4 3 7 6 5 2 1

// 出力
4 3 7 6 5 2 1 8 (1回目の外側ループ終了時)
3 4 6 5 2 1 7 8 (2回目の外側ループ終了時)
3 4 5 2 1 6 7 8 (3回目の外側ループ終了時)
3 4 2 1 5 6 7 8 (4回目の外側ループ終了時)
3 2 1 4 5 6 7 8 (5回目の外側ループ終了時)
2 1 3 4 5 6 7 8 (6回目の外側ループ終了時)
1 2 3 4 5 6 7 8 (7回目の外側ループ終了時)
```

### 解説

1. 外側のループ変数 i で整列済み部分の先頭インデックスをカウントする。
2. 内側のループ変数 j で先頭から未整列部分の末尾まで `[0, n-1-i)` のインデックスをカウントする。
3. 内側のループで隣同士を比較し、順序が逆なら入れ替える。
4. 上記操作を繰り返すと、配列全体が整列する。

最悪の場合の比較回数は (n-1) + (n-2) + ... + 1 = (n^2-n)/2 となるため、計算量は O(N^2) となる。

### 参考

- [バブルソート - Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%90%E3%83%96%E3%83%AB%E3%82%BD%E3%83%BC%E3%83%88)
