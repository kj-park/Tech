---
layout: default
title: [Hybrid Identity]
URL: Delivery/Onboarding-Accelerator-Zero-Trust-Security-for-Identity/Hybrid-Identity
Path: Delivery\Onboarding-Accelerator-Zero-Trust-Security-for-Identity\Hybrid-Identity.md
ms.date: 06/23/2026
---

# Hybrid Identity

*Microsoft Entra ID enables strong authentication, a point of integration for endpoint security, and the core of your user-centric policies to guarantee least-privileged access. Microsoft Entra ID's Conditional Access capabilities are the policy decision point for access to resources based on user identity, environment, device health, and risk verified explicitly at the point of access. We will show how you can implement a Zero Trust identity strategy with Microsoft Entra ID.*

![Zero Trust First Deployment Objectives](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/hybrididentity.png)

### Prerequisites

- Microsoft Entra ID free license or better

### Stakeholders

When beginning your deployment planning for a new capability, it's important to include key stakeholders across your organization. We recommend that you identify and document the person or people who fulfil each of the following roles, and work with them to determine their involvement in the project.

| Role | Description |
| --- | --- |
| End-user | A representative group of users for which the capability will be implemented. Often previews the changes in a pilot program. |
| IT Support Manager | IT support organization representative who can provide input on the supportability of this change from a helpdesk perspective. |
| Identity Architect or Azure Global Administrator | Identity management team representative in charge of defining how this change is aligned with the core identity management infrastructure in your organization. |
| Application Business Owner | The overall business owner of the affected application(s), which may include managing access. May also provide input on the user experience and usefulness of this change from an end user's perspective. |
| Security Owner | A representative from the security team that can sign out that the plan will meet the security requirements of your organization. |
| Compliance Manager | The person within your organization responsible for ensuring compliance with corporate, industry, or governmental requirements. |

## Discuss Authentication Method

*Choosing the correct authentication method is the first concern for hybrid organizations wanting to move their apps to the cloud. As Identity is the new control plane of IT security, the authentication method is a critical component of an organization’s presence in the cloud: it controls access to all cloud data and resources and it's the foundation of all the other advanced security and user experience features in Microsoft Entra ID.*

### Overview

- Briefly introduce authentication options for new deployments
- For existing deployments review authentication method choice and configuration
- Discuss improvements or possible changes (e.g. migrate from Federated to Cloud Authentication)
- Plan next steps, evaluate security considerations for every method, highlight requirements to properly secure environment by design.
- Consider on-premises UPNs flagging non-routable ones, start considering impact for AlternateID scenarios (e.g. [Microsoft Entra hybrid join limitations](https://docs.microsoft.com/en-us/azure/active-directory/devices/hybrid-azuread-join-plan#review-on-premises-ad-users-upn-support-for-hybrid-azure-ad-join)). Please note that Windows Hello for Business **always** requires a routable on-premises UPN!
- Review the documentation on choosing an authentication method - Authentication for Microsoft Entra ID hybrid identity solutions - Active Directory | Microsoft Docs

![Password Hash Sync](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/passwordsync.png)

### Password Hash Sync

- Focus on key points of [Password Hash Synchronization](https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/choose-ad-authn#cloud-authentication-password-hash-synchronization):

    - **Simple,** Password hash synchronization requires the least effort regarding deployment, maintenance, and infrastructure. Microsoft Entra ID can handle sign-in for users **without relying on on-premises components** to verify passwords
    - Enables [leaked credential detection](https://docs.microsoft.com/en-us/azure/active-directory/identity-protection/concept-identity-protection-risks#user-linked-detections) for hybrid accounts. *(Microsoft works alongside dark web researchers and law enforcement agencies to find publicly available username/password pairs)*
    - PHS can also be enabled as a **manual failover** for the other authentication methods
- Describe in detail [how PHS works](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) and discuss [security considerations](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization#security-considerations):

    - Service account must have *Replicate Directory Changes* and *Replicate Directory Changes All* permissions on Active Directory
    - Per-user *Salt* and *PBKDF2 function* (1000 iterations of HMAC-SHA256 keyed hashing algorithm)
- If relevant, discuss how to enable [selective password hash synchronization](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-selective-password-hash-synchronization)
- Clarify the limitations ([password policy consideration](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization#password-policy-considerations), account expiration, ... )
- Note that PHS and Password Write-Back are different and independent features
- If requested show how to [selectively configure PHS](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-selective-password-hash-synchronization), mention the different strategies.

![Microsoft Entra ID hybrid identity with Password hash synchronization](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/image6.png)

### Pass-through Authentication

- [Pass-through Authentication](https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/choose-ad-authn#cloud-authentication-pass-through-authentication) is a **Simple and consistent** Cloud Authentication experience, same as PHS

    - Needs lightweight agents to be installed on-premises (highly-available, automatically receives updates). The agent attempts to validate the username and the password against on-premises Active Directory by using *the Win32 LogonUser API,* thus generating authentication logs also on-premises.
    - Fully complies with all user-level Active Directory security policies such as account expired, disabled account, password expired, account locked out, and sign-in hours on each user sign-in
- Discuss [key benefits and features](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta). Also mention:

    - [Supported scenarios](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-current-limitations#supported-scenarios)
    - Sign-in usernames can be either the on-premises default username (*userPrincipalName*) or another attribute configured in Entra Connect (*known as Alternate ID*).
    - If relevant, discuss Multi-forest environments support and forest trusts requirement (e.g. Kerberos name suffix routing is correctly configured.)
- Talk about **security considerations**:

    - Note that the agent only makes outbound connections from within the network. (no requirement for DMZ or open inbound ports).
    - Discuss how [Microsoft Entra ID Smart-Lockout](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-password-smart-lockout) prevents brute force attacks and recommended configuration.
    - Mention that On-premises passwords are never stored in the cloud in any form, PTA ensures that the password validation doesn't happen in the cloud.

> 
> ![Microsoft Entra ID hybrid identity with Pass-through
> Authentication](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/image7.png)

- Briefly describe [authentication steps](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-how-it-works#how-does-azure-active-directory-pass-through-authentication-work):

![Pass-through
Authentication](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/image8.png)

- If requested, provide [security details](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-security-deep-dive):

    - Secure agent initialization and communication with the cloud
    - Sign-in requests processing flow
    - Operational security of the Authentication Agents
    - Auto-update of the Authentication Agents

### Seamless SSO

![Seamless Single Sign On - Web app
flow](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/image9.png)

- On Windows 10, Windows Server 2016 and later versions, it's recommended to use SSO via primary refresh token (PRT). For windows 7 and 8.1 it's possible to use Seamless SSO. [Briefly discuss difference](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-faq#what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-join-and-seamless-sso-).
- Seamless SSO leverages *Kerberos SSO*, uses the *securityIdentifier* claim in the Kerberos ticket to look up the corresponding user object in Microsoft Entra ID.
- Discuss Group Policy requirement to [update different Browsers *Intranet/Safe zone*](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-quick-start#step-3-roll-out-the-feature)*.*
- If requested detail [how Seamless SSO works](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-how-it-works#how-does-seamless-sso-work):

    - A computer account (*AZUREADSSOACC*) is created in each synchronized AD forest. A number of Kerberos service principal names (SPNs) are associated with the account.
    - The computer account's Kerberos decryption key is shared securely with Microsoft Entra ID. If there are multiple AD forests, each computer account will have its own unique Kerberos decryption key.
- Mention [known issues/limitations](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/tshoot-connect-sso#known-issues) and common troubleshooting patterns for Seamless SSO
- Security Considerations:

    - Protect the *AZUREADSSOACC* account limiting management only to *Domain Admins*, ensure that Kerberos delegation on the computer account is disabled.
    - Ensure the account is safe from accidental deletions . Treat the Kerberos decryption key on the computer account as sensitive, [roll over the key at least every 30 days.](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-faq#how-can-i-roll-over-the-kerberos-decryption-key-of-the--azureadsso--computer-account-)
    - Set the Kerberos encryption type for the account to AES256\_HMAC\_SHA1, or one of the AES types vs. RC4 for added security (the encryption type is stored on the msDS-*SupportedEncryptionTypes* attribute of the account)
    - Consider that in some scenarios common Active Directory attacks might allow adversaries to pivot from on-premises to cloud via Kerberos tickets (Pass-the-Ticket, Golden Ticket, ...).

### Federated Authentication

- [Federated Authentication](https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/choose-ad-authn#federated-authentication-1) **requires effort and complexity**: a federated authentication system relies on an external trusted system to authenticate users. Deployment, management, performance monitoring and proper security configuration of the federated system are crucial and non-trivial tasks.

> 
> ![Graphical user interface Description automatically
> generated](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/image10.png)

- Mention **Specific and Advanced scenarios** that are covered:

    - Windows Hello for Business [Cert Trust model](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-deployment-guide#deployment-and-trust-models) (ADFS)
    - Sign in that requires a sAMAccountName, for example DOMAIN\username, instead of a User Principal Name (UPN)
    - On-premises MFA servers or third-party multifactor providers (*be aware of [custom controls](https://docs.microsoft.com/en-us/azure/active-directory/conditional-access/controls) and [upcoming evolution](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/whats-new-archive#upcoming-changes-to-custom-controls)*)
    - Authentication by using [third-party authentication solutions](https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/how-to-connect-fed-compatibility). Be mindful of the possible limitations in this scenario (e.g. support limitations, [WS-Trust](https://docs.microsoft.com/en-us/azure/active-directory/devices/hybrid-azuread-join-plan#federated-environment))
- Note that **user experience** is more complex, customizable:

    - involves an additional redirect from Microsoft Entra ID to the external trusted system (e.g. ADFS)
    - adapt and configure the access to the federation farm to meet specific security requirements, implement specific SSO methods or authentication steps
- Talk about [Security Considerations](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/best-practices-securing-ad-fs): a federated IdP such as ADFS is a full Tier0 system, potentially exposed to Internet, that needs a proper security configuration:

    - Enable Smart Lockout, to prevent brute force and DDoS attacks
    - Ensure the installed certificates are protected against theft and get renewed before expiring. Protect signing keys/certificates in a hardware security module (HSM) attached to AD FS.
    - Properly configure logging and enable tools like Microsoft Entra ID Connect Health and Defender for Identity
    - Update to the latest AD FS version for security and logging improvements
    - Disable unnecessary endpoints (e.g. WS-Trust Windows endpoints from extranet)
    - As an example, discuss how recent attacks leveraged ADFS to [pivot from on-premises to cloud](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/understanding-quot-solorigate-quot-s-identity-iocs-for-identity/ba-p/2007610) (e.g. Solorigate).

## Implement changes or setup the chosen authentication method

Delivery guidance for Migrating from ADFS to PTA or PHS is available in the IPKit

### Define a candidate authentication method

- See the [table comparing methods](https://docs.microsoft.com/en-gb/azure/active-directory/hybrid/choose-ad-authn#comparing-methods) across features, requirements and limitations
- Review [official recommendations](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/choose-ad-authn#recommendations)

### Pilot migration to Cloud Authentication via Staged rollout

- Ref. [Entra Connect: Cloud authentication via staged rollout | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-staged-rollout)
- Discuss target configuration and intended benefits
- Introduce Staged rollout as an option, mention [supported and unsupported scenarios](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-staged-rollout#supported-scenarios). Please note that it's possible to enable Staged rollout for either PHS or PTA, but not both.
- Highlight again *non-routable* on-premises UPNs and related impact for Alternate-ID managed scenarios (e.g. [Microsoft Entra hybrid join limitations](https://docs.microsoft.com/en-us/azure/active-directory/devices/hybrid-azuread-join-plan#review-on-premises-ad-users-upn-support-for-hybrid-azure-ad-join))
- For staged rollout ensure that *pre-work* has been completed:

    - For PHS, ensure PHS has been enabled via Entra Connect and that a [full password hash sync cycle has run](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/tshoot-connect-password-hash-synchronization#understand-the-results-of-the-troubleshooting-task).
    - For PTA, install at least 2 PTA agents for a highly available deployment and configure Microsoft Entra ID smart lockout
    - For Seamless SSO, ensure that the *AZUREADSSOACC* has been created.
- [Validate sign-ins](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-staged-rollout#validation) (*keep in mind [unsupported scenarios](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-staged-rollout#unsupported-scenarios)*)

### Implement defined Authentication Method (Green Field Only)

- [Password Hash Sync](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization#enable-password-hash-synchronization)
- [Pass Through Authentication](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-pta-quick-start#deploy-azure-ad-pass-through-authentication)
- [Entra Connect: Seamless Single Sign-On](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sso-quick-start)
- Use the Troubleshooting guides as a reference if needed:

    - [Troubleshoot password hash synchronization with Entra Connect sync | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/tshoot-connect-password-hash-synchronization)
    - [Entra Connect: Troubleshoot Pass-through Authentication | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/tshoot-connect-pass-through-authentication)
    - [Microsoft Entra Connect: Troubleshoot Seamless Single Sign-On | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/tshoot-connect-sso)

### Discuss how to perform a complete Migration to Cloud Authentication

- Ref. [Migrate from federation to cloud authentication in Azure Active Directory | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/migrate-from-federation-to-cloud-authentication)
- [Plan](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/migrate-from-federation-to-cloud-authentication#plan-the-project) ahead.
- Please note that the legacy option to "Convert Users" is deprecated and not needed anymore (either via Entra Connect or Powershell). Forcing the conversion following outdated documentation, tools and blogs could also create unexpected results and introduce a 72 hours delay for rollback. (e.g. [**do NOT use!**](https://docs.microsoft.com/en-us/microsoft-365/enterprise/turn-off-directory-synchronization?view=o365-worldwide))

## Filter out service accounts and on-prem privileged accounts in Microsoft Entra Connect

> 
> **Explain Why** : *Only bring the identities you need, use going to the cloud as an opportunity to leave behind service accounts that only make sense on-premises. Also leave on-premises privileged roles behind.*

- Entra Connect will already filter out by design Users where *isCriticalSystemObject* is set to True (e.g. built). See [other objects excluded by default](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration#user-out-of-box-rules).
- Discuss [filtering strategies](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering#filtering-options):

    - OU and Attribute Based
    - All users with exclusions or only approved (*negative* and *positive* filtering)
- Mention some strategies to identify on-premises administrative accounts

    - adminCount=1 - [Appendix C - Protected Accounts and Groups in Active Directory | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory)
    - Group Membership - [Appendix B - Privileged Accounts and Groups in Active Directory | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-b--privileged-accounts-and-groups-in-active-directory)
- Talk about how to Identify Service Accounts -- no direct way to surface service accounts, but [many strategies](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-user-on-premises#find-on-premises-user-accounts-used-as-service-accounts):

    - Accounts trusted for delegation
    - Accounts that have service principal names
    - Accounts with passwords that are set to never expire
- Excluding other unwanted objects (e.g. test accounts, duplicate accounts, ...)
- When changing Entra Connect rules recommend to:

    - Export configuration prior to changes and use the [Entra Connect Configuration Documenter](https://github.com/Microsoft/AADConnectConfigDocumenter) as a reference of before/after.
    - [Verify expected Object and Attribute modifications](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-staging-server#configuration) to avoid unexpected deletions or changes

## Optimize, Upgrade and Harden Entra Connect Installation

> 
> The Entra Connect server must be treated as a Tier 0 component as documented in the Active Directory administrative tier model.
> 
> Also Entra Connect performances are critical to the Hybrid Identity Model as many identity and security features depend on a timely and dependable synchronization.

- Discuss factors influencing [Entra Connect Performances](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/plan-connect-performance-factors)

    - Optimize with better [filtering](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/plan-connect-performance-factors#filtering)
    - [Microsoft Entra ID, AD and SQL](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/plan-connect-performance-factors#azure-ad-connect-dependency-factors) specific factor
    - Review [Hardware requirements](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-prerequisites#hardware-requirements-for-azure-ad-connect)
- Review Entra Connect Service accounts privileges, permissions and lifecycle:

    - [Entra Connect: ADSync service account | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/concept-adsync-service-account)
    - [Entra Connect: Accounts and permissions | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/reference-connect-accounts-permissions)
- Define an Entra Connect update lifecycle:

    - Keep deployment of Entra Connect up to date - [Entra Connect: Version release history | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/reference-connect-version-history)
    - If synchronization rules are customized, define a standard process to review rules after an upgrade, before resuming sync.
    - Optionally apply updates to the *staging server* and [verify changes](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-staging-server#verify)
- Enable TLS 1.2 - [Entra Connect: TLS 1.2 enforcement for Microsoft Entra Connect | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/reference-connect-tls-enforcement)
- Treat Entra Connect servers and relevant *connector accounts* as Tier0 and protect them accordingly. Consider some sensitive scenarios:

    - Control accounts attributes and potentially privileges in the cloud
    - Write back to on-prem *msDS-KeyCredentialLink* adding credentials to any account.
    - Abuse of the *Replicate Directory Changes All* privilege (e.g. export Active Directory Password Hashes)
    - Abuse of Password Reset permissions (if Self Service Password Reset is enabled)
    - Entra Connect *connector accounts* permissions might also extend to *Protected Accounts* if privileged accounts were not excluded from the sync.
- Review additional/generic [Entra Connect hardening guidelines](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-prerequisites#harden-your-azure-ad-connect-server)

## Microsoft Entra Cloud Sync

[Microsoft Entra Cloud Sync](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/what-is-cloud-sync) is a new offering from Microsoft designed to meet and accomplish your hybrid identity goals for synchronization of users, groups, and contacts to Microsoft Entra ID

- Synchronizes users, groups, and contacts.
- Utilizes Microsoft Entra cloud provisioning agent.
- Replaces Microsoft Entra Connect application.

![Microsoft Entra Cloud Sync Overview](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/cloudsync.png)

### Benefits

- Support for synchronizing to a Microsoft Entra tenant from a multi-forest disconnected Active Directory forest environment: The common scenarios include merger & acquisition (where the acquired company's AD forests are isolated from the parent company's AD forests), and companies that have historically had multiple AD forests.
- Simplified installation with light-weight provisioning agents: The agents act as a bridge from AD to Microsoft Entra ID, with all the sync configuration managed in the cloud.
- Multiple provisioning agents can be used to simplify high availability deployments, particularly critical for organizations relying upon password hash synchronization from AD to Microsoft Entra ID.
- Support for large groups with up to 50,000 members. It's recommended to use only the OU scoping filter when synchronizing large groups.

### How is Microsoft Entra Cloud Sync different from Microsoft Entra Connect Sync?

With Microsoft Entra Cloud Sync, provisioning from AD to Microsoft Entra ID is orchestrated in Microsoft Online Services. An organization only needs to deploy, in their on-premises or IaaS-hosted environment, a light-weight agent that acts as a bridge between Microsoft Entra ID and AD. The provisioning configuration is stored in Microsoft Entra ID and managed as part of the service.

For more :[Comparison between Microsoft Entra Connect and cloud sync](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/what-is-cloud-sync)

### Prerequisites for Microsoft Entra Cloud Sync

- [Discuss about the Cloud provisioning agent requirements](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/how-to-prerequisites?tabs=public-cloud#cloud-provisioning-agent-requirements)
- [Group Managed Service Accounts](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/how-to-prerequisites?tabs=public-cloud#group-managed-service-accounts)
- [Firewall and Proxy requirements](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/how-to-prerequisites?tabs=public-cloud#firewall-and-proxy-requirements)
- [NTLM requirement](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/how-to-prerequisites?tabs=public-cloud#ntlm-requirement)
- [Known Limitations](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/how-to-prerequisites?tabs=public-cloud#known-limitations)

### Install the Microsoft Entra provisioning agent

[Discuss installation process for the Microsoft Entra provisioning agent and how to initially configure it in the Microsoft Entra admin center.](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/how-to-install)

### Install the Microsoft Entra provisioning Agent by using a CLI and PowerShell

[Discuss how to install the Microsoft Entra provisioning agent by using PowerShell cmdlets.](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/how-to-install-pshell)

### Microsoft Entra Cloud Sync supported topologies and scenarios

Describes various on-premises and Microsoft Entra topologies that use [Microsoft Entra Cloud Sync.](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/plan-cloud-sync-topologies)

### Migrate Microsoft Entra Connect Sync group writeback V2 to Microsoft Entra Cloud Sync

If client is using Microsoft Entra Connect sync, describes how to [migrate group writeback](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/migrate-group-writeback) using Microsoft Entra Connect Sync (formerly Azure AD Connect) to Microsoft Entra Cloud Sync.

- [Prerequisites](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/migrate-group-writeback#prerequisites)
- Step 1 - Copy adminDescription to msDS-ExternalDirectoryObjectID
- Step 2 - Place the Microsoft Entra Connect Sync server in staging mode and disable the sync scheduler
- Step 3 - Create a custom group inbound rule
- Step 4 - Create a custom group outbound rule
- Step 5 - Use PowerShell to finish configuration
- Step 6 - Remove the Microsoft Entra Connect Sync server from staging mode
- Step 7 - Configure Microsoft Entra Cloud Sync

### Cloud sync deep dive - how it works

- [Discuss about Components](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/concept-how-it-works)
- [Initial setup](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/concept-how-it-works#initial-setup)
- [Agent installation](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/concept-how-it-works#agent-installation)
- [What is System for Cross-domain Identity Management (SCIM)?](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/concept-how-it-works#what-is-system-for-cross-domain-identity-management-scim)
- [Synchronization flow](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/concept-how-it-works#synchronization-flow)
- [Supported scenarios](https://learn.microsoft.com/en-us/entra/identity/hybrid/cloud-sync/concept-how-it-works#supported-scenarios)

## Establish your Identity Foundation with Microsoft Entra ID

Objective is to provide a high-level process to migrate apps and awareness on the organizational and technical challenges involved.

> 
> Put Microsoft Entra ID in the path of every access request. This connects every user and every app or resource through one identity control plane and provides Microsoft Entra ID with the signal to make the best possible decisions about the authentication/authorization risk. In addition, single sign-on and consistent policy guardrails provide a better user experience and contribute to productivity gains.

*Microsoft Entra ID authentication services support an array of **widely adopted and emerging standards**:*

- Legacy federation protocols (WS-Fed, SAML)
- Classic authentication protocols like Kerberos or Header-based AuthN.
- *Modern Authentication* protocols (OAuth2.0, OpenID Connect)
- Newest specifications like FIDO Alliance's FIDO2
- Mobile and IOT devices friendly authentication flows (e.g. [Device Authorization Grant](https://tools.ietf.org/html/rfc8628))

Application onboarding is possible both **for 3rd party SaaS applications** as well as for **custom developed line of business applications** (using Microsoft [MSAL](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-overview) libraries or 3rd party standard code). Single sign-on integration and automated **user provisioning/de-provisioning** (via [SCIM](http://www.simplecloud.info/)) are fully supported.

### Migrate application authentication to Microsoft Entra ID

- [**Planning**](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/plan-sso-deployment#considerations-for-federation-based-sso): be sure to mention communications planning and SSO options
- [Discuss](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/plan-sso-deployment#considerations-for-federation-based-sso) differences between *federation-based* and *password-based* SSO
- Introduce different protocols available (WS-Fed, SAML, OAuth2.0/OIC) and start a discussion about how to [choose a SSO method](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/sso-options#choosing-a-single-sign-on-method).
- Ref. [Resources for migrating apps to Microsoft Entra ID | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migration-resources)

![Decision flowchart for single sign-on
method](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/image11.png)

- Introduce the various [phases of migration](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#the-phases-of-migration)

![A diagram of the phases of
migration](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/media/image12.jpeg)

- As always, highlight the [stakeholders that need to be involved](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#assemble-the-project-team)
- Discuss [communications](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#plan-communications) - *Effective business engagement and communication is the key to success. It is important to give stakeholders and end-users an avenue to get information and keep informed of schedule updates*
- Discover Apps:

    - Introduce the *ADFS [application report](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-adfs-application-activity)* to discover ADFS applications that can be migrated and evaluate the readiness of the application to be migrated. If relevant, review results and discuss findings.
    - Discuss other options like 3rd party IdPs logs or HTTP Kerberos SPNs registered on-premises
    - Consider automated discovery tools (e.g. [*cloud discovery*](https://docs.microsoft.com/en-us/cloud-app-security/set-up-cloud-discovery)) to monitor known and "shadow" usage of SaaS Apps.
    - Review any remaining [manual strategy](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#using-manual-processes) to complete the discovery.
- Discuss [categories of apps discovered](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#type-of-apps-to-migrate) and recommended strategies to migrate. Highlight apps that can be deprecated outright (*functionality is highly redundant, no business owner or no usage*)
- Introduce model for [classification and prioritization of apps](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#phase-2-classify-apps-and-plan-pilot) - *One way to think about this is along the axes of business criticality, usage, and lifespan, each of which is dependent on multiple factors.*![](https://eng.ms/content/docfxhtml/159fb703-a3f9-4626-b4fb-b20916b573f7/20260622T195052376Z/mip/articles_identity/images/image13.png)
- Mention different [prioritization options](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#prioritize-apps-for-migration) for different scenarios
- Discuss how to [document apps](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#document-your-apps) and plan for specific details, limitations and requirements of each application.
- Analyze security posture requirements and discuss how they can be met with Conditional Access Policies
- [Plan testing](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-application-authentication-to-azure-active-directory#plan-testing) : go through the options available for testing SSO

    - For all SAML and Modern (OAuth2.0/OIC) apps verify custom claims. Review some of the [unsupported claims conditions](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-adfs-application-activity#claim-rule-tests).
    - For OAuth2.0/OIC ensure consent is configured
    - For Password based SSO mention [plugin requirement](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510#download-and-install-the-my-apps-secure-sign-in-extension)
    - For Application Proxy SSO discuss [planning options](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-deployment-plan). Also provide an overview of [Kerberos delegation configuration/troubleshooting](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-troubleshoot) and [Header based authentication configuration](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-configure-single-sign-on-with-headers#configure-single-sign-on).
    - [Security considerations](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-security) for Application Proxy:

        - Outline the "*only outbound connections"* [architecture](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-security#accessing-published-applications) and that first client connections always land on Microsoft Entra ID edge (vs customer on-premises)
        - Lightweight agent, auto update process and auto certificate update every 180 days.
- Discuss proper administrators' delegation and users self-service access configuration
- Talk about audit and sign-ins options available in Microsoft Entra ID to monitor and gain insights on apps usage.

### Scenarios and Activities

- SaaS Applications : Microsoft Entra ID documentation contains a list of tutorials detailing the steps required to onboard SaaS Applications. **This is a recommended starting point to migrate key apps with the least effort.**

    - [SaaS App Integration Tutorials for use with Microsoft Entra ID | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/tutorial-list)
- ADFS federated apps can be moved to Microsoft Entra ID with some planning and considerations:

    - Discuss [specific federation configurations](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-adfs-apps-to-azure#apps-and-configurations-that-can-be-moved-today) that are directly supported, require additional steps or are not supported.
    - Make use of the *[mapping tables](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-adfs-apps-to-azure#map-app-configuration-settings)* to simplify the move.
    - *Please note that moving complex, customer developed LoB apps may require analysis and customizations out of scope for the present document.*
    - [Moving application authentication from AD FS to Microsoft Entra ID | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/migrate-adfs-apps-to-azure)
- Application Proxy -- cover an Initial deployment and app publishing:

    - Provide a brief overview of the [Application Proxy](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy#what-is-application-proxy) feature
    - [Plan](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-deployment-plan) the Application Proxy deployment, highlight [pilot best practices](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-deployment-plan#best-practices-for-a-pilot).
    - Briefly go through concepts, best practices and implementation:

        - [connectors](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-connectors) and [connector groups](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-connector-groups)
        - [load balancing](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-high-availability-load-balancing)
        - Detail how to account for [corporate proxies](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-configure-connectors-with-proxy-servers) configuration for Internet access
        - Discuss how to optimize connector groups to [use closest Application Proxy cloud service](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-network-topology#optimize-connector-groups-to-use-closest-application-proxy-cloud-service-preview)
    - If requested, [provide details](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-security#under-the-hood) on how the connector installs and operates
    - [Follow the tutorial](https://microsoft.sharepoint.com/teams/Proactive-ZeroTrust/Shared%20Documents/PA%20for%20Zero%20Trust%20Identity/Onboard%20ZT%20Security%20for%20Identity/Delivery%20Content/Delivery%20Guide/o%09Tutorial%20-%20Add%20an%20on-premises%20app%20-%20Application%20Proxy%20in%20Azure%20Active%20Directory%20%7C%20Microsoft%20Docs) to add pilot apps to Microsoft Entra ID via Application Proxy
    - Configure Single Sign-on according to the app requirements:

        - [Header Based Authentication](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-configure-single-sign-on-with-headers)
        - [Active Directory integrated Authentication](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-configure-single-sign-on-with-kcd) (Kerberos constrained delegation)
        - [Claims based](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-configure-single-sign-on-on-premises-apps) (SAML)
    - See [Application Proxy FAQ](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-configure-connectors-with-proxy-servers) for specific questions or concerns and refer to the [Troubleshooting reference](https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/application-proxy-troubleshoot) if needed.