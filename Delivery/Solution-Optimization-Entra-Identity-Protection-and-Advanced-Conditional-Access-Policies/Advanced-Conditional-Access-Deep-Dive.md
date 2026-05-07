---
layout: default
title: [Advanced Conditional Access - Deep Drive]
filename: Delivery/Solution-Optimization-Entra-Identity-Protection-and-Advanced-Conditional-Access-Policies/Advanced-Conditional-Access-Deep-Dive.md
ms.date: 05/07/2026
---

# Advanced Conditional Access - Deep Drive

## Help defining an enterprise framework

**엔터프라이즈 프레임워크 정의 지원**

조건부 액세스(Conditional Access) 정책은 가장 단순하게 말하면 *if-then* 구조의 문장입니다. 즉, 사용자가 어떤 리소스에 접근하려고 하면, 그에 따라 특정 작업을 수행해야 합니다.예를 들어: 사용자가 Microsoft 365 같은 애플리케이션이나 서비스에 접근하려면, 접근을 위해 다단계 인증(MFA)을 수행해야 합니다.

관리자는 두 가지 주요 목표에 직면합니다:

- 사용자가 언제 어디서나 생산적으로 일할 수 있도록 지원하기
- 조직의 자산을 보호하기

조건부 액세스 정책을 사용하면 필요한 시점에 적절한 액세스 제어를 적용하여 조직을 안전하게 보호할 수 있습니다.

**구조화된 프레임워크(Structured Framework)**를 따르는 것은 정책을 이해하기 쉽게 만들어 줍니다. 또한 모든 시나리오를 빠짐없이 다루고, 문제 해결이 어려운 충돌 정책을 피하는 데에도 도움이 됩니다.

이번 VBD에서는 고급 시나리오를 다룰 예정이지만, 고객의 현재 환경을 반드시 확인해야 합니다. 특히 다음과 같은 항목을 점검하며, 고객이 구조화된 프레임워크를 사용하고 있는지, 그리고 관련 주제를 다시 상기할 필요가 있는지 확인해야 합니다:

    - 사용자 환경 구성 방식
    - 현재 적용 중인 조건부 액세스 정책 구조
    - 정책 간 충돌 여부

- **명명 규칙(Naming convention):** 표준화된 명명 규칙을 사용하면 Azure 관리자 포털을 열어보지 않아도 정책을 쉽게 찾고 목적을 이해할 수 있습니다. 권장되는 명명 형식에는 다음 요소를 포함하는 것입니다:

    - 순서 번호 (A Sequence Number)
    - 적용되는 클라우드 앱 (The cloud app(s) it applies to)
    - 정책의 동작(The response)
    - 적용 대상(Who it applies to)
    - 적용 시점(When it applies (if applicable))

- **템플릿:** Microsoft 권장 사항에 맞춘 새로운 정책을 손쉽게 배포할 수 있는 편리한 방법입니다.

    템플릿을 배포하면, 템플릿을 배포하는 사용자 계정은 자동으로 정책에서 제외됩니다.

    > [!NOTE]
    > 2023년 11월 6일부터 Microsoft는 특정 기준(위험 신호, 현재 사용 상태, 라이선스)을 충족하는 테넌트에 3개의 Microsoft 관리 조건부 액세스 정책(Microsoft Managed Conditional Access Policies)을 자동 배포했습니다. 자세한 내용은 관련 문서를 참고하세요.
    > 
    > 📌 FY24 적용 정책 (3가지)
    > 
    > - **관리자 포털 MFA 요구** 관리자 역할 계정이 Microsoft 관리 포털 로그인 시 MFA 필수
    > - **Per-user MFA 사용자 대상 MFA 요구** 기존 per-user MFA 사용자에게 모든 클라우드 앱에서 MFA 요구
    > - **고위험 로그인 MFA 요구** 고위험 로그인 시 MFA 및 재인증 요구

Protect your administrators: remember that Conditional Access policies support built-in roles. Conditional Access policies are not enforced for other role types including administrative unit-scoped or custom roles.

- **관리자를 보호하세요:**

    조건부 액세스 정책은 **기본 제공 역할(built‑in roles)** 을 지원한다는 점을 기억해야 합니다. 조건부 액세스 정책은 **관리 단위 범위 역할(AU‑scoped roles)** 이나 **커스텀 역할(custom roles)** 과 같은 다른 유형의 역할에는 적용되지 않습니다.

- **Authentication methods (인증 방법)**

    모든 MFA가 동일한 것은 아니라는 점을 기억해야 합니다.

    SMS나 음성 통화 같은 레거시 MFA 방식은 Microsoft Authenticator 앱이나 FIDO2 등과 비교했을 때 보안 수준이 낮습니다.

    [Authentication methods and features](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-authentication-methods#authentication-method-strength-and-security)

- **사용자에게 안내하고 등록을 유도하는 방법**

    - 사용자에게 정보를 제공하기 위한 유용한 커뮤니케이션 템플릿은 다음에서 확인할 수 있습니다:
    
        - Microsoft 공식 다운로드 센터 – [Download Entra templates from Official Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=57600)  
        
        - 대규모 MFA 도입을 성공적으로 진행하려면 **커뮤니케이션이 핵심**입니다.
    
    - 사용자 등록을 관리하기 위해 다음 기능을 활용할 수 있습니다:

        - Registration Campaign(등록 캠페인)
        
            별도의 라이선스가 필요하지 않습니다.
        
            덜 안전한 인증 방식을 사용 중인 경우, 사용자가 Microsoft Authenticator 앱을 등록하도록 유도합니다.
        
        - MFA 등록 정책을 활용한 첫 등록 안내
        
            [MFA registration policy](https://learn.microsoft.com/en-us/entra/id-protection/howto-identity-protection-configure-mfa-policy)
            
            MFA 등록 정책을 사용하여 사용자가 다음번 대화형 로그인 시 등록하도록 안내할 수 있습니다.
            
            사용자는 14일 동안 등록을 완료할 수 있는 기간이 주어집니다. 이 14일 동안 MFA가 필수 조건이 아니라면 등록을 건너뛸 수 있습니다. 하지만 14일이 지나면 등록을 완료해야만 로그인 절차를 마칠 수 있습니다.
        
        - 인증 방법 등록 자체도 보호해야 함
        
            인증 방법을 등록하는 행위는 매우 민감하므로 반드시 보호해야 합니다. 이를 위해 다음 템플릿을 활용할 수 있습니다:
            
            [Control security information registration with Conditional Access](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-policy-registration)

## Automation via PowerShell




