---
title: Descubra os recursos do Azure para gerir no PIM | Documentos da Microsoft
description: Saiba como detetar recursos do Azure para gerir no Azure AD Privileged Identity Management (PIM).
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
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: b5d48b3f854afaa79574e0ec13cff91f60396ac6
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190663"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Descubra os recursos do Azure para gerir no PIM

Saiba como detetar e gerir recursos do Azure quando utiliza o Privileged Identity Management (PIM) no Azure Active Directory (Azure AD). Estas informações podem ser úteis para as organizações que já utilizam o PIM para proteger os recursos de administrador e para os proprietários de subscrições que estão procurando para proteger os recursos de produção.

Quando configura primeiro a PIM para recursos do Azure, terá de descobrir e selecionar recursos a proteger com o PIM. Não existe nenhum limite para o número de recursos que podem ser geridos com o PIM. No entanto, recomendamos começar com os seus recursos (produção) mais críticos.

> [!NOTE]
> Só é possível procurar e selecione recursos de subscrição ou grupo de gestão para gerir com o PIM. Quando gerir um grupo de gestão ou de uma subscrição no PIM, também pode gerir os recursos subordinados.

## <a name="discover-resources"></a>Detetar recursos

No portal do Azure, vá para o **Privileged Identity Management** painel. No menu à esquerda, na **MANAGE** secção, selecione **recursos do Azure**.

![O "Privileged Identity Management - recursos do Azure" painel](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Se esta for a primeira vez que utilizar o PIM para recursos do Azure, primeiro execute a deteção para localizar recursos para gerir. Na **detetar recursos** painel, selecione a **detetar recursos** botão para iniciar a experiência de deteção.

![O painel "Detetar recursos"](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Se outro administrador de recursos ou o diretório da sua organização já está a gerir um recurso do Azure ao utilizar o PIM, ou se tiver uma atribuição de função elegível para um recurso, a vista de lista apresenta a mensagem **Detete recursos ou ative um atribuição de função elegível para continuar**. 

![O botão "Detetar recursos" no "Privileged Identity Manager - recursos do Azure" painel](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Quando seleciona a **detetar recursos** botão, se, no menu superior, ou no meio do painel, é apresentada uma lista de subscrições que pode gerir. As subscrições que estão realçadas já estão protegidas pelo PIM.

> [!NOTE]
> Para impedir que outro administrador de recursos de remover as definições do PIM, depois de uma subscrição está definida como geridos, a subscrição não pode ser gerida.

![Os "recursos do Azure - deteção" painel](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

Na **recursos** coluna, colocar o rato sobre uma subscrição que pretende proteger com o PIM. Em seguida, selecione a caixa de verificação à esquerda do nome do recurso. Pode selecionar múltiplas subscrições por vez.

![A lista de recursos em "recursos do Azure - deteção" painel](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Para iniciar o processo de integração, no menu superior, selecione **gerir recurso**.

![O botão "Gerenciar recursos" em "recursos do Azure - deteção" painel](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Os recursos selecionados são agora geridos pelo PIM. Para fechar o ecrã de deteção, no canto superior direito, selecione **X**. Para começar a gerir definições do PIM e atribuir membros, no menu na parte superior a **Privileged Identity Management - recursos do Azure** painel, selecione a **atualizar** botão.

![No botão "Atualizar" no menu superior do "Privileged Identity Management - recursos do Azure" painel](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Passos Seguintes

- [Configurar as definições de função de recursos do Azure no PIM](pim-resource-roles-configure-role-settings.md)
- [Atribuir funções de recursos do Azure no PIM](pim-resource-roles-assign-roles.md)
