>해당글은 얄팍한 코딩사전님의 [MySQL강의](https://www.youtube.com/watch?v=dgpBXNa9vJc&t=1578s)를 정리해놓은 것 입니다.
# JOIN - 여러 테이블을 조립하는 것
## 1. JOIN(INNER JOIN) - 내부 조인(교집합)
- 양쪽 모두에 NULL

```sql
SELECT * FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID; 

-- Categories와 Products테이블에서 CategoryID가 같은 것들 끼리 묶는다.
-- Categories테이블과 Products 테이블을 위의 조건을 고려해서 한번에 뽑아낸다.


SELECT C.CategoryID, C.CategoryName, P.ProductName  -- 어느 테이블의 컬럼인지 명시
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID; 

-- ambiguous 주의!(명시 안해줄 수 ambiguous에러 뜸)
```
</br>

- 여러 테이블 JOIN가능
```sql
SELECT
  C.CategoryID, C.CategoryName, 
  P.ProductName, 
  O.OrderDate,
  D.Quantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID;
```
</br>

- JOIN한 테이블을 GROUP할 수도 있다.
```sql
SELECT 
  C.CategoryName, P.ProductName,
  MIN(O.OrderDate) AS FirstOrder,
  MAX(O.OrderDate) AS LastOrder,
  SUM(D.Quantity) AS TotalQuantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID
GROUP BY C.CategoryID, P.ProductID;
```
</br>

- SELF JOIN도 가능
```sql
SELECT
  E1.EmployeeID, CONCAT_WS(' ', E1.FirstName, E1.LastName) AS Employee,
  E2.EmployeeID, CONCAT_WS(' ', E2.FirstName, E2.LastName) AS NextEmployee
FROM Employees E1 
JOIN Employees E2
ON E1.EmployeeID + 1 = E2.EmployeeID;

-- 1번의 전, 마지막 번호의 다음은?
```
</br></br>

## 2. LEFT/RIGHT OUTER JOIN - 외부조인
- 반대쪽에 데이터가 있든 없든(NULL), 선택된 방향에 있으면 출력 - 행 수 결정

```sql
SELECT
  E1.EmployeeID, CONCAT_WS(' ', E1.FirstName, E1.LastName) AS Employee,
  E2.EmployeeID, CONCAT_WS(' ', E2.FirstName, E2.LastName) AS NextEmployee
FROM Employees E1 LEFT JOIN Employees E2
-- JOIN기준으로 왼쪽에 있는 테이블(E1)의 데이터 다 출력, 그에 맞는 오른쪽에 있는 테이블(E2)의 데이터가 없을 시 NULL이 들어가서 출력
ON E1.EmployeeID + 1 = E2.EmployeeID
ORDER BY E1.EmployeeID;
-- LEFT를 RIGHT로 바꾸면 반대로 작용함


SELECT
  IFNULL(C.CustomerName, '-- NO CUSTOMER --'),
  IFNULL(S.SupplierName, '-- NO SUPPLIER --'),
  IFNULL(C.City, S.City),
  IFNULL(C.Country, S.Country)
FROM Customers C LEFT JOIN Suppliers S   -- 빈 값에 NO SUPPLIER가 출력이 되고
ON C.City = S.City AND C.Country = S.Country;

-- LEFT를 RIGHT로 바꿨을 땐 NO CUSTOMER가 출력이 됨.
```
</br></br>

## 3. CROSS JOIN - 교차 조인
- 조건없이 모든 조합 반환
```sql
SELECT
  E1.LastName, E2.FirstName
FROM Employees E1
CROSS JOIN Employees E2
ORDER BY E1.EmployeeID;
-- ON을 쓰지않고 E1,E2테이블의 EmployeeID를 조합할 수 있는 모든 경우의 수를 출력 
```
