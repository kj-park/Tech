---
layout: default
title: [Educate - Onboarding Accelerator Microsoft Purview Data Lifecycle and Records Management]
URL: Delivery/OA - Microsoft Purview DLM and RM
Path: Delivery\OA - Microsoft Purview DLM and RM\index.md
ms.date: 07/19/2026
---

# Educate - Onboarding Accelerator Microsoft Purview Data Lifecycle and Records Management


>규정(Compliance) → 보존 일정(Retention Schedule) → Purview DLM → Records Management → 운영 및 감사까지 전체 설계 아키텍처

---

## 1. 핵심 메시지

가장 중요한 문장은 다음입니다.

>**"모든 Retention 설계는 Retention Schedule부터 시작한다"**

즉,
```
규제 요구사항
      ↓
Retention Schedule 작성
      ↓
Purview 기능 매핑
      ↓
DLM / RM 구현
      ↓
모니터링 및 감사
```
순서로 진행해야 한다는 것입니다.

---

## 2. Retention Schedule(보존 일정)이 가장 중요하다

### Retention Schedule이란?

조직 데이터에 대해

- 얼마나 오래 보관할 것인가
- 언제 삭제할 것인가
- 어떤 법률을 준수해야 하는가

를 정의한 기준 문서입니다.

#### 예시

| 데이터 | 보관 기간 |
| --- | --- |
| 회계 문서 | 7년 |
| 직원 인사 정보 | 퇴사 후 7년 |
| 고객 개인정보 | 마지막 거래 후 24개월 |

이런 규칙을 정리한 것이 Retention Schedule입니다.

### 왜 중요한가?

#### 1. 위험 감소

보안 사고 분석 가능

#### 2. 규제 준수

GDPR HIPAA SOX 등 대응

#### 3. 소송 대응

eDiscovery Legal Hold

#### 4. 운영 효율성

감사 대응

#### 5. 의사결정

과거 데이터 분석

#### 6. 비용 절감

스토리지 감소

#### 7. 공격면 감소

불필요한 오래된 데이터 제거

특히 AI 시대에는 생성 데이터가 급증해 공격 표면이 된다고 강조합니다.

---

## 3. Retention Schedule 설계 방법

슬라이드는 설계 단계를 세 부분으로 나눕니다.

### Planning

#### 데이터 파악

- 어떤 데이터인가
- 어디 저장되는가
- 누가 소유자인가

#### 법적 요구사항

- GDPR
- HIPAA
- SOX
- 국가별 법률

#### 이해관계자 참여

- Legal
- Compliance
- Security
- IT
- 사업부

### Design

#### 데이터 분류

카테고리 정의

#### 보존 기간 정의

예)

    계약서 = 10년세금 자료 = 7년

#### 삭제 방법 정의

- 삭제
- Disposition Review
- Event Based

### Maintain

지속적인 유지관리

- 감사
- 모니터링
- 신규 데이터 유형 대응
- 법률 변경 반영

---

## 4. Purview 도입 전에 알아야 할 것

많은 고객이

```
Backup = Retention
```

이라고 생각합니다.

하지만 틀렸습니다.

---

## 5. Backup vs Retention

### M365 Backup

목적:

    복구 (Recovery)

예)

- 랜섬웨어
- 실수 삭제
- 데이터 손상

복원 가능성 확보

### Retention

목적:

    거버넌스

예)

- 법적 보존
- 규제 준수
- 기록 관리

### 핵심 문장

    Backup never overrides retention  
    Retention overrides backup

즉

보존 정책이 우선입니다.

---

## 6. Purview Retention 솔루션 구조

Purview에는 두 가지 솔루션이 있습니다.

### DLM

Data Lifecycle Management

목적

    대규모 보존 및 삭제

### RM

Records Management

목적

    규제 대응법적 기록 보존

---

## 7. DLM과 RM 차이

| 기능 | DLM | RM |
| --- | --- | --- |
| Retention Policy | O | X |
| Standard Label | O | O |
| Record Label | X | O |
| Regulatory Record | X | O |
| File Plan | X | O |
| Proof of Disposition | X | O |

### 한 줄로 기억

#### DLM

    보존하고 삭제

#### RM

    공식 기록으로 선언

---

## 8. Retention의 4대 원칙

Purview에서 가장 중요합니다.

### 원칙 1

Retention wins over deletion

보존이 삭제보다 우선

### 원칙 2

Longest retention wins

더 긴 보존 기간 우선

### 원칙 3

Explicit wins over implicit

Label이 Policy보다 우선

### 원칙 4

Shortest deletion wins

삭제 시점 충돌 시 더 짧은 삭제 기간 적용

## 9. DLM과 RM이 공통으로 사용하는 기능

### Retention Label

파일 단위

메일 단위

보존 설정

### Adaptive Scope

사용자 속성 기반 자동 적용

예)

    부서 = HR  
    직급 = Executive

### Event Based Retention

특정 이벤트가 발생해야 보존 시작

예)

    퇴사계약 종료프로젝트 종료

### Disposition Review

삭제 전 사람 승인

예)

    7년 보존 종료  
    ↓  
    Compliance 팀 검토  
    ↓  
    삭제 승인  

---

## 10. DLM에서 가장 중요한 기능

### Retention Policy

전체 컨테이너 대상

예)

    모든 Exchange  
    모든 SharePoint  
    모든 Teams  

### 적용 대상

- Exchange
- SharePoint
- OneDrive
- Teams
- Viva Engage
- Copilot
- AI Apps

---

## 11. RM에서 가장 중요한 기능

### Record Label

공식 기록 선언

특징

- 삭제 불가
- 수정 제한

### Regulatory Record

가장 강력

특징

- 삭제 불가
- 수정 불가
- Label 제거 불가
- 관리자도 해제 불가

---

## 12. File Plan

Records Management의 꽃

기능

- 모든 레이블 중앙 관리
- CSV Import/Export
- 분류 체계 관리

---

## 13. 보고 및 감사

마지막은 운영 단계입니다.

### Content Search

보존 레이블 적용 콘텐츠 검색

### Data Explorer

실제 내용 열람

### Activity Explorer

레이블 관련 활동 분석

---

## 최종 요약

핵심은 다음입니다.

>Retention Schedule을 먼저 설계하고일반적인 대규모 보존/삭제는 DLM,법적·규제 수준의 기록 관리는 RM으로 구현하며,  
>
> - Retention Label
> - Adaptive Scope
> - Event-Based Retention
> - Disposition Review
>
>를 활용해Microsoft 365 전반의 데이터 수명주기를 통제한다.

그리고 Purview DLM/RM에서 가장 중요한 부분은 DLM vs RM, Shared Capabilities, Record/Regulatory Record, File Plan 입니다. 

기능, 라이선스, 실제 고객 적용 시나리오 기반으로 Purview 아키텍트 관점에서 확인합니다.

---

Part 1. DLM vs RM 비교 + Retention Label

- DLM vs RM
- Retention Principles
- Retention Label
- 라이선스
- 고객 적용 사례

Part 2. Adaptive Scope + Event Based Retention

Part 3. Disposition Review

Part 4 DLM 고유 기능

Part 5. RM 고유 기능 (Record | Regulatory Record | File Plan)