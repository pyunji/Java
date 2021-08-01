# 변수
- 하나의 값을 저장할 수 있는 메모리 공간(RAM)
### 변수의 선언
- 변수의 선언 이유: data를 저장할 공간을 마련하기 위해
- 변수의 선언 방법: `변수타입 변수이름;`
### 변수에 값 저장하기
- `변수타입 변수이름 = 값;`
- 변수의 초기화: 변수에 처음으로 값을 저장하는 것
    - 변수에는 클래스변수, 인스턴스변수, 지역변수가 있다.
    - **지역변수는 자동 초기화되지 않기 때문에 꼭 수동 초기화해야 한다.**

## 변수의 타입
- 변수의 타입은 저장할 값의 타입에 의해 결정된다.
- 저장할 값의 타입과 일치하는 타입으로 변수를 선언.

# 기본형
### 기본형(Primitive type)
- 8개의 기본형 데이터타입 존재
- 문자
    - char
- 숫자
    - 정수
        - byte
        - short
        - int
        - long
    - 실수
        - float
        - double
- 논리
    - boolean

### 참조형(Reference type)
- 기본형을 제외한 나머지(String, System 등)
- 메모리 주소를 저장(64bit JVM에서는 8byte까지 저장 가능)
```java
Date today; // 참조형 변수 today를 선언
today = new Date(); // today에 객체의 주소를 저장
```
## 기본형 - 종류와 크기
- 논리형: true와 false중 하나를 값으로 가지며, 조건식과 논리적 계산에 사용된다.
- 문자형 : 문자를 저장하는데 사용되며, 변수 당 하나의 문자만을 저장할 수 있다.
- 정수형: 정수값을 저장하는데 사용된다.
    - 주로 사용하는 것은 int와 long(아주 큰 정수)이다.
    - byte는 이진데이터를 다루는데 사용된다.(이미지파일, 실행파일...)
    - short는 c언어와의 호환을 위해 추가되었으나 잘 쓰이지 않는다.
- 실수형: 실수 값을 저장하는데 사용된다. float와 double이 있다.

```
1bit = 2진수 1자리  
1byte = 8bit(자바에서는 데이터를 다루는 최소 단위가 바이트임)
```

|종류\크기(byte)|1|2|4|8|
|---|---|---|---|---|
|논리형|boolean||||
|문자형||char|||
|정수형|byte|short|int (default)|long|
|실수형|||float|double (default)|

## 표현범위
- 자바에서 정수형과 실수형은 모두 부호가 있다.
    - n비트로 표현할 수 있는 부호있는 정수의 범위 : -2^(n-1) ~ 2^(n-1) - 1
    - n비트로 표현할 수 있는 부호 없는 정수의 범위 : 0 ~ 2^n -1
        - char 타입은 음수값을 저장할 필요가 없어서 부호가 없다.
- 저장공간 중 1bit는 부호를 나타내는데 쓰인다.
    - ex) 8비트의 정수형 저장 변수인 byte의 맨 앞 비트를 부호비트라고 부르고, 7bit만 값을 저장한다. 양수의 경우 0부터 127까지, 음수의 경우 -1부터 128까지 저장한다.
    - 0부터 127까지 128개, -1부터 -128까지 128개 이므로 256 즉 2^8 크기가 저장된다.

### 기본형 표현범위

|타입|범위|범위|
|---|---|---|
|byte (1byte)|-2^7 ~ 2^7 - 1|-128 ~ 127|
|char (2byte)|0 ~ 2^16 - 1|0 ~ 65535|
|short (2byte)|-2^15 ~ 2^15 - 1|-32768 ~ 32767|
|int (4byte)|-2^31 ~ 2^31 - 1|대략 -20억 ~ 20억|
|long (8byte)|-2^63 ~ 2^63 - 1|대략 -800경 ~ 800경|


<br> 


|타입|저장가능한 값의 범위|정밀도|
|---|---|---|
|float (4byte)|-3.4E38 ~ -1.4E-45, 1.4E-45 ~ 3.4E38|7자리|
|double (8byte)|-1.8E308 ~ -4.9E-324, 4.9E-324 ~ 1.8E308|15자리|

- 실수형은 정수형과 달리 저장형식이 부호비트, 지수비트, 가수비트 형식이라서 범위가 정수형보다 크다.
- 실수형은 저장 시 실제값과의 오차가 발생할 수 있다.
- 정밀도는 오차없이 표현할 수 있는 자리수이다.
- float는 가수의 자리수가 23bit이고, 정규화 시 24bit까지 저장할 수 있다.
    - 10^7 < 2^24 < 10^8 이므로 float의 정밀도는 7자리가 되는 것이다.
- 실수에서는 7자리가 그다지 높은 정밀도가 아니기 때문에 double이 디폴트타입

# 상수와 리터럴
- 상수(constant): 한 번만 값을 저장 가능한 변수
    - 변수와 달리 변경 불가능
    ```java
    final int MAX = 100;
              MAX = 200; // 에러
    ```
- 리터럴(literal) : 기존의 상수. 그 자체로 값을 의미하는 것

## 리터럴의 접두사와 접미사
- 대소문자 구별 안하지만 'L'은 웬만하면 대문자로 씀

```java
int oct = 0100; // 8진수 접두사 '0'
int hex = 0x100; // 16진수 접두사 '0x'
int b = 0b0101; // 2진수 접두사 '0b'

long l = 10_000_000_000L; // 100억(20억이 넘는 값은 long타입에 담아야함)
long l = 10_000_000_000; // 에러. 정수형 디폴트 int형에 값이 담겼으므로.
// 20억 넘는 값은 L을 꼭 붙이고 안넘는 값은 안붙여도 됨

float f = 3.14f; // 생략불가
double d = 3.14d; // 생략가능
// 실수형은 접미사가 붙지 않으면 자동으로 double타입으로 인식됨


10. // 10.0
.10 // 0.1
10f // 10.0f
1e3 // 1000.0d
```
# 데이터의 형변환과 오버플로우
- **단항, 삼항, 대입연산자의 경우 우측의 데이터를 좌측에 대입하는 방식을 취한다는 것을 명심하자!** 

## 암시적 형변환
- byte -> short -> int -> long -> float -> double
- 범위가 `변수 > 리터럴`인 경우 암시적 형변환이 됨
    - 크기가 더 큰 수납공간에 작은 값을 담는거라서 데이터 손실 없음
```java
int i = 'A'; // int > char
long l = 123; // long > int
double d = 3.14f // double > float
```
## 명시적 형변환
- **범위(타입의 크기가 아님)**가 `변수 < 리터럴`인 경우 명시적 형변환을 하지 않으면 에러
    - 명시적 형변환을 하더라도 변수 타입의 범위보다 큰 값을 담게되면 데이터 손실 발생
```java
int i = 30_0000_0000; //에러. int의 범위(+-20억) 벗어남
long l = 3.14f; // 에러. long < float
float f = 3.14; // 에러. float < double
```
### ❗ long type이 8byte이고 float type 4byte인데 왜 명시적 형변환이 필요할까?
-  타입의 크기가 아니라 범위로 생각해야 한다.
- long은 8byte고 float은 4byte이지만 long은 표현범위가 10^19이고 float은 10^38이므로
    - long -> float으로 자동형변환 가능하다.
    - float의 표현 범위가 long의 표현 범위보다 넓으므로 float -> long 명시적 형변환이 필요하다.
### ❗ int의 범위가 byte의 범위보다 넓은데 왜 명시적 형변환을 하지 않아도 될까?
- byte, short 변수에 int 리터럴 저장 가능(단, 변수의 타입의 범위 이내여야 함)
- 정수형 타입 중 접미사 L이 붙으면 long type 리터럴이고, 안붙으면 모두 int형 리터럴이다.
- byte 타입과 short 타입은 리터럴이 없기 때문에 명시적 형변환을 하지 않고 byte 타입의 범위에 맞는 int 타입의 리터럴을 사용할 수 있다.


```java
byte b = 100; // OK. byte의 범위(-128~127)에 속함
byte b = 128; // 에러. byte의 범위를 벗어남
```

## 정수형의 오버플로우
- 오버플로우 : 표현가능한 범위를 넘는 것
    - 최대값 + 1 -> 최소값
    - 최소값 - 1 -> 최대값
```java
short sh = 32767; // short: -32768~32767
int iNum = 32768; // int: -21억~21억
```
```java
sh = (short)iNum; // 명시적 형변환. 오버플로우 발생. sh = -32768
```
- 작은 저장공간인 변수 sh에 int 값을 자동 형변환하지 못한다.
- 이때는 명시적 형변환을 통해 해결할 수 있다.
    - 하지만 명시적 형변환을 할 경우 데이터가 손실되어 오버플로우가 발생할 수 있다.
```java
iNum = sh; // 자동 형변환. 큰 공간에 작은 데이터 저장 가능.
```
#### 반올림 - Math.round()
- 실수를 소수점 **첫 째자리**에서 반올림한 정수를 반환
- 실수형을 int로 형변환하면 소수점 아래는 버림을 할 수 있음

# 문자와 문자열
```java
// char는 홑따옴표를 사용한다.
char ch = 'A';
char ch = 'AB'; // 에러
char ch = ''; // 에러. primitive type은 빈 값으로 초기화 불가능

// 문자열은 쌍따옴표를 사용한다.
String s = "ABC";
String s1 = "AB"; // 대부분 이 방법을 사용
String s2 = new String("AB");

String s = "A"; // OK
String s = ""; // OK

String S1 = "A" + "B" // "AB"
```
## char와 String
```java
'K' // 2byte
"K" // 2byte
```
- 자바에서는 한글과 영문 상관없이 char형은 모두 2byte
- String의 끝에는 항상 "\0"(null)이 들어간다. 이것은 1byte
    - String의 문자 하나는 1byte

## 숫자를 문자열로 간단히 변환하는 방법
```java
"" + 7 // -> "" + "7" -> "7"

// 문자열 결합은 왼쪽에서 오른쪽으로 진행되기 때문에 순서가 중요!
"" + 7 + 7 // 14가 아니라 "77"
7 + 7 + "" // "14"
```
- `문자열` + `any type` = `문자열`
- `any type` + `문자열` = `문자열`



## printf를 이용한 출력
- println()의 단점 : 출력형식 지정불가
    - 실수의 자리 수 제한 출력 불가
    - 10진수로만 출력된다.

### printf()의 지시자
|지시자|설명|
|---|---|
|`%b`|불리언(**b**oolean) 형식으로 출력|
|`%d`|10진(**d**ecimal) 정수의 형식으로 출력|
|`%o`|8진(**o**ctal) 정수의 형식으로 출력|
|`%x`, `%X`|16진(he**x**a-decimal) 정수의 형식으로 출력|
|`%f`|부동 소수점(**f**loating-point)의 형식으로 출력|
|`%e`, `%E`|지수(**e**xponent) 표현식의 형식으로 출력|
|`%c`|문자(**c**haracter)로 출력|
|`%s`|문자열(**s**tring)로 출력|

- 정수를 10진수, 8진수, 16진수로 출력
```java
System.out.printf("%d", 15); // 15
System.out.printf("%o", 15); // 17 : 10진수인 '15'를 8진수인 '17'로 출력
System.out.printf("%x", 15); // f 
// 2진수 지시자는 따로 없고 이렇게 쓴다.
System.out.printf("%s", Integer.toBinaryString(15)); // 1111
```
- 8진수와 16진수에 접두사 붙이기 : `#`
```java
System.out.printf("%#o", 15); // 017
System.out.printf("%#x", 15); // 0xf
// 지시자가 대문자면 출력되는 접두사도 대문자.
System.out.printf("%#X", 15); // 0XF
```
- 실수 출력을 위한 지시자 `%f`, 지수형식 `%e`, 간략한 형식 `%g` (f와 e중 더 간단한 방법을 택함)
- 정수 자리수 지정
```java
System.out.printf("[%5d]\n", 10); // [   10]
System.out.printf("[%-5d]\n", 10); // [10   ]
System.out.printf("[%05d]\n", 10); // [00010]
```
- 실수 자리수 지정
`"%전체자리.소수점아래자리f"`: 전체자리수는 소수점을 포함한다.

- 문자열 자리수 지정
```java
String url = "www.codechobo.com";
System.out.printf("[%s]\n", url); // [www.codechobo.com]
System.out.printf("[%20s]\n", url); // [   www.codechobo.com]
System.out.printf("[%-20s]\n", url); // [www.codechobo.com   ]
System.out.printf("[%.8s]\n", url); // [www.code]
// 지시자 앞에 . 의 의미는 부분출력
```

## 화면에서 입력받기 - Scanner
### Scanner란?
- 화면으로부터 데이터를 입력받는 기능을 제공하는 클래스
### Scanner를 사용하려면
- import문 추가
    `import java.util.*;`
- Scanner객체의 생성
    `Scanner scanner = new Scanner(System.in)`
- Scanner객체를 사용
```java
int num = scanner.nextInt(); // 화면에서 입력받은 정수를 num에 저장
// 위와 동일한 결과가 나오는 코드
String input = scanner.nextLine(); // 화면에서 입력받은 내용을 input에 저장
int num = Integer.parseInt(input); // 문자열(input)을 숫자(num)로 변환
```

## 문자와 정수의 변환방법
- `문자열.charAt(0)` : String이 char로 바뀜
- `'3'- '0'` : 숫자 3으로 변환
```java
String str = "3";

System.out.println(str.charAt(0) - '0'); // 3
System.out.println('3' - '0' + 1); // 4
System.out.println(Integer.parseInt(str) + 1); // 4
System.out.println(str + 1); // "3" + "1" -> "31"
System.out.println(3 + '0'); // 51
```


# 연산자

## 연산자의 우선순위
- 최단산쉬관논삼대콤
- 산술 > 비교(관계) > 논리 > 대입
- 단항 > 이항(산술, 비교, 논리) > 삼항
- - **단항, 삼항, 대입연산자의 경우 우측의 데이터를 좌측에 대입하는 방식을 취한다는 것을 명심하자!** 
## 연산자의 결합규칙
- 우선순위가 같은 연산자가 있을 때 어떤 것을 먼저 연산할 것인가?
    - 대입연산자와 단항연산자를 제외하고 모두 왼쪽에서 오른쪽으로 연산 진행

### 논리 연산자
- 10진 논리: 결과값이 boolean으로 나옴
```java
boolean flag = false;
int a = 10, b = 20, c = 30;

flag = (a > b) && (b < c); // '&&'연산은 앞의 조건이 거짓이면 뒤의 조건은 수행하지 않는다.
flag = (a < b) || (b < c); // '||'연산은 앞의 조건이 참이면 뒤의 조건은 수행하지 않는다.
```

### 비트 연산자
- 2진 논리: 결과값이 10진수 값으로 나옴
```java
int x = 4, y = 7;
System.out.println(x & y); // 0100(2) & 0111(2) = 0100(2) => 4
System.out.println(x | y); // 0100(2) | 0111(2) = 0111(2) => 7
```


### 증감 연산자
- 전위형 : `++i;`
- 후위형 : `i++;`
- **증감 연산자가 독립적으로 사용된 경우 전위형과 후위형의 차이가 없다.**

<br>

```java
j = ++i; // 전위형

// 아래와 같다.
++i; // 증가 후에
j = i; // 참조하여 대입
```

```java
j = i++; // 후위형

// 아래와 같다.
j = i; // 참조하여 대입 후에
i++; // 증가
```

### 부호 연산자(단항 연산자)
- `-` 는 피연산자의 부호를 반대로 변경
- `+` 는 아무런 일도 안하고 실제 사용도 안한다.


### 나머지 연산자 `%`
- 피연산자는 0이아닌 정수만 허용
- 피연산자의 부호는 무시됨


#### 문자열의 비교
- 문자열 비교에는 `==` 대신 `equals()`를 사용해야 한다.

### 논리 부정 연산자 `!`
- `ch < 'a' || ch > 'z'` 는 이렇게 바꿀 수 있다. `!('a' <= ch && ch <= 'z')`
    - 둘다 문자 ch가 소문자가 아니라는 의미이다.

### 조건 연산자 `? :`
- 조건식의 결과에 따라 연산결과를 달리한다.
- `조건식 ? 식1 : 식2` : 조건식이 참이면 식1, 거짓이면 식2 를 연산한다.

### 대입 연산자
- **모든 연산자는 결과값을 반환한다**
- 오른쪽 연산자를 왼쪽 피연산자에 저장 후 **저장된 값을 반환**
```java
int x = 0;
System.out.println(x=3);  // 3이 출력됨
```
```java
x = y = 3;

// 연산의 진행 순서는 다음과 같다.
y = 3;
x = y;
```
- lvalue : 대입 연산자의 왼쪽 피연산자
    - 반드시 변수나 배열과 같은 저장공간이어야 한다.
- rvalue : 대입 연산자의 오른쪽 피연산자
```java
int i = 0;      
3 = i + 3;      // 에러
i + 3 = i;      // 에러

final int MAX = 3;
          MAX = 10;     // 에러
```

### 복합 대입 연산자
- 대입 연산자와 다른 연산자를 하나로 축약
- ex) `i += 3;`
```java
 i *= 10 + j;       // i = (i * 10) + j; 가 아님에 주의
 i *= (10 + j);     
 i = i * (10 + j);
 // 셋다 똑같다. 
```


# 조건문과 반복문 (제어문: flow control statement)
- 조건문 : 조건을 만족할때만 `{}`을 수행(0-1번)
- 반복문 : 조건을 만족하는 동안 `{}`를 수행(0-n번)
## if- else if문
- 조건식이 참일 때 까지 차례대로 순회한다.
```java
if (조건식1) {
    // 조건식1이 참일 때 수행될 문장들을 적는다.
} else if (조건식2) {
    // 조건식2가 거짓일 떄 수행될 문장들을 적는다.
} else if (조건식3) {
    // 조건식3가 거짓일 떄 수행될 문장들을 적는다.
} else {

}
```
- 조건식의 다양한 예

|조건식|조건식이 참일 조건|
|---|---|
|90 <= x && x <= 100|정수 x가 90이상 100이하일 때|
|x < 0 || x > 100|정수 x가 0보다 작거나 100보다 클 때|
|x % 3 == 0 && x % 2 != 0|정수 x가 3의 배수지만 2의 배수는 아닐 때|
|ch == 'y' \|\| ch == 'Y'|문자 ch가 'y'또는 'Y'일 때|
|ch == ' ' \|\| ch == '\t' \|\| ch == '\n'|문자 ch가 공백이거나 탭 또는 개행 문자일 때|
|'A' <= ch && ch <= 'Z'|문자 ch가 대문자일 때 |
|'a' <= ch && ch <= 'z'|문자 ch가 소문자일 때|
|'0' <= ch && ch <= '9'|문자 ch가 숫자일 때|
|str.equals("yes")|문자열 str의 내용이 "yes"일 때(대소문자 구분)|
|str.equalsIgnoreCase("yes")|문자열 str의 내용이 "yes"일 때(대소문자 구분안함)|

### 블럭 `{}`
- if문 블럭에 하나의 문장만 속할 경우 블럭을 안쓰기도 한다.


## switch문
- 처리해야 하는 경우의 수가 많을 떄 유용한 조건문
```java
switch (조건식) {
    case 값1 :
        // 조건식의 결과가 값1과 같을 경우 수행될 문장들
        // ...
        break;
    case 값2 : 
        // 조건식의 결과가 값2와 같을 경우 수행될 문장들
        // ...
        break;      // switch문을 벗어난다. 
    // ...
    default :
        // 조건식의 결과와 일치하는 case문이 없을 때 수행될 문장들
        // ...
}
```
### switch문의 제약조건
- switch문의 조건식 결과는 **정수**또는 **문자열**이어야 한다.
- case문의 값은 정수, 상수(문자 포함), 문자열만 가능하며 변수는 불가능하고, 중복되지 않아야 한다.
- break문을 만날 때 까지 내려간다.


## for문
- 조건을 만족하는 동안 블럭을 반복
- 반복횟수를 알 때 적합
```java
for (초기화; 조건식; 증감식) {
    // 수행될 문장
}
```
- 증감식은 블럭 안의 문장이 수행된 이후 실행된다.
- 초기화 하는 변수는 두 개 사용할 수도 있는데 타입이 같아야만 가능하다.

## while문
- 반복횟수를 모를 때 적합
```java
while (조건식) {
    // 조건식의 결과가 참인 동안 반복될 문장
}
```
for문은 원래 while문에서 유래됐다.
```java
int i = 1;
while (i <= 10) {
    System.out.println(i);
    i++;
}
```
아래의 for문과 위의 while문은 같다.
```java
for(int i=1; i<=10; i++) {
    System.out.println(i);
}
```
- while문의 무한 반복문은 true 생략 불가능 for문은 생략 가능
```java
while(true){

}
```
```java
for( ; ; ){

}
```
## do - while문
- while문은 한 번도 실행되지 않는 경우가 있을 수 있는 반면, do-while문은 적어도 한번은 반드시 실행된다.
```java
do {
    // 조건식의 연산결과가 참일 때 수행될 문장들을 적는다.(처음 한번은 무조건 실행)
} while (조건식);
```
## break문
- 자신이 포함된 하나의 반복문을 벗어난다.

## continue문
- 다음 반복으로 넘어감
- 전체 반복 중 특정 조건시 반복을 건너뛸 때 유용

## 이름붙은 반복문
- 반복문에 이름을 붙여서 하나 이상의 반복문을 벗어날 수 있다.
```java
// for문에 Loop1이라는 이름을 붙였다.
Loop1 : for(int i=2;i <=9;i++) {	
        for(int j=1;j <=9;j++) {
            if(j==5)
                break Loop1;

            System.out.println(i+"*"+ j +"="+ i*j);
        } // end of for i
        System.out.println();
} // end of Loop1
```

# 임의의 정수or실수 만들기
- `Math.random()`: 0.0과 1.0 사이의 임의의 double값을 반환
    - 0.0 <= `Math.random()` < 1.0
### 1부터 3사이의 정수 만들기
1. 각 변에 3을 곱한다.
```
0.0 * 3 <= Math.random() * 3 < 1.0 * 3
0.0 <= Math.random() * 3 < 3.0 
```
2. 각 변을 int형으로 변환한다.
```
(int)0.0 <= (int)(Math.random() * 3) < (int)3.0 
0 <= (int)(Math.random() * 3) < 3
```
3. 각 변에 1을 더한다.
```
0 + 1 <= (int)(Math.random() * 3) + 1 < 3 + 1
1 <= (int)(Math.random() * 3) + 1 < 4
```
### -5부터 5사이의 정수 20번 출력하기
```java
int num = 0;
for (int i = 1; i <= 20; i++) {
    num = (int)(Math.random() * 11) - 5;
    System.out.println(num);
```

# 주석의 종류
- 한 줄 주석
```java
// 한 줄 주석
```
- 여러 줄 주석
```java
/*
    여러 줄 주석
*/
```
- Doc 주석
```java
/**
    Doc 주석
*/
```

# 배열
- **같은타입**의 여러 변수를 하나의 묶음으로 다루는 것
## 배열의 선언
- 배열의 선언: 배열을 다루기 위한 참조변수의 선언
```java
타입[] 변수이름;                // 배열을 선언(배열을 다루기 위한 참조변수 선언)
변수이름 = new 타입[길이];      // 배열을 생성(실제 저장공간을 생성)

int[] score;                    // int타입의 배열을 다루기 위한 참조변수 score선언
score = new int[5];             // int타입의 값 5개를 저장할 수 있는 배열 생성
```
## 배열의 길이
- `배열이름.length`: int형 상수
- 배열은 한번 생성하면 그 **길이를 바꿀 수 없다**.

## 배열의 초기화
- 배열의 각 요소에 처음으로 값을 저장하는 것
```java
int[] score = {50, 60, 70, 80, 90};
```
중괄호로 초기화하는 방법은 초기화할때만 사용가능.  
아래와 같이 사용 불가능
```java
int[] score = new int[3];   // 이미 초기화
score = {1, 2, 3}           // 에러
```

## 배열의 출력
- 배열의 모든 요소를 출력하려면
1. for문 사용
```java
for (int i=0; i<배열.length; i++){
    System.out.println(배열[i])
}
```
2. Arrays 클래스 사용
```java
import java.util.Arrays;    // ctrl + shift + o 자동으로 import문 추가

System.out.println(Arrays.toString(배열));
```

## String배열의 선언과 생성
|자료형|기본값|
|---|---|
|boolean|false|
|char|'\u0000'|
|byte, short, int|0|
|long|0L|
|float|0.0f|
|double|0.0d 또는 0.0|
|**참조형**|**null**|

- 참조형인 String의 배열은 null로 초기화된다.
```java
String[] name = {"Kim", "park", "Yi"};
```
## 커맨드 라인을 통해 입력받기
```java
class Ex5_7 {
	public static void main(String[] args) {
		System.out.println("매개변수의 개수:"+args.length);
		for(int i=0;i< args.length;i++) {
			System.out.println("args[" + i + "] = \""+ args[i] + "\"");
		}
	}
}
```
클래스 Ex5_7의 소스코드가 위와 같을 때,  
커맨드라인에서 다음과 같이 실행하면 args가 설정되어 main 메소드에서 활용할 수 있다.  
```
java Ex5_7 abc 123 "Hello world"
```
## 2차원 배열
- 테이블 형태의 데이터를 저장하기 위한 배열
- 1차원 배열의 배열
```java
int[][] score = new int[4][3];    // 4행 3열의 2차원 배열을 생성한다.
```
## 2차원 배열의 초기화
```java
int[][] arr = {
                    {1, 2, 3}, 
                    {4, 5, 6}
               };
```

# String 클래스
- String클래스 = char[] + 메서드
    - String클래스는 문자열 배열에 메서드를 결합한 것이다.
    - String클래스는 내용을 변경할 수 없다.(read only)

## String클래스의 주요 메서드
|메서드|설명|
|---|---|
|char charAt(int index)|문자열에서 해당 위치(index)에 있는 문자를 반환한다.|
|int length()|문자열의 길이를 반환한다.|
|String substring(int from, int to)|문자열에서 해당 범위(from ~ to)의 문자열을 반환한다.(to는 포함 안 됨)|
|boolean equals(Object obj)|문자열의 내용이 같은지 확인한다. 같으면 결과는 true, 다르면 false|
|char[] toCharArray()|문자열을 문자배열(char[])로 변환해서 반환한다.|

# Arrays클래스로 배열 다루기
### 배열의 비교와 출력 - `equals()`, `toString()`
- 2차원 배열에서는 `deepEquals()`, `deepToString()`
### 배열의 복사 - `copyOf()`, `copyOfRange()`
### 배열의 정렬 - `sort()`
```java
int[] arr = {0,4,2,1,4,3};
int[][] arr2D = {{11,12},{21,22}};

System.out.println(Arrays.toString(Arrays.copyOf(arr, 3)));
System.out.println(Arrays.deepToString(Arrays.copyOf(arr2D, arr2D.length)));
System.out.println(Arrays.toString(Arrays.copyOfRange(arr, 1, 4)));

System.out.println(Arrays.toString(arr));
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));
```
```
[0, 4, 2]
[[11, 12], [21, 22]]
[4, 2, 1]
[0, 4, 2, 1, 4, 3]
[0, 1, 2, 3, 4, 4]
```