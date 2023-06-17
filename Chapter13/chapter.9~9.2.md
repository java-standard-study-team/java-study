# 쓰레드의 동기화

* 쓰레드의 동기화란, 여러 개의 쓰레드가 동시에 실행되는 상황에서 데이터의 일관성과 실행 순서를 보장하기 위해 사용되는 개념입니다.
* 동기화를 통해 쓰레드들은 서로 협력하여 작업을 수행하고 충돌을 방지할 수 있습니다.

* 한 쓰레드가 특정 작업을 끝마치기 전 까지 다른 쓰레드에 의해 방해받지 않도록 하는 것이 필요 -> 임계 영역(critical section)과 잠금(락, lock) 두둥
```agsl
* 임계 영역은 여러 개의 스레드나 프로세스가 동시에 공유 자원에 접근하는 코드 영역을 말한다 <br>
* 동시에 접근하는 것을 방지하여 문제가 발생하지 않도록 조치하는 공간이에요
```
```agsl
락은 한 번에 하나의 스레드만이 특정 코드 또는 자원에 접근할 수 있도록 제어하는 도구입니다.<br>
다른 스레드가 해당 락을 소유하고 있으면 다른 스레드는 대기하고, 락을 소유하고 있는 스레드가 락을 해제할 때까지 기다립니다.
```

## synchronized를 이용한 동기화
-> 임계 영역을 설정하는데 사용된다
* 인스턴스 메서드에 synchronized 적용하기:
* 
```java
public synchronized void synchronizedMethod() {
// 동기화가 필요한 코드
}
```
-> 메서드 전체가 임계 영역으로 간주됩니다. 

한 번에 하나의 쓰레드만이 이 메서드를 실행할 수 있습니다.
다른 쓰레드가 해당 메서드에 진입하려고 하면, 락(lock)을 획득할 때까지 대기하게 됩니다. 메서드 실행이 완료되면 락이 해제되어 다른 쓰레드가 메서드에 접근할 수 있게 됩니다

코드 블록에 synchronized 적용하기:
```java
public void someMethod() {
// 비동기적으로 실행되는 코드

    synchronized (monitorObject) {
        // 동기화가 필요한 코드
    }

    // 비동기적으로 실행되는 코드
}

```

다음은 은행 계좌에서 잔고를 확인하고 임의의 금액을 출금하는 예제입니다.
```java
public class BankAccount {
private int balance;

    public BankAccount(int initialBalance) {
        this.balance = initialBalance;
    }

    public synchronized void withdraw(int amount) {
        if (amount <= balance) {
            System.out.println("Withdrawal amount: " + amount);
            balance -= amount;
            System.out.println("Remaining balance: " + balance);
        } else {
            System.out.println("헐랭");
        }
    }
}

```

위의 예제에서는 BankAccount 클래스를 정의하고, withdraw 메서드에 synchronized 키워드를 사용하여 동기화를 적용했습니다. 
withdraw 메서드는 출금할 금액을 받아와 현재 잔고에서 차감합니다. 잔고가 충분하면 출금을 수행하고, 충분하지 않으면 "헐랭!" 메시지를 출력합니다.

여러 쓰레드가 동시에 withdraw 메서드를 호출할 때, 하나의 쓰레드만이 메서드에 진입하여 출금을 수행하게 됩니다. 다른 쓰레드들은 대기하게 됩니다. 
이를 통해 잔고가 음수로 떨어지는 등의 문제를 방지할 수 있습니다.

## wait()과 notify()

* wait()와 notify()는 자바에서 쓰레드 간의 협력과 동기화를 위해 사용되는 메서드입니다.

* wait(): wait() 메서드는 현재 실행 중인 쓰레드를 일시적으로 멈추고 대기 상태로 만든다 -> 해당 객체의 락(lock)을 반납하고 다른 쓰레드가 그 락을 획득할 수 있도록 한다
->wait()은 주로 조건이 충족되기를 기다리는 경우에 호출됩니다 -> wait() 메서드는 InterruptedException을 던질 수 있으므로 예외 처리가 필요하다!

* notify(): notify()는 대기 중인 쓰레드 중 하나를 무작위로 선택하여 깨우며, 깨어난 쓰레드는 다시 실행 가능한 상태가 됩니다.

* 하나의 쓰레드가 특정 조건이 충족될 때까지 기다려야 하는 경우:
-> 조건을 확인하고 -> 조건이 충족되지 않으면 wait()을 호출 -> 대기 상태 ->
다른 쓰레드가 조건을 충족시키고 작업을 수행 -> notify() 또는 notifyAll()을 호출하여 대기 중인 쓰레드를 깨우기!
-> 작업을 수행합니다.

++ notify()는 임의로 선택된 하나의 쓰레드만을 깨우므로, 여러 쓰레드를 동시에 깨워야 할 경우 notifyAll()을 사용하는 것이 더 안전합니다.
```java
public class SharedResource {
    private boolean isAvailable = false;
    private final Object lock = new Object();

    public void useResource() {
        synchronized (lock) {
            while (!isAvailable) {
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            System.out.println("리소스 사용 후 초기화 또는 다른 작업 수행");

            isAvailable = false;
            lock.notify();
        }
    }

    public void releaseResource() {
        synchronized (lock) {
            while (isAvailable) {
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            System.out.println("리소스 초기화 또는 다른 작업 수행");

            isAvailable = true;
            lock.notify();
        }
    }
}

```
synchronized 키워드를 사용하여 동기화를 수행하고, lock 객체를 사용하여 락을 관리합니다.
synchronized 블록을 사용하여 동기화 범위를 지정하고, lock.wait()와 lock.notify()를 사용하여 스레드 간의 통신을 처리합니다.

## 기아 현상과 경쟁 상태
기아 현상과 경쟁 상태는 다중 쓰레드 프로그래밍에서 발생할 수 있는 문제입니다.

* 기아 현상 (Starvation)
  * 기아 현상은 특정 쓰레드가 원하는 리소스를 오랜 시간 동안 얻지 못하여 영원히 실행이 지연되는 상황을 말합니다.
  * 다른 쓰레드들이 지나치게 많은 CPU 시간, 메모리, I/O 등의 리소스를 사용하여 해당 쓰레드의 우선순위가 떨어지거나, 쓰레드 스케줄러의 부당한 동작 등이 원인이 될 수 있습니다.

* 경쟁 상태 (Race Condition)
   * 경쟁 상태는 두 개 이상의 쓰레드가 공유된 리소스에 접근하거나 변경하려고 할 때, 그 실행 순서에 따라 결과가 달라지거나 예측할 수 없는 동작이 발생하는 상황을 말합니다.
   * 경쟁 상태는 동기화를 적절하게 처리하지 않을 때 발생할 수 있습니다. 예를 들어, 두 개의 쓰레드가 동시에 같은 변수를 증가시킨다면, 증가되는 값이 제대로 반영되지 않을 수 있습니다.