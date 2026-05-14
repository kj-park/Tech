---
layout: default
title: [Ref - Risk-based access policies]
URL: Delivery/Solution-Optimization-Entra-Identity-Protection-and-Advanced-Conditional-Access-Policies/Ref-Risk-based-access-policies
Path: Delivery\Solution-Optimization-Entra-Identity-Protection-and-Advanced-Conditional-Access-Policies\Ref-Risk-based-access-policies.md
ms.date: 05/10/2026
---

# Ref - Risk-based access policies

## 개요: Entra ID Protection의 위험 기반 액세스 정책이란?

Microsoft Entra ID Protection의 **위험 기반 액세스 정책**은 “위험한 로그인이나 계정”이 감지되었을 때, 자동으로 추가 보안 절차(MFA, 비밀번호 변경, 차단 등)를 적용하는 기능입니다.  
핵심은 두 가지 위험 신호를 기반으로 조건부 액세스 정책을 만드는 것입니다. [^1]

- **Sign-in risk(로그인 위험)**
- **User risk(사용자 위험)**

이 두 위험 수준에 따라 **허용/추가 인증 요구/비밀번호 변경 요구/차단** 같은 조치를 자동으로 수행합니다.

---

## Sign-in risk(로그인 위험)

**1. 개념**

- **정의:** 특정 로그인 시도가 실제 사용자 본인이 아닐 가능성(위험도)을 평가한 값입니다.
- **관점:** “이번 로그인 시도 자체가 수상한가?”에 초점을 둡니다. [^2]

**2. 대표적인 탐지 예시**

- **익명 IP/프록시/Tor 사용**
- **불가능한 이동(impossible travel)**

    - 예: 20분 전에 서울에서 로그인, 바로 이어서 유럽에서 로그인 시도
- **악성코드에 감염된 디바이스에서의 로그인**
- **낯선 위치/낯선 디바이스/낯선 브라우저**
- **유출된 자격 증명(이메일·비밀번호) 사용 시도** [^2][^3]

이러한 신호들을 머신러닝이 종합해 **Low / Medium / High** 같은 위험 레벨로 분류합니다.

**3. Sign-in risk 정책에서 보통 하는 일**

예를 들어, 다음과 같이 정책을 구성할 수 있습니다. [^1]

- **Medium 이상 로그인 위험 → MFA 요구**
- **High 로그인 위험 → 로그인 차단**

이렇게 하면 평소에는 사용자 경험을 해치지 않으면서, 수상한 로그인에만 강하게 대응할 수 있습니다.

## User risk(사용자 위험)

**1. 개념**

- **정의:** 특정 계정(사용자)이 이미 탈취되었을 가능성(위험도)을 평가한 값입니다.
- **관점:** “이 계정 자체가 이미 공격자에게 넘어갔을 가능성이 있는가?”에 초점을 둡니다. [^2][^3]

**2. 대표적인 탐지 예시**

- **다크웹/공개 데이터 유출에서 해당 계정의 자격 증명 발견**
- **여러 차례의 위험한 로그인 시도 누적**
- **악성 활동과 연관된 계정 사용 패턴**
- **여러 애플리케이션에서 비정상적인 활동이 반복되는 경우** [^2]

이 역시 **Low / Medium / High** 수준으로 분류됩니다.

**3. User risk 정책에서 보통 하는 일**

User risk는 “계정이 이미 털렸을 수 있다”는 의미이므로, 보통 더 강한 조치를 취합니다. [^1][^3]

- **Medium 이상 사용자 위험 → 안전한 비밀번호 변경 요구**
- **High 사용자 위험 → 비밀번호 변경 완료 전까지 모든 액세스 차단**
- **또는 ‘위험 완화(Require risk remediation)’ 컨트롤 사용**

    - 사용자가 스스로 적절한 인증·비밀번호 변경 절차를 완료하면 위험이 자동으로 해소되도록 함

## Sign-in risk vs User risk 차이 정리

| 구분 | Sign-in risk(로그인 위험) | User risk(사용자 위험) |
| --- | --- | --- |
| 평가 대상 | **이번 로그인 시도** | **사용자 계정 전체** |
| 질문 | “지금 이 로그인 시도가 수상한가?” | “이 계정이 이미 탈취되었을 가능성이 있는가?” |
| 주요 신호 | 위치, 디바이스, IP, 브라우저, 세션 패턴 등 | 유출된 자격 증명, 반복되는 위험 로그인, 이상 활동 등 |
| 일반 대응 방식 | MFA 요구, 로그인 차단 | 비밀번호 변경 요구, 계정 차단, 위험 완화 절차 |

두 정책은 서로 보완 관계입니다.

- **Sign-in risk**는 “순간적인 수상한 로그인”을 잡아내고,
- **User risk**는 “장기적으로 이미 털렸을 가능성이 있는 계정”을 관리합니다. [^2][^3]

## 위험 기반 조건부 액세스 정책 동작 흐름

로그인 시 실제로는 다음과 같은 흐름으로 동작합니다. [^1][^3]

1. **사용자 로그인 시도**
2. **ID Protection이 위험 평가**

    - Sign-in risk: None / Low / Medium / High
    - User risk: None / Low / Medium / High
3. **조건부 액세스 정책 평가**

    - “Sign-in risk가 Medium 이상이면 MFA 요구”
    - “User risk가 High이면 비밀번호 변경 전까지 차단” 등
4. **정책에 따른 조치 적용**

    - 허용 / MFA 요구 / 비밀번호 변경 요구 / 차단
5. **사용자가 요구된 조치를 완료하면**

    - 위험이 자동으로 “해소(remediated)” 처리되고, 관리자가 일일이 개입할 필요가 줄어듭니다.

## 라이선스 및 전제 조건

위험 기반 액세스 정책을 사용하려면 다음이 필요합니다. [^1][^3]

- **Microsoft Entra ID P2 라이선스** (정책 적용 대상 사용자 모두)
- **Global Administrator 또는 Security Administrator 권한**
- **MFA 구성 완료**

    - 많은 위험 정책이 MFA를 요구하므로, 사전에 MFA가 준비되어 있어야 합니다.
- **비상용 계정(break-glass account)**

    - 모든 조건부 액세스 정책에서 제외해 두는 관리자 계정 1~2개

## 설계 시 베스트 프랙티스(권장 패턴)

**1. 처음부터 너무 강하게 막지 말 것**

- 바로 “High 이상은 전부 차단”으로 가기보다는,  
먼저 **MFA 요구 → 비밀번호 변경 요구 → 차단** 순으로 점진적으로 강화하는 것이 좋습니다. [^2][^3]

**2. Sign-in risk와 User risk를 역할에 따라 다르게 적용**

- **일반 사용자**

    - Sign-in risk Medium 이상 → MFA 요구
    - User risk Medium 이상 → 비밀번호 변경 요구
- **관리자/고위험 계정**

    - Sign-in risk Medium 이상 → MFA + 특정 위치/디바이스만 허용
    - User risk High → 즉시 차단 후 보안 팀 확인

**3. 모니터링을 먼저 하고, 그 다음에 정책을 강화**

- Entra 포털의 **Risk detections / Risky users / Risky sign-ins** 보고서를 먼저 확인해  
실제 환경에서 어떤 위험이 얼마나 발생하는지 파악한 뒤,  
그에 맞춰 임계값과 정책을 조정하는 것이 안정적입니다. [^3]






















