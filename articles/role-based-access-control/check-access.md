---
title: Início rápido - funções de exibição atribuídos a um utilizador com o portal do Azure | Documentos da Microsoft
description: Saiba como ver as permissões de controlo (RBAC) de acesso baseado em funções atribuídas a um utilizador, o grupo, o principal de serviço ou a identidade gerida no portal do Azure.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b755dd6223c21084cafea82a1c8857f9f54b03b5
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52638653"
---
# <a name="quickstart-view-roles-assigned-to-a-user-using-the-azure-portal"></a>Início rápido: Ver funções atribuídas a um utilizador com o portal do Azure

Pode utilizar o **controlo de acesso (IAM)** painel no [controlo de acesso baseado em funções (RBAC)](overview.md) ver as atribuições de funções para vários utilizadores, grupos e principais de serviço e identidades geridas, mas, às vezes, Basta ver rapidamente as atribuições de funções para um único utilizador, o grupo, o principal de serviço ou a identidade gerida. A maneira mais fácil de fazê-lo é utilizar o **verificar acesso** recurso no portal do Azure.

## <a name="view-role-assignments"></a>Atribuições de funções da vista

Siga estes passos para ver as atribuições de funções para um único utilizador, grupo, principal de serviço ou uma identidade gerida no âmbito da subscrição.

1. No portal do Azure, clique em **todos os serviços** e, em seguida **subscrições**.

1. Clique na sua subscrição.

1. Clique em **controlo de acesso (IAM)**.

1. Clique nas **verificar acesso** separador.

    ![Controlo de acesso - separador de acesso de verificação](./media/check-access/access-control-check-access.png)

1. Na **encontrar** , selecione o tipo de entidade de segurança que pretende verificar o acesso.

1. Na caixa de pesquisa, introduza uma cadeia de caracteres para procurar o diretório para os nomes a apresentar, endereços de e-mail ou identificadores de objetos.

    ![Verifique a lista de seleção de acesso](./media/check-access/check-access-select.png)

1. Clique a entidade de segurança para abrir o **atribuições** painel.

    ![Painel de atribuições](./media/check-access/check-access-assignments.png)

    Neste painel, pode ver as funções atribuídas para a entidade de segurança selecionado e o escopo. Se existirem quaisquer negar atribuições neste âmbito ou herdadas para este âmbito, estes serão apresentados.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial: Conceder acesso de utilizador utilizando o RBAC e o portal do Azure](quickstart-assign-role-user-portal.md)
