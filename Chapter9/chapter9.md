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
Integer num=new Integer(2); // 박싱
int n=num.intValue(); // 언박싱
```

### 자동 박싱(AutoBoxing)과 자동 언박싱(AutoUnBoxing)

- 기본자료형과 래퍼 클래스간의 변환을 자동으로 해주는 동작
- 명시적 박싱과 언박싱을 생략 가능
- JDK 1.5 이상부터 지원되며 JVM에 의해 처리됨

```java
Integer wrapperNum=new Integer(2);
int primitiveNum=wrapperNum;  // 오토 언박싱
```

```java
int primitiveNum=2;
Integer wrapperNum=primitiveNum; // 오토 박싱
```

```java
int primitiveNum=2;
Integer wrapperNum=primitiveNum;

System.out.println(primitiveNum+wrapperNum); // 오토 언박싱되어 계산됨
```

```java
Integer num1=new Integer(1);
Integer num2=new Integer(1);

System.out.println(num1==num2);  // false
System.out.println(num1.equals(num2)); // frue
```

- 명시적 생성으로 인한 다른 주소값을 가리킨다.

```java
Integer num1=1;
Integer num2=1;

System.out.println(num1==num2);  // true
System.out.println(num1.equals(num2)); // frue
```

- 내부적으로 Integer.valueOf(1) 과 같이 오토 박싱하여 객체가 생성된다.
- 위 메서드는 내부적으로 Integer Cache값을 사용하기 때문에 같은 캐시의 주소값을 참조하게 되고 결국, 같은 객체를 참조하는 것이다.

### Objects 클래스

- Object 와 유사한 이름을 가진 java.util.Objects 클래스는 객체 비교, 해시 코드 생성, null 여부,
  객체 문자열 리턴 등의 연산을 수행하는 정적 메소드들로 구성된 Object 클래스의 유틸리티 클래스이다.
- 인스턴스를 생성하여 사용하는 클래스가 아니라, 정적 메소드를 사용하라고 만든 클래스이다.
- throw new AssertionError("No java.util.Objects instances for you!"); 인스턴스 생성을 막아 놓고 아래와 같이 예외 처리 되어있음.

| 메소드(매개변수)                            | 설명                                 |
|--------------------------------------|------------------------------------|
| compare(T a, T b, Comparator c)      | 두 객체 a,b를 Comparator를 사용해서 비교      |
| deepEquals(Object a, Object b)       | 두 객체의 깊은 비교(배열의 항목까지 비교)           |
| equals(Object a, Object b)           | 두 객체의 얕은 비교(번지 값만 비교)              |
| hash(Object ... values)              | 매개 값이 저장된 배열의 해시코드 생성              |
| hashCode(Object o)                   | 객체의 해시코드 생성                        |
| isNull(Object obj)                   | 객체가 null인지 조사                      |
| nonNull(Object obj)                  | 객체가 null이 아닌지 조사                   |
| requireNonNull(T obj)                | 객체가 null인경우 예외 발생                  |
| requireNonNull(T obj, String message) | 객체가 null 인경우 에외 발생 (주어진 예외 메시지 포함) |
| toString(Object o) | 객체의 toString()리턴 값 리턴|

- equlas 실제 구현 메서드

```java
public static boolean equals(Object a,Object b){
        return(a==b)||(a!=null&&a.equals(b));
        // 내부적으로 null을 체크해주기때문에 NPE에 대해 안전하다.
        }
```

- deepEquals 실제 구현 메서드

```java
public static boolean deepEquals(Object a,Object b){
        if(a==b)return true;
        else if(a==null||b==null)return false;
        else return Arrays.deepEquals0(a,b);
        // 객체의 깊은 값까지 비교해준다
        // 배열이면 값을 모두 비교해줌.
        }
```

### hashCode vs equals

- 둘 다 Java 객체의 최상위 계층인 Object 클래스에 정의되어 있다.
- 결국 모든 객체는 equals와 hashCode를 상속받고 있다.

#### equals

- equals 메소드는 기본적으로 2개의 객체가 동일한지 검사하기 위해 사용된다.
- equals가 구현된 방법은 2개의 객체가 참조하는 것이 동일한지를 확인하는 것이며, 이는 동일성(Identity)을 비교하는 것이다.
- 2개의 객체가 가리키는 곳이 **동일한 메모리 주소** 일 경우에만 **동일한 객체**가 된다.

```java
String a = "hi";
String b = "hi";

System.out.println(a == b); // false 메모리상의 참조 주소값을 비교하기 때문
System.out.println(a.equals(b)); // true
// 다른 주소값을 가리키고 있어서 false가 나와야 되는데 왜 true 출력이 될까?
```

#### String equals 오버라이딩
```java
public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String aString = (String)anObject;
            if (coder() == aString.coder()) {
                return isLatin1() ? StringLatin1.equals(value, aString.value)
                                  : StringUTF16.equals(value, aString.value);
            }
        }
        return false;
    }
```
위와 같이 오버라이딩하여 두 객체의 동일성(Identity)을 비교하는 것이 아닌, <br>
두 객체의 동등성(Equality)을 비교한다.  


#### hashCode
- 객체를 식별할 수 있는 유일한 정수값을 반환한다. 
- 객체의 메모리 번지를 이용해서 반환하도록 되어있다.

> 즉 동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, <br>
> 동일한 객체는 동일한 해시코드를 가져야한다.<br>
> 우리가 클래스를 만든다면 equals와 hashCode는 오버라이딩해줘야 한다.
> 

## Random 클래스
- 다양한 랜덤값을 지원하는 클래스

```java
Random random = new Random();
System.out.println(random.nextBoolean());   // true or false
System.out.println(random.nextInt());       // all 2^32 possible int values
System.out.println(random.nextInt(100));    // range : 0 ~ 99
```


## 정규식(Regular Expression)
- 문자열 데이터 중에서 원하는 조건(패턴)과 일치하는 문자열 부분을 찾아내기 위해 사용하는 것
- 미리 정의된 기호와 문자를 이용해서 작성한 문자열을 말한다.

### 정규식 기본 기호
|기호|설명|
|----|----|
|.|임의의 문자 1개를 의미|
|^|시작을 의미한다[] 괄호 안에 있다면 일치하지 않는 부정의 의미로로 쓰인다|
|$|$앞의 문자열로 문자가 끝나는지를 의미한다	|
|[]|[] 괄호 안의 문자가 있는지를 확인한다	|
|[^]	|[] 대괄호 안에 ^ 문자가 있으면, 제외를 뜻함 <br>- 대괄호 안에 ^ 가 쓰이면 제외의 뜻 <br>- 대괄호 밖에 ^ 가 쓰이면 시작점의 뜻|
|-|사이의 문자 혹은 숫자를 의미한다	|

### 자주 사용되는 정규식
|정규식|설명|
|---|---|
|^[0-9]*$	|숫자|
|^[a-zA-Z]*$	|영문자|
|^[가-힣]*$	|한글|
|\\w+@\\w+\\.\\w+(\\.\\w+)?	|이메일|
|^\d{2,3}-\d{3,4}-\d{4}$	|전화번호|
|\d{6} \- [1-4]\d{6}	|주민번호|
|^\d{3}-\d{2}$	|우편번호|



