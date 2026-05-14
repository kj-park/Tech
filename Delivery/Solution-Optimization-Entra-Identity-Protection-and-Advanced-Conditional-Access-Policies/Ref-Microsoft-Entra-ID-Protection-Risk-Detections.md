---
layout: default
title: [Ref - Microsoft Entra ID Protection의 **Risk Detections**]
URL: Delivery/Solution-Optimization-Entra-Identity-Protection-and-Advanced-Conditional-Access-Policies/Ref-Microsoft-Entra-ID-Protection-Risk-Detections
Path: Delivery\Solution-Optimization-Entra-Identity-Protection-and-Advanced-Conditional-Access-Policies\Ref-Microsoft-Entra-ID-Protection-Risk-Detections.md
ms.date: 05/10/2026
---

# Ref - Microsoft Entra ID Protection - **Risk Detections**

Microsoft Entra ID Protection은 조직 내 계정이 공격받고 있거나 이미 침해되었을 가능성을 판단하기 위해 다양한 위험 신호(Risk Signals) 를 자동으로 수집하고 분석합니다. 

이 페이지는 그중에서도 Sign-in Risk(로그인 위험) 과 User Risk(사용자 위험) 를 어떻게 탐지하는지, 어떤 유형이 있는지, 어떤 라이선스가 필요한지 상세히 설명합니다.

## Risk Detection이란 무엇인가

Microsoft Entra ID Protection은 사용자 로그인 및 계정 활동에서 **의심스러운 패턴**을 감지해 위험을 판단합니다:  

- Microsoft Entra ID Protection은 **광범위한 위험 탐지 기능**을 제공하며, 이를 통해 조직의 의심스러운 활동을 식별할 수 있습니다. [^1].
- 대부분의 상세 탐지 정보는 **Entra ID P2 라이선스**가 있어야 볼 수 있습니다. [^2].
- P2가 없으면 “**Additional risk detected**”라는 일반적인 메시지만 표시됩니다. [^3].

## Risk Detection의 두 가지 범주

### ✔ Sign-in Risk (로그인 위험)

로그인 시도 자체가 의심스러운지 판단하는 탐지.

예:

- 익명 IP에서 로그인
- 불가능한 이동(Impossible travel)
- 의심스러운 MFA 승인
- 비정상 토큰 사용 등

### ✔ User Risk (사용자 위험)

사용자 계정 자체가 이미 침해되었을 가능성을 판단하는 탐지.

예:

- 유출된 자격 증명(Leaked credentials)
- 공격자 중간자(Attacker-in-the-middle)
- 비정상 사용자 활동
- 의심스러운 API 호출 등

## Sign-in Risk 탐지 유형 상세 설명

아래는 페이지에서 제공하는 주요 Sign-in Risk 탐지 유형과 핵심 요약.

### 🔸 Activity from anonymous IP address

익명 프록시(Tor 등)에서 로그인 시도 감지

- 공격자가 위치를 숨기기 위해 사용하는 경우가 많음 [^4]
- 실시간 탐지 가능
- Free/P1에서도 사용 가능

### 🔸 Suspicious MFA authentication approval

사용자가 **본인이 요청하지 않은 MFA 승인**을 했을 가능성

- 사회공학/피싱 공격 가능성 높음
- 위치·브라우저·ASN 등 다양한 신호를 분석해 고위험으로 판단 [^5]
- 실시간 탐지, P2 필요

### 🔸 Atypical travel

사용자의 과거 로그인 패턴과 비교해 **비정상적인 이동 경로** 감지

- 예: 한국 → 10분 후 미국
- VPN 등은 자동으로 필터링하여 오탐을 줄임 [^6]
- 오프라인 계산, P2 필요

### 🔸 Impossible travel

물리적으로 이동이 불가능한 시간 내에 다른 지역에서 로그인

- Defender for Cloud Apps 기반 탐지 [^7]
- P2 + Defender for Cloud Apps 필요

### 🔸 Malicious IP address

악성 IP에서 로그인 시도

- Microsoft 위협 인텔 기반
- 오프라인 계산, P2 필요

### 🔸 Password spray

공격자가 여러 계정에 대해 흔한 비밀번호를 반복 시도

- 성공한 경우에만 탐지 발생 [^8]
- 실시간/오프라인, P2 필요

### 🔸 Unfamiliar sign-in properties

사용자의 과거 로그인 패턴과 다른 속성(IP, 브라우저, 디바이스 등) 감지

- 신규 사용자에게는 학습 기간 존재(최소 5일) [^9]
- 실시간, P2 필요

### 🔸 Verified threat actor IP

국가 기반 공격자 또는 사이버 범죄 조직의 IP로부터 로그인

- 실시간 탐지
- P2 필요

## User Risk 탐지 유형 상세 설명

### 🔸 Leaked credentials

사용자의 자격 증명이 **다크웹·유출 데이터베이스**에서 발견됨

- Microsoft가 다양한 외부 소스에서 지속적으로 스캔하여 탐지 [^10]
- 매우 높은 위험
- Free/P1에서도 사용 가능

### 🔸 Anomalous user activity

관리자 계정의 비정상적인 디렉터리 변경 등

- 오프라인 계산, P2 필요

### 🔸 Attacker in the Middle (AiTM)

악성 리버스 프록시를 통한 인증 가로채기

- 고정밀 탐지
- 사용자를 High risk로 설정 [^11]
- E5 + EMS E5 필요

### 🔸 Possible attempt to access PRT

공격자가 Primary Refresh Token(PRT)을 탈취하려는 시도

- MDE(Defender for Endpoint) 필요
- High risk로 분류 [^12]

### 🔸 Suspicious API traffic

비정상적인 Graph API 호출 또는 디렉터리 열람

- 계정 탈취 후 정찰(Reconnaissance) 가능성
- 오프라인, P2 필요

### 🔸 User reported suspicious activity

사용자가 MFA 요청을 “의심스럽다”고 직접 신고

- MFA 피싱 가능성
- P2 필요 [^13]

## 라이선스 요약

| 탐지 유형 | Free/P1 | P2 | E5 + Defender |
| --- | --- | --- | --- |
| Additional risk detected | ✔ | – | – |
| Anonymous IP | ✔ | – | – |
| Leaked credentials | ✔ | – | – |
| 대부분의 고급 탐지(불가능한 이동, 비정상 토큰 등) | – | ✔ | – |
| AiTM, Mass file access 등 | – | – | ✔ |

## 왜 중요한가

Risk Detection은 다음과 같은 보안 정책과 직접 연결:

- **Risk-based Conditional Access**  

    → 위험 수준에 따라 MFA 요구, 비밀번호 재설정, 차단 등 자동 조치 가능

- **Identity Protection Dashboard**  

    → 위험 사용자·위험 로그인 모니터링

- **자동화된 계정 보호**  

    → 공격 조기 탐지 및 대응


