# Ch12 지네릭스
## 지네릭스란?
- 컴파일시 타입을 체크해 주는 기능(compile-time type check)
- 런타임에러를 예방 가능
- 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여줌
```java
// Tv객체만 저장할 수 있는 ArrayList를 생성
ArrayList<Tv> tvList = new ArrayList<Tv>();

tvList.add(new Tv());       // OK
tvList.add(new Audio());    // 컴파일 에러. Tv외에 다른 타입은 저장 불가
```
### 예제1. 지네릭스는 컴파일 에러를 발생시킴으로써 런타임에러를 막는다.
```java
import java.util.ArrayList;

public class GenericTest {
	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		list.add(10);
		list.add(20);
		list.add("30");
		
		Integer s = (Integer)list.get(2);
		System.out.println(s);
	}
}
```
- 위의 코드는 list의 3번째 요소가 String임에도 불구하고 이클립스 상에서 빨간줄이 안뜬다.
- Object를 Integer로 형변환 하는 것만 인식되기 때문이다.
- 따라서 실행시키면 런타임 에러가 발생한다.
```java
import java.util.ArrayList;

public class GenericTest {
	public static void main(String[] args) {
		ArrayList<Integer> list = new ArrayList<Integer>();
		list.add(10);
		list.add(20);
		list.add("30");	// String 추가하면 컴파일에러
		
		Integer s = list.get(1);
		System.out.println(s);      // 형변환 생략 가능
	}
}
```
- 위의 코드처럼 지네릭스를 사용하면 다른 타입의 추가를 막아준다.
- `list.get(...)`은 Object타입을 반환하기 때문에 형변환이 필요했는데, 지네릭스를 사용하면 형변환을 생략할 수 있다.
- 또한 JDK1.5이후에는 다양한 타입의 객체를 담는 ArrayList여도 지네릭스를 쓰는 것을 권장한다.
```java
// 비권장
ArrayList list = new ArrayList();

// 권장
ArrayList<Object> list = new ArrayList<Object>();
```
### 일반 클래스 -> 지네릭클래스
```java
ArrayList // 일반 클래스
ArrayList<E> // 지네릭 클래스
```

## 타입 변수
- 지네릭 클래스를 작성할 떄, Object타입 대신 타입 변수(E)를 선언해서 사용.
- 객체를 생성시, 타입 변수(E) 대신 실제 타입을 대입
- 타입 변수 대신 실제 타입이 지정되면, 형변환 생략가능  
👀 이클립스에서 F3 누르면 소스를 볼수있고 Ctrl+O를 누르면 소스의 멤버 목록이 나온다.  
# 지네릭스 용어, 지네릭 타입과 다형성
## 지네릭스 용어
```
Box<T>  지네릭 클래스, 'T의 Box' 또는 'T Box'라고 읽는다.
T       타입 변수 또는 타입 매개변수. (T는 타입 문자)
Box     원시타입(raw type) (일반 클래스)
```
```java
Box<String> b = new Box<String>();
    ㄴ 대입된 타입(매개변수화된 타입, parameterized type)
```
## 지네릭 타입과 다형성
- 참조 변수와 생성자의 대입된 타입은 일치해야 한다.
```java
class Product {}
class Tv extends Product {}
class Audio extends Product {}
```
위와 같은 상속 관계일 때
```java
ArrayList<Tv>       list = new ArrayList<Tv>(); //OK. 일치
ArrayList<Product>  list = new ArrayList<Tv>(); //에러. 불일치
```
- 지네릭 클래스간의 다형성은 성립(여전히 대입된 타입은 일치해야)
```java
List<Tv> list = new ArrayList<Tv>();    // OK. 다형성. ArrayList가 List를 구현
List<Tv> list = new LinkedList<Tv>();   // OK. 다형성. LinkedList가 List를 구현
```
- 매개변수의 다형성도 성립.
```java
ArrayList<Product> list = new ArrayList<Product>();
list.add(new Product());
list.add(new Tv());     // OK.
list.add(new Audio());  // OK.
```
``` java
Product p = list.get(0);    // 형변환 필요 없음
Tv t = (Tv)list.get(1); // 형변환 필요
```
# Iterator, HashMap과 지네릭스
## Iterator<E>
- 클래스를 작성할 때, Object타입 대신 타입 변수를 사용
```java
// Iterator 소스
public interface Iterator {
    boolean hasNext();
    Object next();      // Object 반환
    void remove();
}
```
```java
Iterator it = list.iterator();
while(it.hasNext()) {
    Student s = (Student)it.next(); // 형변환 필요
}
```
- 원시 타입의 경우 형변환 해줘야했다.
```java
// Iterator 소스
public interface Iterator<E> {
    boolean hasNext();
    E next();      // 지정된 타입 반환
    void remove();
}
```
```java
Iterator<Student> it = list.iterator();
while(it.hasNext()) {
    Student s = it.next(); // 형변환 불필요
}
```
## HashMap<K,V>
- 여러개의 타입 변수가 필요한 경우, 콤마로 구분
```java
// HashMap 소스
public class HashMap<K,V> extends AbstractMap<K,V> {
    ...
    public V get(Object Key) { /* 내용 생략 */}
    public V put(K key, V value) { /* 내용 생략 */}
    public V remove(Object Key) { /* 내용 생략 */}
    ...
}
```
# 제한된 지네릭 클래스, 지네릭스의 제약
## 제한된 지네릭 클래스
- extends로 대입할 수 있는 타입을 제한
```java
class FruitBox<T extends Fruit> {   // Fruit의 자손만 타입으로 지정가능
    ArrayList<T> list = new ArrayList<T>();
}

// Apple이 Fruit의 자손일 때,
FruitBox<Apple> appleBox = new FruitBox<Apple>();   // OK.
FruitBox<Toy>   toyBox = new FruitBox<Toy>(); // 에러. Toy는 Fruit의 자손이 아님
```
- 인터페이스인 경우에도 implements가 아닌 extends를 사용
```java
interface Eatable {}
class FruitBox<T extends Eatable> {...}
```
### 예제
```java
import java.util.ArrayList;

interface Eatable {}

class Fruit implements Eatable {
	public String toString() { return "Fruit";}
}
class Apple extends Fruit { public String toString() { return "Apple";}}
class Grape extends Fruit { public String toString() { return "Grape";}}
class Toy                 { public String toString() { return "Toy"  ;}}

class Box<T> {
	ArrayList<T> list = new ArrayList<T>();
	void add(T item) { list.add(item);     }
	T get(int i)     { return list.get(i); }
	int size()       { return list.size(); }
	public String toString() { return list.toString();}
}

class FruitBox<T extends Fruit & Eatable> extends Box<T> {} 

class Ex12_3 {
	public static void main(String[] args) {
		FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
		FruitBox<Apple> appleBox = new FruitBox<Apple>();
		FruitBox<Grape> grapeBox = new FruitBox<Grape>();
//		FruitBox<Grape> grapeBox = new FruitBox<Apple>(); // 에러. 타입 불일치
//		FruitBox<Toy>   toyBox   = new FruitBox<Toy>();   // 에러.

		fruitBox.add(new Fruit());
		fruitBox.add(new Apple());
		fruitBox.add(new Grape());
		appleBox.add(new Apple());
//		appleBox.add(new Grape());  // 에러. Grape는 Apple의 자손이 아님
		grapeBox.add(new Grape());

		System.out.println("fruitBox-"+fruitBox);
		System.out.println("appleBox-"+appleBox);
		System.out.println("grapeBox-"+grapeBox);
	}  // main
}


```
- `class FruitBox<T extends Fruit & Eatable> extends Box<T> {}`
    - Fruit의 자손이면서 Eatable을 구현한 객체만 받음
    - 타입변수에 인터페이스를 같이 쓰려면 `,`가 아닌 `&`로 구분해야 함
    - 위의 예제에서는 `& Eatable`굳이 안써도 됨
## 지네릭스의 제약
- 타입 변수에 대입은 인스턴스 별로 다르게 가능
```java
Box<Apple> appleBox = new Box<Apple>(); // OK. Apple객체만 저장가능
Box<Grape> appleBox = new Box<Grape>(); // OK. Grape객체만 저장가능
```
- static멤버에 타입 변수 사용 불가(인스턴스 별로 다르게 대입 가능해야하니까. static은 모든 인스턴스에 공통적임.)
```java
class Box<T> {
    static T item;  // 에러
    static int compare(T t1, T t2) {...} // 에러
    ...
}
```
- 배열이나 객체를 생성할 때 타입 변수 사용 불가(new 연산자 뒤에 타입변수 사용 불가). 타입 변수로 배열 선언은 가능
    - new 연산자는 뒤의 타입이 확정되어 있어야한다.
```java
class Box<T> {
    T[] itemArr;    // OK. T타입의 배열을 위한 참조변수
    ...
    T[] toArray() {
        T[] tmpArr = new T[itemArr.length]; // 에러. 제네릭 배열 생성 불가
    }
}
```
# 와일드카드, 지네릭메서드
## 와일드 카드 <?>
- 하나의 참조 변수로 대입이 된 타입이 다른 객체를 참조 가능
```java
ArrayList<? extends Product> list = new ArrayList<Tv>();    // OK
ArrayList<? extends Product> list = new ArrayList<Audio>(); // OK
ArrayList<Product> list = new ArrayList<Tv>();  // 에러. 대입된 타입 불일치
```
와일드 카드
```
<? extends T>   와일드 카드의 상한 제한. T와 그 자손들만 가능
<? super T>     와일드 카드의 하한 제한. T와 그 조상들만 가능
<?>             제한없음. 모든 타입이 가능. <? extends Object>와 동일
```
- 메서드의 매개변수에 와일드 카드를 사용
```java
static Juice makeJuice(FruitBox<? extends Fruit> box) {
    String tmp = "";
    for(Fruit f: box.getList()) tmp += f + " ";
    return new Juice(tmp);
}
```
```java
System.out.println(Juicer.makeJuice(new FruitBox<Fruit>()));
System.out.println(Juicer.makeJuice(new FruitBox<Apple>()));
```
## 지네릭 메서드
- 지네릭 타입이 선언된 메서드(타입 변수는 메서드 내에서만 유효)
```java
static <T> void sort(List<T> list, Comparator<? super T> c)
```
- 클래스의 타입 매개변수 <T>와 메서드의 타입 매개변수 <T>는 별개
```java
class FruitBox<T> { // 지네릭 클래스 내의 타입변수
    ...
    static <T> void sort(List<T> list, Comparator<? super T> c){ // 지네릭 메서드 내의 타입변수
        ...
    }
}
```
- 메서드를 호출할 때마다 타입을 대입해야함(대부분 생략 가능)
```java
static <T extends Fruit> Juice makeJuice(FruitBox<T> box) {
    String tmp = "";
    for(Fruit f : box.getList()) tmp += f + " ";
    return new Juice(tmp);
}

FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
FruitBox<Apple> fruitBox = new FruitBox<Apple>();
...
System.out.println(Juicer.<Fruit>makeJuice(fruitBox));
System.out.println(Juicer.<Apple>makeJuice(appleBox));
```
- 메서드를 호출할 때 타입을 생략하지 않을 때는 클래스 이름 생략 불가
```java
System.out.println(<Fruit>makeJuice(fruitBox));         // 에러. 클래스 이름 생략불가
System.out.println(this.<Fruit>makeJuice(fruitBox));    // OK
System.out.println(Juicer.<Fruit>makeJuice(fruitBox));  // OK
```
### 아래 두 메서드는 같은 의미.
```java
static <T extends Fruit> Juice makeJuice(FruitBox<T> box) {
    String tmp = "";
    for(Fruit f : box.getList()) tmp += f + " ";
    return new Juice(tmp);
}
```
- 지네릭 메서드는 메서드를 호출할 때마다 다른 지네릭 타입을 대입할 수 있게 한 것
```java
static Juice makeJuice(FruitBox<? extends Fruit> box) {
    String tmp = "";
    for(Fruit f : box.getList()) tmp += f + " ";
    return new Juice(tmp);
}
```
- 와일드 카드는 하나의 참조변수로 서로 다른 타입이 대입된 여러 지네릭 객체를 다루기 위한 것




# 지네릭 형변환
## 지네릭 타입의 형변환
- 지네릭 타입과 원시 타입 간의 형변환은 바람직하지 않다. (경고 발생)
```java
Box<Object> objBox = null;
Box box = (Box)objBox;      // OK. 지네릭 타입 -> 원시 타입. 경고 발생
objBox = (Box<Object>)box;  // OK. 원시 타입 -> 지네릭 타입. 경고 발생
```
```java
objBox = (Box<Object>)strBox;   // 에러. Box<String> -> Box<Object>
strBox = (Box<String>)objBox;   // 에러. Box<Object> -> Box<String>
```
- 와일드 카드가 사용된 지네릭 타입으로는 형변환 가능
```java
Box<Object>         objBox = (Box<Object>)new Box<String>();            // 에러. 형변환 불가능
Box<? extends Object> wBox = (Box<? extends Object>)new Box<String>();  // OK
Box<? extends Object> wBox = new Box<String>();                         // 위 문장과 동일
```
```java
FruitBox<? extends Fruit> fbox = (FruitBox<? extends Fruit>)new FruitBox<Fruit>();

// FruitBox<Apple> -> FruitBox<? extends Fruit> 형변환 가능
FruitBox<? extends Fruit> abox = new FruitBox<Apple>();

// FruitBox<? extends Fruit> -> FruitBox<Apple> 이렇게 형변환도 가능
FruitBox<Apple> appleBox = (FruitBox<Apple>)abox;
```
## 지네릭 타입의 제거
- 컴파일러는 지네릭 타입을 제거하고, 필요한 곳에 형변환을 넣는다.
    1. 지네릭 타입의 경계(bound)를 제거 (하위호환성을 위해)
    ```java
    class Box<T extends Fruit> {
        void add(T t) {
            ...
        }
    }
    ```
    👇
    ```java
    class Box {
        void add(Fruit t) {

        }
    }
    ```
    2. 지네릭 타입 제거 후에 타입이 불일치하면, 형변환을 추가
    ```java
    T get(int i) {
        return list.get(i);
    }
    ```
    👇
    ```java
    Fruit get(int i) {
        return (Fruit)list.get(i);
    }
    ```
    3. 와일드 카드가 포함된 경우, 적절한 타입으로 형변환 추가
    ```java
    static Juice makeJuice(FruitBox<? extends Fruit> box) {
        String tmp = "";
        for(Fruit f : box.getList())
            tmp += f + " ";
        return new Juice(tmp);
    }
    ```
    👇
    ```java
    static Juice makeJuice(FruitBox box) {
        String tmp = "";
        Iterator it = box.getList().iterator();
        while(it.hasNext()){
            tmp += (Fruit)it.next() + " ";
        }
        return new Juice(tmp);
    }
    ```