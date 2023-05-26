## ⭐️ 컬렉션 프레임워크 (Collections Frmaework)

**정의 : 데이트 군을 저장하는 클래스들을 표준화한 설계**

\- Collection : 다수의 데이터(데이터 그룹)

\- Framework : 표준화된 프로그래밍 방식

**장점**

\- 다수의 데이터를 다루는데 필요한 다양하고 풍부한 클래스들을 제공

\- 인터페이스와 다형성을 이용한 객체지향적 설계를 통해 표준화 되어 있어 사용법을 익히기 편하고 재사용성이 높은 코드 작성 가능

[##_Image|kage@mOnUj/btshA45zN39/p2IzT5qwjyPbczGQuqFNV0/img.png|CDM|1.3|{"originWidth":199,"originHeight":101,"style":"alignCenter","width":300,"height":152,"caption":"Collection Framework 핵심 인터페이스 상속계층도"}_##][##_Image|kage@dZ1Q7J/btshCcaXfCz/gxnz4l3KOn3dcbGKbe8U90/img.png|CDM|1.3|{"originWidth":925,"originHeight":430,"style":"alignCenter"}_##]

Collection Framework의 모든 Collection Class들은 List, Set, Map 중 하나를 구현하고 있다.

### 💡 Collection 인터페이스

List와 Set의 조상인 Collection Interface에는 아래와 같은 메서드들이 정의되어 있다.

[##_Image|kage@bb0pzQ/btshAAqayK4/yF5avxqFrhqcXB955QSuB1/img.png|CDM|1.3|{"originWidth":921,"originHeight":638,"style":"alignCenter"}_##]

### 💡 List 인터페이스

List 인터페이스는 중복을 허용하면서 저장순서가 유지되는 컬렉션을 구현하는데 사용된다.

[##_Image|kage@qF1Hg/btshArtpnvO/5vDls3lnCOOprRXn7x2u11/img.png|CDM|1.3|{"originWidth":586,"originHeight":267,"style":"alignCenter","caption":"List Interface의 상속계층도"}_##]

**List Interface에 정의된 메서드**

[##_Image|kage@6TOY3/btshBjuExdd/haAVzDc7xgpWZt2enfGAak/img.png|CDM|1.3|{"originWidth":977,"originHeight":566,"style":"alignCenter"}_##]

### 💡 Set 인터페이스

중복을 허용하지 않고, 저장순서가 유지되지 않는 컬렉션 클래스를 구현하는데 사용된다.

[##_Image|kage@cYo7Xu/btshzYyfAim/wmJIi0zc5a7rkUXppG3skk/img.png|CDM|1.3|{"originWidth":357,"originHeight":270,"style":"alignCenter","caption":"Set Interface의 상속계층도"}_##]

**Set Interface에 정의된 메서드**

[##_Image|kage@bvQ6Jo/btshBkAj4sW/PiRLi4Ty8KK934T5teXyW1/img.png|CDM|1.3|{"originWidth":902,"originHeight":492,"style":"alignCenter"}_##][##_Image|kage@b1s7iC/btshAwgWDpJ/18oUNFU5GmZ9gL67s6Zs81/img.png|CDM|1.3|{"originWidth":1033,"originHeight":199,"style":"alignCenter"}_##]

### 💡 Map 인터페이스

Key(키)와 Value(값)을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용된다.

\- Key는 중복될 수 없지만, Value는 중복을 허용한다.

\- 기존에 저장된 데이터와 중복된 Key 값을 저장하면 기존의 값이 사라지고 마지막에 저장된 값이 남게 된다.

ex) HashTable, HashMap, LinkedHashMap, SortedMap, TreeMap 등 존재함

**Map Interface에 정의된 메서드**

[##_Image|kage@yePvJ/btshzCII3SD/ytgd3lfaFhEduKyzT5sO6K/img.png|CDM|1.3|{"originWidth":1157,"originHeight":568,"style":"alignCenter"}_##]

### 💡 ArrayList

Collection Framework에서 가장 많이 사용되는 Collection Class

**특징**

\- ArrayList는 List 인터페이스를 구현하기 때문에 데이터의 저장순서가 유지되고 중복을 허용한다.

\- Object 배열을 이용하여 데이터를 순차적으로 저장

**장점**

\- 배열의 구조가 간단하고 데이터를 읽는 데 걸리는 시간이 짧다.(access time, 접근시간이 짧다)

**단점**

\- 크기를 변경할 수 없다.

    -크기를 변경해야 하는 경우, 새로운 배열을 생성한 후 데이터를 복사해야 함.

    - 크기 변경을 피하기 위해 충분히 큰 배열을 생성하면 메모리가 낭비된다.

\- 비순차적인 데이터의 추가, 삭제에 시간이 많이 걸린다.

    - 데이터를 추가하거나 삭제하기 위해 다른 데이터를 옮겨야 함

        - 삭제할 데이터의 아래에 있는 데이터를 한 칸씩 위로 복사해서 삭제할 데이터를 덮어쓴다.

    - 하지만 순차적인 데이터의 변경(맨 앞 또는 맨 뒤에 추가/삭제)은 빠르다.

```
import java.util.*;

class ArrayListEx1{
	public static void main(String[] args) {
		ArrayList list1 = new ArrayList(10);
		list1.add(new Integer(5));
		list1.add(new Integer(4));
		list1.add(new Integer(2));
		list1.add(new Integer(0));
		list1.add(new Integer(1));
		list1.add(new Integer(3));

		ArrayList list2 = new ArrayList(list1.subList(1,4)); 
		print(list1, list2);

		Collections.sort(list1);	// list1과 list2를 정렬한다.
		Collections.sort(list2);	// Collections.sort(List l)
		print(list1, list2);

		System.out.println("list1.containsAll(list2):" + list1.containsAll(list2));

		list2.add("B");
		list2.add("C");
		list2.add(3, "A");
		print(list1, list2);

		list2.set(3, "AA");
		print(list1, list2);
		
		// list1에서 list2와 겹치는 부분만 남기고 나머지는 삭제한다.
		System.out.println("list1.retainAll(list2):" + list1.retainAll(list2));	
		print(list1, list2);
		
		//  list2에서 list1에 포함된 객체들을 삭제한다.
		for(int i= list2.size()-1; i >= 0; i--) {
			if(list1.contains(list2.get(i)))
				list2.remove(i);
		}
		print(list1, list2);
	} // main의 끝

	static void print(ArrayList list1, ArrayList list2) {
		System.out.println("list1:"+list1);
		System.out.println("list2:"+list2);
		System.out.println();		
	}
} // class
```

[##_Image|kage@WMVOa/btshyNqMWQO/MMkIq26VVANUzdWiDNY39K/img.png|CDM|1.3|{"originWidth":302,"originHeight":521,"style":"alignCenter"}_##]
