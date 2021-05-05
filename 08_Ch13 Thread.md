# 쓰레드
## 프로세스와 쓰레드
- 프로세스 : 실행 중인 프로그램. 자원과 스레드로 구성
- 스레드 : 프로세스 내에서 실제 작업을 수행
    - 모든 프로세스는 최소한 하나의 스레드를 가지고 있다.
- `프로세스 : 쓰레드 = 공장 : 일꾼`
- 하나의 새로운 프로세스를 생성하는 것보다 하나의 새로운 쓰레드를 생성하는 것이 더 적은 비용이 든다.
## 멀티쓰레드의 장단점
- 장점
    - 시스템 자원을 보다 효율적으로 사용할 수 있다.
    - 사용자에 대한 응답성(responseness)이 향상된다.
    - 작업이 분리되어 코드가 간결해진다.
- 단점
    - 동기화(synchronization)에 주의해야 한다.
    - 교착상태(dead-lock)가 발생하지 않도록 주의해야 한다.
    - 각 쓰레드가 효율적으로 고르게 실행될 수 있게 해야 한다.

# 쓰레드의 구현과 실행
1. Thread 클래스를 상속
```java
// 구현

class MyThread extends Thread {
    public void run() { ... }   // Thread클래스의 run()을 오버라이딩
}
```
```java
// 실행

MyThread t = new MyThread();    // 쓰레드의 생성
t.start(); // 쓰레드의 실행
```
2. Runnable 인터페이스를 구현
```java
// 구현

class MyThread implements Runnable {
    public void run() { ... }   // Runnable인터페이스의 추상메서드 run()을 구현
}
```
```java
// 실행

Runnable r = new MyThread();
Thread t = new Thread(r)    // Thread(Runnable r)
// Thread t = new Thread(new MyThread());
t.start();
```
### 예제
```java
public class Ex13_1 {

	public static void main(String[] args) {
		ThreadEx1_1 t1 = new ThreadEx1_1();
		
		Runnable r = new ThreadEx1_2();
		Thread t2 = new Thread(r);	// 생성자. Thread(Runnable target)
		
		t1.start();
		t2.start();
	}

}

class ThreadEx1_1 extends Thread {
	public void run() {
		for(int i=0; i<5; i++) {
			System.out.println(this.getName());
		}
	}
}

class ThreadEx1_2 implements Runnable {
	public void run() {
		for(int i=0; i<5; i++) {
			// Thread.currentThread() : 현재 실행중인 Thread를 반환한다.
			System.out.println(Thread.currentThread().getName());
		}
	}
}
```
## 쓰레드의 실행 - start()
- 쓰레드를 생성한 후에 start()를 호출해야 쓰레드가 작업을 시작한다.
    - start()를 호출했다고 즉시 실행되는 것은 아니다
    - 먼저 호출했다고 먼저 시작되는 것은 아니다.
    - 실행 순서는 OS의 스케줄러가 결정한다.(JVM이 OS에 독립적이라곤 하지만, 쓰레드는 OS에 종속적이다.)

## start()와 run()
1. main메서드가 호출되면서 start메서드가 호출스택에 올라온다.
![1](./img/thread01.jpg)

2. start메서드가 새로운 호출스택을 생성한다.
![2](./img/thread02.jpg)

3. 새롭게 만들어진 호출스택에 run메서드가 올라오고 start 메서드는 종료된다.
![3](./img/thread03.jpg)

4. 각각의 스레드가 별도의 호출 스택을 가지며 서로 독립적으로 작업을 수행할 수 있게 된다.
![4](./img/thread04.jpg)

# 싱글 쓰레드와 멀티 쓰레드, 쓰레드의 I/O 블락킹
## main 쓰레드
- main 메서드의 코드를 수행하는 쓰레드
- 쓰레드는 `사용자 쓰레드`와 `데몬 쓰레드` 두 종류가 있다.
- 프로그램은 실행중인 **사용자 쓰레드**가 하나도 없을 때 종료된다.
### 예제 : 메인 메서드가 종료되어도 다른 쓰레드가 실행중이면 프로그램이 종료되지 않는다.
```java
class Ex13_11 {
	static long startTime = 0;

	public static void main(String args[]) {
		ThreadEx11_1 th1 = new ThreadEx11_1();
		ThreadEx11_2 th2 = new ThreadEx11_2();
		th1.start();
		th2.start();
		startTime = System.currentTimeMillis();
		
		// 아래의 join메서드가 있는 try catch 블럭을 없애면 소요시간이 먼저 찍힘
		try {
			th1.join();	// th1의 작업이 끝날 때까지 main쓰레드를 기다리게 한다.
			th2.join();	// th2의 작업이 끝날 때까지 main쓰레드를 기다리게 한다.
		} catch(InterruptedException e) {}

		System.out.print("소요시간:" + (System.currentTimeMillis() - Ex13_11.startTime));
	}
}

class ThreadEx11_1 extends Thread {
	public void run() {
		for(int i=0; i < 300; i++) {
			System.out.print(new String("-"));
		}
	}
}

class ThreadEx11_2 extends Thread {
	public void run() {
		for(int i=0; i < 300; i++) {
			System.out.print(new String("|"));
		}
	}
}
```
output
```
------||||-||||||||||||||||||||||||||||||||||||||||||||||||||||
-----------------------------------||||||||||||||---------|||||
-------------------|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
----------------------------------------------------------------------------------------------------------------------------------------------------------------
-------------------|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
||||||||||||||||--|||||||||||||||||||||||||||||||||||||||
----------------------------------------
---------소요시간:11
```

## 싱글쓰레드와 멀티쓰레드
![5](./img/thread05.jpg)
<br>

- 싱글 쓰레드의 경우보다 멀티 쓰레드의 경우가 총 소요시간이 더 많이 걸린다.
- 그 이유는 작업을 번갈아 수행하는 context switching이 발생할 때 시간이 소요되기 때문이다.
- 시간이 더 걸리더라도 동시에 두 작업을 수행할 수 있는 것이 더 장점이다.
    - 채팅을 하면서 파일을 다운로드 받을 수 없는 경우가 싱글쓰레드
    - 다운로드 시간이 더 걸리더라도 채팅과 동시에 파일 다운이 가능한 경우가 멀티쓰레드
## 쓰레드의 I/O 블락킹
```java
// 싱글 쓰레드

class ThreadEx6 {
    public static void main(String[] args) {
        String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");
        System.out.println("입력하신 값은" + input + "입니다.");

        for(int i=10; i>0; i--) {
            System.out.prinln(i);
            try {Thread.sleep(1000);} catch(Exception e) { }
        }
    }
}
```
```java
// 멀티 쓰레드

class ThreadEx7 {
    public static void main(String[] args) {
        ThreadEx7_1 th1 = new ThreadEx7_1();
        th1.start();

        String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");
        System.out.println("입력하신 값은" + input + "입니다.");
    }
}

class ThreadEx7_1 extends Thread {
    public void run() {
        for(int i=10; i>0; i--) {
            System.out.prinln(i);
            try {Thread.sleep(1000);} catch(Exception e) { }
    }
}
```

![6](./img/thread06.jpg)
<br>

- 이런 경우 멀티 쓰레드가 싱글 쓰레드보다 작업을 빨리 마칠 수 있다.

# 쓰레드의 우선순위. 쓰레드 그룹
## 쓰레드의 우선순위(priority of thread)
- 작업의 중요도에 따라 쓰레드의 우선순위를 다르게 하여 특정 쓰레드가 더 많은 작업시간을 갖게 할 수 있다.
- Java에서는 우선순위를 1~10 사이의 값으로 지정할 수 있고, 별도의 지정이 없으면 보통 우선순위인 5로 설정된다.
```
void set priority(int newPriority)  쓰레드의 우선순위를 지정한 값으로 변경한다.
int getPriority()                   쓰레드의 우선순위를 반환한다.
```
```java
public static final int MAX_PRIORITY  = 10  // 최대 우선순위
public static final int MIN_PRIORITY  = 1   // 최소 우선순위
public static final int NORM_PRIORITY = 5   // 보통 우선순위
```

## 쓰레드 그룹
- 서로 관련된 쓰레드를 그룹으로 묶어서 다루기 위한 것
- 모든 쓰레드는 반드시 하나의 쓰레드 그룹에 포함되어 있어야 한다.
- 쓰레드 그룹을 지정하지 않고 생성한 쓰레드는 'main쓰레드 그룹'에 속한다.
- 자신을 생성한 쓰레드(부모 쓰레드)의 그룹과 우선순위를 상속받는다.
```java
// 생성자

Thread(ThreadGroup group, String name)
Thread(ThreadGroup group, Runnable target)
Thread(ThreadGroup group, Runnable target, String name)
Thread(ThreadGroup group, Runnable target, String name, long stackSize)
```
- `ThreadGroup getThreadGroup()`: 쓰레드 자신이 속한 쓰레드 그룹을 반환한다.
- `void uncaughtException(Thread t, Throwable e)`: 처리되지 않은 예외에 의해 쓰레드 그룹의 쓰레드가 실행이 종료되었을 때, JVM에 의해 이 메서드가 자동적으로 호출된다.

### 쓰레드 그룹의 메서드

![7](./img/thread07.jpg)

# 데몬 쓰레드, 쓰레드의 상태
## 데몬 쓰레드(daemon thread)
- 일반 쓰레드(non-daemon thread)의 작업을 돕는 보조적인 역할을 수행
- 일반 쓰레드가 모두 종료되면 자동적으로 종료된다.
- 가비지 컬렉터, 자동 저장, 화면 자동갱신 등에 사용된다.
- 무한루프와 조건문을 이용해서 실행 후 대기하다가 특정조건이 만족되면 작업을 수행하고 다시 대기하도록 작성한다.
```java
public void run() {
    while(true) {
        try {
            Thread.sleep(3 * 60 * 1000);    // 3분마다
        } catch(InterruptedException e) { }

        // autoSave의 값이 true이면 autoSave()를 호출한다.
        if(autoSave) {
            autoSave();
        }
    }
}
```
- `boolean isDaemon()` : 쓰레드가 데몬 쓰레드인지 확인한다. 데몬 쓰레드이면 true를 반환
- `void setDaemon(boolean on)` : 쓰레드를 데몬 쓰레드로 또는 사용자 쓰레드로 변경.
    - 매개변수 on을 true로 지정하면 데몬 쓰레드가 된다.
    - 반드시 start()를 호출하기 전에 실행되어야 한다. 
      그렇지 않으면 `IllegalThreadStateException`이 발생한다.

### 예제
```java
class Ex13_7 implements Runnable  {
	static boolean autoSave = false;

	public static void main(String[] args) {
		Thread t = new Thread(new Ex13_7()); // Thread(Runnable r)
		t.setDaemon(true);		// 이 부분이 없으면 종료되지 않는다.
		t.start();

		for(int i=1; i <= 10; i++) {
			try{
				Thread.sleep(1000);
			} catch(InterruptedException e) {}
			System.out.println(i);

			if(i==5) autoSave = true;
		}

		System.out.println("프로그램을 종료합니다.");
	}

	public void run() {
		while(true) {
			try { 
				Thread.sleep(3 * 1000); // 3초마다
			} catch(InterruptedException e) {}

			// autoSave의 값이 true이면 autoSave()를 호출한다.
			if(autoSave) autoSave();
		}
	}

	public void autoSave() {
		System.out.println("작업파일이 자동저장되었습니다.");
	}
}
```
output
```
1
2
3
4
5
작업파일이 자동저장되었습니다.
6
7
8
작업파일이 자동저장되었습니다.
9
10
프로그램을 종료합니다.
```

## 쓰레드의 상태

|상태|설명|
|---|---|
|NEW|쓰레드가 생성되고 아직 start()가 호출되지 않은 상태|
|RUNNABLE|실행 중 또는 실행 가능한 상태|
|BLOCKED|동기화블럭에 의해서 일시정지된 상태(lock이 풀릴 때까지 기다리는 상태)|
|WAITING, TIMED_WAITING|쓰레드의 작업이 종료되지는 않았지만 실행가능하지 않은(unrunnable) 일시정지상태. <br>TIMED_WAITING은 일시정지시간이 지정된 경우를 의미|
|TERMINATED|쓰레드의 작업이 종료된 상태|
