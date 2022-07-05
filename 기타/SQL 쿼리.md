# SQL 쿼리 간단 정리

> 쿼리문 헷갈릴 때마다 확인할 용도로 제작

<br>

[링크](https://www.w3schools.com/sql/trysql.asp?filename=trysql_asc)

위 링크에서 쿼리문 사용하면서 기록

<br>

### 데이터 검색

###### SELECT 문을 사용하여 데이터 조회

Q1. 고객(Customer)의 이름과 국가를 조회

```sql
SELECT CustomerName, Country FROM Customers;
```

<br>

Q2. 고객(Customer) 정보 전체 조회

```sql
SELECT * FROM Customers;
```

<br>

Q3. 고객(Customer)의 국가 목록 조회(중복X)

```sql
SELECT DISTINCT Country FROM Customers;
```

<br>

###### WHERE문을 사용하여 조건에 맞는 데이터 조회

Q1. 국가가 France인 고객 조회

```sql
SELECT * FROM Customers WHERE Country='France';
```

<br>

Q2. ContactName이 ‘Mar’로 시작하는 고객 조회

```sql
SELECT * FROM Customers WHERE ContactName LIKE 'Mar%';
```

<br>

Q3. FirstName이 ‘et’로 끝나는 직원 조회

```sql
SELECT * FROM Customers WHERE ContactName LIKE '%et';
```

<br>

###### AND, OR, NOT을 사용하여 세밀한 조건 설명

Q1. 국가가 France이고 ContactName이 ‘Mar’로 시작하는 고객(Customers) 조회

```sql
SELECT * FROM Customers WHERE Country='France' AND ContactName LIKE 'Mar%';
```

<br>

Q2. 국가가 France이거나 ContactName이 ‘Mar’로 시작하는 고객 조회

```sql
SELECT * FROM Customers WHERE Country='France' OR ContactName LIKE 'Mar%';
```

<br>

Q3. 국가가 France가 아니고 ContactName이 ‘Mar’로 시작하지 않는 고객 조회

```sql
SELECT * FROM Customers WHERE NOT Country='France' AND NOT ContactName LIKE 'Mar%';
```

<br>

###### IN, BETWEEN을 사용하여 조건 설정

Q1. 국가가 France 혹은 Spain 사는 고객 조회

```sql
SELECT * FROM Customers WHERE Country In ('France', 'Spain');
```

<br>

Q2. 가격이 15에서 20사이인 상품(Products) 조회

```sql
SELECT * FROM Products WHERE Price BETWEEN 15 AND 20;
```

<br>

Q3. 가격이 15에서 20사이인 상품(Products) 의 생산자 목록 조회

```sql
SELECT * FROM Suppliers WHERE SupplierID IN (SELECT SupplierID FROM Products WHERE Price BETWEEN 15 AND 20);
```

<br>

###### NULL값 처리

Q1. 우편번호가 NULL인 고객목록 조회

```sql
SELECT * FROM Customers WHERE PostalCode IS NULL;
```

<br>

Q2. 우편번호가 NULL 이 아닌 고객목록 조회

```sql
SELECT * FROM Customers WHERE PostalCode IS NOT NULL;
```

<br>

---

### 데이터 정렬과 연산

###### ORDER BY를 이용해 조회시 정렬 적용

Q1. 상품 이름 오름차순(ASC)으로 조회

```sql
SELECT * FROM Products ORDER BY ProductName ASC;
```

<br>

Q2. 상품가격 내림차순(DESC)으로 조회

```sql
SELECT * FROM Products ORDER BY Price DESC;
```

<br>

Q3. 상품 이름 오름차순(ASC)으로, 상품가격 내림차순(DESC)으로 조회

```sql
SELECT * FROM Products ORDER BY ProductName ASC, Price DESC;
```

<br>

###### TOP, LIMIT, ROWNUM을 사용해 조회 건수를 제한하여 데이터 조회

Q1. 국가가 UK인 고객 중 이름순 3명 조회

```sql
SELECT * FROM Customers WHERE Country='France' LIMIT 3;
```

<br>

Q2. 국가가 UK인 고객 중 5번 째부터 이름순 3명 조회

```sql
SELECT * FROM Customers WHERE Country='France' LIMIT 3 OFFSET 5;
```

<br>

###### CASE를 사용해 조건에 따라 값을 변환

Q1. 상품 가격이 30 미만이면 '저', 30~50 이면 '중', 50 초과는 '고'로 조회

```sql
SELECT *,
	CASE
		WHEN Price<30 THEN '저'
		WHEN 30<=Price<=50 THEN '중'
       WHEN Price>50 THEN '고'
	END as 가격상태
FROM Products;
```

<br>

---

### 데이터 집계

###### COUNT, AVG, SUM 집계함수 사용

Q1. France에 거주하는 고객수 조회 : 국가명, 고객수 표시

```sql
SELECT Country, COUNT(*) FROM Customers WHERE Country='France'
```

<br>

Q2. 전체상품 평균가격 계산

```sql
SELECT AVG(Price) FROM Products
```

<br>

Q3. 주문 상품 수량 합계 계산: 주문 수량 합계 표시

```sql
SELECT SUM(Quantity) FROM OrderDetails 
```

<br>

###### MIN, MAX 집계함수 사용

Q1. 상품 가격 중 최소값 조회

```sql
SELECT MIN(Price) FROM Products
```

<br>

Q2. 상품 가격 중 최대값 조회

```sql
SELECT MAX(Price) FROM Products
```

<br>

###### GROUP BY를 사용해 열 기준 데이터 그룹핑

Q1. 국가 별 고객수 조회 (고객수 기준 오름차순): 국가명, 고객수 표시

```sql
SELECT Country, COUNT(*) FROM Customers
GROUP BY Country
ORDER BY COUNT(*) ASC;
```

<br>

Q2. 국가 별, 도시 별 고객수 조회 (고객수 기준 내림차순): 국가명, 도시명, 고객수 표시

```sql
SELECT Country, City, COUNT(*) FROM Customers
GROUP BY Country, City
ORDER BY COUNT(*) DESC;
```

<br>

###### HAVING을 사용해 집계데이터를 기준으로 조건 설정

Q1. 국가별 고객수를 조회하고 그 중 5명 초과인 국가만 조회 (고객수 내림 차순): 국가명, 고객수 표시

```sql
SELECT Country, COUNT(*) FROM Customers
GROUP BY Country
HAVING COUNT(*)>5
ORDER BY COUNT(*) ASC;
```

<br>

---

### 기타

###### 주석

```sql
-- select all
SELECT * FROM Customers;
```

<br>

###### Alias

```sql
SELECT Country, COUNT(*) AS Num_of_customers FROM Customers
GROUP BY Country
HAVING COUNT(*)>5
ORDER BY COUNT(*) ASC;

-- AS 생략 가능
SELECT Country, COUNT(*) Num_of_customers FROM Customers
GROUP BY Country
HAVING COUNT(*)>5
ORDER BY COUNT(*) ASC;
```

<br>

---

### 데이터 조인

###### JOIN

Q1. 상품을 조회하는데, 카테고리 이름과 함께 보이도록 조회

```sql
SELECT * FROM Products P
JOIN Categories C
ON P.CategoryID=C.CategoryID
```



