## item04: 인스턴스화를 막으려거든 private 생성자를 사용하라

> 정적 메서드와 정적 필드로 구성된 클래스는 **인스턴스화가 필요없다.** <br> 
인스턴스화를 막기 위해서 어떤 방법을 쓰면 좋을까? 🤔

<br>

### 정적 메서드와 정적 필드만을 담은 클래스
- 정적 메서드와 정적 필드는 객체를 생성하지 않아도 직접 접근이 가능하다.
- 정적 메서드와 정적 필드는 인스턴스화가 필요가 없다. (내부적으로 상태를 유지할 필요가 없다. → 데이터 필드에 값을 저장하거나, 그 값을 바탕으로 동작할 필요가 없다. / 확장할 필요가 없다. → 처음 선언된 기능 그대로 충분하다.)
- 정적 메서드가 포함된 클래스는 상속이 가능하나, 정적 메서드를 오버라이딩 할 수는 없다. → 굳이 확장 필요 없다~
- 보통 유틸성 클래스로 helper의 성격을 갖는 클래스이다. (유틸리티 클래스)
    

<br>

1. 기본 타입 값이나 배열 관련 메서드
- `java.lang.Math` : 기본 타입 값에 대한 수학적 연산 제공
    ```java
    Math.abs(-10); // 10 
    Math.max(10, 20); // 20 
    Math.min(10, 20); // 10
    Math.pow(2, 3); // 2^3, 8
    Math.sqrt(16)l //4
    ```
- `java.util.Arrays` : 배열을 조작하기 위한 유틸리티 메서드 제공
    ```java
    Arrays.toString(array); // [1,2,3,4,5] 배열을 문자열 형태로 반환
    Arrays.sort(array); // 배열을 오름차순으로 정렬
    Arrays.copyOf(array, length); // 배열을 복사하여 새로운 길이 배열 생성 
    ```

<br>

2. 특정 인터페이스를 구현하는 객체를 생성해주는 정적 메서드 
- `java.util.Collections` 
    - List, Set, Map 같은 컬렉션 인터페이스(프로그래밍에 필요한 여러 자료구조)를 구현하는 객체를 생성할 수 있는 정적 메서드를 제공한다. 
    - 컬렉션 조작 메서드를 제공한다. (정렬, 검색, 섞기)
    - 단순히 컬렉션을 생성하거나 조작하는 메서드만 제공한다. 


<br>

3. final 클래스 관련 메서드
- final 클래스는 상속해 하위 클래스를 만들어 확장하는 것이 불가능하다. 


<br>


### 정적 멤버 클래스曰 인스턴스 되고 싶지 않아 ... 
- 정적 멤버만 담은 클래스는 인스턴스화가 필요없다. 
- 하지만, 생성자를 명시하지 않으면 컴파일러가 자동으로 기본 생성자(매개변수를 받지 않는 public 생성자)를 만든다. 

<br>

1. 추상 클래스
- 추상 클래스는 직접 인스턴스화가 불가능하다. 
- 하지만💧 추상 클래스를 상속받은 하위 클래스로 인스턴스화 할 수 있다.

<br>

2. private 생성자 
- 생성자를 명시하여 컴파일러가 기본 생성자를 만들지 못하도록 하자! → private 생성자 추가
```java
public class UtilityClass {
    // 기본 생성자가 만들어지는 것을 막는다. (인스턴스화 방지용)
    private UtilityClass() {
        throw new AssertionError();
    }
}
```
- private이라 클래스 바깥에서 접근할 수 없다. (하위 클래스도 마찬가지)
- 클래스의 생성자를 호출하지 못하도록 강제한다.


(+) AssertionError: 주로 디버깅 목적으로 사용 / `클래스는 인스턴스를 만들려고 했으니 잘못된 동작이야!` 라는 의미
