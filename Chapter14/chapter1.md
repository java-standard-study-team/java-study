## 람다식 (Lambda expression)

### 람다식이란 ?
 * 메서드를 하나의 식(expression)으로 표현한 것이다.
 * 람다식은 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다.
 * 메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다식을 익명함수(anonymous function) 이라고도 함

```java
public int sum(int a, int b) {
        return a + b;
}

(int a, int b) -> a + b  ------>  (a, b) -> a + b //매개변수 타입 생략

(a) -> a + b -----> a -> a + b // 파라미터의 괄호 생략

```
* 컴파일러가 자동으로 추론해주기 때문에 위와같이 생략이 가능하다.

### 람다식 문법
```
(매개변수 목록) -> { 람다식 바디}
```
* 람다식의 시작 부분에는 파라미터들을 명시할 수 있다.

```java
Thread thread = new Thread(new Runnable() {

    @Override
    public void run() {
          System.out.println("Start Thread");
          Thread.sleep(1000);
          System.out.println("End Thread");
   }
});
```

```java
Thread thread = new Thread(() -> {
          System.out.println("Start Thread");
          Thread.sleep(1000);
          System.out.println("End Thread");
});
```

* 이런식으로 좀 더 간단하게 구현이 가능해진다.


### 함수형 인터페이스(Functional interface)
* 추상 메서드가 1개만 있는 인터페이스 <br>
 -> default 메서드와 static 메서드는 존재할 수 있다.

> 한마디로 구현되지 않은 단일 메서드만 포함되어 있으면 함수형 인터페이스이다.

```java
public interface FunctionInterface {
    public void method();
}
// 단일 메서드만 있으므로 함수형 인터페이스이다.
```

```java
@FunctionalInterface
public interface FunctionInterface {
    public void method();
    public default void defaultMethod(){}
    public static void staticMethod(){}
    
    // 기본 메서드와 정적 메서드가 존재하지만,
    // 실제 추상메서드는 하나이므로 함수형 인터페이스이다.
}
// 단일 메서드만 있으므로 함수형 인터페이스이다.
```

> @FunctionalInterface 어노테이션은 함수형 인터페이스를 올바르게 선언하였는지 체크해주는 어노테이션이다.
> 
> 정상적으로 선언하지 않았으면 컴파일 시점에 에러가 발생한다


### 인터페이스 구현

```java
@FuncionalInterface
public interface FunctionInterface<T, R> {
    public R sum (T x, T y);
    // 두 개의 매개변수의 합을 반환하는 제네릭 함수형 인터페이스
}
```

```java
// 함수형 인터페이스 구현
public class FunctionalInterfaceClass 
        implements MyFunctionInterface<Integer, Integer>{
  @Override
  public Integer sum(Integer x, Integer y) {
    return x + y;
  }
}

public class Main {
  public static void main(String args[]) {
    FunctionalInterfaceClass obj = new FunctionalInterfaceClass();
    System.out.println(obj.sum(10, 20));
  }
}
```




