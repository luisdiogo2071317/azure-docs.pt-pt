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
ms.openlocfilehash: 6f73b15ed16cfe26bf14e60a5206568e1a1564fd
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53594186"
---
Entrar para o [portal do Azure](https://portal.azure.com/) com as credenciais para a subscrição do Azure que utilizar. 

Dashboard de área de trabalho do portal é suportado em navegadores Microsoft Edge, Chrome e Firefox.

   ![Portal do Azure](./media/aml-create-in-portal/portal-dashboard.png)

No canto superior esquerdo do portal, selecione **criar um recurso**.

   ![Criar um recurso no portal do Azure](./media/aml-create-in-portal/portal-create-a-resource.png)

Na barra de pesquisa, introduza **Machine Learning**. Selecione o **área de trabalho do Machine Learning serviço** resultado de pesquisa.

   ![Procure uma área de trabalho](./media/aml-create-in-portal/allservices-search.PNG)

Na **área de trabalho do ML service** painel, desloque-se para baixo e selecione **criar** para começar.

   ![Criar](./media/aml-create-in-portal/portal-create-button.png)

Na **área de trabalho do ML service** painel, configurar a sua área de trabalho.

   Campo|Descrição
   ---|---
   Nome da área de trabalho |Introduza um nome exclusivo que identifica a sua área de trabalho. Neste exemplo, utilizamos **docs ws**. Nomes têm de ser exclusivos entre o grupo de recursos. Utilize um nome que é fácil de lembrar e diferenciar de áreas de trabalho criadas por outras pessoas.  
   Subscrição |Selecione a subscrição do Azure que pretende utilizar.
   Grupo de recursos | Utilize um grupo de recursos existente na sua subscrição ou introduza um nome para criar um novo grupo de recursos. Um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure. Neste exemplo, utilizamos **docs aml**. 
   Localização | Selecione a localização mais próxima dos seus utilizadores e os recursos de dados. Esta localização é onde a área de trabalho é criada.

   ![Criar área de trabalho](./media/aml-create-in-portal/workspace-create.png)

Para iniciar o processo de criação, selecione **criar**. Pode demorar alguns minutos a criar a área de trabalho.

Para verificar o estado da implementação, selecione o ícone de notificações **bell**, na barra de ferramentas.

   ![Estado de criação da área de trabalho](./media/aml-create-in-portal/notifications.png)

Quando o processo estiver concluído, é apresentada uma mensagem de êxito da implementação. Ele também está presente na secção notificações. Para ver a nova área de trabalho, selecione **Ir para recurso**.
