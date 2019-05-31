# SQL

 

* SAL가 2000~3000 인 사람들 중 JOB과 ENAME에 'S'가 포함된 사람

```sql
SELECT * FROM EMP 
WHERE SAL >= 2000 AND SAL <= 3000
AND (JOB LIKE '%S%' OR ENAME LIKE '%S%');
```



* 1981년 입사한 사람들의 JOB은 오름차순으로, 연봉은 내림차순으로 정렬하시오.

```sql
SELECT * FROM EMP 
WHERE HIREDATE >= '01/01.1981' AND HIREDATE < '12/31.1981'
ORDER BY JOB ASC, SAL DESC;
```



* 단, 연봉(ANNSAL)은 SAL의 세금은 22%, COMM의 세금은 12%로 계산.

   +) JOB, ENAME 오름차순으로 정렬, 연봉은 25000이상

```sql
SELECT JOB, ENAME, SAL, ((SAL*12)*0.78)+((NVL(COMM,0)*12)*0.88) AS ANNSAL FROM EMP
WHERE ((SAL*12)*0.78)+((NVL(COMM,0)*12)*0.88) > 25000
ORDER BY JOB ASC, ENAME ASC;
```



* SAL 세금 10% COMM 세금 5% 연봉 계산하여 COLUMN 이름 ASAL로 하고,

  연봉이 30000~20000사이인 직원을 내림차순으로 출력하시오.

```SQL
SELECT ENAME, SAL,
((SAL*12)*0.9)+((NVL(COMM,0)*12)*0.95) AS YSAL FROM EMP
WHERE ((SAL*12)*0.9)+((NVL(COMM,0)*12)*0.95) > 20000
AND ((SAL*12)*0.9)+((NVL(COMM,0)*12)*0.95) < 30000
ORDER BY 3 DESC;
```



* EMP 테이블에서  comm이 null인 것 중 연봉계산(30% 세금)해서 

  이름 가운데에 'I'가 들어가는 사람들을 연봉으로 내림차순으로 정렬하시오.

```SQL
SELECT * FROM EMP
WHERE COMM IS NULL
AND ENAME LIKE '%I%'
ORDER BY (SAL*12)*0.7 DESC;
```



* 01/01/1981 부터 01/01/1982 까지 고용된사람 의

  ENAME 과 HIREDATE, SAL 을 SAL 순으로 출력하세요

```SQL
SELECT ENAME, HIREDATE, SAL FROM EMP
WHERE HIREDATE >= '01/01.1981' AND HIREDATE < '01/01.1982'
ORDER BY SAL ASC;
```





* 

  * COMM이 없고 이름에 I나 M이 들어가는 사람 이름의 첫글자는 대문자로 나머지는 소문자로하여 DEPTNO, NAME, JOB, HIREDATE, COMM을 DEPTNO 내림차순으로 정렬하여 출력하시오.

  ```SQL
  SELECT DEPTNO, REPLACE(ENAME,SUBSTR(ENAME, 2, LENGTH(ENAME)), LOWER(SUBSTR(ENAME, 2, LENGTH(ENAME)))) AS NAME,
  JOB, HIREDATE, COMM FROM EMP
  WHERE (ENAME LIKE '%I%' OR ENAME LIKE '%M%')
  AND COMM IS NULL
  ORDER BY DEPTNO DESC;
  ```

  * ENAME, JOB, HIREDATE, SYSDATE와  ASAL = 연봉(월급*12+COMM*12)계산

      REALASAL =  ASAL > 50000 이면 세금 50%,  50000 > ASAL > 35000 이면 세금 30%,  나머지는 세금 20%

     TOTALMONEY = (SAL+COMM)*지금까지 일한 MONTH수(세금X, 소수점은 반올림)

     직업 오름차순으로 정렬 후 , TOTALMONEY 내림차순으로 정렬하세요.:)  

  ```SQL
  SELECT ENAME, JOB, HIREDATE, SYSDATE, 
  (SAL*12)+(NVL(COMM, 0)*12) AS ASAL,
  CASE WHEN (SAL*12)+(NVL(COMM, 0)*12) >= 50000
       THEN (SAL*12)+(NVL(COMM, 0)*12)*0.5
       WHEN (SAL*12)+(NVL(COMM, 0)*12) >= 35000 AND (SAL*12)+(NVL(COMM, 0)*12) < 50000
       THEN (SAL*12)+(NVL(COMM, 0)*12)*0.7
       ELSE (SAL*12)+(NVL(COMM, 0)*12)*0.8
  END AS REALASAL,
  ROUND((SAL+(NVL(COMM, 0)*12))*(MONTHS_BETWEEN (SYSDATE, HIREDATE)),0) AS TOTALMONEY
  FROM EMP
  ORDER BY JOB ASC,((SAL+(NVL(COMM, 0)*12))*(MONTHS_BETWEEN (SYSDATE, HIREDATE))) DESC;
  ```

*  

  * 1. 근무일수(월별)를 계산하여 소수점 이하는 버리시오

    2. 근무일수에 따라서 보너스를 지급한다.

       450일 이상 = 월급의 50%, 440일 이상 = 월급의 20%, 그 아래는 0%

       특별 보너스로 입사일이 5월이면 월급의 50% 보너스 추가 지급

  ```SQL
  SELECT ENAME, ROUND(MONTHS_BETWEEN(SYSDATE, HIREDATE),0) AS WORK,
  SAL,
  CASE WHEN ROUND(MONTHS_BETWEEN(SYSDATE, HIREDATE),0) >= 450
       THEN SAL*0.5
       WHEN ROUND(MONTHS_BETWEEN(SYSDATE, HIREDATE),0) >= 440 AND ROUND(MONTHS_BETWEEN(SYSDATE, HIREDATE),0) < 450
       THEN SAL*0.8
       ELSE 0
  END AS BONUS
  
  FROM EMP
  ORDER BY ROUND(MONTHS_BETWEEN(SYSDATE, HIREDATE),0) DESC;
  ```

  







