## ERD

![](C:\Users\student\SSQL\ERD2.png)



## DDL

```SQL
CREATE TABLE PRODUCTS(
    PDNO NUMBER(10, 0),
    PDNAME VARCHAR2(10),
    PDSUBNAME VARCHAR2(10),
    FACTNO VARCHAR2(5),
    PDDATE DATE,
    PDCOST NUMBER(10, 0),
    PDPRICE NUMBER(10, 0),
    PDAMOUNT NUMBER(10, 0)
);

CREATE TABLE FACTORY(
	FACTNO VARCHAR2(5),
    FACNAME VARCHAR2(14),
    FACLOC VARCHAR2(13)
);
ALTER TABLE PRODUCTS ADD PRIMARY KEY(PDNO);
ALTER TABLE FACTORY ADD PRIMARY KEY(FACTNO);
ALTER TABLE PRODUCTS ADD FOREIGN KEY(FACTNO) REFERENCES FACTORY(FACTNO);

```



## DML

| 공장번호 | 공장명 | 공장위치 |
| :------: | :----: | :------: |
|   A01    | SSCOM  |  SEOUL   |
|   A03    |  VEGA  |   GUMI   |
|   C02    | APPLE  |  CHINA   |

```SQL
INSERT INTO FACTORY VALUES('A01', 'SSCOM', 'SEOUL');
INSERT INTO FACTORY VALUES('A03', 'VEGA', 'GUMI');
INSERT INTO FACTORY VALUES('C02', 'APPLE', 'CHINA');
```



| 제품번호 | 제품카테고리 | 제품명 | 공장번호 | 제품생산일 | 제품원가 | 제품가격 | 재고수량 |
| -------- | ------------ | ------ | -------- | ---------- | -------- | -------- | -------- |
| 195020   | PHONE        | GALAXY | A01      | 2019.05.30 | 30       | 103      | 3        |
| 195030   | PHONE        | IPHONE | C02      | 2019.05.30 | 25       | 102      | 5        |
| 195040   | PHONE        | V50    | A03      | 2019.06.01 | 20       | 88       | 10       |
| 197501   | TV           | OLEDTV | A03      | 2019.06.02 | 100      | 290      | 8        |
| 197505   | TV           | QLEDTV | A01      | 2019306.04 | 101      | 300      | 10       |

```SQL
INSERT INTO PRODUCTS VALUES (195020, 'PHONE', 'GALAXY','A01',
                             TO_DATE('2019-05-30','YYYY-MM-DD'), 30, 103, 3);
INSERT INTO PRODUCTS VALUES (195030, 'PHONE', 'IPHONE', 'C02', 
                             TO_DATE('2019-05-30','YYYY-MM-DD'), 25, 102, 5);
INSERT INTO PRODUCTS VALUES (195040, 'PHONE', 'V50', 'A03', 
                             TO_DATE('2019-05-30','YYYY-MM-DD'), 20, 88, 10);
INSERT INTO PRODUCTS VALUES (197501, 'TV', 'OLEDTV', 'A03', 
                             TO_DATE('2019-06-02','YYYY-MM-DD'), 100, 290, 8);
INSERT INTO PRODUCTS VALUES (197505, 'TV', 'QLEDTV', 'A01', 
                             TO_DATE('2019-06-04','YYYY-MM-DD'), 101, 300, 10);
```



