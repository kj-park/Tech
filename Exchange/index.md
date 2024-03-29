---
layout: default
title: [Exchange]
filename: Exchange/index.md
ms.date: 2023.04.20
---

# Exchange

On-Premier 기반의 Exchange Server를 통해 Messaging 시스템을 구축 및 설정, 관리에 대한 기술들을 명시합니다.
엔지니어 관점에서 steps들을 설명하며 연관 기술에 대하여도 함께 설명합니다. 또한 각 내용들과 Powershell 기반 구성이 가능한 부분이 있다면 script도 같이 공유합니다.

> [!NOTE] 계속적으로 업데이트 예정입니다.

## OnPremises Exchange Server

### Deploy Exchange Server

OnPremises 기반의 Exchange Server 구축에 대하여 기술합니다.

Exchange 2013/2016/2019 서버를 구축하는 과정들에 대하여 기술합니다.

- [Deploy Exchange 2013](OnPremises/Deploy-Exchange-2013)

- [Deploy Exchange 2016]

- [Deploy Exchange 2019]

- [Deploy Exchange Edge](OnPremises/Deploy-Exchange-Edge)

### Configure Exchange Server

Exchange 각 버전 별 설치 후 mail flow 및 client access, High Availability 구성에 대하여 설명합니다. 버전과 크게 다르지 않고 유사하기에 통합하여 기술합니다.

- [Configure mail flow and client access for Exchange Server](OnPremises/Configure-mail-flow-and-client-access-for-Exchange-Server)

    - **SCRIPT**: [Configure-ExchangeMailFlowClientAccess.ps1](https://github.com/kj-park/tech/blob/main/Exchange/src/Configure-ExchangeMailFlowClientAccess.ps1)

- [Configure High availability](OnPremises/Configure-High-availability)

    - **SCRIPT**: [Configure-Exchange-DAG.ps1](https://github.com/kj-park/tech/blob/main/Exchange/src/Configure-Exchange-DAG.ps1)

- [Configure Exchange Hybrid](Configure-Exchange-Hybrid)

- Integration with SharePoint and Skype

- [Deploy ARR as Reverse Proxy for Exchange](OnPremises/Deploy-ARR-as-R-Proxy-for-Exchange)

### Manage Exchange Server

Exchange 인프라를 관리에 필요한 주제들에 대하여 기술합니다.

- [Maintenance or Update Exchange DAG members](OnPremises/Maintenance-or-Update-DAG-Members)

- [Manage MailboxDatabases and Mailbox]

- [Migration on Exchange Infrastructure]
    - [Between On-Premises]
    - [From On-Premises To Exchange Online (On-Boarding Migration)]

- [Manage Address Books (OAB, Global Address List, Address List Address Book Policy)]
    - [Split Address Book]
    - [Hierarchical Address Books]

- [Sharing and Organization Relationship]

## Exchange Online

- [Configure Exchange Hybrid](Configure-Exchange-Hybrid)

- Migrating Mailbox on Exchange Hybrid Environments
    - [Exchange Hybrid On-Boarding Migration](Online/Exchange-Hybrid-On-Boarding-Migration)
    - [Importing PST file to Microsoft 365](Online/Importing-PST-file-to-Microsoft-365)
    - [Migrating your IMAP mailboxes to Microsoft 365](Online/Migrating-your-IMAP-mailboxes-to-Microsoft-365)
    - [Exchange Hybrid Off-Boarding Migration](Online/Exchange-Hybrid-Off-Boarding-Migration)

- [Exchange Online Quarantine](Online/Exchange-Online-Quarantine)

- [Configure Microsoft 365 Groups with on-premises Exchange Hybrid](Online/Configure-Microsoft-365-Groups-with-on-premises-Exchange-Hybrid)

- [Create safe sender lists in EOP](Online/Create-safe-sender-lists-in-EOP)

## Conceptual Documents

- [How to use Sender Policy Framework (SPF) to prevent spoofing](Conceptual/Sender-Policy-Framework)

- [Accepted domains and Email Address Policies in Exchange Server](Conceptual/Accepted-domains-and-Email-Address-Policies-in-Exchange-Server)

- [Understanding the Transport Pipeline](Conceptual/Understanding-the-Transport-pipeline)

- [Digital certificates and encryption in Exchange Server](Conceptual/Certificate-and-Encrpytion-in-Exchange-Server)

- [Exchange Mail Flow and Recipient Types](Conceptual/Exchange-Mail-Flow-and-Recipient-Types)

## Etc

- [Mak Public Certificate](Etc/Make-Public-Certificate)

---
