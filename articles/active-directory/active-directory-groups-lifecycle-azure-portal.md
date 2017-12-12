---
title: "Grupos de expiração para o Office 365 no Azure Active Directory | Microsoft Docs"
description: "Como configurar a expiração de grupos do Office 365 no Azure Active Directory (pré-visualização)"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: d485d2e7d22ea79a87dc52dbc063a811f4a1a2ec
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="configure-expiration-for-office-365-groups-preview"></a>Configurar a expiração de grupos do Office 365 (pré-visualização)

Agora pode gerir o ciclo de vida de grupos do Office 365 através da definição de funcionalidades de expiração das mesmas. Pode definir expiração para apenas os grupos do Office 365 no Azure Active Directory (Azure AD). Depois de definir um grupo a expirar:
-   Os proprietários do grupo são notificados para renovar o grupo que está prestes a expiração
-   Qualquer grupo que não a renovar é eliminado
-   Qualquer grupo do Office 365 é eliminado pode ser restaurado no prazo de 30 dias, os proprietários de grupo ou do administrador

> [!NOTE]
> A definição de expiração para grupos do Office 365 requer uma licença do Azure AD Premium ou uma licença do Azure AD Basic EDU para todos os membros dos grupos aos quais são aplicadas as definições de expiração.
> 
> Para o Azure AD Basic EDU licenciado aos clientes: para configurar esta política pela primeira vez, utilize os cmdlets do Azure Active Directory PowerShell. Depois disso, pode atualizar as definições de expiração utilizando o PowerShell ou o portal do Azure AD, com uma conta que seja um administrador de conta de utilizador ou um Administrador Global no inquilino do Azure AD.

Para obter informações sobre como transferir e instalar os cmdlets do Azure AD PowerShell, consulte [Azure Active Directory PowerShell para gráfico: versão de pré-visualização pública 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="set-group-expiration"></a>Expiração do conjunto de grupo

1. Abra o [Centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que seja um administrador global no inquilino do Azure AD.

2. Abra do Azure AD, selecione **utilizadores e grupos**.

3. Selecione **definições de grupo** e, em seguida, selecione **expiração** para abrir as definições de expiração.
  
  ![Painel de expiração](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. No **expiração** painel, pode:

  * Defina a duração de grupo em dias. Pode selecionar um dos valores predefinidos ou um valor personalizado (deve ser 31 dias ou mais). 
  * Especifique um endereço de correio eletrónico, onde as notificações de expiração e renovação devem ser enviadas quando um grupo não tem nenhum proprietário. 
  * Selecione os grupos do Office 365 expirarem. Pode ativar a expiração de **todos os** grupos do Office 365, pode selecionar entre os grupos do Office 365 ou selecionar **nenhum** para desativar a expiração de todos os grupos.
  * Guardar as definições, quando tiver terminado, selecionando **guardar**.


Notificações de e-mail, tal como esta são enviadas para os proprietários de grupo do Office 365 30 dias, 15 dias e 1 dia antes da expiração do grupo.

![Notificação de correio eletrónico de expiração](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

O proprietário do grupo, em seguida, pode selecionar **renovar grupo** para renovar o Office 365 grupo ou pode selecionar **aceda ao grupo** para ver os membros e outros detalhes sobre o grupo.

Quando expira um grupo, um dia após a data de expiração é eliminar o grupo. Uma notificação por e-mail, tal como esta é enviada para os proprietários de grupo do Office 365, informar sobre a expiração e eliminação subsequente do respetivo grupo do Office 365.

![Notificação de correio eletrónico de eliminação do grupo](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

O grupo pode ser restaurado selecionando **restauro grupo** ou utilizando os cmdlets do PowerShell, conforme descrito em [grupo de restaurar uma eliminada do Office 365 no Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-Groups-Restore-Azure-Portal).
    
Se o grupo que está a restaurar contém documentos, SharePoint sites ou outros objetos persistentes, pode demorar até 24 horas para restaurar completamente o grupo e o respetivo conteúdo.

> [!NOTE]
> * Quando configurar a expiração pela primeira vez, todos os grupos que são mais antigos do que o intervalo de expiração são definidos para 30 dias até a expiração. O primeiro e-mail de notificação de renovação enviado dentro de um dia. 
>   Por exemplo, um grupo foi criado dias 400 há e o intervalo de expiração é definido para 180 dias. Quando aplicar definições de expiração, o grupo A tem 30 dias antes de ser eliminado, a menos que o proprietário renova-lo.
> * Quando um grupo dinâmico é eliminado e restaurado, é apresentado como um novo grupo e novamente preenchida, de acordo com a regra. Este processo poderá demorar até 24 horas.

## <a name="next-steps"></a>Passos seguintes
Estes artigos fornecem informações adicionais sobre os grupos do Azure AD.

* [Consulte os grupos existentes](active-directory-groups-view-azure-portal.md)
* [Gerir as definições de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerir os membros de um grupo](active-directory-groups-members-azure-portal.md)
* [Gerir membros de um grupo](active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas para os utilizadores de um grupo](active-directory-groups-dynamic-membership-azure-portal.md)
