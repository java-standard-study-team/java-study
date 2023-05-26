# Set 인터페이스

## set이란?

* 자료구조 중 하나로, 중복되지 않는 데이터들의 집합

#### 특성

1. 데이터 순서를 보장하지 않는다(삽입한 순서대로 저장되지 않음)
2. 수정이 가능하다.
3. 중복해서 삽입이 불가능하다. (동일한 값이 삽입되면 하나의 값만 저장된다)
4. 주로 중복데이터를 삭제할 때 내부적으로 사용된다.

#### Set 구현 클래스

1. HashSet : 데이터 중복 x, 순서보장 x
2. TreeSet : Set의 특성 + 오름차순으로 데이터를 정렬한다.
3. LinkedHashSet : Set 특성 + 입력한 순서대로 데이터를 저장(순서 보장)

##### 해시(Hash)란?

- 임의의 길이를 갖는 데이터를 고정된 길이의 데이터로 변환하는 것
- 위와 같은 기능을 하는 함수를 해시 함수(Hash Function)이라고 한다.

> 만약 abcd 라는 문자열이 있으면 이를 특정 길이의 데이터로 변환시키는 것이다. <br>
> ex) abcd -> 86aff3ca
>

 ```java
String a="abcd";
        String b="abcd";
        System.out.prinln(a.hashCode()); // 2987074
        System.out.prinln(b.hashCode()); // 2987074

/** 동일한 값은 동일한 hashCode를 갖는다는 걸 알 수 있다. */
```

> > > 왜 Hash로 저장을 할까 ? ?

* 만약 hash로 저장을 하지 않고 해당 객체값으로 저장을 한다면 Set은 내부적으로  <br> 매번 새로운 값이 들어올때마다
  전체 순회를 하며, 같은 값이 있는지 체크를 해야한다. <br>
  이는 곧 엄청난 비효율과 높은 시간복잡도로 이어진다. <br>
  그래서 내부적으로 hash값으로 변환 후 값을 저장하여 중복체크를 빠르게 할 수 있는 것이다.

### HashSet

* set의 자료구조적인 특징 (데이터 중복x ,순서 보장 x) 을 갖고있는 Set의 구현클래스

```java
Set<String> hashSet=new HashSet<>();
        hashSet.add("AA");
        hashSet.add("AA");
        hashSet.add("AA");
        `
        Iterator<String> iterator=hashSet.iterator();

        while(iterator.hasNext()){
        System.out.println(iterOfHashSet.next());
        } // "AA" 하나만 출력된다 (중복 제거)
```

```java
Set<String> hashSet=new HashSet<>();
        hashSet.add("A");
        hashSet.add("C");
        hashSet.add("CC");
        hashSet.add("DD");
        hashSet.add("EE");

        Iterator<String> iterator=hashSet.iterator();

        while(iterator.hasNext()){
        System.out.println(iterOfHashSet.next());
        } // CC -> DD -> EE -> A -> C 순서로 출력 (순서 보장x)
```

### TreeSet

* set의 특징을 그대로 갖고 있지만, 이진 탐색 트리(BinarySearchTree) 구조로 이루어져 있다.
* 데이터가 많아질수록 데이터 추가/삭제에 시간이 더 걸림
* 내부적으로 정렬하여 저장한다.
![image](https://github.com/java-standard-study-team/java-study/assets/73535356/9c422498-4b44-4c3b-ae87-02177acec74a)

> 레드-블랙 트리(Red-Black Tree) <br>
> 부모보다 작은값은 왼쪽으로 큰 값은 오른쪽으로 배치하여 일반적인 이진 탐색트리보다 빠르다.

```java
Set<Integer> treeSet=new TreeSet<>();
        Set<Integer> linkedHashSet=new LinkedHashSet<>();

        treeSet.add(7);
        treeSet.add(4);
        treeSet.add(9);
        treeSet.add(1);
        treeSet.add(5);

        Iterator<Integer> iterator=treeSet.iterator();

        while(iterator.hasNext()){
        System.out.println(iterator.next());
        } // 1 -> 4 -> 5 -> 7 -> 9;
```

![image](https://github.com/java-standard-study-team/java-study/assets/73535356/63bace05-92bb-49ad-878e-c05a7eaee40d)

* treeSet의 유용한 메서드들

```java
TreeSet<Integer> set=new TreeSet<Integer>(Arrays.asList(4,2,3));
        System.out.println(set); //전체출력 [2,3,4]
        System.out.println(set.first());//최소값 출력
        System.out.println(set.last());//최대값 출력
        System.out.println(set.higher(3));//입력값보다 큰 데이터중 최소값 출력 없으면 null
        System.out.println(set.lower(3));//입력값보다 작은 데이터중 최대값 출력 없으면 null

        Iterator iter=set.iterator();    // Iterator 사용
        while(iter.hasNext()){//값이 있으면 true 없으면 false
        System.out.println(iter.next());
        }
```

### LinkedHashSet

* set의 특징을 그대로 갖고 있고, 삽입된 순서대로 출력시 보장을 해줌(Queue 와 같은 First In First Out)
* LinkedList와 HashSet을 결합한 형태의 Set의 구현 클래스이다.
* LinkedList의 특성을 이용하여 인덱스 관리를 한다.

```java
Set<String> linkedHashSet=new LinkedHashSet<>();

        linkedHashSet.add("A");
        linkedHashSet.add("C");
        linkedHashSet.add("CC");
        linkedHashSet.add("DD");
        linkedHashSet.add("EE");

        Iterator<String> iterator=linkedHashSet.iterator();

        while(iterator.hasNext()){
        System.out.println(iterator.next());
        } // A -> C -> CC -> DD -> EE 순서로 출력 

```

## Map이란?

* 키와 값을 하나의 쌍으로 묶어 저장하는 자료구조이다.
* 값(Value)은 중복될 수 있지만, Key는 중복될 수 없다.
* Map은 순서를 보장하지 않고, key를 통하여 Value를 얻어낸다.

### HashMap 이란?

* Map 자료구조의 특성을 갖고 내부적으로 해시함수를 사용하는 구현클래스
* 검색 속도가 빠름

```java
Map<Integer, String> map=new HashMap<>();

        map.put(1,"첫번째");

        System.out.println(map.get(1)); // '첫번째' 출력

```

#### ?? 만약 동일한 Key값을 넣는다면 ??

```java
Map<Integer, String> map=new HashMap<>();

        map.put(1,"첫번째");
        map.put(1,"두번째");

        System.out.println(map.get(1)); // '두번째' 출력 
// value값을 신규 값으로 갱신하는 것을 알 수 있다.
```

#### HashMap 메서드

```java
Map<Integer, String> map=new HashMap<>();
        Map<Integer, String> map2=new HashMap<>();

/** 데이터 추가 */
        1.map.put(1,"데이터 추가"); // key value 저장
        2.map2.putAll(map); // map2에 map의 모든 데이터 저장
        3.map2.putIfAbsent(1,"데이터 변경"); // map 해당 key가 없으면 값 저장

        System.out.println(map2.get(1)); // '데이터 추가' 출력


/** 데이터 삭제 */
        1.map.clear(); // 모든 데이터 삭제
        2.map.remove(1); // 키와 일치하는 데이터 삭제며 value값 반환
        3.map.remove(1,"데이터 추가") // 키와 값이 동시에 일치하는 데이터 삭제


/** 데이터 수정 */
        1.map.replace(1,"해당 키의 value 변경");
        2.map.replace(1,"일치하는","키와 값이 일치하는 value 변경");

/** 데이터 반환*/
        1.map.get(1); // 해당 키와 일치하는 value 값 반환
        2.map.getOrDefault(1,"반환") // 해당 키와 일치하는게 없으면 뒤에 "반환" 값 반환

```

### HashTable vs HashMap

|         | HashTable            | HashMap               |
|---------|----------------------|-----------------------|
| 동기화     | 지원(멀티 스레드에서 사용하기 좋음) | 미지원(단일스레드 에서 사용하기 좋음) |
| 속도      | HashMap에 비해 느림       | 빠름(시간복잡도 -> O(1))     |
| null 여부 | null 불가능             | key,value에 null 가능    |


* java5에서 HashTable의 성능을 향상시켜 나온것이 HashMap이라 HashTable은 거의 사용하지 않는다.

