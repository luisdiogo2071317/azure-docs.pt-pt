---
title: Realizar uma revisão de acesso no Privileged Identity Management para recursos do Azure | Documentos da Microsoft
description: Este documento descreve como realizar uma revisão de acesso no PIM para recursos do Azure, de acordo com a função de recursos.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 336ad1d7eff8eee0f2370e206eb98f20b4efb45b
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617229"
---
# <a name="perform-an-access-review-in-pim-according-to-resource-role"></a>Realizar uma revisão de acesso no PIM, de acordo com a função de recursos
Privileged Identity Management (PIM) para recursos do Azure simplifica como as empresas a gerir o acesso privilegiado a recursos no Azure. 

Se forem atribuídos a uma função administrativa, o administrador com função privilegiada da sua organização pode pedir-lhe para confirmar regularmente que ainda tem essa função para a sua tarefa. Poderá receber um e-mail que inclui uma ligação, ou pode ir diretamente para o [portal do Azure](https://portal.azure.com). Siga os passos neste artigo para efetuar um Self-rever de suas funções atribuídas.

Se for um administrador com função privilegiada interessado nas revisões de acesso, obter mais detalhes em [como iniciar uma revisão de acesso](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Adicionar a aplicação Privileged Identity Management
Pode utilizar a aplicação de PIM do Azure Active Directory (Azure AD) no [portal do Azure](https://portal.azure.com/) executar sua revisão. Se não tiver a aplicação no seu portal, siga estes passos para começar a utilizar.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Selecione o utilizador nome no canto superior direito do portal do Azure e selecione o diretório onde irá ser operacional.
3. Selecione **todos os serviços**e utilizar os **filtro** caixa para procurar *Azure AD Privileged Identity Management*.
4. Verifique **afixar ao dashboard**e, em seguida, selecione **criar**. O aplicativo de PIM é aberto.

## <a name="approve-or-deny-access"></a>Aprovar ou negar o acesso
Quando aprova ou negar o acesso, está apenas dizendo ao revisor se continuar a utilizar esta função ou não. Escolher **aprovar** se pretender manter-se na função, ou **negar** se não precisar de mais o acesso. O estado é alterado apenas quando o revisor aplica-se os resultados.

Siga estes passos para encontrar e concluir a revisão de acesso:
1. Navegue para a aplicação do Azure AD PIM.
2. Selecione o **rever acesso** painel.

   ![Aplicativo de captura de ecrã do PIM, com o painel de revisão de acesso selecionada](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Selecione a revisão de que pretende concluir. 
4. Escolhem **aprovar** ou **negar**. Na **fornecem uma caixa de motivo**, poderá ter de incluir um motivo para a sua decisão.

   ![Página de detalhes de captura de ecrã de revisão](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
