---
title: Privileged Identity Management para recursos do Azure - detetar e gerir recursos do Azure | Microsoft Docs
description: Descreve como proteger recursos do Azure.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 78650e47ec92aa144e4ccc8c57f309240bf31ee3
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="discover-and-manage-azure-resources"></a>Detetar e gerir recursos do Azure

Se a sua organização já está a utilizar o Azure AD PIM para proteger os administradores no seu diretório ou estiver à procura para proteger os recursos de produção um proprietário da subscrição está no sítio certo.

Quando ativa primeiro a PIM para recursos do Azure, tem de detetar e selecionar recursos a proteger com o PIM. Não existe nenhum limite para o número de recursos que podem ser geridas com PIM, mas recomendamos que comece com os recursos (produção) mais importantes primeiro.

> [!Note]
> Apenas os recursos de subscrição podem ser procurados e selecionados para gestão. Escolher gerir uma subscrição no PIM irá ativar a gestão para todos os recursos subordinados, bem como.

## <a name="discover-resources"></a>Detetar recursos

Navegue para o Azure AD PIM e selecione os recursos do Azure na secção Gerir do menu de navegação esquerdo.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Se esta for a primeira vez utilizando o PIM para recursos do Azure terá de executar a deteção para localizar recursos para gerir.
Clique no botão "Detetar recursos" no centro do ecrã, para iniciar a experiência de deteção.

![](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Se outro administrador de recursos ou do diretório da sua organização já está a gerir um recurso do Azure com o PIM ou se tiver uma atribuição de função elegíveis para um recurso, a vista de lista irá conter a mensagem: "detetar recursos ou ativar uma função elegível atribuição ao continuar". 

![](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Quando seleciona o botão, na barra de ação ou no meio do ecrã, para detetar recursos, verá uma lista de subscrições disponíveis para gestão. Neste momento, se vir subscrições realçadas indica que estão protegidos pelo PIM.

> [!Note]
> Para impedir a remoção das definições do PIM, outro administrador de recursos depois de uma subscrição é gerida não pode ser gerido.

![](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

Coloque o cursor sobre uma subscrição que pretende proteger com o PIM e selecione a caixa de extremidade esquerda da linha. Pode seleccionar várias subscrições cada vez.

![](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Para iniciar a integração o processo de selecionar o botão "Gerir recursos" na barra na parte superior do ecrã.

![](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Os recursos selecionados são agora geridos pelo PIM. Fechar o ecrã de deteção a utilizar no "X" no canto superior direito da página e clique em Atualizar na barra na parte superior do ecrã de recursos do Azure gerir para começar a gerir definições do PIM e membros de atribuição.

![](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Passos Seguintes

[Configurar definições de função](pim-resource-roles-configure-role-settings.md)

[Atribuir funções no PIM](pim-resource-roles-assign-roles.md)
