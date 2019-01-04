---
title: Conceder permissão para muitos aplicativos para aceder a um cofre de chaves do Azure - Azure Key Vault | Documentos da Microsoft
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
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: ed0d40ab62a7d75a993ce0bbdd83a5e5ec4a633a
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53998477"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Conceder acesso de aplicações de vários para um cofre de chaves

Política de controlo de acesso pode ser utilizada para conceder acesso de aplicações de vários para um cofre de chaves. Uma política de controlo de acesso pode suportar até aplicativos de 1024 e está configurada da seguinte forma:

1. Crie um grupo de segurança do Azure Active Directory. 
2. Adicione que todos dos aplicativos associados principais de serviço para o grupo de segurança.
3. Conceda o acesso de grupo de segurança ao Cofre de chaves.

Seguem-se a pré-requisitos:
* [Instalar o módulo do Azure Active Directory V2 PowerShell](https://www.powershellgallery.com/packages/AzureAD).
* [Instalar o Azure PowerShell](/powershell/azure/overview).
* Para executar os seguintes comandos, precisa de permissões para criar/editar grupos no inquilino do Azure Active Directory. Se não tiver permissões, poderá ter de contactar o administrador do Azure Active Directory. Ver [sobre o Azure Key Vault chaves, segredos e certificados](about-keys-secrets-and-certificates.md) para obter detalhes sobre o Key Vault permissões de política de acesso.

Agora, execute os seguintes comandos do PowerShell:

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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Se tem de conceder um conjunto diferente de permissões para um grupo de aplicações, crie um grupo de segurança do Azure Active Directory separado para essas aplicações.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como [proteger o seu Cofre de chave](key-vault-secure-your-key-vault.md).
