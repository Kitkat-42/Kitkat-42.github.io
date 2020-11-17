---
layout: single
title: "[Netwhat] 프로젝트 소개, 개념 및 용어 정리"
categories: "Netwhat"
tags: [Netwhat, C]
---

## 1. 프로젝트 소개

Netwhat은 네트워크 문제들에 대한 도입 프로젝트이다.  
이 프로젝트를 하면서 우리가 이미 일상에서 써왔던 것들이 어떻게 작동하는지 이해할 수 있게될 것이다.

## 2. Mandatory Part

아래의 개념들에 대해 먼저 알아야 한다.

◦ What is an **IP address**  
◦ What is a **Netmask**  
◦ What is the **subnet** of an IP with Netmask  
◦ What is the broadcast address of a subnet  
◦ What are the different ways to represent an ip address with the Netmask  
◦ What are the differences between public and private IPs  
◦ What is a class of IP addresses  
◦ What is TCP  
◦ What is UDP  
◦ What are the network layers  
◦ What is the OSI model  
◦ What is a DHCP server and the DHCP protocol  
◦ What is a DNS server and the DNS protocol  
◦ What are the rules to make 2 devices communicate using IP addresses  
◦ How does routing work with IP  
◦ What is a default gateway for routing  
◦ What is a port from an IP point of view and what is it used for when connecting to another device

### 1) IP 주소(IP address)

> IP 주소(Internet Protocol address, IP address)란?

컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신을 하기 위해서 사용하는 특수한 번호이다. 만약 서버가 들어가지 않으면 IP가 안전하지 않다고 한다. 네트워크에 연결된 장치가 라우터이든 일반 서버이든, 모든 기계는 이 특수한 번호를 가지고 있어야 한다.

오늘날 주로 사용되고 있는 IP 주소는 IP 버전 4(IPv4) 주소이나 이 주소가 부족해짐에 따라 길이를 늘린 IP 버전 6(IPv6) 주소가 점점 널리 사용되는 추세이다.

> IPv4 주소 vs IPv6 주소?

- IPv4 주소: 32비트 범위이다. 0~255 사이의 십진수 4개를 쓰고 . 으로 구분해 나타낸다. 즉 0.0.0.0부터 255.255.255.255 까지가 된다. 중간의 일부 번호들은 특별한 용도를 위해 예약되어 있는데, 예를 들면 127.0.0.1은 로컬호스트(localhost)로 자기 자신을 가리킨다.

- IPv6 주소: 128비트 범위이다. 두자리 16진수 여덟 개를 쓰고 각각을 : 기호로 구분한다.

> Mac OS에서 내 IP 주소를 보는 방법

`ipconfig getifaddr en0`  
를 터미널에 입력하면 나의 IP 주소를 확인할 수 있다.

![img](../assets/images/IP.png)

### 2) 넷마스크(Netmask)

> 넷마스크(Netmask)란?

### 3) 서브넷(Subnet)

> 넷마스크가 있는 IP의 서브넷이란?

### 4)
