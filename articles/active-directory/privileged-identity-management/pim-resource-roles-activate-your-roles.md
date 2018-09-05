---
title: Ativar as minhas funções de recursos do Azure no PIM | Documentos da Microsoft
description: Aprenda a ativar as suas funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59bce2c61db5838bb21a29757d4e354311ecffd5
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666252"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Ativar as minhas funções de recursos do Azure no PIM

Utilizar o Azure AD Privileged Identity Management (PIM), os membros da função elegível para recursos do Azure podem agendar ativação numa data futura e a hora. Também podem selecionar uma duração de ativação específico dentro do máximo (configurado por administradores).

Este artigo é para os membros que tem de ativar a respetiva função de recursos do Azure no PIM.

## <a name="activate-a-role"></a>Ativar uma função

Quando precisar da desempenhar uma função de recursos do Azure, pode pedir a ativação ao utilizar o **minhas funções** opção de navegação no PIM.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. Open **do Azure AD Privileged Identity Management**. Para obter informações sobre como adicionar o mosaico do PIM ao dashboard, consulte [começar a utilizar o PIM](pim-getting-started.md).

1. Clique em **minhas funções** para ver uma lista de sua elegíveis funções de diretório do Azure AD e funções de recursos do Azure.

    ![Funções de diretório do Azure AD e funções de recursos do Azure - as minhas funções](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Na **funções de recursos do Azure** lista, localize a função que pretende ativar.

    ![Funções de recursos do Azure - minha lista de funções](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Clique em **Activate** para abrir o painel ativar.

1. Se a sua função requer autenticação multifator (MFA), clique em **verificar a sua identidade antes de continuar**. Só precisa que autenticar uma vez por sessão.

    ![Certifique-se com a MFA antes da ativação de função](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. Clique em **verificar a minha identidade** e siga as instruções para fornecer verificação adicional de segurança.

    ![Verificação adicional de segurança](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Se pretender especificar um âmbito reduzido, clique em **âmbito** para abrir o painel de filtro de recursos.

    É melhor prática para apenas solicitar acesso a recursos de que precisa. No painel de filtro de recursos, pode especificar os grupos de recursos ou recursos que precisam acessar.

    ![Ativar - filtro de recursos](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Se necessário, especifique uma hora de início de ativação personalizado. O membro seria ativado após o período selecionado.

1. Na **motivo** , digite a razão para o pedido de ativação.

    ![Painel de ativar concluída](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. Clique em **ativar**.

    Se a função não necessita de aprovação, está agora ativado e a função é apresentada na lista de funções do Active Directory. Se o [a função requer aprovação](pim-resource-roles-approval-workflow.md) para ativar, uma notificação será exibida no canto superior direito do seu navegador informando o pedido está com aprovação pendente.

    ![Pedido pendente de notificação](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Ver o estado dos pedidos

Pode ver o estado dos seus pedidos pendentes para ativar.

1. Abra o Azure AD Privileged Identity Management.

1. Clique em **os meus pedidos** para ver uma lista de sua função de diretório do Azure AD e a função de recursos do Azure pedidos.

    ![Funções de diretório do Azure AD e funções de recursos do Azure - os meus pedidos](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. Desloque-se para a direita para ver os **estado do pedido de** coluna.

## <a name="use-a-role-immediately-after-activation"></a>Utilize uma função imediatamente após a ativação

Devido à colocação em cache, ativações não ocorrer imediatamente no portal do Azure sem uma atualização. Se precisar de reduzir a possibilidade de atrasos após a ativação de uma função, pode utilizar o **acesso a aplicações** página no portal. Aplicativos acessados a partir desta página verificar a existência de novas atribuições de função imediatamente.

1. Abra o Azure AD Privileged Identity Management.

1. Clique nas **acesso a aplicações** página.

    ![Acesso de aplicativo PIM - captura de ecrã](./media/pim-resource-roles-activate-your-roles/pim-application-access.png)

1. Clique em **recursos do Azure** para reabrir o portal sobre o **todos os recursos** página.

    Quando clicar nesse link, forçar uma atualização e há uma verificação de existência de atribuições de função do novo recurso do Azure.

## <a name="cancel-a-pending-request"></a>Cancelar um pedido pendente

Se não necessitar de ativação de uma função que requer aprovação, pode cancelar um pedido pendente em qualquer altura.

1. Abra o Azure AD Privileged Identity Management.

1. Clique em **os meus pedidos**.

1. Para a função que pretende cancelar, clique nas **Cancelar** ligação.

    Quando clicar em Cancelar, a solicitação será cancelada. Para ativar a função mais uma vez, terá que submeter um novo pedido de ativação.

   ![Cancelar pedido pendente](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="next-steps"></a>Passos Seguintes

- [Expandir ou renovar funções de recursos do Azure no PIM](pim-resource-roles-renew-extend.md)
- [Ativar as minhas funções de diretório do Azure AD no PIM](pim-how-to-activate-role.md)