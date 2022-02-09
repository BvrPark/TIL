>해당글은 얄팍한 코딩사전님의 [MySQL강의](https://www.youtube.com/watch?v=dgpBXNa9vJc&t=1578s)를 정리해놓은 것 입니다.

## UNION - 집합으로 다루기
|연산자|설명|
|--|--|
|UNION|중복을 제거한 집합
|UNION ALL|중복을 제거하지 않은 집합|

```sql
SELECT CustomerName AS Name, City, Country, 'CUSTOMER'
FROM Customers
UNION
SELECT SupplierName AS Name, City, Country, 'SUPPLIER'
FROM Suppliers
ORDER BY Name;
-- JOIN과 비슷하지만 좀 다른것이 JOIN은 좌우로 연결하지만
-- UNION은 위아래로 연결한다.
-- 위의 내용은 각각 ~Name들을 Name으로 이름을 바꿔서 Name을 기준으로 정렬한다.
-- 일반적으로 UNION은 합집합이여서 중복된 것을 제거해서 나타낸다.
-- UNION ALL은 중복된 값을 제거하지 않고 다 나타내 준다.

SELECT CategoryID AS ID
FROM Categories
WHERE 
  CategoryID > 4
  AND CategoryID NOT IN (
    SELECT EmployeeID
    FROM Employees
    WHERE EmployeeID % 2 = 0
  );
-- 차집합을 나타내는 방법

SELECT ID FROM (
  SELECT CategoryID AS ID FROM Categories
  WHERE CategoryID > 4
  UNION ALL
  SELECT EmployeeID AS ID FROM Employees
  WHERE EmployeeID % 2 = 0
) AS Temp 
GROUP BY ID 
HAVING COUNT(*) = 1;
-- 대칭차집합(합집합에서 교집합을 뺀 것)을 나타내는 방법
```
