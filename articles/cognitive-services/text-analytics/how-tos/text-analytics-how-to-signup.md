---
title: Inscrever-se na API de Análise de Texto (Serviços Cognitivos da Microsoft no Azure) | Microsoft Docs
description: Instruções para se inscrever e utilizar a análise de texto e funcionar dentro dos limites.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: get-started-article
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: dfa5ba138a2e0db75dfc097ca2430fe9c82e826f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39623254"
---
# <a name="how-to-sign-up-for-text-analytics-api"></a>Como se inscrever na API de Análise de Texto

Os recursos de análise de texto estão disponíveis permanentemente na cloud. Antes de poder carregar os seus conteúdos para análise, tem de se inscrever para obter uma chave de acesso. Cada chamada à API requer uma chave de acesso no pedido.

+ Comece com uma subscrição do Azure. Pode criar uma [conta gratuita](https://azure.microsoft.com/free/) para experimentar sem custos.

+ Escolha a **API de Análise de Texto** para criar uma [conta de API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). A chave é gerada quando se inscreve.

A Análise de Texto tem disponível um escalão Gratuito para exploração e avaliação, bem como escalões faturáveis para cargas de trabalho de produção. Pode ter várias inscrições em cada subscrição: uma gratuita, uma paga, e assim sucessivamente. Se o seu volume de pedidos aumentar, pode mudar para um escalão que ofereça mais transações.

Os serviços em pré-visualização ou no escalão Gratuito não têm contrato de nível de serviço. Para obter mais informações, veja [SLA para Serviços Cognitivos](https://azure.microsoft.com/support/legal/sla/cognitive-services/v1_1/)

## <a name="how-to-change-tiers"></a>Como alterar o escalão

Comece o escalão Gratuito e mude para um faturável para cargas de trabalho de produção. A faturação standard é oferecida em níveis proporcionais. Pode mudar de escalão e manter o mesmo ponto final e as mesmas chaves de acesso.

1. Inicie sessão no [portal do Azure](https://portal.azure.com) e [localize o seu serviço](text-analytics-how-to-access-key.md).

2. Clique em **Escalão de preço**.

   ![Comando do escalão de preço no menu de navegação do lado esquerdo](../media/portal-pricing-tier.png)

3. Escolha um escalão e clique em **Selecionar**.  Os limites novos entram em vigor assim que a seleção for processada. 

   ![Mosaicos e botão Selecionar na página de seleção do escalão](../media/portal-choose-tier.png)

## <a name="how-billing-works"></a>Como funciona a faturação

A faturação é calculada com base no número de transações. Pode comprar um bloco de transações num determinado escalão num ciclo de faturação mensal e, se o exceder, é aplicada uma pequena taxa de utilização excedida por transação. Se exceder o limite máximo regularmente, considere mudar para um escalão mais alto.

Veja a [página de preços](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) para obter mais informações.

### <a name="what-constitutes-a-transaction-in-the-text-analytics-api"></a>O que constitui uma transação na API de Análise de Texto?
Qualquer anotação num documento conta como uma transação. As chamadas de classificação de batch também considerarão o número de documentos que devem ser classificados nessa transação. Por exemplo, se forem enviados 1000 documentos para análise de sentimentos numa única chamada à API, irão contar como 1000 transações.

## <a name="see-also"></a>Consulte também 

 [Text Analytics Overview](../overview.md) (Descrição Geral da Análise de Texto)  
 [Perguntas Mais Frequentes (FAQ)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Get an access key](text-analytics-how-to-access-key.md) (Obter uma chave de acesso)
