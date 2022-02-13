# Comparable vs Comparator

## 두 개의 공통점
- 인터페이스이다.
- 객체를 비교할 수 있도록 만든다.
</br></br>

## 두 개의 차이점
**1. Comparable**
- `compareTo()`메서드를 오버라이드 해서 사용
- ()안의 매개변수가 1개
- 자기자신과 들어오는 매개변수를 비교
- lang 패키지에 존재하므로 import해줄 필요가 없다.
```Java
public class Class implements Comparable<Type> { 
 
	@Override
	public int compareTo(Type a) {
    // 비교관련 코드 구현
    // Type에 객체의 type을 쓰면 된다.
    // 일반적으로 정렬 기준은 오름차순이다.
    // 현재 객체 > 파라미터로 넘어온 객체: 양수 리턴
    // 현재 객체 == 파라미터로 넘어온 객체: 0 리턴
    // 현재 객체 < 파라미터로 넘어온 객체: 음수 리턴
    // 음수 또는 0이면 객체의 자리가 그대로 유지되며, 양수인 경우에는 두 객체의 자리가 바뀐다.
	}
}
//이런 식으로 Override해서 사용해야된다.
```
</br></br>

**2. Comparator**
- `compare()`메서드를 오버라이드 해서 사용
- ()안의 매개변수가 2개
- 두 매개변수 객체를 비교
- Comparator은 util패키지에 존재하므로 import를 해줘야 된다.
```Java
import java.util.Comparator;	// import가 필요함
import java.util.Arrays;

public class Class implements Comparator<Type> { 
 
 //Comparator과 sort를 사용해서 정렬하는 법
    Arrays.sort(array, new Comparator<Type>() {
            
    @Override
        public int compare(int[] o1, int[] o2) {
        return o1[1] - o2[1];   // 오름차순 정렬
        // 내림차순 정렬을 하고 싶다면 o2와 o1의 위치를 바꿔줍니다
        // return o2[1] - o1[1];
        }
    });
}
```
