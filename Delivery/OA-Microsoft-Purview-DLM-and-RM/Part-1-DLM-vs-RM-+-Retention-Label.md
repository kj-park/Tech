---
layout: default
title: [Educate - Onboarding Accelerator Microsoft Purview Data Lifecycle and Records Management]
URL: Delivery/OA-Microsoft-Purview-DLM-and-RM/Part-1-DLM-vs-RM-+-Retention-Label
Path: Delivery\OA-Microsoft-Purview-DLM-and-RM\Part-1-DLM-vs-RM-+-Retention-Label.md
ms.date: 07/19/2026
---

# Part 1. DLM vs RM 비교 + Retention Label

## 1. DLM vs RM 이해하기

### 많은 고객의 오해

고객들이 가장 많이 하는 질문

    DLM 과 RM 중무엇을 사야 하나요?``

Microsoft 관점은 조금 다릅니다.

    DLM = 기본 계층
    RM = 상위 계층

입니다.

### DLM이란?

Data Lifecycle Management

목적

    보존삭제자동화

핵심 질문

    언제 삭제할까?몇 년 보관할까?

예시

    메일 7년 보관↓자동 삭제

    Teams 채팅3년 보관↓삭제

#### 지원 방식

- Retention Policy
- Standard Retention Label
- Adaptive Scope
- Auto Labeling
- Event-Based Retention
- Disposition Review

### RM이란?

Records Management

목적

    공식 기록 관리

질문이 다릅니다.

    삭제 여부보다이게 공식 기록인가?

를 먼저 판단합니다.

예시

    감사보고서계약서법률문서인사기록

이러한 문서는

    수정 금지삭제 금지감사 추적

요구

그래서 RM 제공

### DLM vs RM 비교

| 항목 | DLM | RM |
| --- | --- | --- |
| 보존 정책 | O | X |
| 표준 Label | O | O |
| Record Label | X | O |
| Regulatory Record | X | O |
| File Plan | X | O |
| Proof of Disposition | X | O |

### 가장 중요한 이해

Microsoft는

    DLM 또는 RM

을 이야기하지 않습니다.

대부분

    DLM + RM

을 이야기합니다.

예)

보험사

    일반 문서 → DLM
    계약서→ RM

예)

은행

    메일→ DLM
    거래기록→ RM

---

## 2. Retention Principles

정책이 충돌할 때

누가 이기는지 정의합니다.

### 원칙 1

#### Retention Wins Over Deletion

    보존vs삭제

보존 승리

예)

    정책 A10년 보관정책 B1년 후 삭제

결과

    10년 보관

### 원칙 2

#### Longest Retention Wins

보존끼리 충돌

예)

    5년vs10년

결과

    10년

### 원칙 3

#### Explicit Wins Over Implicit

가장 중요

Explicit

    Retention Label`

Implicit

    Retention Policy

즉

    Label > Policy

입니다.

### 원칙 4

#### Shortest Deletion Wins

삭제끼리 충돌

↓

가장 빨리 삭제

### 고객 질문

    Policy 와 Label이 충돌하면?

답

    Label 우선

(Explicit Wins Over Implicit)

---

## 3. Retention Label

이제 본격적인 Retention의 핵심

    Retention Label

설명입니다.

### Retention Policy vs Label

가장 중요한 차이

### Policy

Container Level

    Mailbox
    Site
    OneDrive
    Teams

전체

### Label

Item Level

    파일문서메일

개별

### 고객 사례

#### Policy

    전체 HR Site7년 보관

#### Label

    HR Site 안의퇴직자 문서10년
    평가 문서5년
    교육 문서3년

가능

### Retention Label의 3가지 핵심 특징

#### ① Item Level

파일마다 다르게 적용

#### ② Policy 보다 우선

충돌 시

    Label 승리

#### ③ 이동해도 따라감

슬라이드에서 매우 강조

    Label travels with content

예)

    Site A
    ↓
    Site B

Label 유지

### Sensitivity Label과 비교

많은 고객이 헷갈림

#### Sensitivity Label

    보호
    암호화
    접근제어

#### Retention Label

    보존
    삭제
    수명주기

완전히 다른 기능

---

## 4. Label Creation & Auto Labeling

### Publish가 반드시 필요

Label 생성

↓

바로 사용 불가

↓

Label Policy Publish

↓

사용 가능

### Auto Labeling 4가지 방법

#### Option A

Sensitive Information Type

예)

    주민번호
    카드번호
    계좌번호

#### Option B

Trainable Classifier

예)

    계약서
    이력서
    기밀문서

패턴 없는 문서

#### Option C

Keyword

예)

    M&A
    Confidential
    Attorney

#### Option D

Cloud Attachment

예)

    OneDrive 링크
    SharePoint 링크

### 실무적으로 가장 많이 쓰는 조합

귀하가 자주 설계하는 Purview 프로젝트 기준으로는

    Auto Label
    +
    SIT
    +
    EDM
    +
    Trainable Classifier

기반이 가장 많습니다.

(단, 이 슬라이드에서는 EDM은 Auto Label 지원 제한 사항도 언급됩니다.)

## 5. Auto Label Simulation

실제 운영 전 검증

    Simulation Mode

제공

지원

✅ SIT

✅ Keyword

미지원

❌ Trainable Classifier

❌ Cloud Attachment

---

## 6. 사용자가 Label 적용하는 방법

지원 위치

- Outlook Desktop
- Outlook Web
- SharePoint
- OneDrive
- Teams Files

중요

    Teams Chat

은

    Retention Label지원 안 함

대신

    Retention Policy

사용

---

## 7. Standard Retention Label

이 부분이 RM으로 넘어가기 전 가장 중요합니다.

### Standard Label

목적

    보존 기간 정의

제한 없음

사용자

✅ 수정 가능

✅ 삭제 가능

✅ 이동 가능

✅ 이름 변경 가능

✅ Label 제거 가능

즉

    Retention Label≠Record

입니다.

## Purview 아키텍트 관점 최종 정리

핵심은 다음 한 장으로 요약할 수 있습니다.

    DLM
    │
    ├─ Retention Policy
    │
    └─ Standard Retention Label
           │
           ├─ Manual (E3)
           ├─ Auto (E5)
           ├─ SIT
           ├─ Trainable Classifier
           ├─ Keywords
           └─ Cloud Attachment
    
    RM
    │
    ├─ Record Label
    ├─ Regulatory Record
    └─ File Plan
    
    공통
    │
    ├─ Retention Principles
    ├─ Adaptive Scope
    ├─ Event-Based Retention
    └─ Disposition Review
