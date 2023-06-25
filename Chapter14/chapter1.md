## 람다식 (Lambda expression)

### 람다식이란 ?
 * 메서드를 하나의 식(expression)으로 표현한 것이다.
 * 람다식은 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다.
 * 메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다식을 익명함수(anonymous function) 이라고도 함 <br>
  ->  익명함수를 쓰는 이유 : 일시적으로 사용하며 후에 재사용이 되지 않는다. <br>
      즉, 프로그램에서 일시적으로 한번만 사용되고 버려지는 객체라 생각하면 편하다. <br>
      때문에 재사용성이 없으며 확장성을 활용하는 것이 유지보수에 안 좋을 때 사용하는 것이다.

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

### 람다식의 장단점
 * 장점
   1. 코드를 간결하게 작성가능
   2. 코드가 간결하며 개발자의 의도가 명확히 들어나며 가독성이 향상
   3. 함수를 만드는 과정없이 한번에 처리 할 수 있기 때문에 코딩 작성 소요시간이 줄어든다.
   4. 병렬프로그래밍이 용이
* 단점
   1. 람다를 사용하면서 만드는 무명함수는 재사용이 불가
   2. 디버깅이 다소 어렵다.
   3. 재귀로 만들 경우에 다소 부적합한면이 있음

### 함수형 인터페이스(Functional interface)
* 추상 메서드가 1개만 있는 인터페이스 <br>
 -> default 메서드와 static 메서드는 존재할 수 있다.

> 한마디로 구현되지 않은 단일 메서드만 포함되어 있으면 함수형 인터페이스이다.

> 익명함수는 모두 **일급 객체** 라는 특징을 갖고 있다.(메서드를 변수처럼 사용 가능함) <br>
  -> 일급 객체(First-class Object) : 변수나 데이터 구조 안에 담을 수 있으며 매개변수로 전달, 반환값으로 사용할 수 있는 객체를 의미

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

### 자바에서 제공하는 함수형 인터페이스

#### Runnable
* 매개변수 X 반환값 X
* 자바에서 쓰레드를 구현할 때 사용하는 방법 중 하나

```java
@FunctionalInterface
public interface Runnable {
	public abstract void run();
}

class MyThread implements Runnable {    
    @Override
    public void run() {
    	for(int i = 1; i <= 10; i++) {
            System.out.print(i);
            Thread.sleep(1000);
        }
    }
}

// 람다식 사용
Thread thread = new Thread(() -> {
        for(int i = 1; i <= 10; i++) {
                System.out.print(i);
                Thread.sleep(1000);
        }
});
```


#### Supplier<T>
* () -> T(매개변수 X 반환값 O)
* T get()을 추상메서드로 갖는다. get()으로 반환값을 공급(전달) 해준다.

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}

//사용 예
Supplier<Integer> supplier = () -> 1;
System.out.println(supplier.get());
// 1  출력
```

#### Consumer<T>
* T -> void(매개변수 O, 반환값 X)
* void accept(T t)를 추상메서드로 갖는다. 객체 T를 매개변수로 받아서 소비한다.

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
    
    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }

}


Consumer<String> consumer1 = (str) -> System.out.println("I am " + str);
Consumer<String> consumer2 = (str) -> System.out.println("Hi!! " + str + "!");
consumer1.andThen(consumer2).accept("gangEun");
// 출력
  I am gangEun
  Hi!! I am gangEun!
```

#### Function<T>
* T -> R (매개변수 T, 반환값 R)
* R apply(T t)를 추상메서드로 갖는다.
* andThen, compose, indentity가 추가로 제공됨.

```java
@FunctionalInterface
public interface Function<T, R> {

    R apply(T t);

    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}

// 사용 예
Function<Integer, Integer> function = (value) -> value * 2;
System.out.println(function.apply(3));  //출력 6

Function<String, String> function = (value) -> value + "1";
System.out.println(function.apply("string")); // string1;
```

#### Predicate<T>
* T -> boolean (매개변수 T, 반환값 boolean)
* boolean test(T t)를 추상메서드로 갖는다.

```java
@FunctionalInterface
public interface Predicate<T> {

    boolean test(T t);

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> negate() {
        return (t) -> !test(t);
    }

    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }

    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
    
}

Predicate<Integer> predicate = num -> num > 0;
predicate.test(10); // true

//and(), or() 사용 예
Predicate<Integer> isBiggerThanFive = num -> num > 5;
Predicate<Integer> isLowerThanSix = num -> num < 6;
System.out.println(isBiggerThanFive.and(isLowerThanSix).test(10));  // false
System.out.println(isBiggerThanFive.or(isLowerThanSix).test(10));   // true

// isEqual() 사용 예
Predicate<String> predicate = (str) -> str.equals("Hello World");
predicate.test("Hello World"); // true
predicate.test("Helloooo"); // false

```


### 메서드 참조
* 매개변수의 정보와 반환 타입을 파악하여 람다식에서 불필요한 매개변수를 제거하는 것을 의미한다.

```java

String[] names = new String[] {"lee", "choi", "park"};
List<String> list = Arrays.asList(names);

// 일반 방법
for(String name : list)
	System.out.println(name);

// forEach 적용
list.forEach(name -> System.out.println(name));

// forEach에 메소드참조 적용
list.forEach(System.out::println);


// 다른 예시
Function<String, Integer> function = (str) -> str.length();

//메소드참조 적용
Function<String, Integer> function = String::length;




```
