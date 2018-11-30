---
title: Como implementar um serviço Web em várias regiões - Azure Machine Learning Studio | Documentos da Microsoft
description: Passos para implementar (copiar) um novo serviço Web para outras regiões.
services: machine-learning
documentationcenter: ''
author: ericlicoding
manager: hjerez
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.custom: (previous ms.author=aashishb, author=aashishb)
ms.author: amlstudiodocs
ms.openlocfilehash: ab28cce0f973c4798bfd6995cc275c4724b7bcc9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308024"
---
# <a name="azure-machine-learning-studio-deploy-a-web-service-to-multiple-regions"></a>O Azure Machine Learning Studio: Implementar um serviço web em várias regiões
Os novos serviços de Web do Azure permitem-lhe facilmente implementar um serviço web em várias regiões sem a necessidade de várias subscrições ou áreas de trabalho. 

O preço é a região específico, por isso deve definir um plano de faturação para cada região na qual irá implementar o serviço web.

## <a name="to-create-a-plan-in-another-region"></a>Para criar um plano noutra região
1. Inicie sessão no [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/).
2. Clique nas **planos** opção de menu.
3. Nos planos pela página de exibição de mensagens em fila, clique em **New**.
4. Partir do **subscrição** lista pendente, selecione a subscrição na qual o novo plano irá residir.
5. Partir do **região** lista pendente, selecione uma região para o novo plano. As opções de plano de mensagens em fila para a região selecionada serão apresentados no **opções de plano** secção da página.
6. Partir do **grupo de recursos** lista pendente, selecione um recurso de grupo para o plano. A partir de mais informações sobre grupos de recursos, veja [descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
7. Na **nome do plano** escreva o nome do plano.
8. Sob **as opções do plano**, clique em nível de faturação para o novo plano.
9. Clique em **Criar**.

## <a name="deploying-the-web-service-to-another-region"></a>Implementar o serviço web para outra região
1. Clique nas **serviços da Web** opção de menu.
2. Selecione o serviço Web que está a implementar uma nova região.
3. Clique em **cópia**.
4. Na **nome do serviço Web**, escreva um novo nome para o serviço web.
5. Na **descrição do serviço da Web**, escreva uma descrição para o serviço web.
6. Do **subscrição** lista pendente, selecione a subscrição na qual o novo serviço web irá residir.
7. Partir do **grupo de recursos** lista pendente, selecione um recurso de grupo para o serviço web. A partir de mais informações sobre grupos de recursos, veja [descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
8. Partir do **região** lista pendente, selecione a região na qual implementar o serviço web.
9. Partir do **conta de armazenamento** lista pendente, selecione um armazenamento de conta na qual armazenar o serviço web.
10. Partir do **plano de preços** lista pendente, selecione um plano na região que selecionou no passo 8.
11. Clique em **cópia**.

