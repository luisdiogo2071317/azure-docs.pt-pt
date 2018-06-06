---
title: Efetuar revisões de acesso nos recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Explica como iniciar uma revisão do acesso no Privileged Identity Management de recursos do Azure
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c47cdeab4edd0b8dbe9cfe7c388253d02a9148c9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699738"
---
# <a name="perform-access-reviews-in-azure-resources-by-using-privileged-identity-management"></a>Efetuar revisões de acesso nos recursos do Azure utilizando o Privileged Identity Management
Atribuições de função ficam "obsoletas" quando os utilizadores com acesso privilegiado que não precisam de já. Para reduzir o risco que está associada a estes atribuições de funções obsoletos, os administradores de com função privilegiada regularmente devem rever as funções. Este documento aborda os passos para iniciar uma revisão do acesso no Privileged Identity Management (PIM) para recursos do Azure.

Na página principal da aplicação do PIM, aceda a:

* **Aceder revisões** > **adicionar**

![Adicionar revisões de acesso](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Quando seleciona o **adicionar** botão, o **criar uma revisão do acesso** é apresentado o painel. Neste painel, configure a revisão com um nome e o limite de tempo, escolha uma função para rever e, em seguida, decidir que a revisão.

![Criar uma revisão de acesso](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Configurar a revisão
Para criar uma revisão do acesso, primeiro o nome e, em seguida, defina uma data de início e de fim.

![Configurar revisão - captura de ecrã](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Se o comprimento da revisão tempo suficiente para que os utilizadores concluí-la. Se terminarem antes da data de fim, estes podem interrompem sempre a revisão antecipadamente.

### <a name="choose-a-role-to-review"></a>Escolher uma função para rever
Cada revisão centra-se apenas uma função. Exceto se tiver iniciado a revisão do acesso a partir do painel de uma função específica, tem de escolher uma função agora.

1. Aceda a **rever membro da função**
   
    ![Reveja o membro da função - captura de ecrã](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Selecione uma função na lista.

### <a name="decide-who-will-perform-the-review"></a>Decidir que executará a revisão
Existem três opções para executar uma revisão. Pode atribuir a revisão para outra pessoa para concluir, pode fazê-lo por si ou cada utilizador pode rever os suas próprias acesso.

1. Escolha uma das opções:
   
   * **Utilizadores selecionados**: Utilize esta opção se não souber quem tem acesso. Com esta opção, pode atribuir a revisão para um proprietário de recursos ou o Gestor de grupo para concluir.
   * **Atribuído (self)**: Utilize esta opção para que os utilizadores, reveja as suas próprias atribuições de funções.
   
2. Aceda a **selecione revisores**.
   
    ![Selecione os revisores - captura de ecrã](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>Iniciar a revisão
Por fim, pode exigir que os utilizadores forneçam um motivo para aprovação de acesso. Adicione uma descrição da revisão se assim o desejar. Em seguida, selecione **iniciar**.

Certifique-se de que permite aos utilizadores saber que existe uma revisão do acesso a aguardar para os mesmos e apresentar [como executar uma revisão do acesso](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Gerir a revisão do acesso
No dashboard de recursos do PIM Azure, pode acompanhar o progresso à medida os revisores concluir as revisões. Sem direitos de acesso são alterados no diretório até [concluiu a revisão](pim-resource-roles-complete-access-review.md).

Até que o período de avaliação está acima, pode notificar os utilizadores para concluir a respetiva revisão ou pare a revisão numa fase inicial da secção de revisões de acesso.

