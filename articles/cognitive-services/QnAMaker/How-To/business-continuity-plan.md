---
title: Criar um plano de continuty de negócio para o seu serviço de QnA Maker - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como criar um plano de continuidade do negócio para o seu serviço de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: ca6e54b8a8ca8b38e8ef6b1a148f8b2c54bd43da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353984"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Criar um plano de continuidade do negócio para o seu serviço de QnA Maker

É o principal objetivo do plano de continuidade de negócio criar um ponto de final da base de dados de conhecimento resiliente, que que seria não tempo para o Bot ou a aplicação de consumo-lo.

![Plano de bcp Maker de QnA](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

A ideia de alto nível conforme representado acima é o seguinte:

1. Configurar dois paralelo [serviços de QnA Maker](../How-To/set-up-qnamaker-service-azure.md) no [Azure emparelhado regiões](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).

2. Os índices de pesquisa do Azure, primária e secundária manter sincronizadas. Utilize o exemplo de github [aqui](https://github.com/pchoudhari/QnAMakerBackupRestore) para ver como cópia de segurança-restauro índices do Azure.

3. Cópia de segurança utilizando o Application Insights [a exportação contínua](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry).

4. Assim que tem configuradas as pilhas primários e secundários, utilize [Gestor de tráfego](https://docs.microsoft.com/en-us/azure/traffic-manager/) para configurar os dois pontos finais e configurar um método de encaminhamento.

5. Terá de criar um certificado SSL para o ponto de final do Gestor de tráfego. [Vincular o certificado SSL](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-ssl) nos serviços de aplicação.

6. Por fim, utilize o ponto de final do Gestor de tráfego na sua Bot ou aplicação.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Escolha a capacidade para a implementação de QnA Maker](../Tutorials/choosing-capacity-qnamaker-deployment.md)
