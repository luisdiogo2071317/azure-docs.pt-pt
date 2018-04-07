---
title: Como efetuar uma revisão do acesso para recursos do Azure Privileged Identity Management | Microsoft Docs
description: Este documento descreve como efetuar e aceder a revisão no PIM para recursos do Azure.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 4afb923058143dd1771043db8433aa3a65541bf7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---perform-access-review"></a>Efetuar Privileged Identity Management - função de recursos - revisão de acesso
Privileged Identity Management para recursos do Azure simplifica a forma como as empresas gerem com privilégios de acesso a recursos no Azure. 

Se forem atribuídos a uma função administrativa, o administrador com função privilegiada da sua organização poderá pedir-lhe para confirmar regularmente se ainda precisar dessa função a tarefa. Poderá obter um e-mail que inclui uma ligação ou, pode ir diretamente para o [portal do Azure](https://portal.azure.com). Siga os passos neste artigo para efetuar um rever automática das suas funções atribuídas.

Se a um administrador com função privilegiada interessado em revisões de acesso, obtenha mais detalhes em [como iniciar uma revisão do acesso](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Adicionar a aplicação Privileged Identity Management
Pode utilizar a aplicação do Azure AD Privileged Identity Management (PIM) a [portal do Azure](https://portal.azure.com/) para efetuar a revisão.  Se não tiver a aplicação Azure AD Privileged Identity Management no seu portal, siga estes passos para começar a utilizar.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Selecione o seu nome de utilizador no canto superior direito do portal do Azure e selecione o diretório onde irá a estar a funcionar.
3. Selecione **Todos os serviços** e utilize a caixa de texto Filtro para procurar **Azure AD Privileged Identity Management**.
4. Marque **Afixar ao dashboard** e, em seguida, clique em **Criar**. A aplicação de Privileged Identity Management será aberta.

## <a name="approve-or-deny-access"></a>Aprovar ou negar o acesso
Quando aprovar ou negar o acesso, é estiver apenas a informar o revisor se, continuar a utilizar esta função ou não. Escolha **aprovar** se pretender permaneça na função, ou **negar** se não precisa de acesso já. O estado não mudará de imediato, até que o revisor aplica-se os resultados.
Siga estes passos para localizar e concluir a revisão do acesso:
1. Navegue para a aplicação Azure AD Privileged Identity Management.
2. Selecione o painel de acesso de revisão

![](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Selecione a revisão que pretende concluir. 
4. Escolha o **aprovar** ou **negar**. Poderá ter de incluir um motivo para a sua decisão no **de indicar um motivo** caixa de texto.

![](media/azure-pim-resource-rbac/rbac-access-review-choice.png)
