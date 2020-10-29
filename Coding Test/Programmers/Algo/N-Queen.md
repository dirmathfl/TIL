# N-Queen

```python
answer = 0
columns, l_diagonals, r_diagonals = set(), set(), set()

def dfs(x, n):
    global answer

    if x == n:
        answer += 1
        return

    for y in range(n):
        if y in columns or x + y in l_diagonals or x - y in r_diagonals:
            continue
        columns.add(y)
        l_diagonals.add(x + y)
        r_diagonals.add(x - y)
        dfs(x + 1, n)
        columns.remove(y)
        l_diagonals.remove(x + y)
        r_diagonals.remove(x - y)

def solution(n):
    dfs(0, n)
    return answer
```



## 문제 풀이

[프로그래머스: N-Queen](https://dirmathfl.tistory.com/325)

