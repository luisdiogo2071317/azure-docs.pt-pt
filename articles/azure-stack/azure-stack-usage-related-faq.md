---
title: Perguntas mais frequentes relacionadas com a API de utilização | Microsoft Docs
description: Lista de medidores de pilha do Azure, a comparação com a API de utilização do Azure, tempo de utilização e comunicadas hora, códigos de erro.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: ac22ba34bff1d5321c05bc0a0a1b14ca742079a7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051509"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Perguntas mais frequentes sobre a utilização de pilha de Azure API do

Este artigo responde a algumas perguntas mais frequentes sobre a API de utilização de pilha do Azure.

## <a name="what-meter-ids-can-i-see"></a>O IDs de medidor posso ver?
Utilização é comunicada para os seguintes fornecedores de recursos:

**Rede**  
  
**ID de medidor**: F271A8A388C44D93956A063E1D2FA80B  
**Nome de medição**: utilização de endereços IP estáticos  
**Unidade**: endereços IP  
**Notas**: contagem de endereços IP utilizados. Se chamar a API de utilização com uma granularidade diária, o medidor devolve o endereço IP, multiplicado pelo número de horas.  
  
**ID de medidor**: 9E2739BA86744796B465F64674B822BA  
**Nome de medição**: utilização de endereços de IP dinâmico  
**Unidade**: endereços IP  
**Notas**: contagem de endereços IP utilizados. Se chamar a API de utilização com uma granularidade diária, o medidor devolve o endereço IP, multiplicado pelo número de horas.  
  
**Armazenamento**  
  
**ID de medidor**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Nome de medição**: TableCapacity  
**Unidade**: GB\*horas  
**Notas**: Total de capacidade consumida por tabelas.  
  
**ID de medidor**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Nome de medição**: PageBlobCapacity  
**Unidade**: GB\*horas  
**Notas**: Total de capacidade consumida por blobs de páginas.  
  
**ID de medidor**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Nome de medição**: QueueCapacity  
**Unidade**: GB\*horas  
**Notas**: Total de capacidade consumida pela fila.  
  
**ID de medidor**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Nome de medição**: BlockBlobCapacity  
**Unidade**: GB\*horas  
**Notas**: Total de capacidade consumida por blobs de blocos.  
  
**ID de medidor**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Nome de medição**: TableTransactions  
**Unidade**: contagem de 10, de pedido 000's  
**Notas**: tabela pedidos de serviço (em 10 000s).  
  
**ID de medidor**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Nome de medição**: TableDataTransIn  
**Unidade**: dados de entrada em GB  
**Notas**: entrada de dados do serviço em GB de tabela.  
  
**ID de medidor**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Nome de medição**: TableDataTransOut  
**Unidade**: saída em GB  
**Notas**: tabela de saída de dados do serviço em GB  
  
**ID de medidor**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Nome de medição**: BlobTransactions  
**Unidade**: contagem de pedidos nas 10, 000's  
**Notas**: Blob pedidos de serviço (em 10 000s).  
  
**ID de medidor**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Nome de medição**: BlobDataTransIn  
**Unidade**: dados de entrada em GB  
**Notas**: entrada de dados do serviço em GB do Blob.  
  
**ID de medidor**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Nome de medição**: BlobDataTransOut  
**Unidade**: saída em GB  
**Notas**: Blob de saída de dados do serviço em GB.  
  
**ID de medidor**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Nome de medição**: QueueTransactions  
**Unidade**: contagem de pedidos nas 10, 000's  
**Notas**: fila de pedidos de serviço (em 10 000s).  
  
**ID de medidor**: E518E809-E369-4A45-9274-2017B29FFF25  
**Nome de medição**: QueueDataTransIn  
**Unidade**: dados de entrada em GB  
**Notas**: fila de entrada de dados do serviço em GB.  
  
**ID de medidor**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Nome de medição**: QueueDataTransOut  
**Unidade**: saída em GB  
**Notas**: fila de saída de dados do serviço em GB  
  
**RP do SQL Server**  
  
**ID de medidor**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Nome de medição**: DatabaseSizeHourSqlMeter  
**Unidade**: MB\*horas  
**Notas**: capacidade Total DB durante a criação. Se chamar a API de utilização com uma granularidade diária, o medidor devolve MB multiplicada pelo número de horas.  
  
**MySql RP**  
  
**ID de medidor**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Nome de medição**: DatabaseSizeHourMySqlMeter  
**Unidade**: MB\*horas  
**Notas**: capacidade Total DB durante a criação. Se chamar a API de utilização com uma granularidade diária, o medidor devolve MB multiplicada pelo número de horas.  
  
**Computação**  
  
**ID de medidor**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Nome de medição**: horas de tamanho de VM de Base  
**Unidade**: horas de núcleo Virtual  
**Notas**: número de núcleos virtuais multiplicado pelo horas a VM foi executada.  
  
**ID de medidor**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Nome de medição**: horas de tamanho de VM do Windows  
**Unidade**: horas de núcleo Virtual  
**Notas**: número de núcleos virtuais multiplicado pelo horas a VM foi executada.  
  
**ID de medidor**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Nome de medição**: horas de tamanho VM  
**Unidade**: horas para VM  
**Notas**: captura Base e VM do Windows. Não ajuste para núcleos.  
  
**Cofre de Chaves**  
  
**ID de medidor**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Nome de medição**: transações do Cofre de chaves  
**Unidade**: contagem de 10, de pedido 000's  
**Notas**: recebidos pelo plane de dados do Cofre de chaves de pedidos de número de API REST.  
  
**ID de medidor**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Nome de medição**: avançadas transações de chaves  
**Unidade**: 10 mil transações  
**Notas**: RSA 3 K/4K, transações de chaves ECC. (pré-visualização).  
  
*Serviço de aplicações**  
  
**ID de medidor**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Nome de medição**: serviço de aplicações  
**Unidade**: horas de núcleo Virtual  
**Notas**: número de núcleos virtuais utilizada para executar o serviço de aplicações. Nota: A Microsoft utiliza este medidor para cobram o serviço de aplicações na pilha do Azure. Fornecedores de serviços em nuvem pode utilizar as outras App Service medidores (abaixo) para calcular utilização para os seus inquilinos.  
  
**ID de medidor**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Nome de medição**: pedidos de funções  
**Unidade**: 10 pedidos  
**Notas**: número Total de execuções de pedido (por 10 execuções). Execuções são contadas sempre que uma função é executada em resposta a um evento ou é acionada por um enlace.  
  
**ID de medidor**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Nome de medição**: funções - de computação  
**Unidade**: GB-s  
**Notas**: consumo de recursos medido em segundos de gigabyte (GB/s). **Consumo de recursos de observados** é calculado multiplicando o tamanho de memória médio em GB do tempo em milissegundos, que demora a executar a função. Memória utilizada por uma função é medida por arredondamento até o MB mais próximo de 128, até ao tamanho máximo de memória de 1,536 MB, com o tempo de execução calculado por arredondamento até o mais próximo 1 ms. O tempo de execução mínimo e a memória para uma execução única função é 100 ms e 128 mb, respetivamente.  
  
**ID de medidor**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Nome de medição**: horas do serviço de aplicações partilhado  
**Unidade**: 1 hora  
**Notas**: por utilização da hora de partições horizontais plano do App Service. Planos são limitados numa base por aplicação.  
  
**ID de medidor**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Nome de medição**: horas do serviço de aplicações gratuito  
**Unidade**: 1 hora  
**Notas**: por utilização de hora do plano do livre App Service. Planos são limitados numa base por aplicação.  
  
**ID de medidor**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Nome de medição**: pequeno horas do serviço de aplicações Standard  
**Unidade**: 1 hora  
**Notas**: calculado com base no tamanho e número de instâncias.  
  
**ID de medidor**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Nome de medição**: horas média do serviço de aplicações Standard  
**Unidade**: 1 hora  
**Notas**: calculado com base no tamanho e número de instâncias.  
  
**ID de medidor**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Nome de medição**: grandes horas do serviço de aplicações Standard  
**Unidade**: 1 hora  
**Notas**: calculado com base no tamanho e número de instâncias.  
  
**Camadas de trabalho personalizado**  
  
**ID de medidor**: *camadas de trabalho personalizado*  
**Nome de medição**: camadas de trabalho personalizado  
**Unidade**: horas  
**Notas**: ID de medidor determinista é criado com base no SKU e nome de camada de trabalho personalizado. Este ID de medidor é exclusivo para cada camada de trabalho personalizado.  
  
**ID de medidor**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Nome de medição**: SNI SSL  
**Unidade**: por enlace SNI SSL  
**Notas**: serviço de aplicações suportam dois tipos de ligações de SSL: ligações de SSL indicação de nome de servidor (SNI) e ligações de SSL de endereços IP. O SSL baseado em SNI funciona em browsers modernos, enquanto que o SSL baseado em IP funciona em todos os browsers.  
  
**ID de medidor**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Nome de medição**: IP SSL  
**Unidade**: por IP com base em enlace SSL  
**Notas**: serviço de aplicações suportam dois tipos de ligações de SSL: ligações de SSL indicação de nome de servidor (SNI) e ligações de SSL de endereços IP. O SSL baseado em SNI funciona em browsers modernos, enquanto que o SSL baseado em IP funciona em todos os browsers.  
  
**ID de medidor**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Nome de medição**: o processo de Web  
**Unidade**:  
**Notas**: calculado por site do Active Directory por hora.  
  
**ID de medidor**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Nome de medição**: largura de banda de saída externa  
**Unidade**: GB  
**Notas**: pedido de entrada Total de bytes da resposta + total pedidos a enviar bytes + total FTP recebido pedidos bytes da resposta + total web recebido implementar bytes de resposta do pedido.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Como fazer a utilização de pilha do Azure APIs comparam com a [utilização do Azure API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (atualmente em pré-visualização pública)?
* A API de utilização do inquilino é consistente com a API do Azure, com uma exceção: o *showDetails* sinalizador atualmente não é suportado na pilha do Azure.
* A API de utilização de fornecedor só se aplica a pilha do Azure.
* Atualmente, o [RateCard API](https://msdn.microsoft.com/library/azure/mt219004.aspx) que está disponível no Azure não está disponível na pilha do Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>O que é a diferença entre a hora de utilização e tempo comunicado?
Relatórios de dados de utilização tem dois valores de hora principal:

* **Comunicado tempo**. A hora quando o evento de utilização introduziu o sistema de utilização
* **Tempo de utilização**. A hora quando o recurso de pilha do Azure foi consumido

Poderá ver uma discrepância valores de tempo de utilização e tempo comunicado para um evento de utilização específicos. O atraso pode ser tão longo quanto várias horas, em qualquer ambiente.

Atualmente, pode consultar apenas pelo *tempo comunicado*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>O que estes códigos de erro de API de utilização significa?
| **Código de estado HTTP** | **Código de erro** | **Descrição** |
| --- | --- | --- |
| Pedido de 400/incorretos |*NoApiVersion* |O *api-version* parâmetro de consulta está em falta. |
| Pedido de 400/incorretos |*InvalidProperty* |Uma propriedade está em falta ou tem um valor inválido. A mensagem com o código de erro no corpo da resposta identifica a propriedade em falta. |
| Pedido de 400/incorretos |*RequestEndTimeIsInFuture* |O valor para *ReportedEndTime* é no futuro. No futuro não são permitidos valores para este argumento. |
| Pedido de 400/incorretos |*SubscriberIdIsNotDirectTenant* |Uma chamada à API do fornecedor tem de utilizar um ID de subscrição não é um inquilino válidos do chamador. |
| Pedido de 400/incorretos |*SubscriptionIdMissingInRequest* |O ID de subscrição do chamador está em falta. |
| Pedido de 400/incorretos |*InvalidAggregationGranularity* |Foi pedida uma granularidade de agregação inválido. Os valores válidos são diária e por hora. |
| 503 |*ServiceUnavailable* |Ocorreu um erro repetível porque o serviço está ocupado ou está a ser limitada a chamada. |

## <a name="next-steps"></a>Próximos Passos
[Cliente de faturação e encargos na pilha do Azure](azure-stack-billing-and-chargeback.md)

[API de utilização de recursos do fornecedor](azure-stack-provider-resource-api.md)

[API de utilização de recursos de inquilino](azure-stack-tenant-resource-usage-api.md)
