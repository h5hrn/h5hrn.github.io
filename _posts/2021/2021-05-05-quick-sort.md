---
layout: post
title:  "クイックソート (Quick Sort)"
categories: algorithm
---

クイックソートとは、適当な数を基準として、小さい値と大きい値に分割することを繰り返すソートアルゴリズムである。

### ソースコード

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
using namespace std;
using vi = vector<int>;

void print_list(vi A, int n) {
  rep(i, n) cout << (i > 0 ? " " : "") << A[i];
  cout << endl;
}

void quick_sort(vi &A, int left, int right) {
  if (right - left <= 1) return;

  // 右端の値を pivot とする
  int pivot = A[right - 1];
  // pivot 未満となる要素の右端となる添え字
  int i = left;

  // pivot を基準として、pivot 未満の値が左側になるよう交換
  for (int j = left; j < right - 1; j++) {
    if (A[j] < pivot) swap(A[i++], A[j]);
  }
  // pivot を適切な位置に挿入
  swap(A[i], A[right - 1]);

  // pivot の左側を再帰的にソート
  quick_sort(A, left, i);
  // pivot の右側を再帰的にソート
  quick_sort(A, i + 1, right);
}

int main() {
  int n;
  cin >> n;
  vi A(n);
  rep(i, n) cin >> A[i];

  quick_sort(A, 0, n);

  print_list(A, n);

  return 0;
}

```

実行した結果は以下のとおり。

```
// 入力
8
8 4 3 7 1 6 2 5

// 出力
1 2 3 4 5 6 7 8
```

### 解説

1. 初期データを `[8, 4, 3, 7, 1, 6, 2, 5]` とする。
2. 右端の値である `5` を `pivot` とし、`5` 未満の値が左側、`5` 以上の値が右側になるよう整理する。
    1. `4` と `8` を交換
        - `[4, 8, 3, 7, 1, 6, 2, 5]`
    2. `3` と `8` を交換
        - `[4, 3, 8, 7, 1, 6, 2, 5]`
    3. `1` と `8` を交換
        - `[4, 3, 1, 7, 8, 6, 2, 5]`
    4. `2` と `7` を交換
        - `[4, 3, 1, 2, 8, 6, 7, 5]`
    5. `pivot` である `5` を適切な位置に挿入
        - `[4, 3, 1, 2, 5, 6, 7, 8]`
3. 分割した左半分 `[4, 3, 1, 2]` と右半分 `[5, 6, 7, 8]` の要素について、再帰的に同じ処理を繰り返す。

`pivot` の選択に毎回最大値か最小値が選ばれる場合、最悪の計算量は O(N^2) となる。`pivot` が左右均等に分割できる場合、平均計算量は O(N log N) となる。

### 参考

- [クイックソート - Wikipedia](https://ja.wikipedia.org/wiki/%E3%82%AF%E3%82%A4%E3%83%83%E3%82%AF%E3%82%BD%E3%83%BC%E3%83%88)
