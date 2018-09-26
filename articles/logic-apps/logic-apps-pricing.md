---
title: Preços e faturação - Azure Logic Apps | Documentos da Microsoft
description: Saiba como funciona o preços e faturação no Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: logic-apps
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 5f9147035c07bbe4fb3f38b74025015e70dd87b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159573"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Modelo de preços do Azure Logic Apps

Pode criar e executar fluxos de trabalho de integração dimensionável automatizada na nuvem com o Azure Logic Apps. Aqui estão os detalhes sobre como funcionam de faturação e preços para o Logic Apps. 

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Modelo de preços de consumo

Para o novo logic apps que criar com o serviço de aplicações lógicas "global" ou público, paga apenas aquilo que utiliza. Estas aplicações de lógica utilizam um plano com base no consumo e o modelo de preços, o que significa que todas as execuções de ações realizadas por uma aplicação lógica são limitadas. Cada passo numa definição de aplicação lógica é uma ação, o que inclui acionadores, passos de fluxo de controle, chamadas para ações internas e chamadas de conectores. Para obter mais informações, consulte [preços de aplicações lógicas](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Modelo de preços fixo

> [!NOTE]
> O ambiente de serviço de integração está em *pré-visualização privada*. Para pedir acesso [criar o pedido de associação aqui](https://aka.ms/iseprivatepreview).

Para o novo logic apps que criar com um [ *ambiente de serviço de integração* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), que é privada isolada a instância de Logic Apps que utiliza recursos dedicados, paga um preço mensal fixo para ações incorporadas e conectores padrão com etiqueta ISE. O ISE inclui um conector empresarial sem custos, embora os conectores de empresa adicionais são cobrados com base no preço de consumo Enterprise. Para obter mais informações, consulte [preços de aplicações lógicas](https://azure.microsoft.com/pricing/details/logic-apps).

<a name="triggers"></a>

## <a name="triggers"></a>Acionadores

Os acionadores são ações especiais que criar uma instância da aplicação lógica quando ocorre um evento específico. Acionadores agirem de diversas formas, que afetam a como a aplicação lógica é limitada.

* **Acionador de consulta** – este acionador verifica continuamente um ponto final para mensagens que satisfazem os critérios para criar uma instância da aplicação lógica e iniciar o fluxo de trabalho. Cada solicitação de consulta é contabilizado como uma execução, limitada, mesmo quando nenhuma instância da aplicação lógica é criada. Para especificar o intervalo de consulta, configure o acionador através do Estruturador da aplicação lógica.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Acionador de Webhook** – este acionador aguarda que um cliente para enviar um pedido para um ponto de extremidade específico. Cada pedido enviado para o ponto final do webhook é contabilizado como uma execução da ação. Por exemplo, o acionador de pedido e Webhook de HTTP são ambos os acionadores de webhook.

* **Acionador de periodicidade** – este acionador cria uma instância da aplicação lógica com base no intervalo de periodicidade que configurou no acionador. Por exemplo, pode definir um acionador de periodicidade, que é executado a cada três dias ou com base numa agenda mais complexa.

Pode encontrar as execuções de Acionador no painel de descrição geral da sua aplicação lógica na secção de histórico de Acionadores.

## <a name="actions"></a>Ações

Ações incorporadas, tais como ações que chamam HTTP, as funções do Azure ou gestão de API e também controlam os passos de fluxo são medidas como ações nativas, que têm os respetivos tipos. Ações que chamam [conectores](https://docs.microsoft.com/connectors) tem o tipo "ApiConnection". Estes conectores são classificados como standard ou enterprise de conectores, que são medidas com base em seus respectivos [preços][pricing]. Conectores empresariais na *pré-visualização* é cobrada conforme os conectores padrão.

Todas as ações com êxito e sem êxito execução são contadas e medidas como execuções de ação. No entanto, as ações que são ignoradas, devido a condições por cumprir ou ações que não são executados, uma vez que a aplicação lógica terminada antes da conclusão, não contam como execuções de ação. Aplicações lógicas desativado não é possível instanciar a novas instâncias, portanto, eles não são cobrados enquanto eles estão desativados.

> [!NOTE]
> Depois de desativar uma aplicação lógica, quaisquer instâncias em execução podem demorar algum tempo antes que eles parem completamente.

Ações que executam dentro de ciclos são contabilizadas por cada ciclo no loop. Por exemplo, uma única ação num loop "for each" que processa uma lista de itens de 10 é contada ao multiplicar o número de itens de lista (10) pelo número de ações no loop (1) mais um para iniciar o loop. Então, neste exemplo, o cálculo é (10 * 1) + 1, o que resulta em 11 execuções de ação.

## <a name="integration-account-usage"></a>Utilização da conta de integração

Utilização baseado no consumo inclui uma [conta de integração](logic-apps-enterprise-integration-create-integration-account.md) onde pode explorar, desenvolver e testar o [B2B/EDI](logic-apps-enterprise-integration-b2b.md) e [processamento XML](logic-apps-enterprise-integration-xml.md) funcionalidades nas aplicações lógicas ao não custos adicionais. Pode ter uma conta de integração por região e o arquivo até específicas [números dos artefactos](../logic-apps/logic-apps-limits-and-config.md), tais como parceiros comerciais EDI e contratos, mapas, esquemas, assemblies, certificados e configurações de lote.

O Logic Apps também oferece a contas de integração de básico e standard com um SLA de aplicações lógicas suportados. Pode utilizar contas de integração básica quando quiser usar apenas manipulação de mensagens, ou agir como um parceiro de pequenas empresas que possua uma relação de parceiro comercial com uma entidade de negócio maior. As contas de integração Standard suportam relações de B2B mais complexas e aumentam o número de entidades que pode gerir. Para obter mais informações, consulte [os preços do Azure](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre o Logic Apps][whatis]
* [Criar a sua primeira aplicação lógica][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-overview.md
[create]: quickstart-create-first-logic-app-workflow.md

