---
title: Conceder permissão para muitas aplicações para aceder a um cofre de chaves do Azure | Microsoft Docs
description: Saiba como conceder permissão para muitas aplicações para um cofre de chaves de acesso
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
ms.openlocfilehash: ddeaf184138bd48d324799ddb45248b0a0ee8eeb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30174985"
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Conceder permissão para muitas aplicações para um cofre de chaves de acesso

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>P: Posso ter vários (mais de 16) aplicações que precisem de aceder um cofre de chaves. Uma vez que o Key Vault só permite 16 entradas de controlo de acesso, o que posso fazer?

Política de controlo de acesso do Cofre de chaves apenas suporta 16 entradas. No entanto, pode criar um grupo de segurança do Azure Active Directory. Adicione todos os principais de serviço associado a este grupo de segurança e, em seguida, conceder acesso a este grupo de segurança ao Cofre de chaves.

Seguem-se os pré-requisitos:
* [Instalar módulo do Azure Active Directory V2 PowerShell](https://www.powershellgallery.com/packages/AzureAD).
* [Instalar o Azure PowerShell](/powershell/azure/overview).
* Para executar os seguintes comandos, necessita de permissões para criar/editar grupos no inquilino do Azure Active Directory. Se não tiver as permissões, poderá ter de contactar o administrador do Azure Active Directory.

Agora, execute os seguintes comandos do PowerShell.

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

Se precisar de conceder um conjunto diferente de permissões para um grupo de aplicações, crie um grupo de segurança do Azure Active Directory separado para essas aplicações.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como [proteger o seu Cofre de chaves](key-vault-secure-your-key-vault.md).
