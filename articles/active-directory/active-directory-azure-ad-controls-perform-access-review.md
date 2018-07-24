---
title: Rever o acesso ao utilizar o Azure AD as revisões de acesso | Documentos da Microsoft
description: Saiba como rever acesso utilizando as revisões de acesso do Azure Active Directory.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 07/16/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 2e0a522f6125a9e34a37c50f547e725072ec65b4
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205008"
---
# <a name="review-access-with-azure-ad-access-reviews"></a>Rever o acesso com o Azure AD as revisões de acesso

Azure Active Directory (Azure AD) simplifica como as empresas a gerir o acesso a aplicações e membros dos grupos no Azure AD e outros serviços Online da Microsoft com um recurso chamado acesso revisões. Talvez recebeu uma mensagem de e-mail da Microsoft que lhe pede para rever o acesso para membros de um grupo ou os utilizadores com acesso a uma aplicação. 

## <a name="open-an-access-review"></a>Abra uma revisão de acesso

Para ver as revisões de acesso pendente, clique na ligação de acesso de revisão no e-mail. A partir de Agosto de 2018, as notificações de e-mail para funções do Azure AD tem um design atualizado. O código a seguir mostra uma mensagem de e-mail de exemplo que é enviada quando um utilizador é convidado para ser um revisor. 

![E-mail de acesso de revisão](./media/active-directory-azure-ad-controls-perform-access-review/new-ar-email.png)

Se não tiver o e-mail, pode localizar as revisões de acesso ao seguir estes passos:

1. Inicie sessão no [painel de acesso do Azure AD](https://myapps.microsoft.com).

2. Selecione o símbolo de utilizador no canto superior direito da página, que exibe a sua organização nome e a predefinição. Se mais do que uma organização estiver listada, selecione a organização que solicitou uma revisão de acesso.

3. Se o nome de um mosaico **as revisões de acesso** está no lado direito da página, selecione-lo. Se o mosaico não estiver visível, existem não existem revisões de acesso para efetuar dessa organização e é necessária nenhuma ação neste momento.

## <a name="fill-out-an-access-review"></a>Preencha uma revisão de acesso

Quando selecionar uma revisão de acesso na lista, verá os nomes de utilizadores que precisam ser examinadas. Poderá ver apenas um nome, seus próprios – se o pedido foi rever o seu próprio acesso.

Para cada linha na lista, pode decidir se quer aprovar ou negar o acesso do utilizador. Selecione a linha e escolha se pretende aprovar ou negar. (Se não souber o usuário, pode indicar que demasiado.)

O revisor pode exigir que forneça uma justificativa para aprovação de acesso contínuo ou associação de grupo.

## <a name="next-steps"></a>Passos Seguintes

Acesso negado de um utilizador não é removido imediatamente. Este pode ser removido quando a revisão é concluída ou quando um administrador para a revisão. Se deseja alterar sua resposta e aprovar um utilizador anteriormente recusado ou negar um utilizador previamente aprovado, selecione a linha, repor a resposta e selecione uma nova resposta. Pode efetuar este passo até que a revisão de acesso seja concluída.



