#2.4 Optional<T>와 Optionallnt
* Option<T>은 지네릭 클래스로 'T타입의 객체'를 감싸는 래퍼 클래스이다 -> Optional타입의 객체에는 모든 타입의 참조변수를 담을 수 있다
```java
    public final class Optional<T> {
        private final T value; // T타입의 참조변수
    ...
}
```
* 최종 연산의 결과를 그냥 반환하는 게 아니라 Optional객체에 담아서 반환 -> 반환된 결과가 null인 지 매번 if문으로 체크하는 대신 Optional에 정의된 메서드를  통해서 간단히 ㅓ리 가능
* 널 체크를 위한 if문 없이도 NullPointerException이 발생하지 않는 보다 간결하고 안전한 코드를 작성하는 것이 간으해졌다

### Optional객체 생성하기
* Optional객체를 생성할 때는 of() 또는 ofNullable()을 사용한다
* of(value): 주어진 값으로 Optional 객체를 생성합니다. 값이 null이 아닌 경우에만 사용해야 합니다. 값이 null인 경우 NullPointerException이 발생합니다.
* ofNullable(value): 주어진 값으로 Optional 객체를 생성합니다. 값이 null인 경우에도 사용할 수 있습니다.
```java
Optional<String> optional1 = Optional.of("Hello");
// "Hello"라는 값을 갖는 Optional 객체를 생성
```
```java
Optional<String> optional2 = Optional.ofNullable(null);
// null 값을 갖는 Optional 객체를 생성
```

### Optional객체의 값 가져오기

* Optional 객체의 값은 get() 메서드를 사용하여 가져올 수 있음
```java
Optional<String> optional = Optional.of("Hello");
String value = optional.get();
System.out.println(value);
```
-> optional.get()은 Optional 객체의 값을 가져온다 -> "Hello"라는 문자열이 출력됩니다.

* orElse() 메서드를 사용하여 Optional 객체의 값이 없는 경우 대체할 값을 지정할 수 있음
* 값이 존재하는 경우에는 Optional 객체의 값을 반환하며, 값이 없는 경우에는 대체할 값을 반환합니다.

```java
Optional<String> optional = Optional.empty();
String value = optional.orElse("Default Value");
System.out.println(value);
```
-> optional.orElse("Default Value")는 Optional 객체의 값이 없는 경우("empty")에는 "Default Value"를 반환
->"Default Value"를 출력합니다.

# 2.5 스트림의 최종 연산
##### forEach(Consumer)

* forEach(Consumer)는 스트림의 각 요소에 대해 주어진 동작을 수행
-> 각 요소에 대해 순차적으로 적용, 요소의 처리 순서는 보장 X
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.stream()
.forEach(System.out::println);
```

##### anyMatch(Predicate)

* anyMatch(Predicate)는 주어진 조건을 만족하는 요소가 스트림에 하나라도 존재하는지 확인
* 
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
boolean hasEvenNumber = numbers.stream()
.anyMatch(num -> num % 2 == 0);
System.out.println("Has even number: " + hasEvenNumber);
```
-> numbers 리스트에 짝수가 있는지 확인
-> anyMatch(num -> num % 2 == 0)를 통해 스트림에서 짝수인 요소가 하나라도 존재하는지 확인

##### 통계 (Statistics)

스트림의 통계를 계산하는 메서드는 다양한 최종 연산이 있습니다. 예를 들어, count(), sum(), average(), min(), max() 등이 있습니다. 이러한 메서드들은 IntStream, LongStream, DoubleStream과 같은 특정 타입의 스트림에서 사용할 수 있습니다.
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
IntSummaryStatistics statistics = numbers.stream()
.mapToInt(Integer::intValue)
.summaryStatistics();
System.out.println("Count: " + statistics.getCount());
System.out.println("Sum: " + statistics.getSum());
System.out.println("Average: " + statistics.getAverage());
System.out.println("Min: " + statistics.getMin());
System.out.println("Max: " + statistics.getMax());
``` 
-> mapToInt(Integer::intValue)를 통해 numbers의 요소를 IntStream으로 변환 -> summaryStatistics()를 호출하여 통계를 계산

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> sum = numbers.stream()
.reduce((a, b) -> a + b);
System.out.println("Sum: " + sum.orElse(0));
```
-> numbers 리스트의 요소들을 더하여 합계를 구하기!
-> reduce((a, b) -> a + b)를 통해 각 요소들을 더하고 최종 합계를 반환
-> Optional로 감싸져 있음 -> orElse(0)을 통해 값이 없는 경우에는 기본값 0을 출력합니다.

# 2.6 collect()
* collect()는 스트림의 요소를 수집하여 새로운 컬렉션이나 결과 객체로 변환하는 최종 연산
* collect() 메서드는 Collector를 인수로 받아 요소들을 수집하고 원하는 형태로 결과를 생성

### 스트림을 컬렉션과 배열로 변환

###### 스트림을 리스트(List)로 변환하기
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> list = numbers.stream()
.collect(Collectors.toList());
```
->numbers 리스트를 스트림으로 변환한 후 collect(Collectors.toList())를 사용하여 해당 스트림을 리스트로 변환합니다.

###### 스트림을 세트(Set)로 변환하기
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Set<Integer> set = numbers.stream()
.collect(Collectors.toSet());
```
-> Numbers 리스트를 스트림으로 변환한 후 collect(Collectors.toSet())를 사용하여 해당 스트림을 세트로 변환합니다.

###### 스트림을 배열(Array)로 변환하기
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Integer[] array = numbers.stream()
.toArray(Integer[]::new);
```
-> numbers 리스트를 스트림으로 변환한 후 toArray(Integer[]::new)를 사용하여 해당 스트림을 배열로 변환
-> 배열의 타입은 Integer[]로 지정되었습니다.

##### 통계
counting()
-> 스트림의 요소의 개수를 계산하는 통계 메서드입니다.
```java
long count = stream.collect(Collectors.counting());
```
* summingXxx()
-> 스트림의 요소들을 합계로 계산하는 통계 메서드
        -> Xxx는 요소의 타입을 나타냅니다.
```java
int sum = stream.collect(Collectors.summingInt(element -> element));
```
* averagingXxx()
-> 스트림의 요소들의 평균을 계산하는 통계 메서드
        ->Xxx는 요소의 타입을 나타냅니다.
```java
double average = stream.collect(Collectors.averagingInt(element -> element));
```
* minBy()와 maxBy()
-> 스트림의 요소들 중에서 최솟값과 최댓값을 계산하는 통계 메서드
        ->Comparator를 인자로 받는다
```java
Optional<Integer> min = stream.collect(Collectors.minBy(Comparator.naturalOrder()));
Optional<Integer> max = stream.collect(Collectors.maxBy(Comparator.naturalOrder()));
```
### 리듀싱
* reducing() 메서드는 스트림의 요소를 리듀싱하여 단일 결과를 생성하는 메서드입니다. 다음은 reducing() 메서드의 짧은 코드와 설명입니다.

```java
Optional<T> result = stream.collect(Collectors.reducing(binaryOperator));
```
-> binaryOperator는 스트림의 요소를 결합하여 단일 결과를 생성하는 바이너리 연산자
-> 이 메서드는 Optional로 감싸진 결과를 반환합니다.

* 정수 스트림에서 모든 요소의 합계를 계산하는 리듀싱 연산
```java
Optional<Integer> sum = stream.collect(Collectors.reducing((a, b) -> a + b));
```
-> 바이너리 연산자로 두 개의 정수를 더하여 합계를 계산
-> 최종적으로 Optional<Integer>로 합계가 반환

##### 문자열 결합
* 스트림의 요소를 하나의 문자열로 연결하는 메서드
```java
String result = stream.collect(Collectors.joining());
```
-> joining() 메서드는 스트림의 요소를 하나의 문자열로 연결
-> 구분자를 인자로 전달하지 않았기 때문에 요소들이 구분자 없이 연속적으로 결합

* 스트림에서 요소들을 공백으로 구분하여 연결하는 코드
```java
String result = stream.collect(Collectors.joining(" "));
```
-> 공백 문자열을 구분자로 사용하여 요소들을 연결
-> 결과적으로 스트림의 요소들이 공백으로 구분된 하나의 문자열로 반환

##### 그룹화와 분할
-> 그룹화(Grouping)와 분할(Partitioning)은 스트림의 요소를 기준에 따라 나누는 연산입니다.

* 그룹화(Grouping): Collectors.groupingBy(classifier) 메서드를 사용하여 요소를 그룹화합니다. 그룹화의 결과는 Map으로 반환됩니다.
* 분할(Partitioning): Collectors.partitioningBy(predicate) 메서드를 사용하여 요소를 참 또는 거짓의 조건에 따라 분할합니다. 분할의 결과는 Map으로 반환됩니다.
```java
Map<K, List<T>> groups = stream.collect(Collectors.groupingBy(classifier));
Map<Boolean, List<T>> partitions = stream.collect(Collectors.partitioningBy(predicate));
```
-> 그룹화에서 classifier는 그룹화를 위한 함수 또는 메서드 레퍼런스
-> 분할에서 predicate는 분할을 위한 조건을 나타내는 함수 또는 메서드 레퍼런스입니다.

* 사람 객체를 나이에 따라 그룹화하고, 숫자 스트림을 짝수와 홀수로 분할하는 코드
```java
Map<Integer, List<Person>> groups = stream.collect(Collectors.groupingBy(Person::getAge));
Map<Boolean, List<Integer>> partitions = stream.collect(Collectors.partitioningBy(number -> number % 2 == 0));
```
-> Person::getAge를 사용하여 사람 객체를 나이에 따라 그룹화
-> number -> number % 2 == 0를 사용하여 숫자를 짝수와 홀수로 분할
-> 그룹화의 결과는 Map<Integer, List<Person>>에 저장되며, 분할의 결과는 Map<Boolean, List<Integer>>에 저장