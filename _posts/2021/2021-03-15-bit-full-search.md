---
layout: post
title:  "bit 全探索"
categories: algorithm
---

2のべき乗で求められる組み合わせを全探索する方法として、bit 全探索という方法がある。

例えば、n人が「立っている」または「座っている」のどちらかの状態で行列に並んでいるとする。このとき、考えられる行列の並び方を全て列挙する場合に使える。

### ソースコード

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
using namespace std;

void bit_full_search(int n) {
  rep(i, 1 << n) {
    rep(j, n) {
      if (i & (1 << j)) {
        cout << "1";  // 1のときの処理
      } else {
        cout << "0";  // 0のときの処理
      }
    }
    cout << endl;
  }
}

int main() {
  bit_full_search(4);
  return 0;
}
```

`n = 4` のときの出力結果は以下の通り。先ほどの例えで考えると、立っている状態を0、座っている状態を1とすれば、ありうる行列の組み合わせが全て出力されていることになる。

```
0000
1000
0100
1100
0010
1010
0110
1110
0001
1001
0101
1101
0011
1011
0111
1111
```

### 解説

数値同士の論理積は、2進数の各位のビットを比較し、両方が1のときのみ1となる。

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
using namespace std;

void printAndValue(int x, int y) {
  cout << bitset<8>(x) << " = " << x << endl;
  cout << bitset<8>(y) << " = " << y << endl;
  cout << bitset<8>(x & y) << " = " << x << " & " << y << endl;
}

int main() {
  printAndValue(42, 121);
  return 0;
}
```

```
00101010 = 42
01111001 = 121
00101000 = 42 & 121
```

上記の例の場合、42と121を2進数で表したとき、右から4桁目と6桁目はどちらも1であるから、42と121の論理積は `00101000` となる。

また、`1 << n` で `00000001` を `n` ビット左シフトした値を得ることができる。

```cpp
#include <bits/stdc++.h>
#define rep(i, n) for (int i = 0; i < (n); ++i)
using namespace std;

void printLeftShiftValue(int n) {
  rep(i, n) {
    int num = 1 << i;
    bitset<8> bit_num(num);
    cout << "bit: " << bit_num << ", num: " << num << endl;
  }
}

int main() {
  printLeftShiftValue(8);
  return 0;
}
```

1を0ビットから7ビットまで左シフトした出力結果は以下の通り。

```
bit: 00000001, num: 1
bit: 00000010, num: 2
bit: 00000100, num: 4
bit: 00001000, num: 8
bit: 00010000, num: 16
bit: 00100000, num: 32
bit: 01000000, num: 64
bit: 10000000, num: 128
```

ある数iについて、上記の数との論理積をとれば、任意の位が0か1かを判定することができる。

`bitFullSearch()` ではこの性質を組み合わせることで、全探索を実現している。

1. 外側のループで0から2のn乗-1までの数値iを作る。
2. 各iについて、2の0乗からn-1乗までの数値 (`1 << j`) との論理積をとる。
3. 論理積 (`i & (1 << j)`) の値が0となればj+1桁目は0であり、それ以外なら1となる。
