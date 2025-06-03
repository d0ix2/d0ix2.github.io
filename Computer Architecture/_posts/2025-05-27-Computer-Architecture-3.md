---
title: "[컴퓨터아키텍처] 4주차 수업 내용 정리"
excerpt: "논리회로, 릴레이, CMOS에 대해 정리하였습니다."

tags: 
  - [Ocha, Computer Architecture]

date: 2025-05-27
last_modified_at: 2025-05-27

toc: true
toc_sticky: true
---

<br/>

## 논리회로의 표현 방법

- 게이트 회로: AND 또는 OR과 같은 논리함수를 표현한 회로
	- 게이트를 열었다, 닫았다 하는 것으로부터 유래한 이름
- 게이트 회로는 어떻게 만들어지는가?
- 게이트 회로의 '지각' 또는 '소비전력'은 무엇을 가리키는가?
	- 왜 물리적 실체가 없는 정보를 다루는 데 시간과 전력이 들어가는가?

<br/>

## 논리회로의 작성법

- 아래 각각의 방식을 사용해 구성한 회로를 설명
	1. 릴레이
	2. CMOS
- 이러한 논리적인 동작은 사실 거의 동일하게 구성할 수 있음
	- 릴레이 쪽이 보다 직관적이므로 릴레이를 먼저 설명
- 주의
	- 이 자료에서 설명하는 N형/P형을 사용한 방식은 이후 CMOS의 설명을 이해하기 쉽게 하기 위함
	- 실제 사용되었던 릴레이 계산기에서는 조금 더 효율적인 다른 방식이 사용되었음

<br/>

## 릴레이

- 전기적으로 동작하는 스위치

	![relay-01](/attachments/computer-architecture-3-01.png)

- 위의 그림에서,
	1. 평소에는 전극 부분이 스프링(용수철) 구조로 되어 있어 OFF 상태
	2. G–B 간에 전압을 걸면,
	3. 코일이 자석이 되어 위쪽 금속을 끌어당기고,
	4. S에 연결된 단자가 오른쪽으로 밀려나며,
	5. S와 D가 연결되어 ON 상태가 됨
- 알아보기 어려우므로, 앞으로는 아래의 그림과 같이 나타내며 'G~B 사이에 전압을 걸면 S~D가 ON'과 같이 표기

	![relay-02](/attachments/computer-architecture-3-02.png)

### VDD과 GND

- VDD: 고전위(높은 전압), 전류를 공급하는 플러스극에 해당
- GND: 저전위(낮은 전압, 보통 0V), 전류가 빠져나가는 마이너스극에 해당

- 그림의 위쪽에 VDD를, 그림의 아래쪽에 GND를 그림
- VDD와 GND가 그림에 존재한다면, 그림에는 표시되어 있지 않지만 어딘가에서 VDD와 GND에 전원이 연결되어 있다고 생각할 것

- 고전위를 1, 저전위를 0, 개방 상태를 Z로 정의한다
	- VDD에 직접 연결된 신호선의 전위는 1
	- GND에 직접 연결된 신호선의 전위는 0
	- 어디에도 연결되어 있지 않은 신호선의 전위는 Z
- 이때 Z는 고임피던스 High Impedance 상태를 의미하며, 회로상에서 해당 선이 입출력을 하지 않고 떠 있는 상태를 나타냄

	![vdd-gnd](/attachments/computer-architecture-3-03.png)

- VDD와 GND 양쪽을 동시에 직접 연결하는 일은 없음
	- 실제 연결할 경우 VDD에서 GND로 큰 전류가 흘러 회로가 망가짐 -> 이를 쇼트(단락 短絡)라고 함
	- 이러한 일이 발생하지 않도록 회로를 설계할 필요가 있음

<br/>

### 릴레이를 이용한 논리회로 작성

- 아래의 두 가지 연결 방법을 이용해 릴레이로 논리 게이트를 작성함
	- N형
	- P형

- **N형 릴레이**
	- N은 Negative의 약자
	- 아래 그림과 같이 B를 GND에 연결함
		- G가 저전위(= 0) -> 스위치 OFF
			- G~B 사이에 전압차가 존재하지 않기 때문에 코일이 움직이지 않음
		- G가 고전위(= 1) -> 스위치 ON
			- G~B 사이에 전압차가 존재하기 때문에 코일이 동작
		- 즉 G가 0일 때 OFF, G가 1일 때 ON

	![relay-n](/attachments/computer-architecture-3-04.png)

- **P형 릴레이**
	- P는 Positive의 약자
	- 아래 그림과 같이 B를 VDD에 연결함
		- G가 입력, D가 출력
		- 주의
			- N형과는 B와 G, S와 D가 상하 반대로 되어 있음
			- N형의 경우에는 B가 GND에 연결되어 있음
		- G가 저전위(= 0) -> 스위치 OFF
			- B~G 사이에 전압차가 존재하기 때문에 코일이 동작
		- G가 고전위(= 1) -> 스위치 ON
			- B~G 사이에 전압차가 존재하지 않기 때문에 코일이 움직이지 않음
		- 즉 G가 0일 때 ON, G가 1일 때 OFF

	![relay-p](/attachments/computer-architecture-3-05.png)

- P형과 N형의 차이 
	- P형에서는 G가 0일 때 ON, N형에서는 G가 1일 때 ON으로 서로 반대
	- G에 흰 동그라미가 붙어 있는 쪽이 P형, 없는 쪽이 N형
		- 논리회로에서 흰 동그라미는 반전을 나타냄
			- 흰 동그라미가 없는 N형은 1일 때 ON, 0일 때 OFF
			- 흰 동그라미가 있으면 해당 지점에서 반전되므로, P형은 0일 때 ON

	![relay-n-p](/attachments/computer-architecture-3-06.png)

<br/>

### NOT 게이트

- P형과 N형을 연결해 아래와 같이 NOT 게이트를 작성하는 것이 가능함

	![not-gate](/attachments/computer-architecture-3-07.png)

- 두 스위치는 반드시 반대 동작을 하게 됨
	- P형은 입력이 0일 때 ON
	- N형은 입력이 1일 때 ON

<br/>

### NAND 게이트

- NAND는 AND 결과에 NOT을 취한 것
- NAND는 AND 뒤에 흰 동그라미(반전)가 있는 형태로 그려짐
- 아래의 그림과 같이 NAND 게이트를 작성할 수 있음
	- 위쪽의 P형 두 개는 병렬 연결되어 있음
		- a와 b 중 한쪽 입력이라도 0이 들어오면 VDD에 c가 연결됨
	- 아래쪽의 N형 두 개는 직렬 연결되어 있음
		- a와 b 양쪽 입력 모두에 1이 들어오면 두 개 모두 ON이 되어 GND가 c에 연결됨

	![nand-gate-01](/attachments/computer-architecture-3-08.png)

	![nand-gate-02](/attachments/computer-architecture-3-09.png)

- NOT과 NAND와 AND와 OR을 작성할 수 있음
	- AND의 입력과 출력에 NOT을 붙이면 OR을 만들 수 있음
		- 드모르간의 정리: `NOT(a OR b) = NOT(a) AND NOT(b)`
		- 양변에 NOT을 적용하면: `a OR b = NOT(NOT(a) AND NOT(b))`
	- NAND의 두 입력에 같은 값을 넣으면 NOT이 됨

<br/>

### 릴레이를 이용해 컴퓨터를 만들기

- 릴레이를 사용한 NOT, AND, OR의 구성 방법
	1. 릴레이를 사용한 P형과 N형 릴레이의 제작
	2. P형과 N형을 조합한 NOT과 NAND의 제작
	3. NOT과 NAND를 조합한 AND와 OR의 제작

- 컴퓨터 제작 절차 (이전 강의 내용)
	1. NOT, AND, OR을 이용한 임의의 논리 함수 및 기억 회로의 구성 -> 임의의 조합 회로 및 순차 회로의 구성 가능
	2. 임의의 조합 회로 및 순차 회로를 이용한 CPU 부품의 제작 -> PC, 레지스터, FU, 메모리 등의 구성
	3. 해당 부품들의 조합을 통한 컴퓨터의 구성

- 컴퓨터를 통해 가능한 프로그램 실행
	1. 컴퓨터의 기계어 해석 및 실행 기능
	2. 기계어로의 어셈블리어 변환 가능성
	3. 어셈블리어로의 C 언어 컴파일 가능성

- 중간에 여러 단계가 존재하지만, 요약하면 릴레이만 있으면 C 언어 프로그램의 실행이 가능함
- 어떠한 입력에 따라 ON/OFF 가능한 스위치만 있어도 그것으로 프로그램 실행이 가능한 컴퓨터의 제작이 가능함
- 구체적인 회로 구성 방식은 회로의 종류에 따라 상이할 수 있음
- NAND만 만들 수 있다면 나머지는 모두 가능함

<br/>

## CMOS

- MOS: metal-oxide-semiconductor (금속-산화막-반도체)  
	- 위에서부터 금속, 산화막, 반도체가 샌드위치 구조를 이룸  
	- 산화막을 사이에 두고 평행판 콘덴서가 구성됨

- 전계 효과 트랜지스터 (FET: Field-Effect Transistor)  
	- 전계를 이용해 전자를 움직여 스위칭을 수행함
		1. G(게이트)에 전압을 걸어 콘덴서에 충전
		2. 산화막 아래에 전자의 층(채널)이 형성되어, S(소스)와 D(드레인)가 연결되어 ON 상태가 됨

- MOS에는 NMOS와 PMOS 두 가지가 존재
	- Negative(N)
		- 전자가 캐리어
		- 저전위만 흐를 수 있음
		- GND(접지) 쪽에 배치됨
		- 게이트를 1로 하면 ON이 됨
	- Positive(P)
		- 정공 正孔(hole)이 캐리어
		- 고전위만 흐를 수 있음
		- VDD(전원) 쪽에 배치됨
		- 게이트에 O 표시가 존재
		- 게이트를 0으로 하면 ON이 됨

	![nand-gate](/attachments/computer-architecture-3-10.png)

- CMOS: Complementary Metal-Oxide-Semiconductor
	- CMOS는 NMOS와 PMOS의 두 종류 트랜지스터로 구성됨  
		- 전계를 이용한 전하(전자/정공)의 이동으로 ON/OFF 제어 가능함  
	- 기본적으로 N형/P형 릴레이와 거의 동일한 동작 방식  
		- 즉, 동일한 방식으로 논리회로를 구성 가능함  
	- 현대의 컴퓨터는 이 CMOS를 사용한 제작 방식

	- NOT 게이트의 예시
		- 𝑖가 0(저전위)인 경우  
			- PMOS(윗쪽)는 on = VDD와 연결됨  
			- NMOS(아랫쪽)는 off 상태  
		- 출력은 1(고전위)이 됨

	![cmos-not-gate](/attachments/computer-architecture-3-11.png)

	- NAND 게이트의 예시
		- 𝑎와 b가 모두 1(고전위)인 경우  
			- PMOS(윗쪽)는 off 상태  
			- NMOS(아랫쪽)는 둘 다 on = GND와 연결됨  
		- 출력은 0(저전위)이 됨

	![cmos-nand-gate](/attachments/computer-architecture-3-12.png)

	- 정리
		- 모든 논리 게이트는 NMOS/PMOS의 조합을 이용해 구성할 수 있음
		- 릴레이와 동일함