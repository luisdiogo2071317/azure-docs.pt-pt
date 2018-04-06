---
title: Grupos de expiração para o Office 365 no Azure Active Directory | Microsoft Docs
description: Como configurar a expiração de grupos do Office 365 no Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: aa8551db8c3982fa61b3d4731a8aac3e2832f1b1
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Configure a política de expiração para grupos do Office 365

Agora, pode gerir o ciclo de vida de grupos do Office 365 ao definir uma política de expiração das mesmas. Pode definir a política de expiração para apenas os grupos do Office 365 no Azure Active Directory (Azure AD). 

Depois de definir um grupo a expirar:
-   Os proprietários do grupo são notificados para renovar o grupo que está prestes a expiração
-   Qualquer grupo que não a renovar é eliminado
-   Qualquer grupo do Office 365 é eliminado pode ser restaurado no prazo de 30 dias, os proprietários de grupo ou do administrador

> [!NOTE]
> Configurar e utilizar a política de expiração para grupos do Office 365 requerem que tenha no manualmente licenças do Azure AD Premium para todos os membros dos grupos nos quais é aplicada a política de expiração.

Para obter informações sobre como transferir e instalar os cmdlets do Azure AD PowerShell, consulte [Azure Active Directory PowerShell para gráfico 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Funções e permissões
Seguem-se as funções que podem configurar e utilizar a expiração de grupos do Office 365 no Azure AD.

Função | Permissões
-------- | --------
Administrador global ou de administrador de conta de utilizador | Pode criar, ler, atualizar ou eliminar as definições de política de expiração de grupos do Office 365<br>Podem renovar a qualquer grupo do Office 365
Utilizador | Pode renovar um grupo do Office 365 que possuem<br>Pode restaurar um grupo do Office 365 que possuem<br>Pode ler as definições de política de expiração

Para obter mais informações sobre as permissões para restaurar um grupo eliminado, consulte [restaurar grupo eliminado do Office 365 no Azure Active Directory](active-directory-groups-restore-azure-portal.md).

## <a name="set-group-expiration"></a>Expiração do conjunto de grupo

1. Abra o [Centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que seja um administrador global no inquilino do Azure AD.

2. Selecione **grupos**, em seguida, selecione **expiração** para abrir as definições de expiração.
  
  ![Painel de expiração](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. No **expiração** painel, pode:

  * Defina a duração de grupo em dias. Pode selecionar um dos valores predefinidos ou um valor personalizado (deve ser 31 dias ou mais). 
  * Especifique um endereço de correio eletrónico, onde as notificações de expiração e renovação devem ser enviadas quando um grupo não tem nenhum proprietário. 
  * Selecione os grupos do Office 365 expirarem. Pode ativar a expiração de **todos os** grupos do Office 365, pode optar por ativar apenas **selecionados** grupos do Office 365, ou selecione **nenhum** para desativar a expiração de todos os grupos .
  * Guardar as definições, quando tiver terminado, selecionando **guardar**.


Notificações de e-mail, tal como esta são enviadas para os proprietários de grupo do Office 365 30 dias, 15 dias e 1 dia antes da expiração do grupo.

![Notificação de correio eletrónico de expiração](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Do **renovar grupo** e-mail de notificação, diretamente o grupo os proprietários podem aceder a página de detalhes do grupo no painel de acesso. Não existe, os utilizadores podem obter mais informações sobre o grupo, como a descrição, quando este foi pela última vez renovada, quando irá expirar e a capacidade de renovar o grupo. A página de detalhes de grupo também a inclui agora ligações para recursos de grupo do Office 365, para que o proprietário do grupo comodamente pode ver o conteúdo e a atividade no seu grupo.

Quando expira um grupo, um dia após a data de expiração é eliminar o grupo. Uma notificação por e-mail, tal como esta é enviada para os proprietários de grupo do Office 365, informar sobre a expiração e eliminação subsequente do respetivo grupo do Office 365.

![Notificação de correio eletrónico de eliminação do grupo](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

O grupo pode ser restaurado no prazo de 30 dias a eliminação selecionando **restauro grupo** ou utilizando os cmdlets do PowerShell, conforme descrito em [restaurar grupo eliminado do Office 365 no Azure Active Directory](active-directory-groups-restore-azure-portal.md).
    
Se o grupo que está a restaurar contém documentos, SharePoint sites ou outros objetos persistentes, pode demorar até 24 horas para restaurar completamente o grupo e o respetivo conteúdo.

> [!NOTE]
> * Quando configurar a expiração pela primeira vez, todos os grupos que são mais antigos do que o intervalo de expiração são definidos para 30 dias até a expiração. O primeiro e-mail de notificação de renovação enviado dentro de um dia. 
>   Por exemplo, um grupo foi criado dias 400 há e o intervalo de expiração é definido para 180 dias. Quando aplicar definições de expiração, o grupo A tem 30 dias antes de ser eliminado, a menos que o proprietário renova-lo.
> * Atualmente apenas uma política de expiração pode ser configurada para grupos do Office 365 num inquilino.
> * Quando um grupo dinâmico é eliminado e restaurado, é apresentado como um novo grupo e novamente preenchida, de acordo com a regra. Este processo poderá demorar até 24 horas.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Como funciona a expiração de grupo do Office 365 com uma caixa de correio em suspensão legal
Quando um grupo expira e é eliminado, em seguida, 30 dias após a eliminação de dados do grupo de aplicações como o planeador, Sites, ou equipas é eliminado permanentemente, mas a caixa de correio do grupo que está em suspensão legal é mantida e não é eliminada permanentemente. O administrador pode utilizar cmdlets do Exchange para restaurar a caixa de correio para obter os dados. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Como funciona a expiração de grupo do Office 365 com a política de retenção
A política de retenção está configurada a título de centro de conformidade e segurança. Se tiver configurado uma política de retenção para os grupos do Office 365, quando um grupo expira e é eliminado, as conversações do grupo na caixa de correio da grupo e ficheiros no site de grupo são mantidos no contentor de retenção para o número de dias definido na retenção especificado política. Os utilizadores não verão o grupo ou o respetivo conteúdo após a expiração, mas podem recuperar os dados de site e a caixa de correio através de deteção eletrónica.

## <a name="powershell-examples"></a>Exemplos do PowerShell
Seguem-se exemplos de como pode utilizar os cmdlets do PowerShell para configurar as definições de expiração para grupos do Office 365 no seu inquilino:

1. Instalar o módulo de pré-visualização do PowerShell v 2.0 (2.0.0.137) e inicie sessão em linha de comandos da PowerShell:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. Configurar as definições de expiração New-AzureADMSGroupLifecyclePolicy: Este cmdlet define a duração para todos os grupos do Office 365 no inquilino a 365 dias. Notificações de renovação para o Office 365 grupos sem proprietários serão enviados para 'emailaddress@contoso.com'
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. Obter a política existente Get-AzureADMSGroupLifecyclePolicy: Este cmdlet obtém as definições de expiração grupo atuais do Office 365 foram configuradas. Neste exemplo, pode ver:
  * O ID de política 
  * A duração para todos os grupos do Office 365 no inquilino é definir a 365 dias
  * Notificações de renovação para o Office 365 grupos sem proprietários serão enviados para 'emailaddress@contoso.com.'
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. Atualizar a política existente conjunto AzureADMSGroupLifecyclePolicy: Este cmdlet é utilizado para atualizar uma política existente. No exemplo abaixo, a duração de grupo sob a política existente é alterada de 365 dias para 180 dias. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Adicionar grupos específicos para a política de adicionar AzureADMSLifecyclePolicyGroup: Este cmdlet adiciona um grupo para a política de ciclo de vida. Por exemplo: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Remover Remove-AzureADMSGroupLifecyclePolicy de política existente: Este cmdlet elimina as definições de expiração de grupos do Office 365, mas requer o ID de política. Este procedimento desactivará a expiração de grupos do Office 365. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
Os seguintes cmdlets pode ser utilizados para configurar a política mais detalhadamente. Para obter mais informatin, consulte [PowerShell documentação](https://docs.microsoft.com/en-us/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

* Get-AzureADMSGroupLifecyclePolicy
* New-AzureADMSGroupLifecyclePolicy
* Get-AzureADMSGroupLifecyclePolicy
* Set-AzureADMSGroupLifecyclePolicy
* Remove-AzureADMSGroupLifecyclePolicy
* Add-AzureADMSLifecyclePolicyGroup
* Remove-AzureADMSLifecyclePolicyGroup
* Reset-AzureADMSLifeCycleGroup   
* Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais sobre os grupos do Azure AD.

* [Consulte os grupos existentes](active-directory-groups-view-azure-portal.md)
* [Gerir as definições de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerir os membros de um grupo](active-directory-groups-members-azure-portal.md)
* [Gerir membros de um grupo](active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas para os utilizadores de um grupo](active-directory-groups-dynamic-membership-azure-portal.md)
