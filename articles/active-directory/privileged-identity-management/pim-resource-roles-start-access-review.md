---
title: Iniciar uma revisão do acesso no PIM de recursos do Azure | Microsoft Docs
description: Explica como iniciar uma revisão do acesso no Privileged Identity Management de recursos do Azure
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 61ed4e82e0b782b423668564dae6efb272967702
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-role---start-access-review"></a>Revisão do acesso do Privileged Identity Management - função de recursos - início
Atribuições de função ficam "obsoletas" quando os utilizadores com acesso privilegiado que não precisam de já. Para reduzir os riscos associados estes atribuições de funções obsoletos, os administradores de com função privilegiada regularmente devem rever as funções que atribuiu utilizadores. Este documento aborda os passos para iniciar uma revisão do acesso no Privileged Identity Management (PIM) para recursos do Azure.

Página principal da aplicação do PIM, navegue para:

* **Aceder revisões** > **adicionar**

![](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Quando clica no **adicionar** botão, o **criar uma revisão do acesso** é apresentado o painel. Neste painel, vai configurar a revisão com um nome e o limite de tempo, escolha uma função para rever e decidir que executará a revisão.

![](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Configurar a revisão
Para criar uma revisão do acesso, terá de nome e defina uma data de início e de fim.

![Configurar revisão - captura de ecrã](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Se o comprimento da revisão tempo suficiente para que os utilizadores concluí-la. Se concluir antes da data de fim, pode sempre parar a revisão antecipadamente.

### <a name="choose-a-role-to-review"></a>Escolher uma função para rever
Cada revisão centra-se apenas uma função. Exceto se tiver iniciado a revisão do acesso a partir do painel de uma função específica, terá de escolher uma função agora.

1. Navegue para **rever membro da função**
   
    ![Reveja o membro da função - captura de ecrã](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Selecione uma função na lista.

### <a name="decide-who-will-perform-the-review"></a>Decidir que executará a revisão
Existem três opções para executar uma revisão. Pode atribuir a revisão para outra pessoa para concluir, pode fazê-lo por si ou pode fazer com que cada utilizador, reveja os suas próprias acesso.

1. Escolha uma das opções:
   
   * **Utilizadores selecionados**: Utilize esta opção se não souber quem tem acesso. Com esta opção, pode atribuir a revisão para um proprietário de recursos ou o Gestor de grupo para concluir.
   * **Atribuído (self)**: Utilize esta opção para que os utilizadores, reveja as suas próprias atribuições de funções.
   
2. Navegue para **selecione revisores**
   
    ![Selecione os revisores - captura de ecrã](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>Iniciar a revisão
Por fim, tem a opção para exigir que os utilizadores forneçam um motivo se estes aprovar o acesso. Adicione uma descrição da revisão se assim o desejar e selecione **iniciar**.

Certifique-se de que permite aos utilizadores saber que existe uma revisão do acesso a aguardar para os mesmos e apresentar [como executar uma revisão do acesso](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Gerir a revisão do acesso
Pode monitorizar o progresso conforme os revisores concluir as revisões no dashboard de recursos do Azure PIM, na secção de revisões de acesso. Sem direitos de acesso serão alterados no diretório até [conclui a revisão](pim-resource-roles-complete-access-review.md).

Até que o período de avaliação está acima, pode notificar os utilizadores para concluir a respetiva revisão ou pare a revisão numa fase inicial da secção de revisões de acesso.

