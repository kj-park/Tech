---
title: Azure Active Directory Connect Sync Accounts
filename: Microsoft365\Hybrid\AAD-Connect-Sync-Accounts.md
ms.date: 2012.04.29
---

# Azure Active Directory Connect Accounts




![AAD-Connect-Accounts]({{ site.url }}/Microsoft365/images/Hybrid/AAD-Connect-Accounts.png?raw=true)

| Account | Description |
|--|--|
| AD DS Connector Account| On-Premises Active Directory에 read/write하기 위해 사용 |
| ADSync service Account | Synchronization Service를 실행하고 SQL Database에 접근하기 위해 사용 |
| Azure AD Connector Accounts | Azure AD에 정보를 write하기 위해 사용 |


## Changing the AD DS account password

![AAD-connector-adds-account-password](images/AAD-connector-adds-account-password.png)

## Changing the ADSync service account password

Managed Service Account를 사용하기에 별도 변경 필요 없음(?)

## Change Azure AD Connector account