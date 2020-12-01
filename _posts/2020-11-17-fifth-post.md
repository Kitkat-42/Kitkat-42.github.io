---
layout: single
title: "[Netwhat] 프로젝트 소개, 개념 및 용어 정리"
categories: "Netwhat"
tags: [Netwhat, C]
---

# 프로젝트 소개

Netwhat은 네트워크 문제들에 대한 도입 프로젝트이다.  
이 프로젝트를 하면서 우리가 이미 일상에서 써왔던 것들이 어떻게 작동하는지 이해할 수 있게될 것이다.

# Mandatory Part

아래의 개념들에 대해 먼저 알아야 한다.

◦ What is an **IP address**  
◦ What is a **Netmask**  
◦ What is the **subnet** of an IP with Netmask  
◦ What is the **broadcast address** of a subnet  
◦ What are the different ways to represent an ip address with the Netmask  
◦ What are the differences between **public and private IPs**  
◦ What is a **class** of IP addresses  
◦ What is **TCP**  
◦ What is **UDP**  
◦ What are the **network layers**  
◦ What is the **OSI model**  
◦ What is a **DHCP server** and the **DHCP protocol**  
◦ What is a **DNS server** and the **DNS protocol**  
◦ What are the rules to make 2 devices communicate using IP addresses  
◦ How does **routing** work with IP  
◦ What is a default **gateway** for routing  
◦ What is a **port** from an IP point of view and what is it used for when connecting to another device

## 1. IP 주소(IP address)

> IP 주소(Internet Protocol address, IP address)란?

네트워킹이 가능한 장치들이 서로를 인식하고 통신을 하기 위해서 사용하는 특수한 번호이다.  
즉 PC뿐만 아니라 서버장비, 스마트폰, 태블릿PC, 인터넷이 가능한 전자사전 등 인터넷에 연결되는 모든 장치들도 IP주소를 갖고있다.

네트워크 상에서 통신을 하기 위해서는 몇가지 통신규약(Protocol)을 따라야 하는데, 그러한 규약들 중에는 "네트워킹을 하는 장비들에게 고유한 숫자를 부여해서 그 주소로 통신할 상대를 구분하도록 하자!"라는 의미를 가진 규약이 존재한다.  
위의 규약에서 말하는 고유한 주소가 바로 IP 주소인 것이다.

오늘날 주로 사용되고 있는 IP 주소는 IP 버전 4(IPv4) 주소이나 스마트폰, 노트북, 인터넷tv 등등 1인당 4~5개의 네트워크가 가능한 장비들을 소유하게 됨에 따라 이 주소가 고갈되어 길이를 늘린 IP 버전 6(IPv6) 주소가 점점 널리 사용되는 추세이다.

### 1) IPv4 주소 vs IPv6 주소

#### **IPv4 주소**

32비트 범위이다.  
 0~255 사이의 십진수 4개를 쓰고 . 으로 구분해 나타낸다.  
 즉 0.0.0.0부터 255.255.255.255 까지가 된다. 약 40억개의 주소가 존재할 수 있도록 설계되었다.  
 중간의 일부 번호들은 특별한 용도를 위해 예약되어 있는데, 예를 들면 127.0.0.1은 로컬호스트(localhost)로 자기 자신을 가리킨다.  
 위 주소 체계는 내부적으로 32비트로 처리된다. 123.123.123.123을 예로 들면 1111011.1111011.1111011.1111011 로 표시된다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile2.uf.tistory.com%2Fimage%2F995A2E405BF015D60813BE" />

> IP 주소는 대역에 따라 A, B, C, D, E 등의 클래스로 나누어질 수 있다.  
> 클래스 A: 대규모 네트워크 환경용  
> 클래스 B: 중규모 네트워크 환경용  
> 클래스 C: 소규모 네트워크 환경용  
> 클래스 D: 멀티캐스트용  
> 클래스 E: 연구/개발용 IP 주소 혹은 미래에 사용하기 위해 남겨놓은 것

또한 각 클래스 안에서 IP는 `Network ID`와 `Host ID`로 구분할 수 있다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile7.uf.tistory.com%2Fimage%2F990E82425BF017D307CA56" />

> 클래스 내의 Network, Host ID 구분

#### **IPv6 주소**

128비트 범위이다.  
 두자리 16진수 여덟 개를 쓰고 각각을 : 기호로 구분한다. (예: 21DA:00D3:0000:2F3B:02AA:00FF:FE28:9C5A)  
 2의 128제곱개의 주소가 존재할 수 있다.  
 IPv4에 비해서 보안적으로도 뛰어나지만 상용화되려면 기존의 네트워크 장비들을 모두 교체해야 하는 등 문제가 있어 아직은 다른 대안들을 이용하고 있다고 한다.

### 할당 방식에 따른 구분: 유동 IP vs 고정 IP?

- `유동 IP`  
  유동 IP방식은 skt, kt, lg 등과 같은 ISP업체에서 낭비되는 IP가 최소화될 수 있도록 고객들의 IP 주소를 관리하는 방법이다.  
  IP주소를 특정하여 주는 것이 아니라, `일정한 주기로 또는 접속이 바뀔 때마다 매번 새로운 IP를 주는 것`이다.

- `고정 IP`  
  기관, 회사, 사무실 또는 개인이 서버를 운영하는 등의 목적으로 고정 IP가 불가피하게 필요한 경우들이 있다.  
  이러한 경우에는 ISP업체로부터 고정 IP를 구매하여 이용하기도 한다.

> Mac OS에서 내 IP 주소를 보는 방법

`ipconfig getifaddr en0`  
를 터미널에 입력하면 나의 IP 주소를 확인할 수 있다.

![img](/assets/images/IP.png)

## 2. 넷마스크(Netmask)

> 넷마스크(Netmask)란?

네트워크 주소 부분의 비트를 1로 치환한 것이 넷마스크이다.
IP 주소와 넷마스크를 AND연산을 하면 네트워크 주소를 얻을 수 있다.
일반적으로 사용되는 넷마스크는 255.255.255.0 이다.
호스트 컴퓨터의 주소 부분만 IPv4 IP 주소 뒤에 남기거나 필터링하기 위한 문자열이다.

## 3. 서브넷(Subnet)

> IPv4 서브넷 구하기

Pv4는 초기에 클래스로 나누어서 할당을 방법을 택했습니다. 하지만 이 방식은 크게 비효율적이었습니다. 예로들어 클래스 B를 어느 중소기업체에게 할당했을 경우 65000여개의 아이피를 다 쓰는 것이 아닌 10000개 정도만 쓴다고 가정합시다. 그러나 10000개가 아닌 나머지 50000여개의 IP는 쓰이지 않은 채 이 기업체는 클래스 B의 하나를 점유하고 있는 상태가 되죠. 그렇다고 이 기업체에게 클래스 C를 IP를 할당하자니 IP자원이 너무 부족하게 되구요.

이러한 문제를 해결하기 위해 IP를 사용하는 네트워크 장치 수에 따라 효율적으로 사용할 수 있는 서브넷(Subnet)이 등장하게 되었습니다.

출처: https://engkimbs.tistory.com/622?category=688997 [새로비]

서브넷은 말 그대로 부분망이라는 뜻으로, IP주소에서 네트워크 영역을 부분적으로 나눈 부분망, 부분 네트워크를 말한다. 그리고 이 서브넷을 만들 때 쓰이는 것이 **서브넷 마스크**이다. 이 서브넷 마스크를 이용하여 IP주소 체계의 Network ID와 Host ID를 분리할 수 있다.

## 4.
