# 파이프라이닝

RISC-V는 다양한 명령어 세트(Instruction Set)를 제공하며, 각각의 명령어들은 몇 가지 공통적인 형태를 가지고 있습니다. 그 중 몇가지만 살펴보자면

`Load/Store 명령어`

메모리로부터 데이터를 로드하거나 메모리에 데이터를 저장한다. (LW, SW, LH, SH 등)

`산술/논리 명령어`

덧셈, 뺄셈, 곱셈, 나눗셈, AND/OR/NOT 등의 연산을 수행합니다. (ADD, SUB, MUL, DIV, AND, OR, NOT 등)

`분기 명령어`

조건에 따라 분기하여 프로그램의 흐름을 제어합니다. (BEQ, BNE, BLT, BGT 등)

`점프 명령어`
 
프로그램 카운터(PC)의 값을 변경하여 다른 명령어로 점프합니다. (JAL, JALR 등)

`시스템 명령어`

시스템 콜 등의 운영체제와 관련된 기능을 수행합니다. (ECALL, EBREAK 등)

등등 여러가지 명령어의 종류가 있습니다.

<br/>

## pipelining이 필요한 이유

어떤 명령어가 가장 오래 걸리는지는 그 명령어의 구체적인 구현 방식과 컴퓨터 하드웨어의 성능 등에 따라 달라질 수 있습니다. 그러나 대체로 데이터를 메모리로부터 로드하거나 메모리에 저장하는 Load/Store 명령어나 분기 명령어는 점프 명령어나 산술/논리 명령어에 비해 처리 속도가 느릴 수 있죠.

각 Instruction마다 각기다른 clock period를 설정할 수 없기 때문에, 가장 긴 load 명령어에 맞추어 clock period를 정합니다. common case도 아닌 load instruction에 clock period를 맞추었으니  컴퓨터의 속도가 현저히 저하될 수 있는 것입니다.

따라서 컴퓨터는 `파이프라이닝(Pipelining)` 기법을 도입하여 컴퓨터의 명령어 실행 과정을 여러 단계로 나누어 각 단계를 병렬적으로 처리하여 성능을 향상시킵니다.

<br/>

![Untitled](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/f78a40ed-6b28-4230-ae59-0b14c0960a2e)

예를들어 빨래를 한다고 가정하면 빨래마다 세탁기 → 건조기 → 옷개기 → 옷넣기 과정을 모두 마치고 다음 빨래를 하는 것보다, 세탁기를 사용했으면 다음 기다리고 있는 빨래를 사용하지 않는 세탁기에 넣어 시간을 줄이는 자는 것입니다.

세탁기와 같게 파이프라이닝은 컴퓨터의 명령어 실행을 다음과 같은 단계로 나누어 처리합니다.

`Instruction Fetch` 다음 실행할 명령어를 메모리에서 가져온다.

`Instruction Decode` 명령어를 해독하여 해당 명령어에 필요한 데이터를 레지스터에서 읽어온다.

`Execution` 명령어에 따라 연산을 수행한다.

`Operand Fetch` 메모리에 접근하여 데이터를 읽거나 쓴다.

`Write-back` 결과를 레지스터에 저장한다.

<br/>

![Untitled 1](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/c67317c5-9497-47e6-8ef7-d964b461ab56)


하지만 과정을 5개로 나누었다고 해서 속도가 5배 빨라지는 것은 아닙니다. 각 과정이 수행시간이 다르기 때문에, 불균등하게 나누어지기 때문이죠. 또한 전체 처리율이 좋아진 것이지, 위의 그림처럼 하나의 instruction의 수행시간은 오히려 느려질 수 있다.

<br/>

### RISC’s pipelining VS CISC’s pipelining

RISC 아키텍처에서는 대부분의 명령어가 32비트 명령어를 사용하지만, CISC 아키텍처에서는 각 명령어가 매우 복잡하며, 여러 동작을 수행할 수 있도록 설계되어 있기 때문에 명령어의 길이가 일반적으로 RISC 아키텍처보다 길고, 다양하기 때문에 한 cycle에 해당 명령어를 수행하지 못할 수도 있기 때문에, 파이프라이닝 기법을 적용하기 힘듭니다.

CISC 아키텍처에서는 명령어가 복잡하고 길기 때문에, 파이프라이닝이 RISC 아키텍처에 비해 구현이 복잡할 수 있습니다. 또한, 명령어의 길이가 다양하기 때문에 파이프라인에서 명령어를 처리할 때, 명령어 길이에 따라 처리하는 방법이 다르기 때문에 구현이 복잡해질 수 있습니다.

그러나, 현대의 CISC 프로세서에서는 파이프라이닝 기법을 사용하여 높은 처리량과 성능을 달성하고 있습니다. 파이프라이닝은 RISC 아키텍처에서만 사용할 수 있는 것이 아니며, 하드웨어의 발전으로 CISC 아키텍처에서도 효과적으로 사용할 수 있게 되었습니다.

<br/>

# 파이프라이닝 위험

그러나 파이프라이닝은 몇 가지 문제점도 가지고 있습니다.

<br/>


## Struct hazard(구조적 위험)

<img width="118" alt="Untitled" src="https://github.com/gkqkehs7/Computer-Structure/assets/77993709/0ddac550-77a7-49a8-be39-2482f121cb87">

구조적 위험은 여러 명령어가 동시에 같은 하드웨어 자원을 사용하려고 할 때 발생합니다. 예를들어 memory가 하나인데, 한 명령어는 instruction fetch를 위해서, 다른 명령어는 data fetch를 위해서 memory에 접근할 떄가 이 경우겠네요

위의 빨래 사진에서는 콘센트가 하나인데, 세탁기와 건조기가 둘다 콘센트를 사용하려 할때 문제가 발생하는 것이죠. 어떻게 해결할 수 있을까요? 당연히 콘센트를 하나더 늘리면 됩니다. 컴퓨터도 이와 같이 hardware resource를 추가하면 됩니다.

<br/>


## Data hazard(데이터 위험)

파이프라인에서는 명령어 실행 순서를 최적화하기 위해 명령어를 분리하여 여러 단계로 나누어 처리합니다. 이 때, 명령어의 결과가 다음 명령어에서 사용되기 전에 완전히 처리되지 않으면, 오류가 발생합니다.

<br/>

```tsx
add x19, x0, x1 // x0에 있는 값과 x1에 있는 값을 더해 x19에 저장
sub x2, x19, x3 // x3의 갑에서 x19의 값을 빼서 x2에 저장
```

예를 들어 이런 연산이 있다고 가정합시다.

<br/>


![Untitled 2](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/2f1f473b-95f6-4a5e-8fda-22ab0d5f6ee2)

위의 5단계 순서에 따르면 연산의 결과가 register에 저장되는건 5단계중 마지막입니다.

sub연산에서 register x19값을 가져오려면 적어도 3단계에서는 값이 들어가 있어야합니다.

최대한 버텨서 3단계에서 가지고 온다고 해서 두 cycle에 대한 bubble이 일어나고 이건 성능 저하의 요인이 됩니다.

<br/>

### Fowarding을 통한 해결

![Untitled 3](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/e6892aa8-1b97-4f4b-ba9f-2e5bd11835f1)

Forwarding은 이전 단계에서 계산한 결과를 바로 다음 단계로 전달하여 데이터 의존성에 따른 성능저하를 방지합니다. 위에선 연산 결과를 레지스터에 저장하기 전에 바로 forwarding으로 다음 연산의 입력으로 주는 것이죠.

일반적으로, 파이프라인에서 다음 단계에서 사용될 데이터는 이전 단계에서 계산이 완료되어야 합니다. 하지만 이전 단계에서 계산 결과가 다음 단계에서 필요한 경우, 데이터를 메모리에서 읽어오는 것보다 더 빠르게 처리하기 위해 Forwarding을 사용합니다.

ALU(산술 논리 장치)에서 처리한 결과를 레지스터에 저장하기 전에도 바로 다음 단계에서 사용할 수 있도록 전달하는 것입니다. 이전 단계에서 계산한 값을 레지스터 파일에서 읽어오는 것보다, ALU에서 직접 값을 전달함으로써 처리 속도를 높일 수 있습니다.

<br/>

### 근데 어떻게 fowarding을 컴퓨터는 인지할까?
  
<img width="725" alt="스크린샷 2023-07-10 오후 4 36 33" src="https://github.com/gkqkehs7/Computer-Structure/assets/77993709/ed569423-8bf4-40d4-a41f-2819b1836470">

위의 sub연산에선 x1에서 x3을빼서 x2에 저장하고, 다음 연산의 rs1이 x2를 필요로 합니다.

<br/>

```cpp
EX/MEM.RegistreRd == ID/EX.RegisterRs1
EX/MEM.RegistreRd == ID/EX.RegisterRs2
```
Excution과 OperationFetch사이를 EX/MEM.RegistreRd라고 하는 것처럼, 각 단계 사이 선을 지정한다고 하면, 
위와 같은 상황에서 forwarding이 일어난다고 판단할 수 있다.

<br/>

<img width="731" alt="스크린샷 2023-07-10 오후 4 37 16" src="https://github.com/gkqkehs7/Computer-Structure/assets/77993709/812e485e-eb21-4a63-a9f7-62e0e5a3963c">


위의 lw연산에서 0(x2)에서 가져온 값을 x1에 저장합니다.

<br/>

```cpp
MEM/WB.RegisterRd = ID/EX.RegisterRs1
MEM/WB.RegisterRd = ID/EX.RegisterRs2
```

위와 같은 상황에서 forwarding이 일어난다고 판단할 수 있습니다.

위의 공통점은 rd값을 사용하기 때문에 registerWrite값이 1이 된다. RISC-V 아키텍처에서 "x0" 레지스터는 항상 값이 0으로 고정되는 특별한 목적을 가지고 있습니다. 따라서 어셈블리에서 "rd"에 "x0"를 저장한다는 것은 명령어의 실행 결과를 레지스터에 저장하지 않고 버리는 것을 의미합니다. 즉, 명령어 실행 결과는 필요하지 않거나 사용되지 않는다는 것을 나타냅니다.

<br/>



```cpp
if(EX/MEM.RegWrite && EX/MEM.RegisterRd != 0) {
	if(EX/MEM.RegistreRd == ID/EX.RegisterRs1 || EX/MEM.RegistreRd == ID/EX.RegisterRs2) {
		// forwarding 발생!
	}
} 

if(MEM/WB.RegWrite && MEM/WB.RegisterRd != 0) {
	if(MEM/WB.RegisterRd = ID/EX.RegisterRs1 || MEM/WB.RegisterRd = ID/EX.RegisterRs2) {
	  // forwarding 발생!
	}
}
```
그래서 정리하면 위와 같은 코드가 됩니다. 근데 store나 branch명령어는 rd를 아예 사용하지 않습니다. 그럼 rd과 rs가 같을 일이 없을텐데 굳이 봐야할까요? rd에 쓰레기 값이 들어가 있을 수 있기 때문에 이때도 확인을 하여야 합니다.

<br/>

### code scheduling을 통한 해결

![Untitled 1](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/8f9e0a9d-8949-487f-b787-0faca749fcb7)

하지만 이렇게 load명령어 이후 계산 instruction이 온다면 메모리에 접근해서 데이터를 가져와야하므로 어쩔 수 없는 bubble이 하나 발생합니다. 이렇게 forwarding만으로는 bubble(아무것도 못하는 공백 상태)를 다 막을 순 없습니다. 

<br/>

![Untitled 4](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/a326f1a3-0f5a-4e5d-8fb0-9ceff4952682)
따라서 code의 실행 순서를 변경하여 이 공백을 줄일 수 있습니다. 두 방법을 적절히 사용하여 수행시간을 줄이면 됩니다.
<br/>

### Control Hazard(제어 위험)

제어 위험은 보통 Branch명령어를 수행할때 발생합니다. 기본적으로 `프로그램 카운터`는 현재 실행 중인 명령어의 다음 주소로 갱신됩니다. 하지만, 프로그램 실행 흐름이 바뀌어(Beq 같은 명령어 수행) 명령어가 실행면 **프로그램 카운터** 값이 갑작스럽게 변경되면서 (원래는 4씩 증가) 파이프라인에 미리 가지고 와서 처리중이였던 명령어들은 아무 쓸모가 없어집니다.

<br/>

![Drawing_2023-06-06_22 18 46 excalidraw](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/38fa55b5-d6ec-4495-897b-be16f15fc0fa)


아무튼 0x040 번지에 $1과 $2를 비교해 같으면 0x100번지로 Brach(jump)하는 코드가 있다고 합시다.

여기서 중요한 점은 Branch여부를 판별하는 시점 즉, beq에서 $1과 $2가 equal한지 검사하고 pc값을 setting하는 과정은 Memory Stage에서 완료됩니다.  0x040번지에서 branch 구문 beq가 던져졌고 branch여부가 판별되는 memory stage (CC4)이전에는 branch를 할지 안할지 CPU는 알 수가 없죠.

그래서 CPU는 평소 관행대로 address를 4byte씩 늘려가며 (0x044, 0x048, 0x04C ... ) 작업을 합니다.

이때, 대망의 memory stage(CC4)에서 $1과 $2가 같아 Branch를 해야한다고 판단을 내렸다고 한다면 어떻게 될까요?

<br/>

![Drawing_2023-06-06_22 18 46 excalidraw 1](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/3d4aecd6-e860-47b1-a235-bc9041b97338)

CPU가 branch를 할지 안할지 몰라 그냥 평소대로 진행하던 것은 모두 헛수고였습니다.

0x100번지에 있는 instruction을 fetch해야하는데 필요없는 0x044, 0x048.. 번지의 instruction을 fetch하고 있었다는 의미죠.

따라서 실제 branch가 이루어진다면 다음과 같이 잘못 fetch된 것들을 모두 폐기처분, flush를 시킵니다.

이렇게 **branch로 인해 발생하는 hazard**가 바로 **control hazard**이고 위에서 볼 수 있다시피 3-cycle 손해를 보게 됩니다. 

- branch 판별 시점을 앞당기기 ( 1 cycle만 손해보자! )

![Drawing_2023-06-06_22 18 46 excalidraw 2](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/7315f73e-5c8c-465b-af77-d99f329dfd36)

기존 Memory단계에서 Branch 판별 여부가 결정이 되었는데 이 시점을 Decode까지 땡길 수가 있습니다.

branch 명령어시에 Instruction Decode단계에 비교기와 주소계산기를 추가하여  Instruction Decode에 branch여부를 판단하면 어떨까?

비록 1-Cycle Flush가 일어나긴 하지만, 기존 3-Cycle Flush에 비해서 장족의 발전입니다.

<br/>

![Untitled 2](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/8fea654f-a2f1-49b4-9ec5-0f4fc498c503)

또한 두개의 register결과가 beq명령어에 필요하다면 1-Cycle Flush가 일어나게 된다.

<br/>

![Drawing_2023-06-06_22 50 26 excalidraw](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/52dad42d-d09f-419a-97e7-5018fe7d653b)
lw명령어는 어떨까? x1이 Operation Fetch단계에 저장되므로 다음 명령어에 x1을 사용한다면 2-Cycle Flush가 일어나게 된다.

그래도.. 1-Cycle의 손해도 안보는게 Performance적으로 좋겠죠.

그래서 0-Cycle 손해를 실현시키고자 나온 두번째 해결책이 `Branch Prediction`입니다.

<br/>

## branch predict을 통한 해결

### Static Branch Prediction

Static Branch Prediction은 beq와 같은 Branch 구문이 나오면 묻지도 따지지도 말고 branch해라! 혹은 branch 절대 하지마라! 라고 사전에 정의를 해 두는 겁니다.

예를 들어 branch 구문이 나올때, 아.묻.따 branch해라 라고 설정이 되어있다면

branch instruction이 나왔을 때, branch 판별 여부를 기다리지 않고 일단 해당 주소로 branch를 해버립니다.

그렇게 해서 실제로 branch가 일어나야 했으면 예측 성공이 되면서 0-cycle손해를 보는거고

만약 branch를 하지 말아야 했던 상황, 즉 예측 실패를 했다면 그때 flush하고 1-cycle 손해를 보는거죠.

<br/>

### Dynamic Branch Prediction

Dynamic Branch Prediction은 Static과 달리 Run time(프로그램 실행 중)에 Branch Prediction을 합니다.

쉽게 말하면 Branch History를 이용한다고 할 수 있는데요. Branch instruction이 나왔던 주소 값(PC), 그리고 실제 jump를 수행했는지 여부까지 branch prediction buffer(branch history table)에 저장을 합니다.

```tsx
while(true) {
	for(int i=0; i<5; i++) {
	...
	}
}
```

<br/>


### 1-Bit Prediction
    
![Untitled 3](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/a36f2fed-0c4f-4c91-a2c2-f77ab94c5e71)

1-Bit Prediction은 지난번에 history대로 prediction bit대로 진행하고 틀린다면 prediction bit를 뒤집는 방식입니다. 예를들어 저번에 branch이동을 했다면 이번에도 이동을 한다고 예측하고, 맞다면 다음 번에도 이동한다고 예측하고 틀린다면 다음번에는 이동하지 않다고 예측하는 것이죠.

<br/>

### 2-Bit Prediction
    
![Drawing_2023-06-06_23 03 06 excalidraw](https://github.com/gkqkehs7/Computer-Structure/assets/77993709/c3b035b9-b0ea-431a-b64f-e0247f47b972)

2-Bit Prediction은 1-Bit Prediction과 다르게 두 번 연속 예측이 실패했을 시에 다음 번 예측을 바꿉니다.
    
외에 다른 많은 Prediction방식들이 많습니다.
