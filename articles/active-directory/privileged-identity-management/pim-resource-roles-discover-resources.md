---
title: Detetar e gerir recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Descreve como proteger recursos do Azure utilizando o PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: 563c7f24ec4045b46d5bdcf0dc267dbdda2d5a5e
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234476"
---
# <a name="discover-and-manage-azure-resources-by-using-privileged-identity-management"></a>Detetar e gerir recursos do Azure utilizando o Privileged Identity Management

Saiba como detetar e gerir recursos do Azure, quando utiliza o Privileged Identity Management (PIM) no Azure Active Directory (Azure AD). Estas informações podem ser úteis para as organizações que já utilizam o PIM para proteger os recursos de administrador e para os proprietários de subscrição que estão à procura para proteger os recursos de produção.

Quando o primeiro configura PIM para recursos do Azure, tem de detetar e selecionar recursos a proteger com o PIM. Não há nenhum limite para o número de recursos que podem ser geridos com o PIM. No entanto, recomendamos que comece com os recursos (produção) mais críticos.

> [!NOTE]
> Só pode procurar e selecione os recursos de subscrição para gerir utilizando o PIM. Quando gerir uma subscrição no PIM, também pode gerir recursos subordinados na subscrição.

## <a name="discover-resources"></a>Detetar recursos

No portal do Azure, visite o **Privileged Identity Management** painel. No menu à esquerda, no **GERIR** secção, selecione **recursos do Azure**.

![O "Privileged Identity Management - recursos do Azure" painel](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Se esta for a primeira vez utilizando o PIM para recursos do Azure, primeiro execute a deteção para localizar recursos para gerir. No **detetar recursos** painel, selecione o **detetar recursos** botão para iniciar a experiência de deteção.

![Painel "Detetar recursos"](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Se outro administrador de recursos ou do diretório da sua organização já está a gerir um recurso do Azure utilizando o PIM ou, se tiver uma atribuição de função elegíveis para um recurso, a vista de lista apresenta a mensagem **detetar recursos ou ativar um atribuição de função elegível para continuar**. 

![O botão "Detetar recursos" no "Privileged Identity Manager - recursos do Azure" painel](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Quando seleciona o **detetar recursos** botão, se, no menu superior, ou no meio do painel, é apresentada uma lista de subscrições que pode gerir. As subscrições são realçadas já estão protegidas por PIM.

> [!NOTE]
> Para impedir que o outro administrador de recursos a remover definições de PIM, depois de uma subscrição está definida como geridos, a subscrição não pode ser gerida.

![Os "recursos do Azure - deteção" painel](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

No **recursos** coluna, coloque o rato através de uma subscrição que pretende proteger com o PIM. Em seguida, selecione a caixa de verificação à esquerda do nome do recurso. Pode selecionar várias subscrições numa altura.

![A lista de recursos em "recursos do Azure - deteção" painel](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Para iniciar o processo de integração, no menu superior, selecione **gerir recursos**.

![O botão "Gerir recursos" em "recursos do Azure - deteção" painel](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Os recursos selecionados são agora geridos pelo PIM. Para fechar o ecrã de deteção, no canto superior direito, selecione **X**. Para começar a gerir definições do PIM e atribuição de membros, no menu na parte superior do **Privileged Identity Management - recursos do Azure** painel, selecione o **atualizar** botão.

![O botão de "Atualizar" no menu superior do "Privileged Identity Management - recursos do Azure" painel](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Passos Seguintes

- [Configurar as definições de função](pim-resource-roles-configure-role-settings.md)
- [Atribuir funções no PIM](pim-resource-roles-assign-roles.md)
