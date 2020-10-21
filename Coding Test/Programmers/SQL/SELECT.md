# SELECT

#### 모든 레코드 조회 하기

```mysql
SELECT *
FROM ANIMAL_INS
```

 

#### 역순 정렬하기

```mysql
SELECT NAME, DATETIME
FROM ANIMAL_INS 
ORDER BY ANIMAL_ID DESC
```

 

#### 아픈 동물 찾기

```mysql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = 'Sick'
```

 

#### 어린 동물 찾기

```mysql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION != 'Aged'
```

 

#### 동물의 아이디와 이름

```mysql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

 

#### 여러 기준으로 정렬하기

```mysql
SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME, DATETIME DESC
```

 

#### 상위 n개 레코드

```mysql
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1
```



## 문제 풀이

[프로그래머스: SQL - SELECT](https://dirmathfl.tistory.com/304)

