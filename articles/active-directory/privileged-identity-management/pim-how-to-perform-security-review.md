---
title: Como realizar uma revisão de acesso | Documentos da Microsoft
description: Saiba como executar uma revisão com a aplicação do Azure Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4d19f9e04b6288fad70f62e95579475c16f2112c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590339"
---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a>Como realizar uma revisão de acesso no Azure AD Privileged Identity Management
O Azure Active Directory (AD) Privileged Identity Management simplifica como as empresas a gerir o acesso privilegiado a recursos no Azure AD e outros serviços online da Microsoft, como o Office 365 ou o Microsoft Intune.  

Se forem atribuídos a uma função administrativa, o administrador com função privilegiada da sua organização poderá pedir-lhe para confirmar regularmente que ainda tem essa função para a sua tarefa. Poderá receber um e-mail que inclui uma ligação, ou pode ir diretamente para o [portal do Azure](https://portal.azure.com). Siga os passos neste artigo para efetuar um Self-rever de suas funções atribuídas.

Se for um administrador com função privilegiada ou um administrador global interessado nas revisões de acesso, obter mais detalhes em [como iniciar uma revisão de acesso](pim-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Adicionar a aplicação Privileged Identity Management
Pode utilizar a aplicação do Azure AD Privileged Identity Management (PIM) a [portal do Azure](https://portal.azure.com/) executar sua revisão.  Se não tiver a aplicação Azure AD Privileged Identity Management no seu portal, siga estes passos para começar a utilizar.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Selecione o nome de utilizador no canto superior direito do portal do Azure e selecione o diretório em que irá ser operacional.
3. Selecione **Todos os serviços** e utilize a caixa de texto Filtro para procurar **Azure AD Privileged Identity Management**.
4. Marque **Afixar ao dashboard** e, em seguida, clique em **Criar**. A aplicação de Privileged Identity Management será aberta.

## <a name="approve-or-deny-access"></a>Aprovar ou negar o acesso
Quando aprova ou negar o acesso, está apenas dizendo ao revisor se continuar a utilizar esta função ou não. Escolher **aprovar** se pretender manter-se na função, ou **negar** se não precisar de mais o acesso. Seu estado não será alterada de imediato, até que o revisor aplica-se os resultados.
Siga estes passos para encontrar e concluir a revisão de acesso:

1. No aplicativo PIM, selecione **revisão de acesso privilegiado**. Se tiver quaisquer revisões de acesso pendente, são exibidos no painel de revisões de acesso do Azure AD.
2. Selecione a revisão de que pretende concluir.
3. A menos que criou a revisão, aparecem como o único utilizador da revisão. Selecione a marca de verificação junto ao seu nome.
4. Escolhem **aprovar** ou **negar**. Poderá ter de incluir um motivo para a sua decisão no **forneça um motivo** caixa de texto.  
5. Fechar o **funções de análise do Azure AD** painel.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
