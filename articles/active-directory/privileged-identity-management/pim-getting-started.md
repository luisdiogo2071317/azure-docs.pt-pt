---
title: Começar a utilizar o PIM - Azure | Documentos da Microsoft
description: Saiba como começar a utilizar o Azure AD Privileged Identity Management (PIM) no portal do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 5b3bff27821964648713b02589c941c99e3eb03d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190094"
---
# <a name="start-using-pim"></a>Comece a utilizar o PIM

Com o Azure Active Directory (Azure AD) Privileged Identity Management (PIM), pode gerir, controlar e monitorizar o acesso dentro da sua organização. Este âmbito inclui o acesso ao Azure, ao Azure AD e a outros serviços online da Microsoft, como o Office 365 ou o Microsoft Intune.

Este artigo mostra-lhe como adicionar a aplicação Azure AD PIM ao dashboard do Portal do Azure.

## <a name="first-person-to-use-pim"></a>Primeira pessoa a utilizar o PIM

Se for a primeira pessoa a utilizar o PIM no seu diretório, é-lhe automaticamente atribuída a [administrador de segurança](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) e [administrador com função privilegiada](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) funções no diretório. As atribuições de funções de diretório do Azure AD dos utilizadores só podem ser geridas por administradores de funções com privilégios. Além disso, pode optar por executar o [assistente de segurança](pim-security-wizard.md) que o orienta ao longo da experiência inicial de deteção e atribuição.

## <a name="add-pim-tile-to-the-dashboard"></a>Adicionar mosaico PIM ao dashboard

Para facilitar abrir o PIM, deve adicionar um mosaico PIM ao dashboard do portal do Azure.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como um Administrador Global do seu diretório.

1. Clique em **todos os serviços** e localize a **Azure AD Privileged Identity Management** serviço.

    ![O Azure AD Privileged Identity Management em todos os serviços](./media/pim-getting-started/pim-all-services-find.png)

1. Clique para abrir o guia de introdução do PIM.

1. Verifique **afixar painel ao dashboard** para afixar o painel de início rápido do PIM ao dashboard.

    ![Afixar o painel ao dashboard](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    No dashboard do Azure, verá um mosaico como este:

    ![Mosaico de início rápido do PIM](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="navigate-to-your-tasks"></a>Navegue para as suas tarefas

Depois do PIM ser configurado, pode utilizar este painel para realizar as tarefas de gestão de identidade.

![Tarefas de nível superior para PIM - captura de ecrã](./media/pim-getting-started/pim-quickstart-tasks.png)

| Tarefas + Gerir | Descrição |
| --- | --- |
| **Minhas funções**  | Apresenta uma lista de funções elegíveis e ativas atribuídos ao utilizador. Este é o local onde pode ativar quaisquer funções elegíveis atribuídas. |
| **Os meus pedidos** | Apresenta os pedidos pendentes para ativar as atribuições de funções elegíveis. |
| **Acesso à aplicação** | Permite-lhe reduzir os atrasos potenciais e utilizar uma função imediatamente após a ativação. |
| **Aprovar pedidos** | Apresenta uma lista de pedidos de ativação de funções elegíveis pelos utilizadores no seu diretório que está a seu cargo para aprovar. |
| **Rever acesso** | Lista as revisões de acesso do Active Directory que está atribuído a concluir, se estiver a rever o acesso para si próprio ou por outra pessoa. |
| **Funções de diretório do Azure AD** | Apresenta um dashboard e as definições para os administradores de função com privilégios gerirem atribuições de função de diretório do Azure AD. Este dashboard está desativado para todos aqueles que não são um administrador de funções com privilégios. Estes utilizadores têm acesso a um dashboard especial intitulado A minha vista. O dashboard A minha vista só apresenta informações sobre o utilizador que está a aceder ao dashboard, e não sobre o inquilino completo. |
| **Recursos do Azure** | Apresenta um dashboard e as definições para os administradores de função com privilégios gerirem atribuições de função de recursos do Azure. Este dashboard está desativado para todos aqueles que não são um administrador de funções com privilégios. Estes utilizadores têm acesso a um dashboard especial intitulado A minha vista. O dashboard A minha vista só apresenta informações sobre o utilizador que está a aceder ao dashboard, e não sobre o inquilino completo. |

## <a name="next-steps"></a>Passos Seguintes

- [Ativar as minhas funções de diretório do Azure AD no PIM](pim-how-to-activate-role.md)
- [Ativar as minhas funções de recursos do Azure no PIM](pim-resource-roles-activate-your-roles.md)
