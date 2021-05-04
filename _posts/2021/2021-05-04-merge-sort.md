---
layout: post
title:  "マージソート (Merge Sort)"
categories: algorithm
---

マージソートとは、配列を分割し、分割された各配列の要素をソートしてマージするソートアルゴリズムである。

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

void merge_sort(vi &A, int left, int right) {
  if (right - left == 1) return;

  int mid = (left + right) / 2;
  // 左半分をソート
  merge_sort(A, left, mid);
  // 右半分をソート
  merge_sort(A, mid, right);

  // 左半分と右半分のソート結果をコピー（右は逆順）
  vi buf;
  for (int i = left; i < mid; i++) buf.push_back(A[i]);
  for (int i = right - 1; i >= mid; i--) buf.push_back(A[i]);

  // 左半分と右半分の要素から小さい順に要素を取り出してマージ
  int left_i = 0;
  int right_i = buf.size() - 1;
  for (int i = left; i < right; i++) {
    A[i] = buf[left_i] <= buf[right_i] ? buf[left_i++] : buf[right_i--];
  }
}

int main() {
  int n;
  cin >> n;
  vi A(n);
  rep(i, n) cin >> A[i];

  merge_sort(A, 0, n);

  print_list(A, n);

  return 0;
}

```

実行した結果は以下のとおり。

```
// 入力
8
8 4 3 7 6 5 2 1

// 出力
1 2 3 4 5 6 7 8
```

### 解説

1. 初期データを `[8, 4, 3, 7, 6, 5, 2, 1]` とする。
2. 分割できなくなるまで（分割した要素数が1になるまで）再帰的に分割する。
    1. `[8, 4, 3, 7], [6, 5, 2, 1]`
    2. `[8, 4], [3, 7], [6, 5], [2, 1]`
    3. `[8], [4], [3], [7], [6], [5], [2], [1]`
3. 分割した左半分と右半分の要素から、小さい順に要素を取り出してマージする。
    1. `[4, 8], [7, 3], [5, 6], [1, 2]`
    2. `[3, 4, 7, 8], [1, 2, 5, 6]`
    3. `[1, 2, 3, 4, 5, 6, 7, 8]`

分割するときの計算量は O(log N) であり、マージするときの計算量は O(N) となるので、マージソートの計算量は O(N log N) となる。

### 参考

- [マージソート - Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%9E%E3%83%BC%E3%82%B8%E3%82%BD%E3%83%BC%E3%83%88)
