>해당글은 얄팍한 코딩사전님의 [MySQL강의](https://www.youtube.com/watch?v=dgpBXNa9vJc&t=1578s)를 정리해놓은 것 입니다.
### SELECT - 원하는 정보를 가지고 오는 것
* 시작하기전 편의상 테이블명은 **T**</br>
* 각 컬럼명들은 **A**,**B**,**C**로 가정</br>
1. 테이블의 모든 내용 보기
    ```sql
    SELECT * 
    FROM T;
    ```
    - 여기서 *는 테이블의 모든 colum(열)을 말함
    - 주석은 앞에 --붙이고 적으면 된다.</br></br>

2. 원하는 colum(열)만 골라서 보기
    ```sql
    SELECT A 
    FROM T;
    ```
    - 여러개의 colum 선택 가능
        ```sql
        SELECT A,B
        FROM T;
        ```
    - **테이블의 colum이 아닌 다른값들도 선택 가능**
        ```sql
        SELECT A,1,'문자열' 
        FROM T;
        ```

3. 원하는 row(행)만 골라서 보기
    - `WHERE`로 원하는 조건을 설정하여 그것만 따로 볼 수 있다.
        ```sql
        SELECT * 
        FROM T
        WHERE A = 3; -- 조건
        ```

4. 원하는 순서로 데이터 가져오기
    - `ORDER BY`를 사용해서 특정 colum을 기준으로 데이터를 정렬할 수 있다.
    - `ASC`는 오름차순, `DESC`는 내림차순인데 기본적으로 오름차순으로 정렬
        ```sql
        SELECT * 
        FROM T
        ORDER BY B;
        ```
        - B를 기준으로 오름차순으로 정렬</br></br>
    - 이것도 여러개 한꺼번에 정렬할 수 있다.
        ```sql
        SELECT * 
        FROM T
        ORDER BY B, C DESC;
        ```
        1. 우선 B를 기준으로 오름차순으로 정렬
        2. 그 다음, C를 기준으로 내림차순으로 정렬</br></br>
    
5. 원하는 만큼만 데이터 가져오기
    - `LIMIT 가져올 갯수`나 `LIMIT 건너뛸 갯수, 가져올 갯수`를 사용하여, 원하는 만큼 데이터를 가져올 수 있다.
        ```sql
        SELECT *
        FROM T
        LIMIT 10;
        ```
        - 처음부터 10개의 데이터만 가져온다.</br></br>
        ```sql
        SELECT *
        FROM T
        LIMIT 30, 10;
        ```
        - 앞의 30개의 데이터를 건너뛰고 10개의 데이터를 가져온다.</br></br>

6. 원하는 이름으로 데이터 가져오기
    - `AS`를 사용해서 컬럼명을 변경해서 가져올 수 있다.
        ```sql
        SELECT
            A AS APPLE
            B AS BANANA
            C AS CAT
        FROM T;
        ```
        - 데이터 조회시 A가 APPLE로, B가 BANAN로 C가 CAT으로 바뀜.</br></br>
    
    - 한글로도 변경 가능
        ```sql
        SELECT
            A AS '아이디'
            B AS '비밀번호'
            C AS '이름'
        FROM T;
        ```
