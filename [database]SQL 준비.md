# SQL 준비



## 데이터조회하기 (SELECT)



MIN MAX COUNT ROUND AVG()



## 정렬

두번 정령

ORDER BY NAME ASC, DATETIME DESC;

, 하고 다음 정렬을 적어주면 된다



LIMIT 출력 개수 지정 가능



* 중복 제거

SELECT COUNT(DISTINCT NAME) COUNT FROM ANIMAL_INS
WHERE NAME IS NOT NULL;





SELECT ANIMAL_TYPE, COUNT(ANIMAL_ID)  COUNT FROM ANIMAL_INS 

GROUP BY ANIMAL_TYPE



### GROUP  BY HAVING

SELECT NAME, COUNT(NAME) COUNT
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
GROUP BY NAME
having COUNT(NAME) >=2
ORDER BY NAME;



그룹으로 묶었을때의 조건을 AHVING으로 한다.



SELECT HOUR(DATETIME) HOUR, COUNT(DATETIME) COUNT
FROM ANIMAL_OUTS
GROUP BY HOUR(DATETIME)
HAVING HOUR>=9 AND HOUR<=19
ORDER BY HOUR ASC;







입양 시각 구하기 JOIN

```sql
-- 코드를 입력하세요
SET @hour =-1;
SELECT (@hour:=@hour +1) HOUR , 
( SELECT COUNT(DATETIME) FROM ANIMAL_OUTS WHERE HOUR(DATETIME)=@hour) COUNT
FROM ANIMAL_OUTS
WHERE @hour <23
```







### JOIN

SELECT A.ANIMAL_ID, A.NAME FROM ANIMAL_OUTS A LEFT JOIN ANIMAL_INS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE B.ANIMAL_ID IS NULL
ORDER BY A.ANIMAL_ID







## 테이블 구조 참조하기 (DESC)

asc 오름차순

desc 내림차순



order by a.animal_id asc



order by a.animal_id desc







## 검색 조건 지정하기 (WHERE)



### WHERE절 조건 조합하기



like 's%'

in 어떤 값들의 집합





between

not in

<>

!=

is null  //  is not null




