```SQL
-- 직업별 입사 월별 월급의 최대값을 구하시오. 단 이름에 S가 들어간 사람을 대상으로 하고 입사 월이 12월인 사람을 대상으로 하시오.
 
SELECT JOB,TO_CHAR(HIREDATE, 'MM') AS MONTH, MAX(SAL) FROM EMP
WHERE TO_CHAR(HIREDATE, 'MM') = '12' AND ENAME LIKE '%S%'
GROUP BY JOB, TO_CHAR(HIREDATE, 'MM');
```

```SQL
-- JOB 별 SALARY의 평균을 구하고 평균이 2000이상인 그룹중에서 가장 큰 평균을 갖은 그룹과 가장 작은 평균을 갖은 그룹의 액수를 각각 출력하시오.

SELECT MAX(AVG(SAL)) AS MAX_SALARY, MIN(AVG(SAL)) AS MIN_SALARY FROM EMP
GROUP BY JOB
HAVING AVG(SAL) >= 2000;
```

```SQL
-- EMP 테이블에서 JOB별, MGR별 ENAME을 구하시오. 단, HIREDATE가 9월, 12월이며, MGR이 7698이다. 그리고 JOB 오름차순으로 정렬하시오.

SELECT JOB, MGR, ENAME FROM EMP
WHERE TO_CHAR(HIREDATE, 'MM') IN (9, 12) AND MGR = 7698
ORDER BY JOB ASC;
```

```SQL
-- 입사한 달로 사원들을 그룹짓고 DEPTNO가 20,30 이고 4, 5, 12월에 입사한 사원들의 평균임금을 출력한다. MONTH 오름차순으로 출력한다.

SELECT TO_CHAR(HIREDATE, 'MM') AS MONTH, AVG(SAL) FROM EMP
WHERE DEPTNO IN (20,30)
GROUP BY TO_CHAR(HIREDATE, 'MM')
HAVING TO_CHAR(HIREDATE, 'MM') IN(4,5,12)
ORDER BY MONTH ASC;
```

```SQL
-- 고용 월별, 직업별로 SAL+COMM의 평균을 출력하고 SAL+COMM의 평균이 2000이상이며 ENAME에 A가 있는 경우를 고용 월별을 기준으로 내림차순으로 정렬하시오.

SELECT JOB, TO_CHAR(HIREDATE, 'MM') AS MM, AVG(SAL+NVL(COMM,0)) AS MEAN FROM EMP
WHERE ENAME LIKE '%A%'
GROUP BY TO_CHAR(HIREDATE, 'MM'), JOB
HAVING AVG(SAL+NVL(COMM,0))>=2000
ORDER BY MM DESC;
```

```SQL
-- 연도별 직업별 SAL평균 1981년도에 입사한 사람중 부서 10,20 중에서 SAL 평균 1500 이상인 사람의 평균SAL을 출력하세요

SELECT TO_CHAR(HIREDATE, 'YYYY') AS 입사년도, JOB, AVG(SAL) FROM EMP
WHERE TO_CHAR(HIREDATE, 'YYYY')=1981 AND DEPTNO IN (10,20)
GROUP BY TO_CHAR(HIREDATE, 'YYYY'),JOB
HAVING AVG(SAL) >= 1500
ORDER BY AVG(SAL) ASC;
```

```SQL
-- 부서 별 월급의 평균을 조회하는 VIEW를 작성 하시오
CREATE VIEW DEPTSAL(DEPTNO, SALAVG)
AS
SELECT DEPTNO, AVG(SAL) FROM EMP
GROUP BY DEPTNO;


```

```SQL
-- 지역 별 입사 일이 가장 늦은 사원의 정보를 출력하시오 LOC, ENAME, HIREDATE

SELECT d.LOC, e.ENAME, e.HIREDATE FROM EMP e, DEPT d
WHERE e.DEPTNO = d.DEPTNO
AND (SYSDATE-e.HIREDATE) <= (
    SELECT MIN(SYSDATE-e2.HIREDATE) FROM EMP e2, DEPT d2
    WHERE d.LOC = d2.LOC
    GROUP BY d.LOC)
```

