---
title: Gerir o acesso de utilizador com as revisões de acesso do Azure AD | Microsoft Docs
description: Saiba como gerir o acesso dos utilizadores, como a associação a um grupo ou a atribuição a uma aplicação, com as revisões de acesso do Azure Active Directory
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
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 9ada31066b905ca4335acf78ae87534e4c359651
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331865"
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Gerir o acesso de utilizador com as revisões de acesso do Azure AD

Com o Azure Active Directory (Azure AD), pode facilmente garantir que os utilizadores têm acesso adequado. Pode pedir aos utilizadores ou a um decisor para participar na revisão de acesso e voltar a certificar (ou confirmar) o acesso dos utilizadores. Os revisores podem dar o seu parecer relativamente à necessidade de acesso contínuo de cada utilizador, com base nas sugestões do Azure AD. Quando a revisão de acesso estiver concluída, pode introduzir as alterações necessárias e remover o acesso dos utilizadores que já não precisam dele.

> [!NOTE]
> Se quiser rever apenas o acesso dos utilizadores convidados, sem abranger a revisão de acesso de todos os tipos de utilizador, veja [Gerir o acesso de utilizadores convidados com revisões de acesso](manage-guest-access-with-access-reviews.md). Se quiser rever a associação do utilizador a funções administrativas como a de administrador global, veja [Iniciar uma revisão de acesso no Azure AD Privileged Identity Management](../privileged-identity-management/pim-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Pré-requisitos 


As revisões de acesso estão disponíveis com a edição Premium P2 do Azure AD, que está incluída no Microsoft Enterprise Mobility + Security, E5. Para obter mais informações, consulte [Edições do Azure Active Directory](../fundamentals/active-directory-whatis.md). Cada utilizador que interage com esta funcionalidade, incluindo para criar uma revisão, preencher uma revisão ou confirmar o acesso, precisa de uma licença. 

## <a name="create-and-perform-an-access-review"></a>Criar e realizar uma revisão de acesso

Uma revisão de acesso pode ter um ou mais utilizadores como revisores.  

1. Selecione um grupo no Azure AD que tenha um ou mais membros. Ou selecione uma aplicação ligada ao Azure AD que tenha um ou mais utilizadores atribuídos à mesma. 

2. Decida se quer que cada utilizador faça a revisão do seu próprio acesso ou se quer ter um ou mais utilizadores incumbidos de rever o acesso de todos os utilizadores.

3. Como administrador global ou administrador de conta de utilizador, aceda à [página de revisões de acesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

4. Crie a revisão de acesso. Para obter mais informações, veja [Criar uma revisão de acesso](create-access-review.md).

5. Quando a revisão de acesso é iniciado, pedir os revisores para dar entrada. Por predefinição, cada um deles recebe um e-mail do Azure AD com uma ligação para o painel de acesso, onde [realizam a revisão de acesso](perform-access-review.md).

6. Se os revisores não tiverem dado o seu parecer, pode pedir ao Azure AD para lhes enviar um lembrete. Por predefinição, o Azure AD envia automaticamente um lembrete aos revisores que ainda não responderam a meio do prazo para a data de fim.

7. Depois de os revisores darem o respetivo parecer, pare a revisão de acesso e aplique as alterações. Para obter mais informações, veja [Concluir uma revisão de acesso](complete-access-review.md).


## <a name="next-steps"></a>Passos Seguintes

[Criar uma revisão de acesso para os membros de um grupo ou o acesso a uma aplicação](create-access-review.md)




