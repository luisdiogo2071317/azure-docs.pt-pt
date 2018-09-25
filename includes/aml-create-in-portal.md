---
title: incluir ficheiro
description: incluir ficheiro
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/24/2018
ms.openlocfilehash: 6d6e6a2aea867c5b603d950a4bbaa33f14695f45
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47011037"
---
Inicie sessão no [portal do Azure](https://portal.azure.com/) com as credenciais da subscrição do Azure que vai utilizar. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) agora.

Dashboard de área de trabalho do portal é suportado em navegadores Edge, Chrome e Firefox.

   ![Portal do Azure](./media/aml-create-in-portal/portal-dashboard.png)

Selecione o botão (+) **Criar um recurso**, no canto superior esquerdo do portal.

   ![Criar um recurso no portal do Azure](./media/aml-create-in-portal/portal-create-a-resource.png)

Introduza **Machine Learning** na barra de pesquisa. Selecione o resultado da pesquisa com o nome **área de trabalho do Machine Learning**.

   ![Procure a área de trabalho](./media/aml-create-in-portal/allservices-search.PNG)

Na **área de trabalho do Machine Learning** painel, desloque-se para baixo e selecione **criar** para começar.

   ![criar](./media/aml-create-in-portal/portal-create-button.png)

Na **área de trabalho do ML** painel, configurar a sua área de trabalho.

   Campo|Descrição
   ---|---
   Nome da área de trabalho |Introduza um nome exclusivo que identifica a sua área de trabalho.  Aqui, usaremos ws do docs. Nomes têm de ser exclusivos entre o grupo de recursos. Utilize um nome fácil de lembrar e diferenciar de áreas de trabalho criadas por outros utilizadores.  
   Subscrição |Escolha a subscrição do Azure que pretende utilizar. Se tiver múltiplas subscrições, escolha a subscrição adequada na qual o recurso é cobrado.
   Grupo de recursos | Utilize um grupo de recursos existente na sua subscrição ou introduza um nome para criar um novo grupo de recursos. Um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure.  Aqui, usaremos aml do docs. 
   Localização | Escolha a localização que esteja mais próxima dos seus utilizadores e dos recursos de dados. Isso é onde a área de trabalho é criada.

   ![Criar área de trabalho](./media/aml-create-in-portal/workspace-create.png)

Selecione **Criar** para iniciar o processo de criação.  Pode demorar alguns minutos a criar a área de trabalho.

   Para verificar o estado da implementação, selecione o ícone de notificações (campainha) na barra de ferramentas.

   ![Criar área de trabalho](./media/aml-create-in-portal/notifications.png)

   Quando terminar, é apresentada uma mensagem de êxito da implementação.  Ele também está presente na secção notificações.   Clique nas **Ir para recurso** botão para ver a nova área de trabalho.
