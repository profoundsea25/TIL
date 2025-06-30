# Java 가상 스레드, 깊이 있는 소스 코드 분석과 작동 원리

## [1편 - 생성과 시작](https://techblog.lycorp.co.jp/ko/about-java-virtual-thread-1)
- Java의 가상 스레드(Virtual Thread)는 경량 스레드.
  - 기존 스렐드보다 더 적은 자원으로 더 많은 수의 스레드를 효율적으로 관리할 수 있다.
  - 특히 블로킹 I/O에 효율적

#### 가상 스레드의 장점
- 기존 Java Thread = 플랫폼 스레드(platform thread)
- 가상 스레드는 이 플랫폼 스레드 내부에서 실행.
- 가상 스레드를 실행해주는 역할을 하는 플랫폼 스레드를 캐리어 스레드(carrier thread)라고 함.
- 장점
  - 블로킹 I/O 발생시 발생하는 컨텍스트 스위칭 비용 감소
  - 스레드 생성 비용 감소

#### 블로킹 I/O 작업 시 발생하는 컨텍스트 스위칭 비용 감소
- 기존 스레드 모델에서는, 스레드가 블로킹되면 다른 스레드가 수행됨.
- 이때 플랫폼 스레드는 OS 스레드와 1:1 매핑되며, OS 레벨의 컨텍스트 스위칭이 발생함.
- 가상 스레드에서는, 플랫폼 스레드 내부에 스케쥴러를 통해 여러 개의 가상 스레드가 매핑된다.
  - 가상 스레드 하나가 블로킹되면, 스케쥴러 내부에서 다른 가상 스레드로 스위칭.
  - 컨텍스트 스위칭이 OS 레벨이 아닌, JVM(애플리케이션) 레벨이다. 그래서 비용이 더 적다.

#### 스레드 생성 비용(메모리, 연산) 감소
- 기존 스레드는 새로 생성할 때마다 JVM 메모리 영역에 스레드별로 필요한 영역(PC 레지스터, 스택, 네이티브 메서드 스택)을 새롭게 할당
- 작동 중인 가상 스레드는 캐리어 스레드의 영역에 할당되며, 작동 중이지 않은(블로킹된) 가상 스레드 정보는 힙 영역에 할당
- 정리 : 기존 스레드는 스레드마다 별도의 메모리 영역을 사용하는 반면, 가상 스레드는 소수의 스레드 메모리 영역과 힙 메모리를 사용
  - 따라서 메모리 공간을 줄일 수 있고, 스레드 생성 비용이 더 적음

#### 가상 스레드
```java
final class VirtualThread extends BaseVirtualThread {
    private final Executor scheduler;
    private final Continuation cont;
    private final Runnable runContinuation;
    private volatile int state;
    private volatile Thread carrierThread;
    ...
}
```
- `private final Executor scheduler`
  - 스케줄러가 현재 작업을 수행하는 가상 스레드를 캐리어 스레드와 연결
  - 현재의 가상 스레드가 사용하는 스케줄러의 참조값 저장
  - 생성자에서 직접 지정하거나, 자식 가상 스레드의 경우 부모 가상 스레드의 것을 그대로 사용하거나, 기본 스케쥴러 `Fork/Join Pool`을 사용
- `private final Continuation cont`
  - 실행해야 할 작업을 미리 저장, 컨텍스트 스위칭이 발생하면 기존 작업 정보를 저장하고 미리 저장해 놓았던 실행해야 할 작업 정보를 불러옴.
- `private final Runnable runContinuation`
  - `cont`를 실행시킴. 메서드 실행 전후에 가상 스레드 상태 변경 등의 작업을 실행
- `private volatile int state`
  - 가상 스레드의 현재 상태를 나타냄.
- `private volatile Thread carrierThread`
  - 가상 스레드를 실행하는 플랫폼 스레드를 참조하는 멤버 변수

## [2편 - 컨텍스트 스위칭](https://techblog.lycorp.co.jp/ko/about-java-virtual-thread-2)

- 가상 스레드가 블로킹 I/O 작업을 만나면 컨텍스트 스위칭이 발생
  - 가상 스레드는 실행 중이던 캐리어 스레드와의 매핑이 끊어지며, 이후 블로킹 I/O 작업이 완료되었을 때 다시 캐리어 스레드 작업이 재개
  - 가상 스레드의 `park`와 `unpark` 메서드가 수행

#### 컨텍스트 스위칭 예시 살펴보기 -  `NioSocketImpl` 클래스
- 내부에 Poller(폴링을 위한 객체)를 사용해 I/O 작업이 완료되었는지 확인한다.
- 읽기 요청을 보내고 작업이 바로 완료 되지 않으면 'park' 메서드를 호출한다.
- 폴링 중 소켓 데이터 수신이 확인 되면, `unpark`가 진행된다.

## [3편 - 고정 이슈와 한계](https://techblog.lycorp.co.jp/ko/about-java-virtual-thread-3)

#### 고정 이슈(pinned)
- 가상 스레드가 블로킹 I/O를 만나 모종의 이유로 캐리어 스레드와 연결된 가상 스레드가 교체되지 않고 고정되는 현상
  - 캐리어 스레드는 더 이상 작업을 진행하지 않고 CPU와 격리됨.
  - 이때 전체 시스템이 저하되거나 데드락 같은 치명적인 이슈가 발생할 수 있음.
