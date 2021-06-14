---
layout: post
title: "iOS 계층 구조" 
date: 2021-06-12
category: read 
excerpt: ""

---

## iOS 계층 구조 (iOS Architecture)

> iOS : 유닉스 기반의 맥 OS X를 기반으로 해서 다윈 커널을 가지고 있는 모바일 OS

<img src="https://user-images.githubusercontent.com/28949235/121845611-ae309880-cd20-11eb-9d09-d226169018cf.png" alt="image" width=200px /> 아래로 갈수록 기본(iOS의 핵심 / 하드웨어 관련), 위로 갈수록 사용자와 관련이 있음.

### Application

> Apple App, Third-Party App

* iOS의 가장 바깥 계층
* Apple App, Third - Party App이 여기에 속함
* 사용자와 가장 맞닿아 있는 계층

### Cocoa Touch

> UIKit, Foundation

* 화면의 그래픽 UI 및 터치의 기능 제공
* UIKit, MapKit, MessageUI 등이 여기에 속함
* 실제로 개발할 때 가장 많이 접하게 되는 계층

### Media

> Open GLES, Quarts, Core Graphics, Core Audio, OpenAL, MediaPlayer

* 그래픽이나 오디오, 비디오 등 멀티미디어 기능을 제공하는 계층
* C와 Objective-C가 혼합되어 있는 형태
* AvFoundation(미디어 재생 관련), MediaPlayer(플레이어), Core Image(이미지 가공) 등의 기능이 있음

### Core Service

> Address Book, Core Foundation, Core Location, CFNetwork, SQLite, XML Support

* GPS 나침반, 가속도 센서, 자이로스코프 디바이스 등 하드웨어적 기능들
* Core OS에서 제공하지 않는 기능들을 포함
* 내부 데이터/위치/센서 등의 기능을 제공
* CoreMotion(기기 센서), Accounts(계정 관리), Core Foundation(데이터 관리) 등의 기능 제곰

### Core OS

> System Utilities, Mach Kernel

* 하드웨어와 가장 가까이 있는 최하위 계층
* 시스템의 핵심 기능을 포함하는 기본적인 부분들을 관리



> SOPT 28기 iOS 파트 1차 복습 자료 내용 中

