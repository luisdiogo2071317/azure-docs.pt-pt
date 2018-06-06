---
title: Executar uma revisão do acesso numa Privileged Identity Management para recursos do Azure | Microsoft Docs
description: Este documento descreve como executar uma revisão de acesso no PIM para recursos do Azure, de acordo com a função de recursos.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 43245907a16019ac5396c41db036c3160359270f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699612"
---
# <a name="perform-an-access-review-in-pim-according-to-resource-role"></a>Executar uma revisão de acesso no PIM, de acordo com a função de recursos
Privileged Identity Management (PIM) para recursos do Azure simplifica a forma como as empresas gerem com privilégios de acesso a recursos no Azure. 

Se forem atribuídos a uma função de administrador, administrador com função privilegiada da sua organização poderá pedir-lhe regularmente confirmar que ainda tem essa função para a tarefa. Poderá obter um e-mail que inclui uma ligação ou, pode ir diretamente para o [portal do Azure](https://portal.azure.com). Siga os passos neste artigo para efetuar um rever automática das suas funções atribuídas.

Se a um administrador com função privilegiada interessado em revisões de acesso, obtenha mais detalhes em [como iniciar uma revisão do acesso](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Adicionar a aplicação Privileged Identity Management
Pode utilizar a aplicação do Azure Active Directory (Azure AD) PIM no [portal do Azure](https://portal.azure.com/) para efetuar a revisão. Se não tiver a aplicação no seu portal, siga estes passos para começar a utilizar.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Selecione o utilizador nome no canto superior direito do portal do Azure e selecione o diretório onde irá a estar a funcionar.
3. Selecione **todos os serviços**e utilizar o **filtro** caixa para procurar *do Azure AD Privileged Identity Management*.
4. Verifique **afixar ao dashboard**e, em seguida, selecione **criar**. Abre a aplicação do PIM.

## <a name="approve-or-deny-access"></a>Aprovar ou negar o acesso
Quando aprovar ou negar o acesso, é estiver apenas a informar o revisor se, continuar a utilizar esta função ou não. Escolha **aprovar** se pretender permaneça na função, ou **negar** se não precisa de acesso já. As alterações de estado apenas quando o revisor aplica-se os resultados.

Siga estes passos para localizar e concluir a revisão do acesso:
1. Navegue para a aplicação do Azure AD PIM.
2. Selecione o **rever o acesso** painel.

   ![Aplicação de captura de ecrã do PIM, com o painel de acesso de revisão selecionada](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Selecione a revisão que pretende concluir. 
4. Escolha o **aprovar** ou **negar**. No **fornecer uma caixa de razão**, poderá ter de incluir um motivo para a sua decisão.

   ![Página de detalhes de captura de ecrã de revisão](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
