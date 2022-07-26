# Section 2.1. Introduction
네트워크란 ****노드와 링크가 서로 연결되어 있거나 연결되어 있지 않은 집합체****이다.   
**노드는 서버, 라우터, 스위치 등의 네트워크 장비**를 의미하고
**링크는 유선 또는 무선**을 의미한다.    
구성할 수 있는 네트워크 토폴로지를 알아보고, 그에 따른 장단점과 네트워크 성능 분석을 위한 명령어를 알아본다.
    
# 네트워크
## 트리 
계층형 구조를 가진 네트워크이다.   
장점 : 노드의 추가, 삭제가 쉽다.   
단점 : 특정 노드에 트래픽이 집중 시 하위 노드에 영향을 미칠 수 있다.

## 버스
중앙 회선 하나에 여러 개의 노드를 연결하고, 회선을 공유하는 방식이고, LAN에 주로 사용한다.    
장점 : 회선을 공유하기 때문에 설치 비용이 적고 노드를 추가하거나 삭제가 쉽다.    
단점 : 스푸핑 공격을 받을 수 있다.

## 스타
중앙 노드에 모든 노드가 연결되는 방식이다.    
장점 : 노드 추가가 쉽고, 어느 노드에 장애가 발생해도 에러가 쉽게 발견된다. 패킷 충돌 발생 가능성이 적다.     
단점 : 중앙 노드에 장애가 발생하면 전체가 문제가 된다.

## 링형
각 노드가 양 노드와 연결되어 고리형태의 망 구성이다.    
장점 : 노드 수가 증가 되어도 네트워크 손실이 거의 없고 충돌이나 고장 발견이 쉽다.    
단점 : 네트워크 구성 변경이 어렵고, 회선에 장애가 발생 시 전체에 영향이 간다.

## 메시
모든 노드가 그물망처럼 서로 연결된 구조이다.
장점 : 트래픽 분산 처리, 한 노드가 고장나도 다른 노드에서 통신이 가능하다.
단점 : 노드 추가 시 구축 비용이 고가고 어렵다.

### 스푸핑이란?
LAN에서 송신부 패킷을 송신과 관련없는 다른 호스트에
가지 않도록 스위칭 기능을 마비 시킨다.

# 네트워크 성능 분석 명령어
## ping
대상 노드와 통신이 가능한지 알아보기 위해 많이 쓰이는 명령어다.
**ICMP 프로토콜을 지원하지 않는 단말은 쓸 수 없다.**

### ICMP란?
- TCP/IP 4계층 중 인터넷 계층에 속한다.    
 -TCP/IP의 IP 계층에서 추가적으로 필요한 기능들을 수행하기위한 프로토콜로, 
IP 패킷을 처리할 때 발생되는 문제를 알리거나, 문제의 진단 등을 수행한다.
-IP(Internet Protocol)와 하나의 쌍을 이루며 동작한다.

## netstat
접속된 서비스들의 네트워크 상태를 보여준다. 주로 특정 서비스의 포트가 열려있는지 확인할 때 쓴다.
아래와 같이 뜬다.
왼쪽부터 **프로토콜, 내 ip 주소:포트, 상대의 ip 주소:포트, 상태**가 뜬다.    
![](./../../../assets/images/2022-08-04-network_basic_images/1659708664120.png)

## nslookup
도메인과 매핑된 IP를 확인하는 명령어다.   
![](./../../../assets/images/2022-08-04-network_basic_images/1659708800024.png)

## tracert
목적지 노드까지 경로를 확인하는 명령어다. 응답 시간이 어느 구간에서 느려지는 지 확인하여 병목현상을 줄일 때 도움이 될 수도 있겠다.

![](./../../../assets/images/2022-08-04-network_basic_images/1659708932270.png)

# 질문


# Reference
면접을 위한 CS 전공지식 노트 - 주홍철 지음      
[ICMP 정의](http://www.ktword.co.kr/test/view/view.php?m_temp1=94)


# Section 2.2. Introduction
인터넷 프로토콜 스위트(Internet Protocol Suite)는 인터넷에서 컴퓨터들이 서로 정보를 주고받는 데 쓰이는 프로토콜의 집합이다.    
보통 TCP/IP 4계층을 많이 쓰기 때문에, TCP/IP 4계층으로 이 프로토콜 집합을 설명한다. 
아래는 OSI 7 계층과 TCP/IP 4계층의 비교 이미지이다.

![](./../../../assets/images/2022-08-05-TCP_IP_images/1659709596059.png)    

참고로 OSI 7 계층에서 각 계층에 포함되는 프로토콜 (또는 장치, 신호)는 다음과 같다.
위키백과를 참고했다. 

![](./../../../assets/images/2022-08-05-TCP_IP_images/1659709911728.png)

- **각 계층들은 다른 계층이 변경되어도 영향을 받지 않도록 설계된다**는 점에서 구별된다.

여기서는 TCP/IP 계층 위주로 프로토콜을 설명한다.

# TCP/IP 4계층 설명
## 애플리케이션 계층
응용 프로그램이 사용되는 프로토콜 계층이다. 실질적으로 사람들에게 제공하는 층이다.
- FTP, HTTP, SSH, SMTP, DNS 등 네트워크 관련 종사자가 아닌 일반 인터넷 사용자들이 주로 사용하는 프로토콜 들이 포진되어 있다.

## 전송 계층
송신자와 수신자를 연결하기 위한 통신서비스를 제공한다.    
즉, 컨텐츠 자체보다는 데이터 스트림 지원, 신뢰성, 흐름제어를 통해 단말간 중계 역할을 하는 계층이다.    
대표적으로 TCP, UDP가 있다.
### TCP : Transmission Control Protocol
패킷 사이의 **순서를 보장**하고, 연결지향 프로토콜을 사용한다. **가상회선 패킷 교환 방식**을 사용한다.    

### 가상회선 교환 방식
![](./../../../assets/images/2022-08-05-TCP_IP_images/1659711181708.png)
- 각 패킷마다 가상회선 식별자를 가지며, 이 식별자로 순서대로 도착하는 것을 보장하므로 오류 제어가 쉽다.
- 가상회선을 결정하는 Call setup 이라는 과정이 필요하며, 이로 인한 오버헤드로 인해 소량의 데이터만 옮길 경우에는 부적합하다.
- 단, Call setup은 첫 패킷이 도착할 때 한 번 결정되면 되며 각 노드에서 처리 시간은 데이터 그램보다 적다.
- 따라서 대량의 데이터를 옮긴다면 오히려 효율적이다.

### UDP : User Datagram Protocol
패킷 사이 **순서를 보장하지 않고**, 수신 여부를 확인하지 않으며, 단순히 대이터만 주는 **데이터그램 패킷 교환 방식**을 사용한다.
### 데이터그램 패킷 교환 방식
![](./../../../assets/images/2022-08-05-TCP_IP_images/1659711169412.png)
- 가상회선과 달리 여러 경로로 패킷을 보낼 수 있으므로, 망 상황에 따라 유연하게 패킷 교환을 할 수 있다.
- Call setup 과정이 필요없어 오버헤드가 적다.
- 
## 인터넷 계층
장치로 부터 받은 네트워크 패킷을 IP 주소로 지정된 목적지로 전송하기 위해 사용된다.
IP, ARP, ICMP 등이 잇고 패킷을 수신해야 할 상대의 주소를 지정해 데이터를 저장한다.

## 링크 계층
전선, 광섬유, 무선 등 실질적으로 데이터를 전달하며 장치 간에 신호를 주고받는 규칙을 정하는 계층이다.

# 캡슐화와 비캡슐화
## 캡슐화
상위 계층의 헤더와 데이터를 하위 계층의 데이터에 포함시키고, 하위 계층에 헤더를 포함시키는 것이다.
예를 들면 애플리케이션 계층 데이터가 전송 계층으로 전달될 때, 전송 계층의 헤더와 데이터가 포함된다.   
**애플리케이션에서 전송 계층**으로 데이터가 전달되면 **세그먼트** 또는 데이터 그램화라고 하며, **인터넷 계층으로 가면 패킷화**, **링크 계층으로 가면 프레임화** 된다고 표현한다.
![](./../../../assets/images/2022-08-05-TCP_IP_images/1660391321913.png)


## 비캡슐화
비캡슐화는 캡슐화 과정을 반대로 거치는 것으로 각 계층의 헤더부를 제거 하는 것이다.

# Protocol Data Unit (PDU) 
계층 간 데이터 이동 시, 데이터 덩어리의 단위를 PDU라고 한다.
PDU는 아래와 같이 계층마다 다르게 부른다.
- 애플리케이션 계층 : 메시지
- 전송 계층 : 세그먼트(TCP), 데이터그램(UDP)
- 인터넷 계층 : 패킷(IP)
- 링크 계층 : 프레임(데이터링크 계층), 비트(물리 계층)

# Reference
면접을 위한 CS 전공지식 노트 - 주홍철 지음    
[TCP/IP 모델 위키 백과](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C_%EC%8A%A4%EC%9C%84%ED%8A%B8)    
[패킷 교환 방식 종류](https://ko.wikipedia