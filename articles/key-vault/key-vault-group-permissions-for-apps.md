---
title: Conceder permissão para muitos aplicativos para aceder a um cofre de chave do Azure | Documentos da Microsoft
description: Saiba como pode conceder permissão para muitos aplicativos para aceder a um cofre de chaves
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 14e6e8bb723eb236f8fb315454b8697a3bd947ef
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286432"
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Conceder permissão para muitos aplicativos para aceder a um cofre de chaves

## <a name="q-i-have-several-applications-that-need-to-access-a-key-vault-how-can-i-give-these-applications-up-to-1024-access-to-key-vault"></a>P: Tenho vários aplicativos que precisam de aceder a um cofre de chaves, como posso dar estas aplicações acesso (até 1024) para o Key Vault?

Política de controlo de acesso do Key Vault suporta até 1024 entradas. No entanto pode criar um grupo de segurança do Azure Active Directory. Adicionar todos os principais de serviço associado a este grupo de segurança e, em seguida, conceder acesso a este grupo de segurança para o Key Vault.

Seguem-se a pré-requisitos:
* [Instalar o módulo do Azure Active Directory V2 PowerShell](https://www.powershellgallery.com/packages/AzureAD).
* [Instalar o Azure PowerShell](/powershell/azure/overview).
* Para executar os seguintes comandos, precisa de permissões para criar/editar grupos no inquilino do Azure Active Directory. Se não tiver permissões, poderá ter de contactar o administrador do Azure Active Directory.

Agora, execute os seguintes comandos no PowerShell.

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionsToKeys all –PermissionsToSecrets all –PermissionsToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Se tem de conceder um conjunto diferente de permissões para um grupo de aplicações, crie um grupo de segurança do Azure Active Directory separado para essas aplicações.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como [proteger o seu Cofre de chave](key-vault-secure-your-key-vault.md).
