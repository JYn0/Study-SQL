```SQL
CREATE VIEW T_EMP (ENO,ENM,SAL,DNO) AS (SELECT EMPNO, ENAME, SAL, DEPTNO FROM EMP)

CREATE VIEW T_DEPT (DNO,DNM,LOC) AS (SELECT DEPTNO, DNAME, LOC FROM DEPT)

SELECT * FROM T_EMP

SELECT * FROM T_DEPT

CREATE VIEW T_SAL (ENO,ASAL)
AS (SELECT EMPNO, (SAL*12)+(NVL(COMM,0)*12) FROM EMP);

-- 직원 정보를 조회하시오 ENO,ENM,SAL,ASAL,DNM.LOC
SELECT e.ENO, e.ENM, e.SAL, a.ASAL, d.DNM, d.LOC FROM T_EMP e, T_SAL a, T_DEPT d
WHERE e.DNO = d.DNO AND e.ENO=a.ENO

-- 표준
SELECT e.ENO, e.ENM, e.SAL, a.ASAL, d.DNM, d.LOC
FROM T_EMP e INNER JOIN T_DEPT d ON(e.DNO = d.DNO)
INNER JOIN T_SAL a ON (e.ENO = a.ENO)

-- JONES 부서원만 조회하시오
SELECT e.ENO, e.ENM, e.SAL, a.ASAL, d.DNM, d.LOC FROM T_EMP e, T_SAL a, T_DEPT d
WHERE e.DNO = d.DNO AND e.ENO=a.ENO AND e.DNO 
IN (SELECT DNO FROM T_EMP WHERE ENM = 'JONES')

-- 직원 정보를 조회하시오 ENO,ENM,SAL,ASAL,DNM.LOC
-- 부서 별 연봉의 평균 보다 많이 받는 직원을 조회
SELECT e.ENO, e.ENM, e.SAL, a.ASAL, d.DNM, d.LOC FROM T_EMP e, T_SAL a, T_DEPT d
WHERE e.DNO = d.DNO AND e.ENO=a.ENO AND a.ASAL >= (
SELECT AVG(s1.ASAL) FROM T_EMP e1, T_SAL s1
WHERE e1.ENO = s1.ENO AND e.DNO = e1.DNO
GROUP BY (e1.DNO) )


SELECT e.ENAME, d.LOC FROM EMP e INNER JOIN DEPT d
USING (DEPTNO)
```





```SQL
-- 전체 연봉의 평균보다 많게 받는 직원의 ENO, ENM, SAL, ASAL, RANK를 연봉이 높은순서로 정렬하여 조회하시오.

SELECT e.ENO, e.ENM, e.SAL, a.ASAL, RANK() OVER (ORDER BY a.ASAL DESC) AS RANKING, d.DNM
FROM T_EMP e, T_SAL a, T_DEPT d
WHERE e.DNO = d.DNO(+) AND e.ENO=a.ENO AND a.ASAL >= (
SELECT AVG(s1.ASAL) FROM T_SAL s1)
ORDER BY RANKING ASC

--ENO, EN, ASAL, DNM ,LO , RANK, TOTAL 을 출력하시오 부서별 월급의 평균 보다 많이 받는 직원을 출력하고, 순위를 2위까지만 출력한다. 단, DALLAS 지역 제외
SELECT e.ENO, e.ENM AS EN, a.ASAL, d.DNM, d.LOC AS LO,
RANK() OVER (ORDER BY a.ASAL DESC) AS RANK, (SELECT COUNT(*) FROM EMP) AS TOTAL
FROM T_EMP e, T_SAL a, T_DEPT d
WHERE RANK<3 AND e.DNO = d.DNO AND e.ENO = a.ENO AND NOT d.LOC = 'DALLAS' AND e.SAL >= (
    SELECT AVG(e1.SAL) FROM T_EMP e1, T_SAL s1 
    WHERE e1.ENO = s1.ENO AND e.DNO = e1.DNO
    GROUP BY (e1.DNO) )
```

```SQL
-- 입사 년도별로 월급을 가장 많이 받는 사람의 이름, SAL, 입사년도, 근속 년수를 구하고, 순위를 매기시오
SELECT ENAME, SAL, TO_CHAR(HIREDATE, 'YYYY') AS EMPYEAR, 
TRUNC(MONTHS_BETWEEN(SYSDATE, HIREDATE)/12,0) AS YEAR, 
RANK() OVER (ORDER BY TO_CHAR(HIREDATE, 'YYYY') ASC) AS RANKING FROM EMP e
WHERE SAL >= (SELECT MAX(SAL) FROM EMP e2
WHERE TO_CHAR(e.HIREDATE, 'YYYY')= TO_CHAR(e2.HIREDATE, 'YYYY')
GROUP BY TO_CHAR(HIREDATE, 'YYYY'))

-- LOCATION 별로 연봉(세금 계산은 하지 않는다.)과 연봉의 순위를 구한 뒤, 직원의 이름, 연봉, 부서 정보, 부서 내 랭킹을 LOC과 랭킹 순으로 모두 출력하시오.

SELECT e.ENM AS ENAME, s.ASAL, d.DNO, d.LOC, RANK() OVER (ORDER BY s.ASAL ASC) AS RANKING 
FROM T_EMP e, T_SAL s, T_DEPT d
WHERE e.ENO=s.ENO AND e.DNO=d.DNO
```

```SQL
--직원들의 정보와 해당 직원의 상사 정보를 같이 출력하시오. 왼쪽이 부하직원들의 정보, 오른쪽은 상사의 정보. MGR은 상사의 EMPNO를 나타낸다고 가정한다
SELECT e.EMPNO, e.ENAME, e.JOB, e.MGR, t.EMPNO, t.ENAME, t.JOB, t.MGR FROM EMP e, EMP t 
WHERE e.MGR = t.EMPNO

```

