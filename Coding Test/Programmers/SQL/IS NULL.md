# IS NULL

#### 이름이 없는 동물의 아이디

```mysql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL
```

 

#### 이름이 있는 동물의 아이디

```mysql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
ORDER BY ANIMAL_ID
```

 

#### NULL 처리하기

```mysql
SELECT
    ANIMAL_TYPE,
    IF (NAME IS NULL, 'No name', NAME) AS NAME,
    SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```



## 문제 풀이

[프로그래머스: SQL - IS NULL](https://dirmathfl.tistory.com/306)

