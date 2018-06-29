---
title: 'Azure Active Directory dos serviços de domínio: Criar uma conta de serviço geridas de grupo | Microsoft Docs'
description: Administrar o Azure Active Directory Domain Services de domínios geridos
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: maheshu
ms.openlocfilehash: 4b4ce876760d024170020ae3faf618c7e9e76773
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036352"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Criar uma conta de serviço geridas de grupo (gMSA) um domínio gerido dos serviços de domínio do Azure AD
Este artigo mostra como criar contas de serviço geridas num domínio gerido dos serviços de domínio do Azure AD.

## <a name="managed-service-accounts"></a>Contas de serviço geridas
Uma conta de serviço geridas autónomas (sMSA) é uma conta de domínio geridos cuja palavras-passe é gerida automaticamente. Simplifica a gestão de nome principal (SPN) do serviço e permite delegado gestão a outros administradores. Este tipo de conta de serviço geridas (MSA) foi introduzido no Windows Server 2008 R2 e Windows 7.

A conta de serviço geridas de grupo (gMSA) fornece as mesmas vantagens para vários servidores no domínio. Todas as instâncias de um serviço alojado num farm de servidores tem de utilizar o mesmo principal de serviço para protocolos de autenticação mútua funcione. Quando uma gMSA é utilizada como principal de serviço, o sistema operativo Windows gere palavra-passe a conta em vez de depender do administrador.

**Mais informações:**
- [Descrição geral das contas de serviço geridas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Introdução às contas de serviço geridas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Utilizar contas de serviço nos serviços de domínio do Azure AD
Domínios geridos de serviços de domínio do AD do Azure são bloqueados e geridos pela Microsoft. Existem algumas considerações-chave ao utilizar as contas de serviço com os serviços de domínio do Azure AD.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Criar contas de serviço no personalizado unidades organizacionais (UO) no domínio gerido
Não é possível criar uma conta de serviço no incorporada 'AADDC utilizadores' ou 'AADDC computadores' unidades organizacionais. [Criar uma UO personalizada](active-directory-ds-admin-guide-create-ou.md) no seu domínio gerido e, em seguida, crie contas de serviço nessa UO personalizada.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>A chave de raiz de serviços de distribuição de chave (KDS) já está criada previamente
A chave de raiz de serviços de distribuição de chave (KDS) é previamente criada num domínio gerido dos serviços de domínio do Azure AD. Não precisa de criar uma de raiz KDS da chave e não tem privilégios para executar, por isso. Não é possível ver se a chave de raiz KDS no domínio gerido.

## <a name="sample---create-a-gmsa-using-powershell"></a>Exemplo - criar uma gMSA com o PowerShell
O exemplo seguinte mostra como criar uma UO personalizada com o PowerShell. Em seguida, pode criar uma gMSA dentro nessa UO, utilizando o ```-Path``` parâmetro para especificar a UO.

```powershell
# Create a new custom OU on the managed domain
New-ADOrganizationalUnit -Name "MyNewOU" -Path "DC=CONTOSO100,DC=COM"

# Create a service account 'WebFarmSvc' within the custom OU.
New-ADServiceAccount -Name WebFarmSvc  `
-DNSHostName ` WebFarmSvc.contoso100.com  `
-Path "OU=MYNEWOU,DC=CONTOSO100,DC=com"  `
-KerberosEncryptionType AES128, AES256  ` -ManagedPasswordIntervalInDays 30  `
-ServicePrincipalNames http/WebFarmSvc.contoso100.com/contoso100.com, `
http/WebFarmSvc.contoso100.com/contoso100,  `
http/WebFarmSvc/contoso100.com, http/WebFarmSvc/contoso100  `
-PrincipalsAllowedToRetrieveManagedPassword CONTOSO-SERVER$
```

**Documentação de cmdlet do PowerShell:**
- [Cmdlet de novo ADOrganizationalUnit](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [Cmdlet de novo-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Passos Seguintes
- [Criar uma UO personalizada um domínio gerido](active-directory-ds-admin-guide-create-ou.md)
- [Descrição geral das contas de serviço geridas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Introdução às contas de serviço geridas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
