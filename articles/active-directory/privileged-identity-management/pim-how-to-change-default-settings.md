---
title: Como gerir as definições de ativação de função | Documentos da Microsoft
description: Saiba como alterar as predefinições para as identidades privilegiadas com a extensão do Azure Active Directory Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 13e00ac57bfc867ad13ae3d7958fca37eaf5beee
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590451"
---
# <a name="how-to-manage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Como gerir as definições de ativação de função no Azure AD Privileged Identity Management
Um administrador com função privilegiada pode personalizar o Azure AD Privileged Identity Management (PIM) na sua organização, incluindo a alteração da experiência de para um utilizador que consiste em ativar uma atribuição de função elegível.

## <a name="manage-the-role-activation-settings"></a>Gerir as definições de ativação de função
1. Vá para o [portal do Azure](https://portal.azure.com) e selecione o **Azure AD Privileged Identity Management** aplicação a partir do dashboard.
2. Selecione **gerir funções com privilégios** > **definições** > **funções com privilégios**.
3. Escolha a função cujas definições pretende gerir.

Na página de definições para cada função, há uma série de definições que pode configurar. Estas definições afetam apenas os utilizadores que são administradores elegíveis, os administradores não permanentes.

**Ativações**: O tempo, em horas, o que uma função fica ativa antes de expirar. Isso pode ser entre 1 e 72 horas.

**Notificações**: pode escolher se é ou não o sistema envia mensagens de correio eletrónico para os administradores que confirmam que ativaram uma função. Isso pode ser útil para detetar ativações não autorizadas ou ilegítimo.

**Pedido de incidente/pedido**: pode escolher se deve ou não necessitam de administradores elegíveis para incluir um número de pedido de suporte quando ativar a respetiva função. Isso pode ser útil ao realizar auditorias de acesso de função.

**Multi-factor Authentication**: pode escolher se deve ou não exigir que os utilizadores verifiquem a respetiva identidade com a MFA para poderem ativar as suas funções. Eles só precisam verificar este vez por sessão, não toda vez que os utilizadores ativar uma função. Há duas dicas para ter em mente ao ativar a MFA:

* Os utilizadores com contas Microsoft para os respetivos endereços de e-mail (normalmente @outlook.com, mas não sempre) não é possível registar para MFA do Azure. Se pretender atribuir funções a utilizadores com contas Microsoft, deve torná-los a administradores permanentes ou desativar a MFA para essa função.
* Não é possível desativar o MFA para funções com privilégios elevados para o Azure AD e Office 365. Esse é um recurso de segurança, uma vez que estas funções devem ser protegidas com cuidado:  
  
  * Administrador de aplicações
  * Administrador de servidor de Proxy de aplicações
  * Administrador de faturação  
  * Administrador de Conformidade  
  * Administrador de serviço CRM
  * Aprovador de acesso ao Sistema de Proteção de Dados do Cliente
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

Para obter mais informações sobre como utilizar o MFA com o PIM, consulte [como requerer MFA](pim-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

