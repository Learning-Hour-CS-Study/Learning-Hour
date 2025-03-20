## 주소공간과 가상 메모리

## 주소 공간<br>

CPU와 실행 중인 프로그램은 저장된 공간이 시시각각 변하기에 현재 메모리 몇 번지에 무엇이 저장되어 있는지 다 알 수 없음<br>

이러한 체계를 극복하고자 논리 주소와 물리 주소로 나눔

**논리 주소** <br>

CPU와 실행 중인 프로그램 입장에서 바라본 주소 <br>

실행 중인 프로그램 각각에게 부여된 0번지부터 시작하는 주소

<br>

**물리 주소**<br>

메모리 입장에서 바라본 주소

즉, 정보가 실제로 저장된 하드웨어상의 주소<br>

<br>

**Address Binding**

프로그램 논리 주소를 실제 메모리의 물리 주소로 매핑(mapping)하는 작업

Ex. int a; - 논리 주소

15000번 주소에 있다 - 물리 주소

<br>

### 프로그램이 실행되기까지의 과정

![alt text](<img/08_Address Space_Virtual Memory/1.png>)

Source Code를 컴파일하여 Object module로 변환한다. (→ compile time binding) <br>

Linker를 통해 Obiect module을 다른 object module과 묶어 실행 가능한 형태인 Load module 만들어준다. <Br>

Loder가 Load module을 memory로 올려준다. <br>

(→Load time binding)

<br>

## 매핑하는 시점에 따른 분류

### Compile time binding<br>

프로세스가 메모리에 적재(지정)될 위치를 컴파일러가 결정할 수 있는 경우

### 특징

- 위치가 변하지 않음<br>
- 프로그램 전체가 메모리에 올라가야 함<br>

### Load time binding <br>

메모리 적재 위치를 컴파일 시점에서 모르면,

대체 가능한 상대 주소를 생성하고 적재 시점(load time)에 시작 주소를 반영하여 사용자 코드 상의 주소를 재설정함<br>

프로그램 전체가 메모리에 올라가야 함<br>

![alt text](<img/08_Address Space_Virtual Memory/2.png>)

<br>

### Run-time binding

Address binding을 수행 시간까지 연기

프로그램 실행 중에 CPU 메모리 관리 장치(MMU)에 의해 수행됨

### 특징

- 프로세스가 수행 도중 다른 메모리 위치로 이동할 수 있음
- 동적 메모리 할당이나 가상 메모리에 사용됨

  대부분의 OS가 사용

  <Br>

### 알아두면 유용한 개념

**MMU란?**

논리 주소와 베이스 레지스터 값(시작 주소)을 더하여 논리 주소를 물리 주소로 변환

**Dynamic Loading**

모든 루틴을 교체 가능한 형태로 디스크에 저장

실제 호출 전까지는 루틴을 적재하지 않음

- 메인 프로그램만 메모리에 적재하여 수행
- 루틴 호출 시점에 address binding 수행
- 메모리 공간의 효율적 사용
  <br>

**Swapping**

프로세서 할당이 수행 완료된 프로세스는 swap-device로 보내고 (Swap-out)

새롭게 시작하는 프로세스는 메모리에 적재 (Swap-in)

<br>

## Memory Allocation

### Continuous Memory Allocation (연속 할당)

- **Uni-programming**
- **Multi-programming**

  - **Fixed partition(FPM)**

  - **Variable partition(VPM)**

### Non-continuous Memory Allocation(비연속할당)

<br>

## 연속 할당 방식

프로세스를 하나의 연속된 메모리 공간에 할당하는 정책

**메모리 구성 정책**

- 메모리에 동시에 올라갈 수 있는 프로세스 수

  - Multiprogramming degree

- 각 프로세스에게 할당되는 메모리 공간 크기
- 메모리 분할 방법

<br>

### Uni-programming

하나의 프로세스만 메모리 상에 존재

가장 간단한 메모리 관리 기법

Kernel 밑에 존재함

![alt text](<img/08_Address Space_Virtual Memory/3.png>)

<br>

**단점**

프로그램의 크기가 메모리 크기보다 클 수 있음

커널을 보호해야 함

<br>

**해결책**

- **Overlay structure**

실행에 필요한 오버레이만 메모리에 로드하고, 나머지 오버레이는 디스크에 저장

실행 중에 필요한 다른 오버레이가 있을 시, 현재 오버레이와 교체

사용자가 프로그램의 흐름 및 자료구조를 모두 알고 있어야 함

- **경계 레지스터(boundary register) 사용**

<br>

### Fixed Partition Multiprogramming

메모리 공간을 고정된 크기로 미리 분할하고

각 파티션에 하나의 프로세스를 할당하여 동시에 여러 프로그램을 실행하는 것

Partition의 수 =K

Multiprogramming degree=K

![alt text](<img/08_Address Space_Virtual Memory/4.png>)

**장점**<br>

- 고정된 파티션 크기<br>
- 메모리 관리가 비교적 간단함<br>

  **단점** <br>

  - 내부 단편화(Fragmentation)<Br>

  → 프로그램 크기가 파티션 크기보다 작을 경우 남는 공간이 발생하여 메모리 낭비가 일어날 수 있음

   <br>

### 단편화 (Fragmentation)<Br>

**내부 단편화**<Br>

Partition 크기 > process 크기

**외부 단편화**<br>

남은 메모리 크기 > Process 크기지만 연속된 공간이 아님 <br>

프로세스들이 실행되고 종료되는 반복 과정에서 메모리 사이에 빈 공간이 생김 <Br>

현재 빈 공간은 총 50MB일 때 비연속적으로 작은 메모리 공간으로 인한 프로세스를 할당하기 어려움

즉, 외부 단편화는 프로세스를 할당하기 어려울 만큼 작은 메모리 공간들로 인해 메모리가 낭비되는 현상을 뜻함

<br>

**해결책**

1. **메모리 압축 (Compaction)**

 <br>

![alt text](<img/08_Address Space_Virtual Memory/14.png>)

여기저기 흩어져 있는 빈 공간들을 하나로 모으는 방식

프로세스를 재배치시켜 흩어져 있는 작은 빈공간을 하나의 큰 빈 공간으로 만드는 방식

1. **페이징**<br>
   프로세스의 논리 주소 공간을 페이지(page)라는 일정 단위로 자르고,
   메모리의 물리 주소 공간을 프레임(frame)이라는 페이지와 동일한 일정 단위로
   자른 뒤 페이지를 프레임에 할당하는 가장 메모리 관리 기법

<br>

### Variable Partition Multiprogramming<br>

초기 전체가 하나의 영역이지만 프로세스를 처리하는 과정에서 메모리 공간을 동적으로 할당하는 방식

![alt text](<img/08_Address Space_Virtual Memory/5.png>)
(a) 초기 상태는 u에서 시작하고 120MB 크기를 가짐

(b) 프로세스 A가 20MB 요청하고 해당 크기만큼 메모리를 할당함

→ 즉 , 파티션은 요청한 크기에 맞추어 동적으로 나뉨

( f ) 10MB를 차지했던 B가 나가게 되면, none 상태가 됨

none: 아무도 쓰지 않는 상태

(g) E가 15MB를 요청하면 B가 나갔던 자리에는 들어올 수 없고 남은 45MB 공간에 들어가야 함
<br>

![alt text](<img/08_Address Space_Virtual Memory/6.png>)
![alt text](<img/08_Address Space_Virtual Memory/7.png>)

### 배치 전략<Br>

**만약 P가 5MB를 요청한다면 어디에 배치할 것인가?**<Br>
![alt text](<img/08_Address Space_Virtual Memory/8.png>)

### First-fit(최초 적합)<br>

프로세스 크기만큼 비어 있는 메모리 공간을 찾아 차례대로 프로세스를 로드하는 방식
<br>

![alt text](<img/08_Address Space_Virtual Memory/9.png>)

충분한 크기를 가진 첫 번째 partition을 선택<Br>

구현이 간단하고 탐색 시간이 짧음 <br>

공간 활용률이 떨어질 수 있음<br>

(프로세스보다 큰 공간에 할당되는 경우가 자주 발생하기에)<br>

### Best-fit(최적 적합)<Br>

프로세스가 들어갈 수 있는 partition 중 가장 작은 곳을 선택
<Br>

![alt text](<img/08_Address Space_Virtual Memory/10.png>)

크기가 큰 partition을 유지할 수 있음<Br>

탐색 시간이 오래 걸림<Br>
(모든 partition을 살펴봐야 함)<br>

작은 크기의 partition이 많이 발생함<br>

<br>

### Worst-fit(최악 적합)<br>

프로세스가 들어갈 수 있는 partition 중 가장 큰 곳을 선택
<Br>

![alt text](<img/08_Address Space_Virtual Memory/11.png>)

탐색 시간이 오래 걸림<Br>

(모든 partition을 살펴봐야 함)

작은 크기의 partition 발생을 줄일 수 있음<br>

큰 크기의 partition 확보가 어려움 <Br>

### 비연속 메모리 할당 Non-continuous allocation

프로세스 메모리 영역을 여러 개의 block으로 분할하고 실행 시 필요한 block들만

메모리 적재하는 방식<Br>

- 나머지 block들은 swap device에 존재<Br>
  ![alt text](<img/08_Address Space_Virtual Memory/12.png>)

프로그램은 가상 주소를 사용하며 운영체제가 이를 물리 주소(실제 주소)로 변환해줌

기법

- **페이징**
- **세그먼테이션**

가상 주소: 가상 메모리의 특정 위치에 배정된 주소

<br>

## 가상 메모리

보조기억장치를 주기억장치처럼 주소 지정이 가능하게 만든 저장 공간 할당 체제

프로그램을 일부만 메모리에 적재하여 실제 물리 메모리 크기보다 더 큰 프로세스를 실행할 수 있게 하는 기술

**가상 주소 공간**

한 프로세스가 메모리에 저장되는 논리적인 모습을 가상메모리에 구현한 공간

현재 필요치 않은 메모리 공간은 실제 물리 메모리에 올리지 않으므로 물리 메모리를
절약할 수 있음

<br>

### 특징

물리 메모리보다 큰 프로그램도 실행할 수 있음

각 프로세스의 메모리 공간을 독립적으로 관리할 수 있음

프로세스 간 메모리를 공유하는 것을 가능하게 함

→ 운영체제의 커널 코드나 라이브러리를 동시에 사용 가능

<br>

### 장점

메모리 효율성 증대

프로세스 격리 보호

다중 프로세스 지원

### 단점

페이지 폴트 발생 시 성능 저하

복잡한 하드웨어 및 소프트웨어 필요

메모리 오버헤드

페이지 폴트 : 필요한 페이지가 물리 메모리(RAM)에 존재하지 않을 때 발생하는 예외 상황

이때 OS는 필요한 페이지를 보조 기억 장치에서 메모리로 불러오는 작업(Swapping)을 수행함

<br>

## 퀴즈

1. 메모리에서의 논리 주소와 물리 주소에 대해 설명해주세요.<Br>
2. 외부 단편화와 내부 단편화에 대해 설명해주세요.<br>
