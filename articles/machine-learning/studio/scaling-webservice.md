---
title: Como aumentar a simultaneidade de um serviço web Azure Machine Learning | Documentos da Microsoft
description: Saiba como aumentar a simultaneidade de um serviço web Azure Machine Learning ao adicionar pontos finais adicionais.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.custom: (previous ms.author yahajiza)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
keywords: do Azure machine learning, serviços da web, operacionalização, dimensionamento, ponto de extremidade, simultaneidade
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.component: studio
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.openlocfilehash: f0b639d27dd5114c47bd5a1cfa0f6a72a6d78d83
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824188"
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Dimensionar um serviço web Azure Machine Learning ao adicionar pontos finais adicionais
> [!NOTE]
> Este tópico descreve técnicas aplicáveis a uma **clássico** serviço Web do Machine Learning. 
> 
> 

Por predefinição, cada serviço Web publicado está configurado para suportar 20 pedidos simultâneos e pode ser tão elevado como 200 pedidos em simultâneo. O Azure Machine Learning otimiza automaticamente a definição para proporcionar o melhor desempenho do web Service e o valor de portal é ignorado. 

Se tiver de suportar plano para chamar a API com uma carga maior que um valor de chamadas simultâneas do máximo de 200, deve criar vários pontos de extremidade no mesmo serviço da Web. Em seguida, pode distribuir aleatoriamente a carga em todos os.

O dimensionamento de um serviço Web é uma tarefa comum. São algumas razões para dimensionar para suportar mais de 200 pedidos em simultâneo, aumentar a disponibilidade por meio de vários pontos de extremidade ou fornecer pontos de extremidade separados para o serviço web. Pode aumentar a escala ao adicionar pontos finais adicionais para o mesmo serviço da Web através da [serviço da Web do Azure Machine Learning](https://services.azureml.net/) portal.

Para obter mais informações sobre como adicionar novos pontos de extremidade, consulte [criando pontos de extremidade](create-endpoint.md).

Tenha em atenção que pode ser prejudicial se não estiver a chamar a API com uma taxa elevada do mesmo modo com uma contagem alta de simultaneidade. Poderá ver tempos limite esporádica e/ou picos na latência se colocar uma carga relativamente baixa numa API configurada para uma carga elevada.

As APIs síncronas são normalmente utilizadas em situações em que uma baixa latência for o pretendido. Latência aqui indica o tempo necessário para a API concluir um pedido e, não se responsabiliza por qualquer atrasos de rede. Digamos que tem uma API com uma latência de 50 ms. Para consumir totalmente a capacidade disponível com o nível de limitação do alto e máx. de chamadas simultâneas = 20, precisa chamar esta API 20 * 1000 / 50 = 400 horas por segundo. Expandir isso ainda mais, um máximo de chamadas simultâneas de 200 permite que chamar a API 4000 de vezes por segundo, supondo que uma latência de 50 ms.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
