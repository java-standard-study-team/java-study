# Chapter9

## 래퍼(wrapper) 클래스

### 래퍼클래스란?

> primitive type (기본 자료형)을 object로 사용할 수 있게 해주는 클래스

### 래퍼클래스 종류

| Primitive(UnBoxing) | Wrapper Class(Boxing) |
|---------------------|-----------------------|
| byte                | Byte                  |
| short               | Short                 |
| int                 | Integer               |
| long                | Long                  |
| float               | Float                 |
| double              | Double                |
| char                | Character             |
| boolean             | Boolean               |

### 박싱(Boxing)과 언박싱(UnBoxing)
- **박싱** : 기본 타입의 값을 래퍼 클래스로 만드는 과정
- **언박싱** : 래퍼 클래스에서 기본 타입의 값을 받아오는 과정
```java
Integer num = new Integer(2); // 박싱
int n = num.intValue(); // 언박싱
```

### 자동 박싱(AutoBoxing)과 자동 언박싱(AutoUnBoxing)
 - 기본자료형과 래퍼 클래스간의 변환을 자동으로 해주는 동작
 - 명시적 박싱과 언박싱을 생략 가능
 - JDK 1.5 이상부터 지원되며 JVM에 의해 처리됨

```java
Integer wrapperNum = new Integer(2);
int primitiveNum = wrapperNum;  // 오토 언박싱
```

```java
int primitiveNum = 2;
Integer wrapperNum = primitiveNum; // 오토 박싱
```

```java
int primitiveNum = 2;
Integer wrapperNum = primitiveNum;

System.out.println(primitiveNum + wrapperNum); // 오토 언박싱되어 계산됨
```

```java
Integer num1 = new Integer(1); 
Integer num2 = new Integer(1);

System.out.println(num1 == num2);  // false
System.out.println(num1.equals(num2)); // frue
```
- 명시적 생성으로 인한 다른 주소값을 가리킨다.

```java
Integer num1 = 1; 
Integer num2 = 1;

System.out.println(num1 == num2);  // true
System.out.println(num1.equals(num2)); // frue
```
- 내부적으로 Integer.valueOf(1) 과 같이 오토 박싱하여 객체가 생성된다.
- 위 메서드는 내부적으로 Integer Cache값을 사용하기 때문에 같은 캐시의 주소값을 참조하게 되고 결국, 같은 객체를 참조하는 것이다.
