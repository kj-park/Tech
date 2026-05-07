---
layout: default
title: [Solution Optimization - Entra Identity Protection and Advanced Conditional Access Policies]
filename: Delivery/Solution-Optimization-Entra-Identity-Protection-and-Advanced-Conditional-Access-Policies/index.md
ms.date: 05/06/2026
---

# Solution Optimization - Entra Identity Protection and Advanced Conditional Access Policies


이 기술 가이드의 목표는 ID 보호 및 조건부 접근 정책의 현재 채택 현황을 검토하는 방법론을 제공하여 고객의 보안 태세를 개선하기 위해 최적화하는 데 있습니다. 고객이 Entra ID 및/또는 Zero Trust Foundation 평가에 대해 주문형 평가를 받았는지 CSAM에 확인해 주시기 바랍니다. 이 참여 보고서는 출발점으로 활용할 수 있습니다.

## Basic configuration

검토는 CAP와 관련된 기본 설정 설정을 먼저 확인하며 시작합니다.

**처음에는 인증 방법을 검토할 수 있습니다.** 이 방법은 조직이 어떤 옵션을 사용하는지 알려줍니다. MFA 검증 방법은 동일하지 않다는 점을 명심하세요. SMS나 음성 통화 같은 '레거시' MFA 방법은 Microsoft Authenticator App만큼 안전하지 않고, OATH 토큰 등도 있습니다

![Authentication methods](image.png)

- [ ] 가장 강력한 검증 방법이 활성화되어 있는지 확인하세요.

- [ ] 기존 SSPR / Multifactor authentication에서 Authentication methods으로 마이그레이션 상태를 확인합니다.

    ![Authentication methods Migration status](image-1.png)
    
    > [[NOTE]]
    >
    > [How to migrate MFA and SSPR policy settings to the Authentication methods policy for Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-authentication-methods-manage)
    

**[Usage and Insight](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/AuthenticationMethodsMenuBlade/~/AuthMethodsActivity/menuId/AuthMethodsActivity)를 활용해 최종 사용자의 강력한 인증에 대한 준비도를 확인할 수 있습니다.**

아래 세 가지 보고서를 살펴보세요:

- Users capable of Azure multifactor authentication.
- Users capable of Passwordless authentication.
- Users capable of self-service password reset.

![Usage and Insight](image-3.png)

모든 사용자는 강력한 인증을 받을 준비가 되어 있어야 합니다. 그렇지 않은 경우, 고객에게 Identity Protection registration 정책을 이용하도록 권장하십시오.

인증 방식별로 등록된 사용자를 검토하십시오. 모든 사용자가 MS Authenticator를 등록한 상태여야 합니다.

![Users registered by authentication method](image-2.png)

고객에게 [Registration Campaign]()을 실행하여 사용자들이 MS Authenticator를 구성하도록 유도하십시오.

>**NOTE**
>
>Registration Campaign은 전화 통화나 SMS 같은 약한 MFA 방식으로 이미 등록한 사용자들에게 영향을 준다는 점을 기억하세요.

등록에 대한 더 많은 정보가 필요하다면 [User registration details](https://entra.microsoft.com/#view/Microsoft_AAD_IAM/AuthenticationMethodsMenuBlade/~/UserRegistrationDetails/menuId/AuthMethodsActivity)를 사용하세요.

![User registration details](image-4.png)

**이제 단일 요소 인증과 다중 요소 인증의 비율을 확인해 볼 시간입니다.**

![Sign-ins by authentication requirement](image-5.png)

>**NOTE**
>
>인증 요구 사항별 로그인(Sign-ins by authentication requirement)은 Microsoft Entra ID에서 단일 요소 인증과 다중 요소 인증이 각각 요구된 성공적인 사용자 대화형 로그인 수를 보여줍니다.
>타사 MFA 공급자에 의해 MFA가 적용된 로그인은 포함되지 않습니다.

다음 기능들을 고객이 활성화하도록 검토하고 권장하세요:

- [ ] **의심스러운 활동 신고**
- [ ] **시스템 기본 다중 요소 인증(System-preferred multifactor authentication)**

![Authentication methods | Settings](image-6.png)

>TODO: 위 내용에 대한 설명을 더 추가

**알려진 위치가 정의되어 있는지 확인하세요.**

조건부 액세스에서 명명된 위치(named locations)를 구성하고, VPN 대역을 Defender for Cloud Apps에 추가하는 것이 중요합니다.
신뢰되거나 알려진 위치로 표시된 명명된 위치에서 발생한 로그인은 Microsoft Entra ID Protection의 위험 계산 정확도를 향상시킵니다.
이러한 위치에서 인증하면 사용자의 위험도가 낮아지며, 이는 환경에서 특정 탐지 항목의 오탐(false positive)을 줄이는 데 도움이 됩니다.

## Basic Conditional Access Policies

우리는 최소한 고객이 다음과 같은 보호 조치를 갖추고 있어야 한다고 믿습니다.
아래 시나리오를 다루는 조건부 액세스 정책(CAP)이 있는지 고객과 함께 검토하세요.

- **관리자에 대한 강력한 인증**

    MFA는 최소 요구사항입니다.
    
    필요하다면 인증 강도(Authentication Strength)를 활용해 더 강력한 인증(예: 피싱 저항 MFA, 패스워드리스 MFA 등)을 권장할 수 있습니다.

- **보안 정보(Security info) 등록 보호**

    강력한 인증, 신뢰할 수 있는 위치, 규정 준수 장치(compliant device) 등의 사용을 요구하도록 구성할 수 있습니다.

- **디바이스 등록 및 Intune 등록 보호**

    Entra에서의 디바이스 등록(Entra Join, Entra Registered)과 Intune 등록 역시 보호되어야 합니다.

    여기에는 강력한 인증, 신뢰할 수 있는 위치 등이 포함될 수 있습니다.

- 레거시 인증 차단*

    기본 인증(Basic Authentication)은 모든 테넌트의 Exchange Online에서 이미 비활성화되어 있으며 다시 활성화할 수 없습니다. 그럼에도 불구하고 Entra ID 수준에서 레거시 인증을 차단하는 것을 권장합니다.

    레거시 인증 사용 식별: [Sign-in logs]() 또는 [legacy authentication workbook]()을 통해 레거시 인증 사용 여부를 파악할 수 있습니다.

- 위험한 로그인(Risky Sign-in): 강력한 인증, 규정 준수 장치(compliant device), Conditional Access App Control 등을 권장할 수 있습니다.

- 위험한 사용자(Risky User): SSPR, 규정 준수 장치, 차단 등의 조치가 필요할 수 있습니다.

- 모든 사용자에 대해 MFA 요구: 모든 사용자는 어떤 리소스에 접근하더라도 강력한 인증을 충족해야 합니다. 고객이 모든 사용자에게 강력한 인증을 적용하는 조건부 액세스 정책(CAP)을 보유하고 있는지 확인하세요.


