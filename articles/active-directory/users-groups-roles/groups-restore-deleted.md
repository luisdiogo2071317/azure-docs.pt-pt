---
title: Restaurar um grupo eliminado do Office 365 no Azure AD | Microsoft Docs
description: Como restaurar um grupo eliminado, ver grupos restauráveis e eliminar permanentemente um grupo do Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d2e756676ee1abde88f75a1629640239f3162ea
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430646"
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Restaurar um grupo eliminado do Office 365 no Azure Active Directory

Quando elimina um grupo do Office 365 no Azure Active Directory (Azure AD), o grupo eliminado é retido, mas não é visível durante 30 dias a contar da data de eliminação. Este comportamento é assim para que o grupo e o respetivo conteúdo possam ser restaurados, se necessário. Esta funcionalidade está restringida exclusivamente a grupos do Office 365 no Azure AD. Não está disponível para grupos de segurança e grupos de distribuição.

> [!NOTE]
> Não utilize `Remove-MsolGroup` porque remove permanentemente o grupo. Utilize sempre `Remove-AzureADMSGroup` para eliminar um grupo do O365.

As permissões necessárias para restaurar um grupo podem ser qualquer um dos seguintes procedimentos:

Função | Permissões
--------- | ---------
Administrador da Empresa, Parceiro de Suporte de Escalão 2 e Administradores de Serviço do InTune | Pode restaurar qualquer grupo do Office 365 eliminado
Suporte de Conta de Utilizador e Parceiro de Suporte de Escalão 1 | Pode restaurar qualquer grupo do Office 365 eliminado, exceto os grupos atribuídos à função de Administrador da Empresa
Utilizador | Pode restaurar qualquer grupo eliminado do Office 365 do qual era proprietário


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a>Ver os grupos do Office 365 eliminados que estão disponíveis para restaurar

Os seguintes cmdlets podem ser utilizados para ver os grupos eliminados, para verificar se os que lhe interessam ainda não foram removidos permanentemente. Estes cmdlets fazem parte do [Módulo do PowerShell do Azure AD](https://www.powershellgallery.com/packages/AzureAD/). Pode encontrar mais informações sobre este módulo no artigo [Versão 2 do PowerShell do Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).

1.  Execute o cmdlet seguinte para apresentar todos os grupos eliminados do Office 365 no seu inquilino que ainda estão disponíveis para restaurar.
   
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Em alternativa, se conhecer o objectID de um grupo específico (e pode obtê-lo a partir do cmdlet no passo 1), execute o cmdlet seguinte para verificar se o grupo eliminado específico ainda não foi permanentemente removido.
  
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```

## <a name="how-to-restore-your-deleted-office-365-group"></a>Como restaurar o grupo do Office 365 eliminado
Assim que tiver verificado que o grupo ainda está disponível para restaurar, restaure o grupo eliminado com um dos seguintes passos. Se o grupo contiver documentos, sites de SP ou outros objetos persistentes, poderá demorar até 24 horas para que um grupo e o respetivo conteúdo sejam completamente restaurados.

1.  Execute o cmdlet seguinte para restaurar o grupo e o respetivo conteúdo.
 
 ```
 Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
 ``` 

Em alternativa, o cmdlet seguinte pode ser executado para remover o grupo eliminado permanentemente.
 ```
 Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
 ```

## <a name="how-do-you-know-this-worked"></a>Como sei se funcionou?

Para verificar se restaurou com êxito um grupo do Office 365, execute o cmdlet `Get-AzureADGroup –ObjectId <objectId>` para apresentar informações sobre o grupo. Após o restauro, o pedido é concluído:

- O grupo é apresentado na barra de navegação esquerda no Exchange
- O plano para o grupo será apresentado no Planner
- Todos os sites SharePoint e todos os seus conteúdos vão estar disponíveis
- O grupo pode ser acedido a partir de qualquer um dos pontos finais do Exchange e outras cargas de trabalho do Office 365 que suportam grupos de trabalho do Office 365

## <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais sobre os grupos do Azure Active Directory.

* [Ver grupos existentes](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Gerir definições de um grupo](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Gerir membros de um grupo](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Gerir associações de um grupo](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas dos utilizadores num grupo](groups-dynamic-membership.md)
