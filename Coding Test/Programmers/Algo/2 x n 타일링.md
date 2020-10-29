# 2 x n 타일링

```python
def solution(n):
    a, b = 1, 1
    
    for _ in range(n):
        a, b = b, a + b
        
    return a % 1000000007
```



## 문제 풀이

[프로그래머스: 2 x n 타일링](https://dirmathfl.tistory.com/324)

