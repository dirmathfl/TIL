# N으로 표현

```python
from math import inf


answer = inf


def dfs(n, cnt, num, number):
    global answer

    if answer < inf and cnt > answer:
        return
        
    if cnt > 8:
        return

    if num == number:
        answer = min(answer, cnt)
        return

    next_num = 0
    for i in range(8):
        next_num = next_num * 10 + n
        dfs(n, cnt + 1 + i, num + next_num, number)
        dfs(n, cnt + 1 + i, num - next_num, number)
        dfs(n, cnt + 1 + i, num * next_num, number)
        dfs(n, cnt + 1 + i, num / next_num, number)


def solution(N, number):
    dfs(N, 0, 0, number)
    return -1 if answer == inf else answer
```



## 문제 풀이

[프로그래머스: N으로 표현](https://dirmathfl.tistory.com/333)

