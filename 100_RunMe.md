## SECCON 2017 RunMe
[問題](https://github.com/SECCON/SECCON2017_online_CTF/tree/master/programming/100_RunMe)

```text
Run me!
-----  RunMe.py
import sys
sys.setrecursionlimit(99999)
def f(n):
    return n if n < 2 else f(n-2) + f(n-1)
print "SECCON{" + str(f(11011))[:32] + "}"
-----
```

11011番目のフィボナッチ数の下32桁がフラグ．

問題文のコードだとめちゃくちゃ時間がかかるのかな(試してない)？
動的計画法でフィボナッチ数を求めると計算量は $O(n)$ なので10^4程度ならすぐに計算できる．

```python
def f(n):
  if n < 2:
    return n

  n1, n2 = 0, 1
  sum = 1
  for i in range(n-1):
    sum = n1 + n2
    n1, n2 = n2, sum
  return sum

print("SECCON{" + str(f(11011))[:32] + "}")
```
↑これを実行してflagゲット．