# 1.11 TreeMap

------------------------------------------------------------------

###### 이진 검색 트리(Binary Search Tree):

* 노드들이 계층적으로 구성된 트리 형태의 자료구조
* 각 노드는 최대 두 개의 자식 노드를 가지며, 왼쪽 자식 노드는 현재 노드보다 작은 값을, 오른쪽 자식 노드는 큰 값을 가진다
* 이진 검색 트리의 중요한 특징은 데이터가 정렬된 순서로저장되어 있어 검색, 삽입, 삭제 등의 연산이 효율적으로 이루어진다
###### Map:
* 키(key)와 값(value)의 쌍으로 이루어진 데이터를 저장하는 자료구조
* 키는 고유한 값으로 중복되지 않으며, 값은 키와 연결되어 저장된다
* Map은 키를 통해 값을 검색하고, 키를 기준으로 정렬된 순서를 유지하지 않습니다
* Map의 주요 동작은 키를 기반으로 값을 저장('put(key, value)'), 키를 기반으로 값ㅇ르 조회('get(key)'), 키-값 쌍을 삭제('remove(key)')하는 것이다

##### 이진 검색 트리와 Map의 관계:
* Map은 키-값 쌍을 저장하는 자료구조로, 이진 검색 트리는 Map의 구현 방식 중 하나다
* 이진 검색 트리를 이용한 Map 구현은 키를 기준으로 정렬된 순서로 데이터를 저장하고 검색할 수 있다
* TreeMap은 자바에서 이진 검색 트리를 기반으로한 Map의 구현체 중 하나로, 키를 기준으로 정렬된 상태로 데이터를 저장한다

##### 이전 검색 트리는 데이터를 효율적으로 정렬하고 검색할 수 있는 트리 형태의 자료구조. Map은 키-값 쌍을 저장하는 자료구조로, 이진 검색 트리를 활용한 TreeMap은 키를 기준으로 정렬된 상태로 데이터를 저장하고 검색할 수 있는 Map의 구현체이다

------------------------------------------------------------------------

### TreeMap
* 정렬된 키-값 쌍을 저장하는 자료구조
* 이진 검색 트리의 한 종류인 레드-블랙 트리(Red-Black Tree)로 구현되어 있다 -> 이를 통해 키의 정렬된 순서를 유지하면서 데이터를 저장하고 검색할 수 있다
* 특징
1. 정렬된 순서: TreeMap은 내부적으로 키를 기준으로 정렬된 상태로 데이터를 저장한다
2. 자동정렬: 데이터를 추가하거나 제거할 때 마다 TreeMap은 자동으로 정렬을 유지한다: 새로운 데이터가 추가되면 트리 내에서 올바른 위치에 삽입, 데이터가 삭제되면 트리가 재구성된다
3. 검색과 범위 조회: TreeMap은 이진 검색 트리의 특성을 활용하여 데이터를 검색하고, 특정 범위의 데이터를 조회하는 기능을 제공한다 -> 키를 기준으로 데이터를 빠르게 찾을 수 있다

데이터 정렬이 필요한 상황이나 특정 범위의 데이터를 조회해야 하는 경우에 유용하다 ex) 사전, 주소록, 시간표 등

![img.png](img.png)

* 레드-블랙 트리는 내부적으로 노드들을 레드 또는 블랙으로 표시하여 트리의 균형을 유지하는 이진 검색 트리입니다.
* 이를 통해 키를 기준으로 정렬된 상태를 유지하면서 효율적인 데이터 검색을 가능하게 합니다.
레드-블랙 트리는 이진 검색 트리의 특별한 형태로, 균형을 유지하며 데이터를 저장하는 자료구조입니다. 이진 검색 이를 통해 효율적인 데이터 검색과 정렬된 순서로 데이터를 유지할 수 있습니다.


* 검색성능에 관한한 대부분의 경우에서 HashMap이 TreeMap보다 더 뛰어나므로 HashMap을 사용하는 것이 좋다. 다만, 범위검색이나 정렬이 필요한 경우에는 TreeMap을 사용
```java
import java.util.TreeMap;

public class Main {
    public static void main(String[] args) {
        // TreeMap 객체 생성
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        // put() 메서드를 사용하여 키-값 쌍 추가
        treeMap.put(5, "사과");
        treeMap.put(3, "바나나");
        treeMap.put(7, "오렌지");

        // containsKey() 메서드를 사용하여 특정 키의 존재 여부 확인
        boolean containsKey = treeMap.containsKey(5);
        System.out.println("Contains Key 5: " + containsKey);

        // containsValue() 메서드를 사용하여 특정 값의 존재 여부 확인
        boolean containsValue = treeMap.containsValue("Banana");
        System.out.println("Contains Value 'Banana': " + containsValue);

        // keySet() 메서드를 사용하여 TreeMap의 모든 키 조회
        System.out.println("Keys: " + treeMap.keySet());

        // values() 메서드를 사용하여 TreeMap의 모든 값 조회
        System.out.println("Values: " + treeMap.values());
    }
}

```
* 출력값
```
Contains Key 5: true
Contains Value 'Banana': false
Keys: [3, 5, 7]
Values: [바나나, 사과, 오렌지]
```

--------------------------------------
# 1.12 Properties
* (String, String)의 형태로 저장하는 보다 단순화된 컬렉션클래스
* Java에서 키-값 쌍으로 구성된 설정 파일을 다루기 위한 클래스
```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

public class Main {
    public static void main(String[] args) {
        // Properties 객체 생성
        Properties properties = new Properties();

        try {
            FileInputStream input = new FileInputStream("config.properties");
            properties.load(input);

            // 특정 키에 해당하는 값 조회
            String username = properties.getProperty("username");
            String password = properties.getProperty("password");

            System.out.println("Username: " + username);
            System.out.println("Password: " + password);

            // 새로운 설정 값 추가 또는 수정
            properties.setProperty("timeout", "5000");
            properties.setProperty("language", "en");

            // 설정 파일 저장
            FileOutputStream output = new FileOutputStream("config.properties");
            properties.store(output, "Sample Configuration");

            System.out.println("Properties saved successfully.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```
# 1.13 Collections

* Collections는 컬렉션과 관련된 메서드를 제공한다

* Java의 Collections는 자료구조를 다루고 관리하기 위한 유틸리티 클래스입니다. 
* 정적 메서드를 제공하여 배열 또는 컬렉션 객체를 조작하고 정렬하며, 검색, 변경, 추출 등 다양한 작업을 수행할 수 있습니다.

### 컬렉션의 동기화(Synchronization):
* Java의 Collections 클래스는 동기화된 컬렉션을 생성하기 위한 메서드인 synchronizedXXX()를 제공합니다. 
* 여러 스레드가 동시에 컬렉션을 수정하는 것을 방지하여 스레드 안전성을 보장할 수 있습니다.
```java
List<String> synchronizedList = Collections.synchronizedList(new ArrayList<>());
Set<Integer> synchronizedSet = Collections.synchronizedSet(new HashSet<>());
```
위의 코드는 synchronizedList와 synchronizedSet라는 동기화된 리스트와 세트를 생성하는 예시입니다.

### 싱글톤 컬렉션 만들기:
* Java의 Collections 클래스는 싱글톤(Singleton)으로 구성된 읽기 전용 컬렉션을 생성하기 위한 메서드인 singletonXXX()를 제공합니다.
* 싱글톤 컬렉션은 단일 요소만을 포함하고 수정이 불가능한 읽기 전용 컬렉션입니다.
```java
List<String> singletonList = Collections.singletonList("Hello");
Set<Integer> singletonSet = Collections.singleton(42);
```
싱글톤 리스트와 싱글톤 세트를 생성하는 예시입니다.

### 한 종류의 객체만 저장하는 컬렉션 만들기:
* Java의 Collections 클래스는 특정 타입의 객체만을 저장하도록 제한하는 컬렉션을 생성하기 위한 메서드인 checkedXXX()를 제공합니다.
* 컬렉션에 다른 타입의 객체를 추가하는 것을 방지할 수 있습니다.
```java
List<String> checkedList = Collections.checkedList(new ArrayList<>(), String.class);
Set<Integer> checkedSet = Collections.checkedSet(new HashSet<>(), Integer.class);
```
위의 코드는 checkedList와 checkedSet라는 문자열과 정수만을 저장할 수 있는 체크된 리스트와 체크된 세트를 생성하는 예시입니다.

# 1.14 컬렉션 클래스 정리 & 요약
* Collection은 Java에서 데이터를 저장하고 관리하는 데 사용되는 자료구조입니다.

| 종류      | 클래스 이름         | 특징                                       |
|-----------|---------------------|--------------------------------------------|
| List      | ArrayList           | 가변 크기 배열                             |
|           | LinkedList          | 이중 연결 리스트                           |
|           | Vector              | 동기화된 리스트                             |
| Set       | HashSet             | 해시 테이블을 기반으로 한 집합              |
|           | LinkedHashSet       | 순서를 유지하는 해시 테이블 기반 집합        |
|           | TreeSet             | 레드-블랙 트리 기반으로 한 정렬된 집합       |
| Queue     | LinkedList          | 이중 연결 리스트를 기반으로 한 큐            |
|           | PriorityQueue      | 힙(Heap) 기반으로 한 우선순위 큐            |
| Map       | HashMap             | 해시 테이블을 기반으로 한 맵                |
|           | LinkedHashMap       | 순서를 유지하는 해시 테이블 기반 맵          |
|           | TreeMap             | 레드-블랙 트리를 기반으로 한 정렬된 맵       |
| 유틸리티 | Collections         | 컬렉션을 다루기 위한 유틸리티 클래스       |
|           | Arrays              | 배열을 다루기 위한 유틸리티 클래스         |
