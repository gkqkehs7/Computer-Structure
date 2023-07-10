# cpu의 구조

![Drawing_2023-06-15_22 34 00 excalidraw](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/d3f15474-369d-4d9f-ae46-ddda3d7bef8a)

cpu는 메모리에 저장된 명령어를 읽어 들이고, 해석하고, 실행하는 장치입니다.

도대체 cpu에선 어떻게 명령어를 처리하는 걸까요? 그 전에, 명령어가 무엇인지 부터 알아봅시다.

<br/>

## 명령어

명렁어에 대한 글은 너무 길어 따로 작성하였습니다.

[명령어란 무엇일까?](https://github.com/gkqkehs7/Computer-Structure/blob/main/instruction.md)

<br/>

## ALU

ALU는 산술 연산과 논리 연산을 수행하는 논리 회로의 집합으로 구성됩니다. 주로 산술 연산(덧셈, 뺄셈, 곱셈, 나

눗셈 등)과 논리 연산(AND, OR, NOT, XOR 등)을 처리하는데 사용됩니다. ALU가 연산할때마다 메모리에 저장한다면, 메모리에 자주 접근하게 되어 프로그램 실행속도가 늦춰지기 때문에 수행한 연산의 결과를 메모리에 바로 저장하지 않고, 일시적으로 레지스터에 저장합니다.

<br/>

## 플래그

ALU에서 연산을 수행하면서 발생하는 조건을 표시하기 위해 플래그(Flag)라고 불리는 특수한 비트들을 설정합니다.  플래그는 주로 CPU의 상태 레지스터에 저장되며, 프로그램 실행 중에 조건 분기, 반복문 등을 제어하는 데 사용됩니다.

`부호 플래그`는 연산한 결과의 부호

`제로 플래그`는 연산의 결과가 0인지 여부

`캐리 플래그`는 연산 도중 올림수나 빌림수 발생했는지 여부

`오버플로우 플래그`는 오버플로우가 발생했는지 여부 ( 연산 결과가 레지스터보다 큰 상황 )

`인터럽트 플래그`는 인터럽트 가능 여부

`슈퍼바이저 플래그`는 커널모드로 실행 중인지, 사용자 모드로 실행 중인지 여부

를 저정합니다.

<br/>

## 레지스터

레지스터(Registers)는 중앙 처리 장치(CPU) 내부에 있는 고속의 저장소로, CPU가 사용하는 데이터를 메모리에서 가져와 저장하거나, 메모리 주소, 명령어의 위치 등을 저장하는 데 사용됩니다. 레지스터의 종류에 대해 몇가지를 살펴보고 어떻게 데이터를 가져와 cpu에게 전달하는지 알아봅시다.

### 프로그램 카운터(pc)
프로그램 카운터(pc)는 메모리에서 가져올 명령어의 주소를 저장합니다.

### 메모리 주소 레지스터 
메모리 주소 레지스터는 cpu가 메모리에서 읽을 데이터의 주소 값을 주소 버스로 보냅니다. 

### 메모리 버퍼 레지스터 
메모리 버퍼 레지스터는 메모리와 주고받을 값을 저장합니다.

### 명령어 레지스터
명령어 레지스터는 메모리에서 읽어 들인 명령어를 저장합니다. 이 명령어는 제어장치가 해석해서 제어 신호를 발생시킵니다.

### 범용 레지스터
범용 레지스터는 다양하고 일반적인 상황에서 자유롭게 사용하는 레지스터입니다.

### 플래그 레지스터
플래그 레지스터는 ALU의 연산결과에 따른 플래그들을 저장하는 레지스터입니다

<br/>

## 메모리에서가 레지스터가 데이터를 가져오는 과정

![Untitled](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/cf787bb0-4edc-4244-85ed-92a4b81c1b9f)

`프로그램 카운터`에 저장된 명령어의 주소를 버스로 보내기전에 `메모리 주소 레지스터`에 저장합니다. 그 다음 주소 버스를 통해 해당 주소에 있는 실행하고자 하는 명령어를 메모리에서 가져와 `메모리 버퍼 주소`를 거쳐 `명령어 레지스터`에 저장됩니다. 이제 이렇게 가져온 명령어는 제어장치에서 해석하고 처리하게 됩니다.

<br/>

## 제어장치

위에서 봤듯이 제어장치는 명령어를 해석하는 부품입니다. 이외에도 제어장치는 다양한 기능을 수행합니다.

### 명령어 해석 
제어 장치는 프로그램의 명령어를 해석하여 이해합니다. 명령어는 기계어 형태로 제공되며, 제어 장치는 이를 이해하고 다음에 수행할 동작을 결정합니다.

### 제어 신호 생성 
제어 장치는 명령어를 분석하여 CPU 내의 다른 구성 요소에 대한 제어 신호를 생성합니다. 이러한 제어 신호는 연산 장치(ALU), 레지스터, 메모리, 입출력 장치 등과 상호작용하여 명령어의 실행을 지시하고 데이터를 전달합니다.

### 명령어 실행 순서 제어
제어 장치는 명령어 실행의 순서를 제어합니다. 명령어는 일련의 단계로 이루어지며, 제어 장치는 명령어의 단계를 순서대로 실행하고 다음 단계로 진행합니다.

### 예외 처리
제어 장치는 예외 상황에 대한 처리를 담당합니다. 예를 들어, 오류가 발생한 경우나 중단 요청이 있는 경우에는 제어 장치가 이를 감지하고 적절한 조치를 취합니다.

<br/>

## CPU의 성능 높이기
그럼 cpu의 성능을 높이려면 어떻게 해야할까요? 여러가지 방법이 있겠지만 그 중 몇가지에 대해 알아봅시다.

<br/>

### 클럭속도 높이기

컴퓨터 부품은 `클럭` 신호마다 작동한다. 클럭의 단위는 Hz이다. Hz는 1초에 클럭이 몇 번 반복되는지를 나타낸다. 1초에 한번 클럭이 반복되면 cpu의 클럭 속도는 1Hz인 것이고 1초에 하나의 부품이 작동하는 것이다. 보통 cpu의 클럭 속도는 2.5GHz ~ 4.9GHz이다. 1초에 25억~49억번 깜빡이는 것이다. 이는 1초에 40억 가량 부품이 작동하는 것을 의미한다.

cpu는 보통 25GHz를 유지하다, 고성능을 요하는 순간에 속도를 높인다 이를 `오버클러킹(Overclocking)` 이라 한다. 클럭 속도가 높아지면 cpu는 명령어 사이클을 더 빠르게 반복하므로 클럭 속도가 높으면 cpu의 성능이 좋다.

그럼 클럭속도를 높이면 컴퓨터의 속도가 빨라지지 아닐까? 하지만 클럭속도만 높이면, cpu의 속도는 빨라지겠지만 발열이 생겨 속도를 올리는 것에는 한계가 있다.

<br/>

### 코어와 멀티코어

컴퓨터에 대해 살펴보다 보면 듀얼코어, 헥사코어 라는 용어를 볼 수 있다. 

우리가 앞에서 살펴본 cpu의 구성요소들의 집합을 코어라고 한다. 과거의 cpu는 하나의 코어로 이루어졌지만 현대의 cpu는 여러개의 코어로 이루어져있다. 당연히 혼자서 모든일을 하는 것보다, 여러명이 나누어서 하는게 훨씬 작업속도가 빠를 것이다. 

![Untitled](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/7281f1dc-4078-4b4e-95fb-3538f8625dbe)


<br/>

### 스레드와 멀티스레드

하드웨어에서 `스레드`는 하나의 코어가 동시에 처리는 명령어의 단위를 의미한다. 초기의 cpu는 하나의 코어로 한 클럭에 하나의 명령어를 처리하는 1코어 1스레드였다.

현재의 cpu는 하나의 코어로도 여러 개의 명령어를 실행할 수 있다. 이걸 멀티스레드라고 한다. 예를 들어, 8코어 16스레드는 한 번에 16개의 명령어를 처리할 수 있는 코어가 8개가 달린 cpu를 의미한다.

명령어를 처리하기 위해선 **프로그램 카운터, 메모리 주소 레지스터, 메모리 버퍼 레지스터, 명령어 레지스터** 등 여러 레지스터들의 협력이 필요 했다. 이렇게 하나의 코어에 명령어를 처리하는 레지스터 세트들을 여러개 가지고 있으면 멀티스레드가 가능하다.

스레드를 이용해 하나의 코어로도 여러개의 명령어를 동시에 처리할 수 있다 하였다. 그러나 프로그램의 입장에선 스레드도 하나의 명령어를 처리하는 cpu이기 떄문에, 2코어 4스레드의 cpu를 프로그램이 봤을때는 4개의 cpu가 있는 것 처럼 보인다. 따라서 하드웨어 스레드를 `논리프로세서라고` 부르기도 한다.

소프트웨어 스레드는 하드웨어의 스레드와는 조금 다른의미이다. 이 부분은 OS에서 알아보자.

<br/>

### 파이프라이닝

[파이프라이닝 기법](https://github.com/gkqkehs7/Computer-Structure/blob/main/pipelining.md)