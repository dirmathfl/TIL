# String, Date

### 루시와 엘라 찾기

```mysql
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
```

 

### 이름에 el이 들어가는 동물 찾기

```mysql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE NAME LIKE '%EL%' AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME
```

 

### 중성화 여부 파악하기

```mysql
SELECT 
    ANIMAL_ID, NAME,
    IF (SEX_UPON_INTAKE REGEXP 'Neutered|Spayed', 'O', 'X') AS 중성화
FROM ANIMAL_INS
```

 

### 오랜 기간 보호한 동물(2)

```mysql
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_INS INS
RIGHT JOIN ANIMAL_OUTS OUTS
ON OUTS.ANIMAL_ID = INS.ANIMAL_ID
WHERE INS.ANIMAL_ID IS NOT NULL
ORDER BY INS.DATETIME - OUTS.DATETIME
LIMIT 2
```

 

### DATETIME에서 DATE로 형 변환

```mysql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS 날짜
FROM ANIMAL_INS
```



## 문제 풀이

[프로그래머스: SQL - String, Date](https://dirmathfl.tistory.com/320)

