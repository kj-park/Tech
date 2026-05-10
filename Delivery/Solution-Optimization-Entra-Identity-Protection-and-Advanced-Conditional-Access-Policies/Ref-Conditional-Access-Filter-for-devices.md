---
layout: default
title: [Conditional Access – Filter for Devices]
filename: Delivery/Solution-Optimization-Entra-Identity-Protection-and-Advanced-Conditional-Access-Policies/Ref-Conditional-Access-Filter-for-devices.md
ms.date: 05/11/2026
---

# #️⃣ Conditional Access – *Filter for Devices*

## 1. Filter for Devices란 무엇인가?

**Filter for devices**는 Conditional Access 정책에서 특정 디바이스를 **정교하게 포함하거나 제외**할 수 있게 해주는 조건입니다.  
문서에서는 이를 “관리자가 특정 디바이스를 타깃팅할 수 있는 기능”이라고 명시합니다 [현재 페이지](citation-section://1868559948/3).

또한, 이 필터는 기존의 사용자·그룹·앱·위치 조건과 함께 사용할 수 있는 **추가적인 Assignment 조건**이라고 설명합니다 [현재 페이지](citation-section://1868559948/5).

## 2. 왜 필요한가? — 실제 조직에서의 활용 시나리오

문서에서는 대표적으로 **3가지 핵심 시나리오**를 제시합니다.

### 2.1 특권 리소스 접근 제한 (Privileged Access Control)

특정 관리자 역할을 가진 사용자가 **보안성이 높은 디바이스(SAW 등)**에서만 특정 API에 접근하도록 제한하는 시나리오입니다.

문서에서는 다음 조건을 가진 사용자를 예로 듭니다:

- 관리자 역할이 있음 [현재 페이지](citation-section://1868559948/7)
- MFA 완료함 [현재 페이지](citation-section://1868559948/8)
- SAW 등 보안 디바이스이며 준수 상태(compliant)임 [현재 페이지](citation-section://1868559948/9)

이를 위해 **두 개의 정책**을 조합해야 한다고 설명합니다:

#### 정책 1

- 관리자 역할 사용자
- Windows Azure Service Management API 접근
- MFA + 준수 디바이스 요구  
[현재 페이지](citation-section://1868559948/10)

#### 정책 2

- 동일한 사용자/앱
- 단, **SAW 디바이스는 제외**
- 나머지는 차단(Block)  
[현재 페이지](citation-section://1868559948/11)

이 구조는 “허용 정책 + 예외 정책” 패턴으로, 보안성이 높은 디바이스만 접근하도록 강제하는 대표적인 설계입니다.

### 2.2 지원되지 않는 OS 차단

Windows 10 미만 버전의 Windows OS를 사용하는 디바이스를 차단하는 정책입니다.

문서에서는 다음과 같이 설명합니다:

- Windows OS
- OS 버전이 10.0으로 시작하지 않으면 차단  
[현재 페이지](citation-section://1868559948/14)

즉, 다음 규칙을 만족하는 디바이스만 **예외 처리**됩니다:

    device.operatingSystem == 'Windows'AND device.operatingSystemVersion startsWith '10.0'

그 외 Windows 7/8/8.1 등은 모두 차단됩니다.

### 2.3 특정 디바이스에서 MFA 면제

Teams Phone, Surface Hub 같은 장비에서 서비스 계정이 로그인할 때 MFA를 요구하지 않도록 설정하는 시나리오입니다.

문서에서는 다음과 같이 설명합니다:

- 서비스 계정은 MFA 요구 정책에서 제외
- 특정 디바이스(예: Teams Phone)는 필터로 제외  
[현재 페이지](citation-section://1868559948/17)

정책 구성은 다음 두 개로 나뉩니다:

#### 정책 1

- 일반 사용자
- MFA 요구  
[현재 페이지](citation-section://1868559948/18)

#### 정책 2

- 서비스 계정 그룹
- 특정 디바이스(예: extensionAttribute2 = TeamsPhoneDevice)는 제외
- 나머지는 차단  
[현재 페이지](citation-section://1868559948/19)

## 3. 디바이스 등록 상태에 따른 필터 동작

문서에서 가장 중요한 부분 중 하나입니다.

### ✔ 등록되지 않은 디바이스는 모든 속성이 null

문서에서는 다음과 같이 명확히 설명합니다:

- “등록되지 않은 디바이스는 모든 속성이 null로 간주된다” [현재 페이지](citation-section://1868559948/20)
- “이 디바이스는 디렉터리에 존재하지 않으므로 속성을 판단할 수 없다” [현재 페이지](citation-section://1868559948/21)

### ✔ 따라서 부정 연산자(NotEquals 등)를 사용해야 함

문서에서는 다음과 같이 강조합니다:

- “등록되지 않은 디바이스를 타깃팅하려면 negative operator를 사용해야 한다” [현재 페이지](citation-section://1868559948/22)
- “positive operator는 디바이스가 디렉터리에 존재할 때만 적용된다” [현재 페이지](citation-section://1868559948/23)

즉:

| 연산자 유형 | 등록되지 않은 디바이스에 적용됨? | 이유 |
| --- | --- | --- |
| Positive (Equals, StartsWith 등) | ❌ | null 값은 어떤 positive 조건도 만족하지 않음 |
| Negative (NotEquals 등) | ✔ | null 값은 대부분의 negative 조건에 해당됨 |

이 원리를 이해해야 정책이 의도대로 동작합니다.

## 4. 지원되는 디바이스 속성 및 연산자

문서에서는 매우 상세한 속성 목록을 제공합니다.  
대표적인 속성만 정리하면 다음과 같습니다.

| 속성 | 설명 | 인용 |
| --- | --- | --- |
| deviceId | GUID 기반 디바이스 ID | [현재 페이지](citation-section://1868559948/59) |
| deviceOwnership | Personal / Company | [현재 페이지](citation-section://1868559948/59) |
| enrollmentProfileName | Intune 등록 프로필 | [현재 페이지](citation-section://1868559948/65) |
| isCompliant | 준수 여부 | [현재 페이지](citation-section://1868559948/59) |
| operatingSystem | Windows, iOS, Android 등 | [현재 페이지](citation-section://1868559948/66) |
| operatingSystemVersion | OS 버전 | [현재 페이지](citation-section://1868559948/59) |
| physicalIds | Autopilot ZTDId 등 | [현재 페이지](citation-section://1868559948/59) |
| profileType | RegisteredDevice, SecureVM 등 | [현재 페이지](citation-section://1868559948/67) |
| systemLabels | M365Managed 등 | [현재 페이지](citation-section://1868559948/68) |
| trustType | AzureAD, ServerAD, Workplace | [현재 페이지](citation-section://1868559948/69) |
| extensionAttribute1~15 | 사용자 정의 확장 속성 | [현재 페이지](citation-section://1868559948/70) |

### ⚠ 중요한 제약

문서에서는 extensionAttribute 사용 시 다음 조건을 명확히 제시합니다:

- “디바이스는 Intune 관리, 준수 상태, 또는 Hybrid Joined 상태여야 extensionAttribute 값이 제공된다”  
[현재 페이지](citation-section://1868559948/72)

## 5. 필터 규칙 길이 제한 및 Contains 동작 차이

### ✔ 규칙 길이 제한

- “필터 규칙의 최대 길이는 3072자”  
[현재 페이지](citation-section://1868559948/73)

### ✔ Contains 동작 차이

문서에서는 Contains가 속성 타입에 따라 다르게 동작한다고 설명합니다:

- 문자열 속성: substring 포함 여부 검사 [현재 페이지](citation-section://1868559948/75)
- 문자열 컬렉션 속성: 전체 문자열 매칭 여부 검사 (예: physicalIds) [현재 페이지](citation-section://1868559948/76)

## 6. 정책 적용 여부 — Positive/Negative × 등록 상태

문서에서는 정책 적용 여부를 표로 정리합니다. 핵심 요약은 다음과 같습니다.

### ✔ Positive operator (Equals 등)

- 등록되지 않은 디바이스 → 적용 안 됨
- 등록된 디바이스 → 조건 충족 시 적용
- extensionAttribute 사용 시 → 디바이스가 **준수 또는 Hybrid Joined**여야 적용  
[현재 페이지](citation-section://1868559948/79)

### ✔ Negative operator (NotEquals 등)

- 등록되지 않은 디바이스에도 적용됨
- extensionAttribute 사용 시 → 준수 또는 Hybrid Joined 필요  
[현재 페이지](citation-section://1868559948/80)

### ✔ 등록되지 않은 디바이스는 모든 속성이 null

- **등록되지 않은 디바이스는 모든 속성이 null로 간주된다**
- 따라서 속성 기반 필터가 정상적으로 평가되지 않음

### ✔ 등록되지 않은 디바이스를 타깃팅하려면 Negative operator 사용

- **Negative operator를 사용해야 규칙이 적용된다**
- Positive operator는 디바이스가 디렉터리에 존재할 때만 적용됨

## 7. Graph API 구성

문서에서는 Graph API(v1.0)로 필터를 구성하는 예시를 제공합니다.

예시 JSON:

    { "conditions": { "devices": { "deviceFilter": { "mode": "exclude", "rule": "device.extensionAttribute1 -ne \"SAW\"" } } }}

이 예시는 SAW가 아닌 디바이스를 제외하는 규칙입니다 [현재 페이지](citation-section://1868559948/55).

## 8. 전체 요약

Filter for devices는 Conditional Access 정책을 디바이스 속성 기반으로 매우 정교하게 제어할 수 있게 해주는 핵심 기능입니다.

특히 다음 상황에서 필수적입니다:

- SAW 같은 보안 디바이스만 허용
- 특정 OS 버전 이하 차단
- 특정 장비에서 MFA 면제
- 등록되지 않은 디바이스를 정책 대상으로 포함/제외

정책 설계 시 반드시 고려해야 할 핵심 요소:

- 디바이스 등록 상태(null 속성)
- Positive/Negative operator 차이
- extensionAttribute 사용 조건(Intune 관리/준수/Hybrid Join)
- Contains 동작 차이
- 규칙 길이 제한(3072자)







