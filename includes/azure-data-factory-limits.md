---
title: incluir ficheiro
description: incluir ficheiro
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 1/8/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 203168f27e06d58c192bc5009db077461738011a
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54122790"
---
Data factory é um serviço de multi-inquilino, que tem os seguintes limites predefinidos no local para se certificar de subscrições de cliente estão protegidas de cargas de trabalho entre si. Muitos dos limites podem ser facilmente aumentados para a sua subscrição até ao limite máximo ao contactar o suporte.

### <a name="version-2"></a>Versão 2

| Recurso | Limite Predefinido | Limite Máximo |
| -------- | ------------- | ------------- |
| Fábricas de dados numa subscrição do Azure | 50 | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Número total de entidades (Pipeline, conjuntos de dados, disparadores, serviços ligados, runtimes de integração) dentro de uma fábrica de dados | 5000 | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Núcleos de CPU total para Runtime(s) de integração de SSIS do Azure sob uma subscrição | 256 | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Execuções de pipeline em simultâneo por fábrica de dados (partilhada entre todos os pipelines na fábrica de) | 10,000  | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Atividades de máx. por pipeline (inclui atividades internas para contentores) | 40 | 40 |
| Parâmetros de máx. por pipeline | 50 | 50 |
| Itens ForEach | 100 000 | 100 000 |
| Paralelismo ForEach | 20 | 50 |
| Carateres por expressão | 8,192 | 8,192 |
| Intervalo mínimo de Acionador de janela em cascata | 15 min | 15 min |
| Tempo limite de máximo de execuções de atividades do pipeline | 7 dias | 7 dias |
| Bytes por objeto para objetos do pipeline <sup>1</sup> | 200 KB | 200 KB |
| Bytes por objeto para o conjunto de dados e objetos do serviço ligado <sup>1</sup> | 100 KB | 2000 KB |
| Unidades de integração de dados por execução de atividade de cópia <sup>3</sup> | 256 | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Escrever chamadas de API | 2500/h<br/><br/> Este limite é imposto pelo Azure Resource Manager, não o Azure Data Factory. | [Contacte o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). |
| Chamadas de API de leitura | 12,500/h<br/><br/> Este limite é imposto pelo Azure Resource Manager, não o Azure Data Factory. | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Monitorização de consultas por minuto | 1000 | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Operações CRUD de entidade por minuto | 50 | [Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>Versão 1

| **Recurso** | **Limite Predefinido** | **Limite Máximo** |
| --- | --- | --- |
| Fábricas de dados numa subscrição do Azure |50 |[Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Pipelines numa fábrica de dados |2500 |[Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Conjuntos de dados dentro de uma fábrica de dados |5000 |[Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Setores simultâneos por conjunto de dados |10 |10 |
| Bytes por objeto para objetos do pipeline <sup>1</sup> |200 KB |200 KB |
| Bytes por objeto para o conjunto de dados e objetos do serviço ligado <sup>1</sup> |100 KB |2000 KB |
| Núcleos de cluster a pedido do HDInsight numa subscrição <sup>2</sup> |60 |[Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Unidades de movimento de dados por execução de atividade de cópia da cloud <sup>3</sup> |32 |[Contactar o suporte](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| Número de execuções de atividades do pipeline de tentativas |1000 |MaxInt (32 bits) |

<sup>1</sup> pipeline, conjunto de dados e objetos do serviço ligado representam um agrupamento lógico de sua carga de trabalho. Limites para estes objetos não estão relacionadas com a quantidade de dados, pode mover e processar com o serviço do Azure Data Factory. Fábrica de dados foi concebida para escalar para processar petabytes de dados.

<sup>2</sup> núcleos de HDInsight a pedido são alocados fora a subscrição que contém a fábrica de dados. Como resultado, o limite acima é a fábrica de dados aplicado o limite de núcleos para núcleos de HDInsight a pedido e é diferente do que o limite de núcleos associado à subscrição do Azure.

<sup>3</sup> unidade de integração de dados (DIU) para v2 ou na Cloud dados de movimento de unidade (DMU) para a v1 está a ser utilizado numa operação de cópia na cloud para a cloud. É uma medida que representa o poder (uma combinação de CPU, memória e alocação de recursos de rede) de uma única unidade no Data Factory. Pode obter um maior débito de cópia utilizando DMUs mais em alguns cenários. Consulte a [unidades de integração de dados (V2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) e [unidades de movimento de dados (V1) da Cloud](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) secção em detalhes, e [página de preços do Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) para implicação de faturação.

<sup>4</sup> o Integration Runtime (IR) é a infraestrutura de computação utilizada pelo Azure Data Factory para fornecer as seguintes capacidades de integração de dados em diferentes ambientes de rede: movimento de dados, distribuição de atividades para serviços, de computação execução dos pacotes do SSIS. Para obter mais informações, consulte [descrição geral do Runtime de integração](../articles/data-factory/concepts-integration-runtime.md).

| **Recurso** | **Limite inferior predefinido** | **Limite mínimo** |
| --- | --- | --- |
| Intervalo de agendamento |15 minutos |15 minutos |
| Intervalo entre tentativas de repetição |1 segundo |1 segundo |
| Repita o valor de tempo limite |1 segundo |1 segundo |

#### <a name="web-service-call-limits"></a>Limites de chamada de serviço Web
O Gestor de recursos do Azure tem limites para chamadas de API. Pode fazer chamadas à API a uma taxa dentro de [limita a API do Azure Resource Manager](../articles/azure-subscription-service-limits.md#resource-group-limits).
