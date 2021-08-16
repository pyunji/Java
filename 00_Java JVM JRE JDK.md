# 자바의 특징
- 객체지향 언어(c++, python, js...)
- 자동 메모리 관리
    - 가비지 컬렉터(GC)
- 멀티 쓰레드 지원
    - 채팅하면서 파일을 주고받을 수 있는 것은 해당 프로그램이 멀티쓰레드를 지원하는 언어로 만들어졌기 때문
- 운영체제에 독립적

# ❗ JVM
- 자바 프로그램이 실행되는 가상 컴퓨터
- 일반적으로 app은 os위에서 실행되는데, java는 os위의 JVM위에서 실행되기 때문에 운영체제 독립적이다
    - jvm이 운영체제별로 이미 만들어져있음
## JVM 메모리 구조
- Class Loader
- Execution Engine
- Garbage Collector(GC)
- Runtime Data Area
### Rumtime Data Area의 영역 구성
- **Method Area**
- **Heap Area**
- **Stack Area**
- PC register
- Native Method Stack
## JDK 자바 개발도구
## 자바 API 문서 설치와 사용법
- `C:\jdk1.8\docs\api`의 index.html
## 첫 번째 자바프로그램 작성
- javac.exe: 자바 컴파일러. 사람이 작성한 문장을 기계어로 번역
    - 소스 파일(.java)을 클래스 파일(.class)로 변환
    - `javac Hello.java` : 컴파일 명령
- java.exe: 자바 인터프리터. 자바프로그램(클래스파일)을 실행
- 클래스: 자바 프로그램의 단위
```java
class 클래스이름 {
    // 모든 문장은 클래스의 {}안에 있어야 한다.
}
```
- main 메서드: 자바 프로그램의 시작점. 이 메서드 없이 실행불가
```java
class 클래스이름 {
    public static void main(String[] args) {
        // 첫 문장부터 순서대로 실행된다.
    }
}
```
## 이클립스에서 자바 프로그램을 작성하는 순서
1. 프로젝트 생성
2. 클래스 생성
3. 소스파일 작성 후 저장 (자동컴파일됨)
4. 실행
- build: 소스파일(.java)로부터 프로그램을 만들어 내는 전 과정

