---
title: Configurar definições de função de diretório do Azure AD no PIM | Documentos da Microsoft
description: Saiba como configurar definições de função de diretório do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 20a704a0d5b61134a61b5cbf02a1c71dbc7039e1
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189342"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>Configurar definições de função de diretório do Azure AD no PIM

Um administrador com função privilegiada pode personalizar o Azure AD Privileged Identity Management (PIM) na sua organização, incluindo a alteração da experiência de para um utilizador que consiste em ativar uma atribuição de função elegível.

## <a name="open-role-settings"></a>Abra as definições de função

1. Open **do Azure AD Privileged Identity Management**.

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **definições**.

    ![Funções de diretório do Azure AD - definições](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Clique em **funções**.

1. Clique na função cujas definições pretende configurar.

    ![Funções de diretório do Azure AD - definições de funções](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Na página de definições para cada função, existem várias definições que pode configurar. Estas definições afetam apenas os utilizadores que estão **elegíveis** atribuições, não **permanente** atribuições.

## <a name="activations"></a>Ativações

O **ativações** controlo de deslize é o tempo máximo, em horas, o que uma função fica ativa antes de expirar. Este valor pode ser entre 1 e 72 horas.

## <a name="notifications"></a>Notificações

O **notificações** comutador permite-lhe escolher se o sistema envia mensagens de correio eletrónico para confirmar que ativaram uma função de administradores. Isso pode ser útil para detetar ativações não autorizadas ou ilegítimo.

## <a name="incidentrequest-ticket"></a>Pedido de suporte de incidente/pedido 

O **pedido de incidente/pedido** comutador permite-lhe escolher se exigir que os administradores elegíveis incluir um número de pedido de suporte quando ativar a respetiva função. Isso pode ser útil ao realizar auditorias de acesso de função.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

O **multi-factor Authentication** comutador permite-lhe escolher se pretende exigir que os utilizadores verifiquem a respetiva identidade com a MFA para poderem ativar as suas funções. Eles só precisam verificar este vez por sessão, não toda vez que os utilizadores ativar uma função. Há duas dicas para ter em mente ao ativar a MFA:

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

Para obter mais informações sobre como utilizar o MFA com o PIM, consulte [exigir autenticação multifator para funções de diretório do Azure AD no PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Exigir aprovação

O **exigem a aprovação** comutador permite-lhe escolher se pretende exigir a aprovação para ativar esta função.

1. Se definir a opção como **ativado**, o painel expande-se com as opções para selecionar os aprovadores.

    ![Funções de diretório do Azure AD - Settings - exigir a aprovação](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Se **fazer não** especificar qualquer aprovadores, os administradores de função com privilégios se tornarem aprovadores padrão. Os administradores de função com privilégios seriam necessários para aprovar **todos os** solicitações de ativação para esta função.

1. Para especificar os aprovadores, clique em **selecionar aprovadores**.

    ![Funções de diretório do Azure AD - Settings - exigir a aprovação](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Selecione um ou mais aprovadores e, em seguida, clique em **selecione**. Pode selecionar utilizadores ou grupos. Recomenda-se, pelo menos, 2 aprovadores. Aprovação automática não é permitida.

    As suas seleções aparecerá na lista de aprovadores selecionados.

1. Depois de especificar as todas as suas definições de função, clique em **guardar** para guardar as alterações.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Passos Seguintes

- [Exigir autenticação multifator para funções de diretório do Azure AD no PIM](pim-how-to-require-mfa.md)
- [Configurar alertas de segurança para funções de diretório do Azure AD no PIM](pim-how-to-configure-security-alerts.md)
