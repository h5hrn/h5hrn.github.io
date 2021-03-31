---
layout: post
title:  "挿入ソート (Insertion sort)"
categories: algorithm
---

挿入ソートとは、未ソートの部分から取り出した要素を、ソート済みの部分の適切な位置に挿入するソートアルゴリズムである。

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

void insertion_sort(vi &A, int n) {
  rep(i, n - 1) {
    // i番目までソート済み
    // i+1番目の要素を挿入対象とする
    int tmp = A[i + 1];
    // 整列済みの要素 (A[j], A[j-1], ..., A[0]) の中で適切な挿入位置を探す
    int j = i;
    while (j >= 0 && A[j] > tmp) {
      A[j + 1] = A[j];
      j--;
    }
    A[j + 1] = tmp;

    print_list(A, n);
  }
}

int main() {
  int n;
  cin >> n;
  vi A(n);
  rep(i, n) cin >> A[i];

  insertion_sort(A, n);

  return 0;
}
```

実行した結果は以下のとおり。

```
// 入力
8
8 4 3 7 6 5 2 1

// 出力
4 8 3 7 6 5 2 1 (1回目のループ終了時)
3 4 8 7 6 5 2 1 (2回目のループ終了時)
3 4 7 8 6 5 2 1 (3回目のループ終了時)
3 4 6 7 8 5 2 1 (4回目のループ終了時)
3 4 5 6 7 8 2 1 (5回目のループ終了時)
2 3 4 5 6 7 8 1 (6回目のループ終了時)
1 2 3 4 5 6 7 8 (7回目のループ終了時)
```

### 解説

以下の操作を繰り返すことでソートする。

1. 0番目の要素を整列済みの配列とし、1番目の要素を適切な位置に挿入する。
2. 0番目と1番目までの配列は整列済みなので、2番目の要素を適切な位置に挿入する。
3. 上記操作を配列の最後の要素まで繰り返すと、配列全体が整列する。

挿入対象となる値は `tmp` に格納し、適切な位置が見つかるまで `A[j + 1] = A[j]` で整列済みの配列の要素を後ろにずらす。

ループの条件を抜けたときの `j + 1` が挿入位置となるため、`A[j + 1] = tmp` で挿入する。

最悪の場合、iループの各処理がi回行われるため、計算量はO(N^2)となる。

### 参考

- [挿入ソート - Wikipedia](https://ja.wikipedia.org/wiki/%E6%8C%BF%E5%85%A5%E3%82%BD%E3%83%BC%E3%83%88)
