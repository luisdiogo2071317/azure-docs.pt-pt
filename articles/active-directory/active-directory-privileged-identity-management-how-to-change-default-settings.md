---
title: Como gerir as definições de ativação de função | Microsoft Docs
description: Saiba como alterar as predefinições para as identidades privilegiadas com a extensão do Azure Active Directory Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 06/06/2017
ms.author: curtand
ms.custom: pim
ms.openlocfilehash: 972fd1e322e578516073307d01548132473bc52c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Como gerir as definições de ativação de função no Azure AD Privileged Identity Management
Um administrador com função privilegiada pode personalizar o Azure AD Privileged Identity Management (PIM) na sua organização, incluindo a alterar a experiência de para um utilizador que está a ativar uma atribuição de função elegível.

## <a name="manage-the-role-activation-settings"></a>Gerir as definições de ativação de função
1. Vá para o [portal do Azure](https://portal.azure.com) e selecione o **do Azure AD Privileged Identity Management** aplicação a partir do dashboard.
2. Selecione **gerir funções privilegiadas** > **definições** > **funções com privilégios**.
3. Escolha a função cujas definições pretende gerir.

Na página de definições para cada função, existem várias definições que pode configurar. Estas definições afetam apenas os utilizadores que são elegíveis administradores, administradores permanentes não.

**Ativações**: O tempo, em horas, o que uma função permanece ativa antes de expirar. Isto pode ser entre 1 e 72 horas.

**Notificações**: pode escolher se pretende ou não o sistema envia mensagens de correio eletrónico para administradores confirmar tem ativado a uma função. Isto pode ser útil para detetar ativações não autorizadas ou illegitimate.

**Pedido de incidente/solicitar**: pode escolher se deve ou não necessitam de admins elegível para incluir um número da permissão, quando ativar a respetiva função. Isto pode ser útil quando efetua as auditorias de acesso de função.

**Autenticação Multifator**: pode escolher se deve ou não exigir que os utilizadores verifiquem a respetiva identidade com a MFA possa ativar as respetivas funções. Apenas têm de verificar este vez por sessão, não sempre que ativam uma função. Existem dois sugestões a ter em mente quando ativar a MFA:

* Utilizadores com contas Microsoft para os respetivos endereços de e-mail (normalmente @outlook.com, mas nem sempre) não é possível efetuar o registo do MFA do Azure. Se pretender atribuir funções a utilizadores com contas Microsoft, deve tornar administradores permanentes ou desative a MFA para essa função.
* Não é possível desativar o MFA para funções com privilégios elevados para o Azure AD e o Office 365. Esta é uma funcionalidade de segurança porque estas funções devem ser cuidadosamente protegidas:  
  
  * Administrador de aplicações
  * Administrador do servidor de Proxy de aplicações
  * Administrador de faturação  
  * Administrador de Conformidade  
  * Administrador de serviço do CRM
  * Aprovador de acesso de LockBox de cliente
  * Escritor de diretório  
  * Administrador do Exchange  
  * Administrador global
  * Administrador de serviço do Intune
  * Administrador de caixa de correio  
  * Suporte para parceiros do escalão 1  
  * Suporte para parceiros do escalão 2  
  * Administrador com Função Privilegiada   
  * Administrador de segurança  
  * Administrador do SharePoint  
  * Administrador do Skype para Empresas  
  * Administrador de conta de utilizador  

Para obter mais informações sobre como utilizar a MFA com PIM consulte [como requerer MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

