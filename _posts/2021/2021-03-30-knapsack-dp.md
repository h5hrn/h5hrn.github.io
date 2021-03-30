---
layout: post
title:  "ナップサック問題 (動的計画法)"
categories: algorithm
---

ナップサック問題とは、n種類の品物（価値v, 重量w）のうちいくつかを選び、ナップサックに収まる範囲で品物の価値の合計が最大になる選び方を求める問題である。

様々な解き方があるが、動的計画法で解いてみる。（問題は [D - Knapsack 1](https://atcoder.jp/contests/dp/tasks/dp_d)）

### ソースコード

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
#define maxs(x, y) (x = max(x, y))
using namespace std;
using ll = long long;
using vl = vector<ll>;
using vvl = vector<vl>;

int main() {
  int N, W;
  cin >> N >> W;
  vl weight(N), value(N);
  rep(i, N) cin >> weight[i] >> value[i];

  vvl dp(N + 1, vl(W + 1, 0));
  rep(i, N) rep(w, W + 1) {
    // i個目の品物を選ぶ場合
    if (w >= weight[i]) maxs(dp[i + 1][w], dp[i][w - weight[i]] + value[i]);
    // i個目の品物を選ばない場合
    maxs(dp[i + 1][w], dp[i][w]);
  }

  cout << dp[N][W] << endl;
  return 0;
}

```

### 解説

`dp[i][w]` には、最初のi個の品物の中から重さの合計がw以下になるよう選んだときの価値の合計の最大値を格納する。

i個目の品物を選ぶ場合は、選ぶ前の状態 `dp[i][w - weight[i]]` に i個目の品物の価値 `value[i]` を追加した値を格納する。（ナップサックの容量を超えてはならないため、`w >= weight[i]` の条件が必要となる。）

`maxs(dp[i + 1][w], dp[i][w - weight[i]] + value[i])`

i個目の品物を選ばない場合は、選ぶ前の状態 `dp[i][w]` と何の変化もないため、そのままの値を格納する。

`maxs(dp[i + 1][w], dp[i][w]);`

ちなみに、i=0のときは品物を1つも選ばないので、すべてのwについて価値の合計の最大値は0となる。また、任意のiについて、w=0のときは品物を1つも入れることができないので、価値の合計の最大値は0となる。

### 参考

- [ナップサック問題 - Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%8A%E3%83%83%E3%83%97%E3%82%B5%E3%83%83%E3%82%AF%E5%95%8F%E9%A1%8C)
- [D - Knapsack 1](https://atcoder.jp/contests/dp/tasks/dp_d)
