---
title: Configurar as definições de função de recursos do Azure no PIM | Documentos da Microsoft
description: Saiba como configurar as definições de função de recursos do Azure no Azure AD Privileged Identity Management (PIM).
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
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 901eb5ef43ddb2840ed7a3d83fc08f2f05849461
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189740"
---
# <a name="configure-azure-resource-role-settings-in-pim"></a>Configurar as definições de função de recursos do Azure no PIM

Quando configurar as definições de função, define as configurações padrão que são aplicadas a atribuições no ambiente de Privileged Identity Management (PIM). Para definir estas definições para o seu recurso, selecione o **definições de função** separador no painel à esquerda. Também pode selecionar o botão de definições de função da barra de ação (em qualquer função) para ver as opções atuais.

## <a name="overview"></a>Descrição geral

Com o fluxo de trabalho aprovação no Privileged Identity Management (PIM) para funções de recursos do Azure, os administradores ainda mais podem proteger ou restringir o acesso a recursos críticos. Ou seja, os administradores podem exigir aprovação para ativar as atribuições de funções. 

O conceito de uma hierarquia de recurso é exclusivo para as funções de recursos do Azure. Esta hierarquia permite que a herança de atribuições de funções a partir de um objeto de recurso principal para baixo para todos os recursos subordinados dentro do contêiner pai. 

Por exemplo: João, um administrador de recursos, utiliza o PIM para atribuir a Alice como um membro elegível à função de proprietário na subscrição Contoso. Com esta atribuição, Alice é um proprietário elegível de todos os contentores do grupo de recursos dentro da subscrição da Contoso. Alice é também um proprietário elegível de todos os recursos (como máquinas virtuais) dentro de cada grupo de recursos da subscrição. 

Vamos supor que existem três grupos de recursos na subscrição de Contoso: Fabrikam teste, desenvolvimento do Fabrikam e Fabrikam Prod. Cada um destes grupos de recursos contém uma única máquina virtual.

Definições do PIM são configuradas para cada função de um recurso. Ao contrário de atribuições, estas definições não são herdadas e aplicam-se estritamente a função de recursos. [Saiba mais sobre as atribuições elegíveis e visibilidade de recurso](pim-resource-roles-eligible-visibility.md).

Continuando com o exemplo: João utiliza o PIM para exigir que todos os membros na função de proprietário de aprovação de pedidos de subscrição da Contoso ser ativado. Para ajudar a proteger os recursos no grupo de recursos de Fabrikam Prod, Bob também requer aprovação para os membros da função de proprietário deste recurso. As funções de proprietário na Fabrikam teste e desenvolvimento do Fabrikam não necessita de aprovação para ativação.

Quando Alice solicita ativação da sua função de proprietário para a subscrição da Contoso, um aprovador deve aprovar ou negar o pedido antes de ela fica ativa na função de. Se decidir Alice [escopo de sua ativação](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) para o grupo de recursos da Fabrikam Prod, um aprovador deve aprovar ou negar este pedido, demasiado. Mas, se decidir Alice definir o âmbito sua ativação para um ou ambos os Fabrikam teste ou desenvolvimento do Fabrikam, aprovação não é necessária.

O fluxo de trabalho de aprovação pode não ser necessário para todos os membros de uma função. Considere um cenário em que a sua organização contrata vários de contrato de associados, para ajudar com o desenvolvimento de um aplicativo que serão executados numa subscrição do Azure. Enquanto administrador de recursos, mesmo que os funcionários têm acesso elegível sem aprovação necessária, mas associa o contrato deve solicitar a aprovação. Para configurar o fluxo de trabalho de aprovação para apenas os associates de contrato, pode criar uma função personalizada com as mesmas permissões que a função atribuída aos funcionários. Pode exigir a aprovação para ativar essa função personalizada. [Saiba mais sobre as funções personalizadas](pim-resource-roles-custom-role-policy.md).

Para configurar o fluxo de trabalho de aprovação e especificar quem pode aprovar ou negar pedidos, utilize os procedimentos seguintes.

## <a name="require-approval-to-activate"></a>Exigir aprovação para ativar

1. Navegar para o PIM no portal do Azure e selecione um recurso na lista.

   ![Painel "Recursos do azure" com um recurso selecionado](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

2. No painel esquerdo, selecione **definições de função**.

3. Procure e selecione uma função e, em seguida, selecione **editar** para modificar as definições.

   ![Botão "Editar" para a função de operador](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_view_settings.png)

4. Na **ativação** secção, selecione a **exigir aprovação para ativar** caixa de verificação.

   ![Secção de "Ativação" das definições de função](media/azure-pim-resource-rbac/aadpim_rbac_settings_require_approval_checkbox.png)

## <a name="specify-approvers"></a>Especifique aprovadores

Clique em **selecionar aprovadores** para abrir o **selecione um utilizador ou grupo** painel.

>[!NOTE]
>Tem de selecionar, pelo menos, um utilizador ou grupo para atualizar a definição. Não há nenhum aprovadores de predefinição.

Os administradores de recursos podem adicionar qualquer combinação de utilizadores e grupos para a lista de aprovadores. 

![Painel "Selecionar um utilizador ou grupo" com um utilizador selecionado](media/azure-pim-resource-rbac/aadpim_rbac_role_settings_select_approvers.png)

## <a name="request-approval-to-activate"></a>Pedir aprovação para ativar

O pedido de aprovação não tem impacto sobre o procedimento que um membro tem de seguir para ativar. [Reveja os passos para ativar uma função](pim-resource-roles-activate-your-roles.md).

Se a ativação de uma função que requer aprovação de pedidos de um membro e a função já não é necessária, o membro pode cancelar o pedido no PIM.

Para cancelar, navegue até o PIM e selecione **os meus pedidos**. Localize a pedido e selecione **Cancelar**.

![Painel "Os meus pedidos"](media/azure-pim-resource-rbac/aadpim_rbac_role_approval_request_pending.png)

## <a name="next-steps"></a>Passos Seguintes

- [Exigir autenticação multifator para funções de recursos do Azure no PIM](pim-resource-roles-require-mfa.md)
- [Configurar alertas de segurança para funções de recursos do Azure no PIM](pim-resource-roles-configure-alerts.md)
