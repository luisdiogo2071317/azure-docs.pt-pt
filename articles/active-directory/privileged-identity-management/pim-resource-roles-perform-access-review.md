---
title: Realizar uma revisão de acesso de minhas funções de recursos do Azure no PIM | Documentos da Microsoft
description: Saiba como realizar uma revisão de acesso de suas funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
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
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2a8ef3d0ff602ce4580755db0ada97fcf7cdbbc
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208236"
---
# <a name="perform-an-access-review-of-my-azure-resource-roles-in-pim"></a>Realizar uma revisão de acesso de minhas funções de recursos do Azure no PIM
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

## <a name="next-steps"></a>Passos Seguintes

- [Efetuar uma revisão de acesso das minhas funções de diretório do Azure AD no PIM](pim-how-to-perform-security-review.md)
