---
title: Fluxos de trabalho de aprovação de gestão de identidades privilegiadas do Azure | Documentos da Microsoft
description: Saiba mais sobre fluxos de trabalho de aprovação no Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/28/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 14d0cdc0bde1081f1a020c7039596a5b6880070f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619753"
---
# <a name="approvals"></a>Aprovações

## <a name="overview"></a>Descrição geral

Com aprovações para Privileged Identity Management, pode configurar funções para exigir a aprovação para a ativação e escolher um ou vários utilizadores ou grupos como aprovadores delegados. Mantenha a ler para saber como configurar funções e selecione os aprovadores.


## <a name="new-terminology"></a>Terminologia nova

*Utilizador de função elegível* – um utilizador de função elegível é um utilizador na sua organização foi atribuída a uma função do Azure AD como elegíveis (necessita de função ativação).

*Delegado aprovador* – um aprovador de delegado é um ou vários indivíduos ou grupos do Azure AD, responsável pela aprovação de pedidos para ativar as funções.

## <a name="scenarios"></a>Cenários

A pré-visualização privada suporta os seguintes cenários:

**Como um privilegiado função de administrador (PRA), pode:**

-   [Ativar a aprovação para funções específicas](#enable-approval-for-specific-roles)

-   [Especifique os utilizadores de aprovador e/ou grupos para aprovar pedidos](#specify-approver-users-and/or-groups-to-approve-requests)

-   [Ver histórico de solicitação e aprovação para funções de contas privilegiadas](#view-request-and-approval-history-for-all-privileged-roles)

**Como um aprovador designado, pode:**

-   [Ver pendentes aprovações (pedidos)](#view-pending-approvals-requests)

-   [aprovar ou rejeitar pedidos de elevação de função (único e/ou em massa)](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [fornecer a justificação para meu aprovação/rejeição](#provide-justification-for-my-approval/rejection) 

**Como um utilizador de função elegível, pode:**

-   [pedir a ativação de uma função que requer aprovação](#request-activation-of-a-role-that-requires-approval)

-   [ver o estado do seu pedido para ativar](#view-the-status-of-your-request-to-activate)

-   [Concluir sua tarefa no Azure AD, se a ativação foi aprovada](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a>Navegação

Atualizámos a navegação para suportar as aprovações

![](media/azure-ad-pim-approval-workflow/image001.png)

A predefinição de página de aterrissagem fornece acesso conveniente para obter informações sobre o PIM e a nova documentação de aprovações.

![](media/azure-ad-pim-approval-workflow/image002.png)

Também adicionámos uma nova secção para todos os utilizadores do PIM, "Meu histórico de auditoria". Aqui pode encontrar todas as informações relevantes para a sua identidade. Isto inclui todos os seus pedidos pendentes e concluídos, tomar qualquer decisão que fez sobre os pedidos de que resolver e todas as suas últimas ativações de função num único local conveniente.

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a>Ativar a aprovação para funções específicas

Para ativar a aprovação para uma função específica, primeiro Selecione funções de diretório no painel de navegação esquerda.

![](media/azure-ad-pim-approval-workflow/image004.png)

Localize e selecione as definições na navegação à esquerda de funções de diretório

![](media/azure-ad-pim-approval-workflow/image006.png)

Selecione as funções com privilégios:

![](media/azure-ad-pim-approval-workflow/image009.png)

Selecione a opção "Ativar" na exigir a secção de aprovação:

![](media/azure-ad-pim-approval-workflow/image011.png)

Uma vez ativada, o painel será expandido para mostrar os seguintes detalhes:

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
Se não especificar qualquer aprovadores, o PRA(s) tornam-se de es de predefinição. PRA(s) seriam necessários para aprovar todos os pedidos de ativação para esta função.

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a>Especifique os utilizadores de aprovador e/ou grupos para aprovar pedidos

Para delegar a aprovação, clique na opção para "Selecionadas aprovadores":

![](media/azure-ad-pim-approval-workflow/image015.png)

Quando o painel de aprovadores selecione for carregada, pode procurar um utilizador específico ou um grupo com a barra de pesquisa na parte superior, ou selecionar da lista preenchida previamente, em seguida, clique em "Selecionar" quando terminar:

![](media/azure-ad-pim-approval-workflow/image017.png)

Nota: Pode selecionar múltiplos utilizadores e grupos de cada vez.

A seleção irá aparecer na lista de aprovadores selecionados, como mostrado abaixo:

![](media/azure-ad-pim-approval-workflow/image019.png)

Para remover um aprovador, basta clicar no botão Remover junto do respetivo nome.

Para adicionar mais aprovadores, repita o processo.

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a>Ver histórico de solicitação e aprovação para funções de contas privilegiadas

Para ver o histórico de solicitação e aprovação para funções de contas com privilégios, selecione o histórico de auditoria a partir do dashboard:

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Pode classificar os dados por ação e procure "Ativação aprovada"

### <a name="view-pending-approvals-requests"></a>Ver pendentes aprovações (pedidos)

Como um aprovador de delegados, receberá notificações por e-mail quando um pedido está com aprovação pendente. Para ver estes pedidos no portal do PIM, a partir do dashboard (na navegação do novo) selecione o separador de "Pedidos de aprovação pendentes" na barra de navegação esquerdo.

![](media/azure-ad-pim-approval-workflow/image023.png)

A partir daí, verá uma lista de pedidos com aprovação pendente:

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a>aprovar ou rejeitar pedidos de elevação de função (único e/ou em massa)

Seleccione os pedidos que pretende aprovar ou negar e clique no botão na barra de ação que corresponde à sua decisão:

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a>fornecer a justificação para meu aprovação/rejeição

Esta ação irá abrir um novo painel para aprovar ou negar pedidos de várias ao mesmo tempo. Introduza uma justificativa para a sua decisão, e clique em aprovar (ou negar) na parte inferior ou o painel:

![](media/azure-ad-pim-approval-workflow/image029.png)

Quando o processo de pedido for concluído, o símbolo de estado irá refletir a decisão que tiver feito (neste exemplo, a decisão é aprovar):

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a>pedir a ativação de uma função que requer aprovação

Pode ser iniciado a solicitando a ativação de uma função que requer aprovação de navegação do PIM antiga ou nova navegação, como o processo de ativação de função permanece o mesmo. Basta Selecione uma função na lista de funções para ativar:

![](media/azure-ad-pim-approval-workflow/image033.png)

Se precisar de uma função com privilégios multi-factor Authentication, será solicitado para concluir essa tarefa pela primeira vez:

![](media/azure-ad-pim-approval-workflow/image035.png)

Depois de concluído, clique em ativar e fornecer uma justificação (se necessário):

![](media/azure-ad-pim-approval-workflow/image037.png)

O requerente verá uma notificação que o pedido está com aprovação pendente:

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a>ver o estado do seu pedido para ativar

Visualizar o estado de um pedido pendente para ativar tem de ser acedido no painel de navegação nova. Na barra de navegação esquerdo, selecione o separador de "Os meus pedidos":

![](media/azure-ad-pim-approval-workflow/image041.png)

O estado do pedido está predefinida como "Pendentes", mas pode alternar para ver todos ou negado pedidos.

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a>Concluir sua tarefa no Azure AD, se a ativação foi aprovada

Depois do pedido for aprovado, a função está ativa e pode continuar com qualquer trabalho que necessita desta função.

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a>Passos Seguintes

Seus comentários são valiosos para nós. À vontade partilhar comentários ou comentários connosco aqui!
