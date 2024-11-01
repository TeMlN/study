## Garbage Collection

<br>

### Description

- 자바 메모리 구조의 영역중 Heap 영역에 대한 Garbage (더 이상 사용되지 않는 불필요한 메모리)를 정리해주는 작업의 주체

### Premise

- 자바의 메모리 구조의 Heap 영역 특성상 2가지의 전제가 있음
    - 대부분의 객체는 Unreachable한 상태가 된다.
        - Unreachable - 접근 불가능한 상태
    - 오래된 객체에서 새로운 객체로의 참조는 흔하지 않다.

### Structure
![이해를 위한 heap memory 이미지](/study/image/java-heap-memory.png)

- Minor Gc - (Young 영역에서 발생하는 Garbage Collection)
    - 실행 속도는 Major Gc에 비해 상대적으로 빠른편으로 애플리케이션에 크게 영향을 주지 않는다.
    - Young 영역
        - (대부분의 객체는 금방 Unreachable한 상태가 되기 때문에 대개의 객체는 Young 영역에 생성되었다 사라진다.)
        - 1개의 Eden 영역과 2개의 Survivor 영역으로 구성 되어있다. 영역은 총 3가지인 셈
            - Eden 영역
                - 새롭게 생성된 객체가 할당되는 영역
                - Eden 영역이 꽉차게 되면 Minor Gc가 동작함.
                - Minor Gc가 동작하고 나서도 살아남은 객체는 Survivor 영역으로 이동
            - Survivor 영역 - 한번 이상의 Minor Gc가 일어났음에도 살아남은 객체가 존재하는 영역
                - 위 과정이 반복되어 한개의 Survivor의 영역이 가득차게 되면 다른 Survivor 영역으로 이동시킴
                - Survivor 영역에서 반복되어 오래 살아남은 객체들은 Old 영역으로 Promotion(이동) 된다.
- Major Gc
    - 실행 속도가 Minor Gc에 비해 상대적으로 느린편으로 크게는 10배 이상으로 차이가 난다.
    - 실행 속도가 느리므로 애플리케이션의 문제 발생을 야기 할시 Gc 튜닝을 통해 Stop the world의 Latency를 줄여 문제를 극복한다.
    - Old 영역
        - Survivor 영역에서 Promotion 된 객체들로 인해 메모리가 부족해 지면 Major Gc가 동작한다.

### Mechanism

- Stop The World
    - Gc가 동작될때 JVM이 Gc를 동작할 Thread를 제외한 나머지 Thread의 작업을 중단시키는 작업
- Mark And Sweep
    - Stop the world 작업이 실행되면 Gc는 모든 변수, Unreachable 한 객체를 스캔한다
    - 스캔을 하며 객체들의 레퍼런스를 탐색하며 동시에 사용되고 있는 메모리를 식별함
    - 이러한 과정을 통틀어 Mark 라 칭한다
    - 이후에 Mark되지 않은 객체들을 메모리에서 제거 하는데 이 과정을 Sweep 이라 칭한다.