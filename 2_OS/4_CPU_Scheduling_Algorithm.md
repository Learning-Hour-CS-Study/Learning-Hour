## 프로세스 스케줄링의 정의

프로세스 스케줄링이란 운영체제가 어떤 프로세스에게 CPU를 할당할지 결정하는 과정이다.

![7375221515_486616_1bd590c46f4479d316f1ef4277442400.gif.thumb.jpg](/img/4_CPU_Scheduling_Algorithm/1.jpg)

우리가 컴퓨터를 할 때, 보통 하나의 프로세스만 실행시키지 않는다. 하지만 CPU는 컴퓨터의 핵심 자원이기 때문에 여러 프로세스가 동시에 CPU를 사용하려고 할 때, 어떤 프로세스에게 먼저 CPU를 할당할지를 효율적으로 결정하는 것이 중요하다.

## 프로세스 스케줄링의 목적

- CPU 활용률 극대화 : CPU가 놀고 있는 시간을 최소화 해서 시스템 전체의 성능 향상

1. 공정성 확보 : 모든 프로세스에게 공정한 기회를 제공하여 특정 프로세스가 CPU를 독점적으로 사용하는 것을 방지
2. 응답 시간 최소화 : 사용자의 요청에 대한 응답 시간을 줄여서 사용자가 체감하는 시스템 성능을 향상
3. 처리량 증가 : 주어진 시간 동안 더 많은 작업을 완료하여 시스템의 효율성 향상

## 스케줄링 기준 (Criteria)

우리가 핸드폰을 살 때 기준이 필요하듯이(ex. 저렴하니까 갤럭시, 디자인이 이쁘니까 아이폰 사야지~) 다음 순서에 프로세서를 어떤 프로세스에게 할당해 줄지에 대한 기준이 필요하다.

스케줄링 기법이 고려하는 항목

- 프로세스의 특성
  - I/O-bounded 와 compute-bounded
- 시스템 특성
  - Batch system 과 interactive system
- 프로세스의 긴급성(urgency
  - Hard or soft real time, non-real time system
- 프로세스 우선순위
- 프로세스 총 실행 시간
- …

### CPU Burst vs I/O burst

![제목 없음.jpg](/img/4_CPU_Scheduling_Algorithm/2.jpg)

프로세스가 작업을 수행하는 과정은 CPU 사용(계산)과 I/O 대기(or 사용)의 반복으로 이루어진다.

- CPU burst : CPU 사용 시간. CPU burst time이 더 긴 경우 (CPU burst > I/O burst)를 compute-bounded라 한다. 즉, 연산에 의해서 성능이 결정(bound)된다는 뜻

- I/O burst : I/O 대기 시간. I/O burst time이 더 긴 경우 (CPU burst < I/O burst)를 I/O bounded 라고 한다. I/O에 의해서 성능이 결정(bound)된다는 뜻

어디에서 더 시간을 많이 사용하는지도 스케줄링의 중요한 기준이 된다.

## 스케줄링의 단계 (Level)

![2.png](/img/4_CPU_Scheduling_Algorithm/3.png)

프로세스가 CPU를 할당받고 작업을 수행하는 과정 전반에 걸쳐, 스케줄링의 각 단계가 시스템 내에서 얼마나 자주 개입하고, 어떤 자원을 관리하는지에 따라 구분

스케줄링은 3단계로 나뉜다.

- Long-term Scheduling (가장 낮은 빈도)
- Mid-term Scheduling (중간 정도 빈도)
- Short-term Scheduling (가장 빈번한 빈도)

### Long-term Scheduling

![1.png](/img/4_CPU_Scheduling_Algorithm/4.png)

- Job scheduling : 시스템에 제출 할 (Kernel에 등록 할) 작업(Job)을 결정, Job이 커널에 등록이 되면 프로세스가 되는데, 이때 대기하고 있는 Job들 중에 어느 것을 커널에 등록할지를 결정하는 것
- 주요 역할 : 다중 프로그래밍 정도(degree) 조절하는 것(시스템 내에 동시에 실행하는 프로세스의 수를 조절) → 시스템 리소스를 효율적으로 관리하고, 시스템의 안정성과 성능 유지
- I/O bounded와 compute-bounded 프로세스들을 잘 섞어서 선택해야 한다.
  - 만약 compute-bounded 만 엄청 많고 I/O bounded가 별로 없다고 해보자. CPU는 열심히 일하지만 I/O device들은 놀게 되면 전체적인 시스템적인 측면에서 보았을 때 비효율적인 상황이 연출된다.

### Mid-term Scheduling

![2.png](/img/4_CPU_Scheduling_Algorithm/5.png)

- Swapping : 메모리에 올라와 있는 프로세스 중에서 현재 실행되지 않는 프로세스를 일시적으로 메모리에서 내보낼지 (Swap out), 어떤 프로세스를 다시 메모리에 로드할지(Swap in) 결정하는 것
- 주요 역할 : 시스템의 메모리 공간 확보. 다중 프로그래밍 정도 조절 등
- tip) Long-term 스케줄링과 Mid-term 스케줄링 모두 다중 프로그래밍 정도를 조절하지만 약간의 차이가 있다.
  - Long-term 스케줄링 : 서류 통과 후 면접 볼 인원 수를 결정
  - Mid-term 스케줄링 : 면접 보러 온 인원 중 몇 명을 면접장에 배치하고 몇 명을 휴게실에 배치할 지를 결정

### Short-term Scheduling

![5.png](/img/4_CPU_Scheduling_Algorithm/6.png)

- Process Scheduling : 프로세서를 할당할 프로세스를 결정. Ready 상태(메모리에 적재되어 있고, 실행될 준비가 완료된 상태)에 있는 프로세스를 running 상태로 만드는 것
- 주요 역할 : 프로세스 선택(Ready Queue에 들어있는 Ready 상태의 프로세스들 중에서 CPU를 할당할 프로세스를 선택하는 것)

## 스케줄링 정책(Policy)

위에서 배운 Criteria를 어떻게 활용하여 스케줄링 결정을 내릴지에 대한 규칙과 가이드라인.

즉, “어떤 기준으로 스케줄링 할지를 정했으면, 이제 어떻게 스케줄링할 것인가”

### Preemptive vs Non-Preemptive scheduling

- Non-preemptive scheduling (누구도 내 것을 뺏을 수 없다)
  - 할당 받을 자원을 스스로 반납할 때 까지 사용
  - 장점
    - Context switch overhead가 적음
  - 단점
    - 잦은 우선순위 역전(우선순위가 높은 프로세스가 와도 처리를 못함), 평균 응답 시간 증가(내가 되게 금방 끝나는 작업이여도 먼저 선점한 프로세스가 끝날 때까지 기다려야 함)
- preemptive scheduling(누가 와서 내 것을 빼앗을 수 있다)
  - 타의에 의해 자원을 빼앗길 수 있음
    - ex) 할당 시간 종료, 우선순위가 높은 프로세스 등장
  - context switch overhead가 큼 (프로세스가 자주 바뀜)

## 프로세스 스케줄링 알고리즘

![6.png](/img/4_CPU_Scheduling_Algorithm/7.png)

굉장히 다양한 스케줄링 알고리즘이 존재한다.

### FCFS (First-Come-First-Service)

선착순 알고리즘 : 먼저 오는 프로세스에게 먼저 프로세서를 할당해준다.

- Non-preeptive scheduling (누구도 내 것을 뺏을 수 없음)
- 스케줄링 기준
  - 도착 시간 (ready queue에 도착한 기준)
  - 먼저 도착한 프로세스를 먼저 처리
- High resource utilization = scheduling overhead가 낮다 = CPU의 효용이 높다. (그냥 오는 순서대로 프로세서를 할당해주면 되므로 스케줄링에 대한 오버헤드가 굉장히 작고 프로세서가 계속 일을 할 수 있다는 장점이 있다.)
- Convoy effect : 하나의 수행시간이 긴 프로세스에 의해 다른 프로세스들이 긴 대기시간을 갖게 되는 현상을 겪을 수 있다. (ex. 나는 2초만 쓰면 되는 작업인데 먼저 쓰고 있는 애가 1시간이 걸리면 나는 1시간 2초 뒤에 결과를 받음)
- 긴 평균 응답시간(response time) : 내가 작업을 요청했는데 언제 결과를 받을지 모름

### RR (Round-Robin)

FCFS에서 제한 시간을 두고 돌아가면서 프로세서를 사용하는 방법

- Preemptive scheduling (누가 와서 내 것을 빼앗을 수 있음)
- 스케줄링 기준
  - 도착 시간 (ready queue에 도착한 기준)
  - 먼저 도착한 프로세스를 먼저 처리
- 자용 사용 **제한 시간(time quantum)**이 있음
  - 프로세스는 할당된 시간이 지나면 자원을 반납(timer-runout)
  - 특정 프로세스의 자원 독점(monopoly)를 방지
  - context switch overhead가 큼

### SPN (Shortest-Process-Next)

burst time이 가장 짧은 프로세스에게 프로세서를 할당해준다.

FCFS 알고리즘의 경우 나의 burst time이 짧을 지라도 오래 기다려야 한다는 문제점이 있었다. 그래서 burst time이 짧은 애를 먼저 처리하자는 idea가 등장

- Non-preemptive scheduling
- 스케줄링 기준
  - 실행시간 (burst time 기준)
  - Burst time이 가장 작은 프로세스를 먼저 처리
- 장점
  - 평균 대기시간 최소화
  - 시스템 내 프로세스 수 최소화
  - 많은 프로세스들에게 빠른 응답 시간 제공
- 단점
  - Starvation (무한 대기) 현상 발생
    - Burst time이 긴 프로세스는 자원을 할당받지 못할 수 있음
  - 정확한 실행시간을 알 수 없음

### HRRN (High-Response-Ratio-Next)

실행시간 대비 얼마나 기다렸는가를 기준으로 우선순위를 매기는 스케줄링 기법

- Non-preemptive scheduling
- SPN + Aging concepts
- Aging concepts (Age 즉, 나이가 많은 사람을 고려하겠다 는 의미)
- 스케줄링 기준
  - Response ratio가 높은 프로세스 우선
- Response ratio = (waiting time + burst time) / burst time = 내가 필요한 시간 대비 얼마나 기다렸느냐
  - SPN의 장점 + starvation 방지
  - burst time 예측 기법이 필요함

SPN, SRTN, HRRN 스케줄링 기법들은 효율성, 성능들을 높일 수는 있었지만 Burst time을 예측해야하고 이것이 어렵다는 문제점이 있었다. 그래서 Burst time을 예측하지 않고도 비슷한 효과를 내고자 MLQ(Multi-Level Queue), MFQ(Multi-level Feedback Queue) 스케줄링 기법들이 등장하게 되었다.

### MLQ (Multi-Level Queue)

![7.png](/img/4_CPU_Scheduling_Algorithm/8.png)

- 프로세스들을 여러 개의 queue로 분류하고, 각 queue마다 다른 스케줄링 기법을 적용하는 방식.
- 시스템 내의 다양한 프로세스들의 특성을 고려하여 전체 시스템의 성능 향상을 목표로 함

- 작업 (or 우선순위)별 별도의 ready queue를 가짐
  - 최초 배정된 queue를 벗어나지 못함
  - 각각의 queue는 자신만의 스케줄링 기법 사용
- Queue 사이에는 우선순위 기반의 스케줄링 사용
  - Fixed-priority preemptive scheduling 등
- 장점
  - 다양한 프로세스 특성(compute-bounded, I/O bounded)을 고려하여 각 queue에 적합한 스케줄링 알고리즘을 적용할 수 있음
  - 시스템 성능 향상 : 각 큐에 적합한 스케줄링 알고리즘을 적용함으로써 전체 시스템의 CPU 활용률, 응답 시간, 처리량을 향상 시킬 수 있음
- 단점
  - 여러 개의 Queue 관리 등 스케줄링 overhead
  - 우선순위가 낮은 queue는 starvation 현상 발생 가능

### MFQ (Multi-Level Feedback Queue)

- 프로세스의 Queue간 이동이 허용된 MLQ
- 동작 방법
  1. 새로운 프로세스는 가장 높은 우선순위 큐에 삽입됨
  2. 프로세스가 CPU를 할당받아 실행되다가 시간 할당량이 초과되면, 다음 우선순위 큐로 이동
  3. 프로세스가 I/O 작업을 요청하면, 해당 작업을 완료한 후 상위 우선순위 큐로 이동할 수 있음
  4. 특정 큐에서 오랫동안 대기하는 프로세스는 aging 기법에 의해 우선순위가 높아질 수 있음
- 장점
  - 프로세스에 대한 사전 정보(Burst time) 없이 SPN, SRTN, HRRN 스케줄링 기법의 효과를 볼 수 있음
- 단점
  - 설계 구현 복잡
  - 스케줄링 오버헤드 큼

## 질문

1. CPU burst와 I/O burst 란 무엇인가?
2. MLQ와 MFQ의 차이점이 무엇인가?
