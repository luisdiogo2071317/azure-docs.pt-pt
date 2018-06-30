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
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: ajaycode
ms.component: na
ms.openlocfilehash: 96eb26d6a4faf8c6907d23ebf21f2446722c913b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127098"
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>As alterações de preço para o Monitor de desempenho de rede do Azure

Estamos a ter listened para os seus comentários e introduzidas recentemente um [novos preços experiência](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) para várias monitorização serviços através do Azure. Este artigo captura as preços alterações relacionadas com o Azure [Monitor de desempenho de rede](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) num formato de fácil leitura pergunta e resposta.

Monitor de desempenho da rede consiste em três componentes:
* [Monitor de desempenho](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Monitor de ponto final de serviço](https://docs.microsoft.com/azure/networking/network-monitoring-overview#service-endpoint-monitor)
* [Monitor do ExpressRoute](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

As secções seguintes explicam as alterações de preços para os componentes NPM.

## <a name="performance-monitor"></a>Monitorização de Desempenho

**Como foi a utilização do Monitor de desempenho cobrados no modelo antigo?**

A faturação de NPM foi com base na utilização e consumo dos dois componentes:
* **Nós**: todas as transações sintéticas provir e terminam os nós. Nós são também conhecidos como agentes ou agentes de gestão da Microsoft.
* **Dados**: os resultados dos testes de rede de vários são armazenados no repositório de Log Analytics do Azure.

No modelo de antigo, a fatura foi calculada com base no número de nós e o volume de dados gerados. 

**Como é cobrada a utilização do Monitor de desempenho no modelo de novo?**

A funcionalidade de Monitor de desempenho no NPM agora é faturada com base numa combinação de: 

* Ligações de sub-rede monitorizadas
* Volume de dados

**O que é uma ligação de sub-rede?**

Monitor de desempenho monitoriza a conectividade entre dois ou mais localizações na rede. A ligação entre um grupo de nós ou agentes de uma sub-rede e um grupo de nós na outra sub-rede, chama-se uma ligação de sub-rede.

**Posso ter duas sub-redes (A e B) e tiver vários agentes em cada sub-rede. Monitor de desempenho monitoriza a conectividade de todos os agentes na sub-rede d para todos os agentes na sub-rede B. Será posso cobrado com base no número de ligações de sub-rede entre?**

Não. Para efeitos de faturação, todas as ligações de sub-rede A sub-rede B são agrupadas para uma ligação de sub-rede. Será cobrado para uma ligação única. Monitor de desempenho continua a monitorizar a conectividade entre vários agentes em cada sub-rede.

**Quais são os custos para monitorizar uma ligação de sub-rede?**

O custo de uma ligação de sub-rede única de monitorização para o mês completo, consulte o [Ping Mesh](https://azure.microsoft.com/pricing/details/network-watcher/) secção.

**Quais são os custos de dados que gera o Monitor de desempenho?**

A taxa ingestão (carregamento de dados para análise de registos, processamento e indexação) está disponível na [página de preços](https://azure.microsoft.com/pricing/details/log-analytics/) para análise de registos, na secção de ingestão de dados. A taxa de retenção de dados (ou seja, dados retidos na opção do cliente, para além do primeiro mês) também está disponível na [página de preços](https://azure.microsoft.com/pricing/details/log-analytics/), na secção de retenção de dados.


## <a name="expressroute-monitor"></a>Monitor do ExpressRoute

**Quais são os custos de utilização do Monitor de ExpressRoute?**

Custos de Monitor do ExpressRoute são cobrados com base no volume de dados gerados durante a monitorização. Para obter mais informações, consulte "Quais são os custos de dados que gera o Monitor de desempenho?"

**Utilizar o Monitor de ExpressRoute para monitorizar vários circuitos do ExpressRoute. Estou posso cobrado com base no número de circuitos a ser monitorizado?**

Não lhe serem cobrados com base no número de circuitos ou o tipo de peering (por exemplo, peering privado, peering da Microsoft). São-lhe cobrados com base no volume de dados, conforme explicado anteriormente.

**O que é o volume de dados gerados ao ExpressRoute monitoriza um único circuito?**

O volume de dados gerados por mês, quando o ExpressRoute monitoriza uma ligação de peering privada, é o seguinte:

|Percentil      |Dados/mês (MB)|
| :---:          |           ---:|
|50<sup>ésimo</sup> |            192|
|60<sup>ésimo</sup> |            256|
|70<sup>ésimo</sup> |            360|
|80<sup>ésimo</sup> |            498|
|90<sup>ésimo</sup> |            870|
|95<sup>ésimo</sup> |           1560|


Em conformidade com esta tabela, os clientes, o percentil 50th paga 192 MB de dados. EUR $2.30/GB para o primeiro mês, o custo tarifas para a monitorização um circuito está EUR $0.43 (= 192 * 2.30 / 1024).

**Quais são algumas razões para acomodar eventuais variações no volume de dados?**

O volume de dados gerados de monitorização depende em diversos fatores, tais como:
* Número de agentes. Aumenta a precisão do isolamento de falhas com um aumento no número de agentes.
* Número de saltos na rede.
* Número de caminhos entre a origem e destino.

Os clientes nos percentiles superiores (na tabela anterior), normalmente, monitorizar os circuitos do vários pontos de vantage na sua rede no local. Múltiplos agentes também são colocados mais profundo na rede, farther from o router de limite de fornecedor de serviço. Os agentes, muitas vezes, são colocados em vários sites de utilizador, ramos e bastidores nos centros de dados.

## <a name="service-endpoint-monitor"></a>Monitor de ponto final de serviço

**Quais são os custos de utilização do Monitor de ponto final de serviço?**

Custos de utilização do Monitor de ponto final de serviço são calculados com base em:
* Número de ligações
* Volume de dados

**O que é uma ligação?**

Uma ligação é um teste de acessibilidade para um ponto final (serviço de rede ou URL) de um único agente para o mês completo. Por exemplo, a monitorização de uma ligação para bing.com de três agentes constitui três ligações.

**Quais são os custos de Monitor de ponto final de serviço?**

Consulte o [ligação monitorização](https://azure.microsoft.com/pricing/details/network-watcher/) secção o custo de um ponto final de monitorização para o mês completo. A taxa de dados está disponível na [página de preços](https://azure.microsoft.com/pricing/details/log-analytics/) para análise de registos, na secção de ingestão de dados.

## <a name="references"></a>Referências

[FAQ sobre preços de análise de registo](https://azure.microsoft.com/pricing/details/log-analytics/): secção das FAQ o tem informações no escalão gratuito, por nó preços e outros detalhes de preços.

