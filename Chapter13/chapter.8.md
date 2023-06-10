# 8. 쓰레드의 실행제어
도와주세요
##### sleep(long millis) - 일정시간 동안 쓰레드를 멈추게 한다
```java
public class SleepExample {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("시작");

        Thread.sleep(3000); // 3초 동안 멈춤

        System.out.println("종료");
    }
}
```
* Thread.sleep() 메서드는 현재 실행 중인 스레드를 일정 시간 동안 정지시키는 역할을 합니다.
* 따라서 Thread.sleep() 메서드는 InterruptedException 예외를 던지도록 정의되어 있습니다.
* 예외를 전파시킨다는 것은 예외가 발생한 메서드에서 해당 예외를 직접 처리하지 않고, 메서드 선언부에 throws 키워드를 사용하여 해당 예외를 호출자에게 전달하는 것을 의미합니다. 이렇게 하면 호출자가 해당 예외를 처리할 수 있도록 할 수 있습니다.
```java
시작
(3초 대기)
종료
```

##### interrupt()와 interrupted() - 쓰레드의 작업을 취소한다
* interrupt(): interrupt() 메서드는 스레드의 인터럽트 상태를 설정하여 스레드가 현재 작업을 취소하도록 시도합니다. 
다른 스레드에서 해당 스레드를 중단시키려면 interrupt()를 호출합니다.

* interrupted(): interrupted() 메서드는 현재 스레드의 인터럽트 상태를 확인하고 초기화합니다. 
이 메서드를 호출하면 현재 스레드의 인터럽트 상태를 반환하고, 상태를 초기화하여 인터럽트 상태를 false로 설정합니다.

*Thread.sleep() 메서드를 사용하여 현재 스레드를 일정 시간 동안 멈춤! -> interrupt()를 호출 -> 다른 스레드가 해당 스레드를 중단-> 
sleep() 메서드는 InterruptedException을 발생시키고 스레드는 중단 -> 그리고 interrupted()를 호출하여 해당 스레드의 인터럽트 상태를 확인
```java
public class InterruptExample {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            while (!Thread.interrupted()) { // while 루프에서 Thread.interrupted()를 호출하여 스레드의 인터럽트 상태를 확인
                System.out.println("작업 중...");
            }

            System.out.println("작업 종료");
        });

        thread.start(); // 스레드 시작

        try {
            Thread.sleep(2000); // 2초 동안 멈춤
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        thread.interrupt(); // 스레드 인터럽트
        // thread.interrupt()가 호출되면 while 루프를 빠져나와 "작업 종료"를 출력
    }
}
```
##### suspend(), resume(), stop()
이 세 가지 메서드는 사용을 권장하지 않으며, deprecated 되었으므로 사용하지 않는 것이 좋습니다. 
-> deprecated가 머임?
-> 아 애너테이션 깜빡
-> @Deprecated 애너테이션이 붙은 요소를 사용하는 것은 여전히 가능하지만!!! 사용 시 컴파일러에서 경고를 발생시킵니다
* suspend(): suspend() 메서드는 스레드를 일시적으로 중단시킵니다. 스레드는 중단된 상태에서 일시적으로 대기하며, resume() 메서드가 호출되기 전까지 실행을 재개하지 않습니다.

* resume(): resume() 메서드는 suspend() 메서드에 의해 중단된 스레드를 다시 실행합니다. 중단된 스레드의 실행을 재개시킵니다.

* stop(): stop() 메서드는 스레드를 즉시 중단시킵니다

#### 엥
suspend()랑 interrupt() 둘 다 스레드를 중단시키는 건데 구체적으로 차이가 궁금하다!
-> suspend() : 스레드가 멈춘 상태에서 다른 스레드가 필요한 자원을 점유하고 있으면, 멈춘 스레드는 해당 자원을 얻을 수 없어 계속 기다리게 됩니다. 이러한 상황에서 모든 스레드가 서로를 기다리면서 아무것도 진행하지 못하는 상태가 될 수 있습니다.
-> interrupt() : 호출하면 스레드는 현재 작업을 중단하고 인터럽트 상태를 확인합니다. 만약 스레드가 블록된 상태에 있다면(예: sleep(), wait() 등), 해당 블록을 해제하고 InterruptedException을 발생시켜 다른 작업을 수행하거나 종료할 수 있도록 합니다. 이는 스레드가 더 이상 필요하지 않은 작업을 중지하고 안전한 상태로 종료하도록 돕는 역할을 합니다.

```java
public class ThreadControlExample {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(() -> {
            while (true) {
                System.out.println("작업 중...");
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        thread.start(); // 스레드 시작

        Thread.sleep(2000); // 2초 동안 대기

        thread.suspend(); // 스레드 중단
        System.out.println("스레드 중단");

        Thread.sleep(2000); // 2초 동안 대기

        thread.resume(); // 스레드 재개
        System.out.println("스레드 재개");

        Thread.sleep(2000); // 2초 동안 대기

        thread.stop(); // 스레드 종료
        System.out.println("스레드 종료");
    }
}
```
msuspend() 메서드를 호출하여 스레드를 중단 -> 그 후 2초 동안 대기하고 resume() 메서드를 호출하여 스레드를 재개-> stop() 메서드를 호출하여 스레드를 종료

##### yield() - 다른 쓰레드에게 양보한다.
yield()는 현재 실행 중인 스레드가 다른 스레드에게 실행을 양보(yield)하고 실행 대기 상태로 들어가도록 한다.
-> 다른 스레드에게 실행 기회를 주는 것 -> 스레드 스케줄러에게 현재 스레드의 실행이 완료될 때까지 기다리지 않고 다른 스레드를 실행시킬 것을 알려주는 역할을 합니다.

-> 주의할 점은 yield() 메서드는 단지 다른 스레드에게 실행 기회를 주는 것이지만, 스레드 스케줄러가 어떻게 동작할지는 보장할 수 없습니다. 
-> 따라서 스레드 간의 우선순위 설정이나 다른 동기화 메커니즘을 사용하여 원하는 동작을 구현해야한다 
yield() 메서드는 일반적으로 스레드 간의 공정한 실행을 유도하거나 스레드 간의 경합 상황에서 상대적인 우선순위를 조절하는데 사용될 수 있습니다.

yield() 메서드는 스레드를 일시적으로 실행 대기 상태로 전환시키는 역할을 하며, 스레드 스케줄링에 영향을 줄 수 있습니다. 하지만 정확한 실행 시점이나 순서는 보장되지 않기 때문에 사용 시 적절한 상황과 목적에 맞게 사용해야 합니다.

```java
public class ThreadYieldExample {
    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Thread 1: " + i);
                Thread.yield();
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Thread 2: " + i);
                Thread.yield();
            }
        });

        thread1.start();
        thread2.start();
    }
}
```
각각의 스레드는 0부터 4까지의 값을 출력하고, Thread.yield()를 호출하여 다른 스레드에게 실행을 양보

main 메서드에서는 thread1과 thread2를 시작하고 동시에 실행됩니다.
```agsl
Thread 1: 0
Thread 2: 0
Thread 1: 1
Thread 2: 1
Thread 1: 2
Thread 2: 2
Thread 1: 3
Thread 2: 3
Thread 1: 4
Thread 2: 4
```
##### join() - 다른 쓰레드의 작업을 기다린다.
join() 메서드는 한 스레드가 다른 스레드의 작업을 기다리는 메서드입니다. 호출된 스레드는 대상 스레드의 작업이 끝날 때까지 대기하며, 그 후에 실행을 계속합니다.
```java
public class ThreadJoinExample {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(() -> {
            try {
                Thread.sleep(2000); // 2초 동안 작업 수행
                System.out.println("작업 완료");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        thread.start(); // 스레드 시작
        thread.join(); // 대상 스레드의 작업이 끝날 때까지 대기

        System.out.println("메인 스레드 종료");
    }
}
```
thread.start()를 호출하여 스레드를 시작하고, thread.join()을 호출하여 해당 스레드의 작업이 끝날 때까지 대기합니다.

thread.join()은 대상 스레드인 thread의 작업이 끝날 때까지 대기하며, 작업이 완료되면 호출된 스레드는 다음 코드를 실행합니다. 
따라서 "메인 스레드 종료"가 출력되는 시점은 thread의 작업이 완료된 이후입니다.

join() 메서드를 사용하여 스레드의 작업을 기다리는 것으로, 스레드 간의 작업 순서를 조절하거나 스레드의 작업 결과를 사용할 수 있습니다.
