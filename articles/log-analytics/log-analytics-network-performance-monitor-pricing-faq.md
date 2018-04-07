---
title: FAQ sobre preços para o Monitor de desempenho de rede do Azure | Microsoft Docs
description: Perguntas mais frequentes - Monitor de desempenho de rede do Azure
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: cherylmc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.openlocfilehash: 5b2335ee2584af07ed23ce87be92a869f3a07ba1
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>As alterações de preço para o Monitor de desempenho de rede do Azure

Iremos ouvir os seus comentários e recentemente ter introduzido um [novos preços experiência](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/), para várias monitorização serviços através do Azure.

Este documento captura as preços alterações relacionadas com o Azure [Monitor de desempenho de rede](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM), em qualquer fáceis de ler o formato de pergunta e resposta.

Monitor de desempenho da rede consiste em três componentes:
* [Monitor de desempenho](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Monitor de ponto final do serviço](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor) e
* [ExpressRoute Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

A secção abaixo explica as alterações de preços para os componentes acima.

## <a name="performance-monitor-pm"></a>Monitor de desempenho (horas)

**Como foi a utilização do Monitor de desempenho cobrados no modelo antigo?**

A faturação de NPM foi com base no consumo/utilização dois componentes:
* Nós: Todas as transações sintéticas provir e terminam os nós. Nós são também conhecidos como agentes ou MMA (agentes de gestão da Microsoft).
* Dados: Os resultados dos testes de rede de vários são armazenados no repositório de análise de registos.

No modelo de antigo, a fatura foi calculada com base no número de nós e o volume de dados gerados. 

**Como é a utilização do Monitor de desempenho, cobrado no modelo de novo?**

A funcionalidade de Monitor de desempenho no NPM agora é faturada numa combinação de: 

* ligações de sub-rede monitorizadas e
* Volume de dados

**O que é uma ligação de sub-rede?**

Monitor de desempenho monitoriza a conectividade entre dois ou mais localizações na rede.  A ligação entre um grupo de nós/agentes de uma sub-rede e um grupo de nós na outra sub-rede, é referido como uma ligação de sub-rede.

**Posso ter duas sub-redes (sub-rede A e B) e têm vários agentes em cada sub-rede.  Monitor de desempenho monitoriza a conectividade de todos os agentes na sub-rede d para todos os agentes na sub-rede B.  Será posso cobrado com base no número de ligações de sub-rede entre?**

Não. Para efeitos de faturação, todas as ligações de sub-rede A sub-rede B, são agrupadas uma ligação de sub-rede e é faturado por uma ligação única.  Monitor de desempenho irá continuar a monitorizar a conectividade entre vários agentes em cada sub-rede.

**Quais são os custos para monitorizar uma ligação de sub-rede?**

Consulte a secção intitulada [Ping Mesh](https://azure.microsoft.com/pricing/details/network-watcher/) o custo de uma ligação de sub-rede única de monitorização para o mês completo.

**Quais são os encargos para os dados gerados pelo Monitor de desempenho?**

A taxa ingestão (carregamento de dados para análise de registos, processamento e indexação) está disponível na [página de preços](https://azure.microsoft.com/pricing/details/log-analytics/) para análise de registos.  (Secção: ingestão de dados).

A taxa de retenção de dados (ou seja, dados retidos na opção do cliente, para além do primeiro mês) também está disponível na [página de preços](https://azure.microsoft.com/pricing/details/log-analytics/).  (Secção: retenção de dados).


## <a name="expressroute-monitor-erm"></a>Monitor de ExpressRoute (ERM)

**Quais são os custos de utilização do Monitor de ExpressRoute?**

Custos de Monitor do ExpressRoute são cobrados com base no volume de dados gerados durante a monitorização.   Consulte a questão "Quais são os custos de dados gerados pelo Monitor de desempenho?" Para obter mais informações.

**Posso utilizar ERM para monitorizar vários circuitos do ExpressRoute. Estou posso cobrado com base no número de circuitos a ser monitorizado?**

Não lhe serem cobrados com base no número de circuitos ou o tipo de peering (por exemplo, peering privado, peering da Microsoft).  São-lhe cobrados no volume de dados, tal como explicado anteriormente.

**O que é o volume de dados gerados ao monitorizar um único circuito?**

O volume de dados gerados por mês, quando a monitorização de uma ligação de peering privada está da seguinte forma:

|Percentil      |Dados/mês (MB)|
| :---:          |           ---:|
|50<sup>ésimo</sup> |            192|
|60<sup>th</sup> |            256|
|70<sup>ésimo</sup> |            360|
|80<sup>th</sup> |            498|
|90<sup>ésimo</sup> |            870|
|95<sup>ésimo</sup> |           1560|


Pela tabela acima, os clientes, o percentil 50th paga 192 MB de dados. EUR $2.30/GB para o primeiro mês, o custo tarifas para a monitorização um circuito está EUR 0.43 (= 192 * 2.30 / 1024) para o mês primeiro.

**Quais são algumas das razões para variações no volume de dados?**

O volume de dados gerados de monitorização depende de vários fatores, tais como:
* número de agentes - a precisão do isolamento de falhas aumenta com um aumento no número de agentes
* número de saltos na rede
* número de caminhos entre a origem e destino

Os clientes nos percentiles superiores (na tabela acima), monitorizar, normalmente, os circuitos do vários pontos de vantage na sua rede no local.  Múltiplos agentes também são colocados mais profundo na rede, farther from o router de limite de fornecedor de serviço. Os agentes, muitas vezes, colocados em vários sites de utilizador, ramos e bastidores nos centros de dados.

## <a name="service-endpoint-monitor-sepm"></a>Monitor de ponto final de serviço (SEPM)

**Quais são os custos de utilização do Monitor de ponto final de serviço?**

Custos de utilização do Monitor de ponto final de serviço são calculados com base em:
* número de ligações
* volume de dados

**O que é uma ligação?**

Uma ligação é um teste de acessibilidade para um ponto final (serviço de rede ou URL) de um único agente para o mês completo. Por exemplo, a monitorização de uma ligação para bing.com de três agentes constitui três ligações.

**Quais são os custos de Monitor de ponto final de serviço?**

- Consulte o [ligação monitorização](https://azure.microsoft.com/pricing/details/network-watcher/) secção, o custo de um ponto final de monitorização para o mês completo.
- A taxa de dados está disponível na [página de preços](https://azure.microsoft.com/pricing/details/log-analytics/) para análise de registos.  (Secção: ingestão de dados).

## <a name="references"></a>Referências

- [FAQ sobre preços de análise de registo](https://azure.microsoft.com/pricing/details/log-analytics/) -secção das FAQ o tem informações no escalão gratuito, por nó preços, etc.

