---
date: '2025-10-25T19:10:46+09:00'
draft: false
title: 'Computer Network - Chap. 1'
tag: ['Computer Network']
summary: 컴퓨터 네트워크 1장 정리
---

## What's the Internet: "nuts and bolts" view

인터넷을 구성하고 있는 요소들은 다음과 같다.

### Network edge (LAN)

#### 호스트

- 엔드 시스템이라고 부른다.
- 인터넷의 가장자리에서 네트워크 앱을 가동하며, 네트워크 트래픽을 생성/소비한다. 

#### Access network

- 네트워크 가입자가 처음 접속하는 망을 의미한다.
- 어떤 집단의 네트워크인가에 따라 다른 기술이 사용된다. (아래에서 언급)
  - transmission rate
  - 공유된 / 할당된 라인을 사용하는지?

#### DC(Data Center) vs CDN(Content Delivery Network)

- **DC**
  - 대규모 시설
  - 데이터 저장 + 가입자 처리 및 운영 기능 수행
- **CDN**
  - 빠른 서비스를 목표로 전 세계에 서버를 분산
  - 원본의 컨텐츠를 서버에 저장함.

### Network core (WAN)

- 라우터들의 집합이다.
- 네트워크의 네트워크를 의미한다.

#### packet switches

두 개의 host 사이의 통신을 위해 packet을 전송하는 장치이다.    
**라우터**와 **스위치**로 나뉘어진다. 

**라우터** : 주로 L3에서 라우팅을 수행한다. 즉, 한 패킷이 목적지까지 도달할 수 있는 경로를 추론해 다른 라우터로 전송한다.

**스위치** : 주로 L2에서 forwarding을 수행한다. 즉, 현재 프레임을 다른 장치로 전달한다.

#### ISP vs NSP

- **ISP** : 인터넷 서비스를 제공하는 사업체이다.
- **NSP** : Network Infrastructure (link, router, ...)를 제공하는 사업체이다.
- 대한민국은 대부분의 ISP가 NSP도 함께 하고 있다.

### communication links

기기들을 물리적으로 연결하는 링크이다. fiber, copper, radio, satellite... 다양한 종류가 있다.  
이들의 전송 속도를 **bandwidth**라고 부른다.

> **bandwidth != throughput**  
> 후자는 기기 간의 end-to-end 전송 속도를 의미한다.

IoT는 low power & high connctivity가 핵심 요구사항이다.


### 프로토콜

- 두 소프트웨어 모듈 사이의 동작을 정의한 것이다.
  - 이 때, 두 모듈은 반드시 **같은 계층**의 모듈이다.
- 메세지의 전송, 수신을 관리한다.  
ex) Skype(L5), TCP(L4), IP(L3), WiFi(L2)...

### 인터넷 표준

- **IETF** : L3, 4, 5에 대한 표준을 만드는 기구이다.
- **RFC** : IETF에 이 형태로 안건이 올라가고, 확정이 되면 표준이 만들어진다.  

+) **IEEE** : L2의 표준을 만드는 기구이다.

## The Internet: a service view

위에서는 인터넷의 기초적인 구성에 대해서 살펴보았다.  
인터넷을 서비스를 제공하는 주체로 생각한다면 다음과 같이 볼 수 있다.

- 어플리케이션에 서비스를 제공하는 infrastructure
- 어플리케이션에 프로그래밍 인터페이스를 제공
  - app들이 전송, 수신과 같은 기능을 인터넷에 연결하여 사용할 수 있도록 한다.
  - 서비스 옵션 또한 제공하는데, 이를 **QoS**(Quality of Service)라고 한다.
    - No loss
    - Max latency
    - Min throughput...

## Access Network

- 인터넷 사용자 (source host)를 인터넷에 연결된 다른 host로 연결해주는 경로에서 첫 번째 ISP 네트워크를 벗어나는 첫 번째 라우터(= edge router)까지를 포함한 네트워크.
  - Residential network
  - Enterprise network
  - Mobile network

### xDSL

- **전화선**을 사용해 연결. 
- 각 집까지 CO(Central Office)까지 **전용 회선**을 사용함.
- ADSL은 집과 CO까지의 거리가 멀 수록 데이터 속도가 떨어짐.

### HFC

- **케이블 TV 네트워크**를 이용해 연결. 
- 여러 집이 coaxial cable의 속도를 공유
- CO쪽에는 fiber가 사용됨.

### FTTH (Fiber To The Home)

- 집으로 optical fiber가 직접 연결되는 기술.
- 한국에서는 집이 아니라, 관리사무소 또는 아파트 동까지만 들어오고 그 사이는 Ethernet을 사용하는 FTTB가 보편적임.
  - FTTB + Ethernet to Home -> 광랜
- ODN의 구조
  - **AON(Active Optical Network)** : OLT와 ONU 사이에 Ethernet switch를 사용해 연결. 추가적인 설비가 필요하나, 두 지점 사이를 point-to-point로 연결 가능.
  - **PON(Passive Optical Network)** : OLT에서 여러 ONU 사이에 splitter를 두어 point-to-multpoint로 연결. 추가적인 설비가 필요 없지만, 속도가 분산됨.

### 5G FWA (Fixed Wireless Access)

- 추가 설비 없이 5G 속도를 집 혹은 회사에서 누릴 수 있는 서비스.
- 유사 서비스로 OpenRoaming 기술이 있음 : 5G 가입자가 자연스럽게 Wifi6로 연결됨.

## Host: sends *packets* of data

- host가 data를 보내는 과정
  1. 어플리케이션 메세지를 **L bits의 packet**으로 나눔.
  2. access network로 **transmission rate R**의 속도로 전송.
- 긴 메세지 대신 짧은 패킷을 보내는 이유?
  1. 코어 NW에서 데이터 전송 효율을 향상시키기 위해 (WAN 관점)
     - Retx 효율 : 재전송 시 보낼 데이터의 수가 감소.
     - 한 메세지를 라우터 간 parallel하게 전송 : 메세지가 packet으로 나뉘어 동시에 여러 라우터를 통해 전송됨. 
     - -> e2e delay 감소
  2. LAN의 여러 유저가 link를 효율적으로 공유할 수 있음. (LAN 관점)
     - -> max delay 감소
- L-bit packet transmission delay(sec) = L(bits) / R(bits/sec)
  - 이 때, L은 MTU (Maximum Transition Unit)

### MTU란?

- fragmentation 없이 전송될 수 있는 데이터의 최대 용량
- header + payload의 용량임.
- 링크 종류에 따라 다르고, MTU가 크다고 해서 **속도가 빠른 것은 아님.**
  - 속도를 결정하는 것은 bandwidth.

## Circuit Switching

- 통신 전 **e2e 자원 확보**가 이루어지고 연결 중에도 그 자원이 유지됨.
- 전화에서 주로 사용됨. (Loss가 일어나면 안 되는 network)

### FDM & TDM & Statistical Time Division Multiplexing

- **FDM (Frequency Division Multiplexing)**
  - 사용하는 대역폭에 따라 유저를 분배.
- **TDM (Time Division Multiplexing)**
  - 시간에 따라 유저를 분배.
- **Statistical Time Division Multiplexing**
  - 위 둘은 Circuit Switching에서 사용.
  - 이 경우는 Packet Switching에서 사용하는데, 수요가 일정하지 않은 그 성질을 반영한 것임.

## Packet Switching

### 기본 동작

1. L bits를 input buffer에 저장
2. 어느 output link를 통해 나가야 하는지 forwarding table을 확인
3. L-bit packet을 input buffer에서 output buffer로 switching
4. output buffer에서 대기 (queueing delay)
5. 전송 (transmission delay, L/R)

### Routing vs Forwarding

- **Routing** : global action 
  - source로부터 dest에 도착하기 위한 router 간의 e2e path를 고려.
- **Forwarding** : local action
  - 라우터 **하나**를 통해 한 포트에서 다른 포트로 전송하는 것.

## Packet Switching vs Circuit Switching

- 만약 1Gbps link에 접속자들이 10%의 확률로 100Mbps를 전송한다면?
  - Packet Switching 유리
  - 10명보다 많은 유저가 active할 확률이 매우 낮기 때문.
  - Circuit은 언제나 10명의 유저만 가능.
- 하지만, 같은 조건에서 100%의 확률이라면?
  - Circuit Switching 유리
  - Packet Switching도 가능하지만, hop마다 지연이 발생하므로 전체 시간이 느려짐.

|Packet Switching|Circuit Switching|
|--|--|
|No Call set-up|Call set-up|
|No resource reservation, 수요에 따라 할당|수요와 상관 없이 항상 배정|
|Packet delay/loss 발생|항상 보장된 서비스 제공|
|Queueing delay|Call set-up delay|
|많은 유저들 사용 가능, 단 고정적이지 않아야 함|유저의 수가 항상 제한됨|
|간헐적인, bursty한 데이터|보장된 대역폭이 필요한 데이터|
|대역폭의 유동적 사용|고정적 사용|

## Internet structure

- ISP 간의 연결로 인터넷이 만들어짐.
- **IXP** : 여러 ISP가 서로 데이터를 교환할 수 있는 지점.
- **peer** : 규모가 비슷한 ISP끼리 비용 절감을 위해 서로 직접 연결하고 데이터를 주고 받는 것.
- **PoP(Point of Presence)** : provider ISP에 customer ISP가 연결할 수 있는 라우터
- **Multi-home** : 여러 provider ISP를 선택하는 것 -> 한 쪽이 만약 연결이 되지 않더라도 다른 쪽으로 연결하면 되므로 reliable함.

### CDN

- 유저와 가까운 곳에 캐시 서버를 둠으로써 유저가 느끼는 latency를 감소시킴.
- content 사업자가 network 사업자에게 통신을 위해 지불하는 **outbound traffic 사용 비용을 줄이고** 자사가 구축한 망을 통해 사용자와 **보다 가까운 위치에서 저렴하게 content를 전송.**

## Performance: Loss, Delay, Throughput

### Loss

- Packet Switching : output queue의 데이터가 용량을 초과했을 때 발생 - **packet loss**

### Delay

- processing delay -> queueing delay -> transmission delay -> propagation delay의 순서로 진행.
- **TI(Traffic Intensity)** : output queue에서의 queueing delay를 측정하는 지표.
  - (arrival rate into output queue) / (departing rate from output queue)
  - 값이 1에 가까워질수록 queueing delay가 급격하게 증가.
- 실제로 측정하는 방법 : **traceroute**
  1. ttl value를 i로 두고 i번 router에 packet 3번 전송
  2. router i로부터 packet을 전송받음. (ICMP time exceeded message / ICMP Port Unreachable message)
  3. 이 때 경과된 시간을 측정함.

### Throughput

- 두 호스트 간 전송되는 비트의 양을 의미함.
- 특정 시점동안 실제로 측정되는 개념.
- 두 host 사이의 path의 bottleneck link를 통해 결정됨.

- **transmission rate**? : *한 링크*의 용량을 의미, e2e가 아님, 실제 측정도 X.

## Protocol layers

- 전체 네트워크의 동작의 **복잡**하기 때문에 layering을 함.
- encapsulation 개념.
- 법칙을 위반하는 경우도 있음: TCP, NAT...

## Network security

- **Virus** : 기생해서 실행, 사용자 개입 필요, 컴퓨터 내 공격
- **Worm** : 독자적으로 실행, 사용자 개입 필요 없음, 네트워크 공격
- **Sniffing** : 지나가는 traffic을 몰래 훔쳐보는 것.
- **IP spoofing** : 잘못된 source address의 packet을 전송하는 것.
- **Zero-day attack** : 소프트웨어 출시 후 보안 취약점을 개선한 patch가 나오기 전 공격.

## History of the Internet

- ARPAnet -> ALOHAnet -> NSFnet -> Internet
- Cerf와 Kahn의 4법칙
  1. **minimalism between ISPs, autonomy within ISP** : ISP들이 각 네트워크를 자치적으로 관리함.
  2. **No guarenteed service **: 라우터는 어떤 서비스도 보장하지 않으며 fast delivery에만 집중.
  3. **No record in routers (stateless routing)** : 라우터는 처리한 패킷에 대한 어떠한 정보도 기록/사용하지 않음.
  4. **No centralized control** : 각 라우터들이 어떤 중앙의 제어를 받지 않고 독자적으로 라우팅을 결정함.