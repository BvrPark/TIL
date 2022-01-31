# List, Set, Map에 대한 설명
</br></br>

## 1. List, Set, Map의 차이점(간단)
</br>

|종류|설명|
|--|--|
|**List**|기본적으로 데이터들이 순서대로 저장되며 중복을 허용한다.|
|**Set**|데이터들의 중복은 허용되지 않고, 순서도 보장할 수 없다.|
|**Map**|Key-Value의 형태로 저장이 되며, Key값의 중복은 허용하지 않고 Value값의 중복은 허용되지만 순서는 보장할 수 없다.|

</br></br>

## 2. List, Set, Map의 차이점(자세히)

### **List**
**1. 특징**
- 순서대로 저장되며 중복을 허용한다.
- 인덱스로 데이터에 접근이 가능.
- 크기가 가변적이다.
</br></br>

**2. 종류**
- `ArrayList`
    - 단방향 포인터 구조로 데이터에 순차적으로 접근함.
    - 배열을 기반으로 데이터를 저장.
    - 데이터의 삽입,삭제가 느림.
    - 데이터의 검색이 빠름.
    </br></br>
- `LinkedList`
    - 양방향 포인터 구조이다.
    - 데이터의 삽입, 삭제가 빠르지만 메커니즘은 복잡하다.
    - 데이터의 검색이 ArrayList보다 느림.
</br></br>

### **Set**
**1. 특징**
- 단순한 데이터의 집합이므로 순서가 존재하지 않는다.
- 데이터의 중복을 허용하지 않는다.
- 빠른 검색 속도를 가진다.
- Index값이 따로 존재하지 않기 때문에 iterator을 사용해서 데이터를 다룬다.
</br></br>

**2. 종류**
- `HashSet`
    - 인스턴스의 해시값을 기준으로 저장한다.
    - 순서를 보장하지 않는다.
    - NULL값을 허용한다.
    - TreeSet보다 삽입, 삭제가 빠르다.
</br></br>
- `TreeSet`
    - 이진 탐색 트리를 기반으로 한다.
    - 데이터들이 오름차순으로 정렬.
    - 데이터의 삽입과 삭제에는 시간이 오래 걸리지만, 정렬이 빠르다.
</br></br>

### **Map**
**1. 특징**
- Key-Value 한쌍으로 저장되는 데이터들의 집합.</br>
- Key에 대한 중복이 없고 순서를 보장하지 않는다.</br>
- 검색속도가 빠르다.</br>
- Set과 마찬가지로 Index값이 따로 존재하지 않기 때문에 iterator을 사용해서 데이터를 다룬다.
</br></br>

**2. 종류**
- `HashMap`
    - Key-Value값으로 NULL을 허용.
    - 동기화가 보장되지 않는다.
    - 검색에서 가장 뛰어난 성능을 보인다.
</br></br>
- `HashTable`
    - Key-Value값으로 NULL을 허용하지 않는다.
    - 동기화가 보장된다.
    - 병렬 프로그래밍이 가능하지만 HashMap보다 처리속도가 느리다.
</br></br>
- `TreeMap`
    - 이진 탐색 트리를 기반으로 Key와 Value를 저장한다.
    - Key값을 기준으로 오름차순으로 정렬되고 빠른 검색이 가능하다.
    - 저장을 하면서 동시에 정렬을 하기 때문에 시간이 오래 걸린다.
</br></br>

**3. 자주 사용하는 메서드**
- `getOrDefault(key,default-value)`
    - key값이 존재하면 그 키 값의 value를 반환
    - key값이 존재하지 않으면 default-value를 반환
</br></br>
- `put(key,value)`
    - Map안에 key-value형태로 값 넣기
</br></br>
- `get(key)`
    - Map안에 key값에 대응되는 value값 반환
</br></br>
- `containsKey(key)/containsValue(value)`
    - Map안에 특정 key/value값이 있는지 확인
</br></br>
- `remove(key)`
    - Map안의 내용삭제
</br></br>
- `keySet()`
    - Map안의 key값을 다 반환
</br></br>
- `entrySet()`
    - Map안의 key-value값을 다 반환

