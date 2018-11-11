---
title: FAQs relacionados com a API de utilização | Documentos da Microsoft
description: Lista de medidores do Azure Stack, comparação com a API de utilização do Azure, o tempo de utilização e o tempo comunicado, códigos de erro.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: 95b63b525365a5a31856c2f14ac965aaa1113cff
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515629"
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Perguntas mais frequentes sobre na API de utilização do Azure Stack

Este artigo responde a algumas perguntas mais frequentes sobre a API de utilização do Azure Stack.

## <a name="what-meter-ids-can-i-see"></a>O IDs de medidor posso ver?
É comunicada a utilização para os seguintes fornecedores de recursos:

### <a name="network"></a>Rede
  
**ID do medidor**: F271A8A388C44D93956A063E1D2FA80B  
**Nome do medidor**: utilização de endereços IP estáticos  
**Unidade**: endereços IP  
**Notas de**: contagem de endereços IP utilizados. Se chamar a API de utilização com uma granularidade diária, o medidor devolve o endereço IP, multiplicado pelo número de horas.  
  
**ID do medidor**: 9E2739BA86744796B465F64674B822BA  
**Nome do medidor**: utilização de endereços de IP dinâmico  
**Unidade**: endereços IP  
**Notas de**: contagem de endereços IP utilizados. Se chamar a API de utilização com uma granularidade diária, o medidor devolve o endereço IP, multiplicado pelo número de horas.  
  
### <a name="storage"></a>Armazenamento
  
**ID do medidor**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Nome do medidor**: TableCapacity  
**Unidade**: GB\*horas  
**Notas de**: capacidade consumida pelas tabelas Total.  
  
**ID do medidor**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Nome do medidor**: PageBlobCapacity  
**Unidade**: GB\*horas  
**Notas de**: consumida por blobs de páginas de capacidade Total.  
  
**ID do medidor**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Nome do medidor**: QueueCapacity  
**Unidade**: GB\*horas  
**Notas de**: capacidade consumida por fila de Total.  
  
**ID do medidor**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Nome do medidor**: BlockBlobCapacity  
**Unidade**: GB\*horas  
**Notas de**: consumida por blobs de blocos de capacidade Total.  
  
**ID do medidor**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Nome do medidor**: TableTransactions  
**Unidade**: contagem em 10, de pedidos 000's  
**Notas de**: tabela pedidos de serviço (em 10 000s).  
  
**ID do medidor**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Nome do medidor**: TableDataTransIn  
**Unidade**: dados de entrada em GB  
**Notas de**: tabela de entrada de dados do serviço em GB.  
  
**ID do medidor**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Nome do medidor**: TableDataTransOut  
**Unidade**: saída em GB  
**Notas de**: tabela de saída de dados do serviço em GB  
  
**ID do medidor**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Nome do medidor**: BlobTransactions  
**Unidade**: contagem de pedidos nas 10, 000's  
**Notas de**: pedidos de serviço de BLOBs (em 10 000s).  
  
**ID do medidor**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Nome do medidor**: BlobDataTransIn  
**Unidade**: dados de entrada em GB  
**Notas de**: BLOBs de entrada de dados do serviço em GB.  
  
**ID do medidor**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Nome do medidor**: BlobDataTransOut  
**Unidade**: saída em GB  
**Notas de**: Blob de saída de dados do serviço em GB.  
  
**ID do medidor**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Nome do medidor**: QueueTransactions  
**Unidade**: contagem de pedidos nas 10, 000's  
**Notas de**: pedidos de serviço de fila (em 10 000s).  
  
**ID do medidor**: E518E809-E369-4A45-9274-2017B29FFF25  
**Nome do medidor**: QueueDataTransIn  
**Unidade**: dados de entrada em GB  
**Notas de**: fila de entrada de dados do serviço em GB.  
  
**ID do medidor**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Nome do medidor**: QueueDataTransOut  
**Unidade**: saída em GB  
**Notas de**: fila de saída de dados do serviço em GB  

### <a name="compute"></a>Computação 
  
**ID do medidor**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Nome do medidor**: horas de tamanho de VM de Base  
**Unidade**: horas de núcleo Virtual  
**Notas de**: número de núcleos virtuais multiplicado por horas a VM foi executada.  
  
**ID do medidor**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Nome do medidor**: horas de tamanho de VM do Windows  
**Unidade**: horas de núcleo Virtual  
**Notas de**: número de núcleos virtuais multiplicado por horas a VM foi executada.  
  
**ID do medidor**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Nome do medidor**: horas de tamanho VM  
**Unidade**: horas VM  
**Notas de**: captura Base e a Windows VM. Não é ajustar de núcleos.  
  
### <a name="managed-disks"></a>Managed Disks

**ID do medidor**: 380874f9-300c-48e0-95a0-d2d9a21ade8f   
**Nome do medidor**: S4   
**Unidade**: contagem de discos\*mês   
**Notas de**: Standard gerido disco – 32 GB 

**ID do medidor**: 1b77d90f-427b-4435-b4f1-d78adec53222   
**Nome do medidor**: S6   
**Unidade**: contagem de discos\*mês   
**Notas de**: Standard gerido disco – 64 GB 

**ID do medidor**: d5f7731b-f639-404a-89d0-e46186e22c8d   
**Nome do medidor**: S10   
**Unidade**: contagem de discos\*mês   
**Notas de**: Standard gerido disco – 128 GB 

**ID do medidor**: ff85ef31-da5b-4eac-95dd-a69d6f97b18a   
**Nome do medidor**: S15   
**Unidade**: contagem de discos\*mês   
**Notas de**: Standard gerido disco – 256 GB 

**ID do medidor**: 88ea9228-457a-4091-adc9-ad5194f30b6e   
**Nome do medidor**: S20   
**Unidade**: contagem de discos\*mês      
**Notas de**: Standard gerido disco – 512 GB 

**ID do medidor**: 5b1db88a-8596-4002-8052-347947c26940   
**Nome do medidor**: S30   
**Unidade**: contagem de discos\*mês   
**Notas de**: Standard gerido disco – 1024 GB 

**ID do medidor**: 7660b45b-b29d-49cb-b816-59f30fbab011   
**Nome do medidor**: P4   
**Unidade**: contagem de discos\*mês   
**Notas de**: disco – 32 GB de geridos Premium 

**ID do medidor**: 817007fd-a077-477f-bc01-b876f27205fd   
**Nome do medidor**: P6   
**Unidade**: contagem de discos\*mês   
**Notas de**: disco – 64 GB de geridos Premium 

**ID do medidor**: e554b6bc-96cd-4938-a5b5-0da990278519   
**Nome do medidor**: P10   
**Unidade**: contagem de discos\*mês   
**Notas de**: disco – 128 GB de geridos Premium  

**ID do medidor**: cdc0f53a-62a9-4472-a06c-e99a23b02907   
**Nome do medidor**: P15  
**Unidade**: contagem de discos\*mês   
**Notas de**: disco – 256 GB de geridos Premium 

**ID do medidor**: b9cb2d1a-84c2-4275-aa8b-70d2145d59aa   
**Nome do medidor**: P20   
**Unidade**: contagem de discos\*mês   
**Notas de**: disco – 512 GB de geridos Premium 

**ID do medidor**: 06bde724-9f94-43c0-84c3-d0fc54538369   
**Nome do medidor**: P30   
**Unidade**: contagem de discos\*mês   
**Notas de**: Premium geridos disco – 1024 GB 

**ID do medidor**: 7ba084ec-ef9c-4d64-a179-7732c6cb5e28   
**Nome do medidor**: ActualStandardDiskSize   
**Unidade**: GB\*mês      
**Notas de**: O tamanho real no disco do disco gerido standard  

**ID do medidor**: daef389a-06e5-4684-a7f7-8813d9f792d5  
**Nome do medidor**: ActualPremiumDiskSize   
**Unidade**: GB\*mês      
**Notas de**: O tamanho real no disco do premium de disco gerido 

**ID do medidor**: 75d4b707-1027-4403-9986-6ec7c05579c8  
**Nome do medidor**: ActualStandardSnapshotSize   
**Unidade**: GB\*mês   
**Notas de**: O tamanho real no disco do instantâneo padrão gerido.  

**ID do medidor**: 5ca1cbb9-6f14-4e76-8be8-1ca91547965e   
**Nome do medidor**: ActualPremiumSnapshotSize   
**Unidade**: GB\*mês   
**Notas de**: O tamanho real num disco gerido premium.   

**ID do medidor**: 5d76e09f-4567-452a-94cc-7d1f097761f0   
**Nome do medidor**: S4   
**Unidade**: contagem de discos\*horas   
**Notas de**: Standard gerido disco – 32 GB (preterido) 

**ID do medidor**: dc9fc6a9-0782-432a-b8dc-978130457494   
**Nome do medidor**: S6   
**Unidade**: contagem de discos\*horas   
**Notas de**: Standard gerido disco – 64 GB (preterido) 

**ID do medidor**: e5572fce-9f58-49d7-840c-b168c0f01fff   
**Nome do medidor**: S10   
**Unidade**: contagem de discos\*horas   
**Notas de**: Standard gerido disco – 128 GB (preterido) 

**ID do medidor**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8   
**Nome do medidor**: S15   
**Unidade**: contagem de discos\*horas   
**Notas de**: Standard gerido disco – 256 GB (preterido) 

**ID do medidor**: 5938f8da-0ecd-4c48-8d5a-c7c6c23546be   
**Nome do medidor**: S20   
**Unidade**: contagem de discos\*horas      
**Notas de**: Standard gerido disco – 512 GB (preterido) 

**ID do medidor**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6   
**Nome do medidor**: S30   
**Unidade**: contagem de discos\*horas   
**Notas de**: Standard gerido disco – 1024 GB (preterido) 

**ID do medidor**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc   
**Nome do medidor**: P4   
**Unidade**: contagem de discos\*horas   
**Notas de**: Premium geridos disco – 32 GB (preterido) 

**ID do medidor**: 518b412b-1927-4f25-985f-4aea24e55c4f   
**Nome do medidor**: P6   
**Unidade**: contagem de discos\*horas   
**Notas de**: Premium geridos disco – 64 GB (preterido) 

**ID do medidor**: 5cfb1fed-0902-49e3-8217-9add946fd624   
**Nome do medidor**: P10   
**Unidade**: contagem de discos\*horas   
**Notas de**: Premium geridos disco – 128 GB (preterido)  

**ID do medidor**: 8de91c94-f740-4d9a-b665-bd5974fa08d4   
**Nome do medidor**: P15  
**Unidade**: contagem de discos\*horas   
**Notas de**: Premium geridos disco – 256 GB (preterido) 

**ID do medidor**: c7e7839c-293b-4761-ae4c-848eda91130b   
**Nome do medidor**: P20   
**Unidade**: contagem de discos\*horas   
**Notas de**: Premium geridos disco – 512 GB (preterido) 

**ID do medidor**: 9f502103-adf4-4488-b494-456c95d23a9f   
**Nome do medidor**: P30   
**Unidade**: contagem de discos\*horas   
**Notas de**: Premium geridos disco – 1024 GB (preterido) 

**ID do medidor**: 8a409390-1913-40ae-917b-08d0f16f3c38   
**Nome do medidor**: ActualStandardDiskSize   
**Unidade**: Byte\*horas      
**Notas de**: O tamanho real no disco do disco gerido standard (preterido)  

**ID do medidor**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Nome do medidor**: ActualPremiumDiskSize   
**Unidade**: Byte\*horas      
**Notas de**: O tamanho real no disco do premium geridos disco (preterido) 

**ID do medidor**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Nome do medidor**: ActualStandardSnapshotSize   
**Unidade**: Byte\*horas   
**Notas de**: O tamanho real no disco do instantâneo padrão gerido (preterido) 

**ID do medidor**: 95b0c03f-8a82-4524-8961-ccfbf575f536   
**Nome do medidor**: ActualPremiumSnapshotSize   
**Unidade**: Byte\*horas   
**Notas de**: O tamanho real num disco gerido premium (preterido) 

### <a name="sql-rp"></a>RP de SQL
  
**ID do medidor**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82  
**Nome do medidor**: DatabaseSizeHourSqlMeter  
**Unidade**: MB\*horas  
**Notas de**: capacidade de Total DB durante a criação. Se chamar a API de utilização com uma granularidade diária, o medidor devolve MB multiplicada pelo número de horas.  
  
### <a name="mysql-rp"></a>RP de MySql   
  
**ID do medidor**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Nome do medidor**: DatabaseSizeHourMySqlMeter  
**Unidade**: MB\*horas  
**Notas de**: capacidade de Total DB durante a criação. Se chamar a API de utilização com uma granularidade diária, o medidor devolve MB multiplicada pelo número de horas.    
### <a name="key-vault"></a>Cofre de Chaves   
  
**ID do medidor**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Nome do medidor**: transações do Key Vault  
**Unidade**: contagem em 10, de pedidos 000's  
**Notas de**: pedidos de número de REST API recebidos pelo plano de dados do Cofre de chaves.  
  
**ID do medidor**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Nome do medidor**: Advanced transações de chaves  
**Unidade**: 10 mil transações  
**Notas de**: RSA 3 mil/4K, transações de chaves ECC. (pré-visualização).  
  
### <a name="app-service"></a>Serviço de aplicações   
  
**ID do medidor**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Nome do medidor**: serviço de aplicações  
**Unidade**: horas de núcleo Virtual  
**Notas de**: número de núcleos virtuais utilizados para executar o serviço de aplicações. Nota: A Microsoft utiliza este medidor a cobrar o serviço de aplicações no Azure Stack. Fornecedores de serviços cloud pode utilizar o serviço de aplicações medidores (abaixo) para calcular a utilização dos inquilinos deles.  
  
**ID do medidor**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE  
**Nome do medidor**: pedidos de funções  
**Unidade**: 10 pedidos  
**Notas de**: número Total de execuções pedidas (por 10 execuções). As execuções são contabilizadas sempre que uma função é executado em resposta a um evento ou é acionada por um enlace.  
  
**ID do medidor**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Nome do medidor**: as funções - computação  
**Unidade**: GB-s  
**Notas de**: o consumo de recursos, medido em segundos de gigabytes (GB/s). **Observado o consumo de recursos** é calculado ao multiplicar o tamanho médio da memória em GB pelo tempo em milissegundos que demora a executar a função. Memória utilizada por uma função é medida ao arredondar para o MB mais próximo de 128, até ao tamanho de memória máxima de 1,536 MB, com o tempo de execução calculado ao arredondar até o mais próximo 1 ms. O mínimo de execução de tempo e memória para execução de uma única função é 100 ms e 128 mb, respetivamente.  
  
**ID do medidor**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Nome do medidor**: horas de serviço de aplicações partilhado  
**Unidade**: 1 hora  
**Notas de**: por uso de horas de partição horizontal plano do serviço de aplicações. Planos são limitados numa base por aplicação.  
  
**ID do medidor**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9  
**Nome do medidor**: horas de serviço de aplicações gratuito  
**Unidade**: 1 hora  
**Notas de**: por utilização da hora do plano do serviço de aplicações gratuito. Planos são limitados numa base por aplicação.  
  
**ID do medidor**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Nome do medidor**: pequeno horas de serviço de aplicações Standard  
**Unidade**: 1 hora  
**Notas de**: calculado com base no tamanho e número de instâncias.  
  
**ID do medidor**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Nome do medidor**: horas de serviço de aplicações médio Standard  
**Unidade**: 1 hora  
**Notas de**: calculado com base no tamanho e número de instâncias.  
  
**ID do medidor**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Nome do medidor**: grande horas do serviço de aplicações Standard  
**Unidade**: 1 hora  
**Notas de**: calculado com base no tamanho e número de instâncias.  
  
### <a name="custom-worker-tiers"></a>Escalões de Worker personalizado   
  
**ID do medidor**: *escalões de Worker personalizado*  
**Nome do medidor**: escalões de Worker personalizado  
**Unidade**: horas  
**Notas de**: ID do medidor Determinística é criado com base na SKU e o nome de camada de trabalho personalizado. Este ID de medidor é exclusivo para cada escalão de worker personalizado.  
  
**ID do medidor**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Nome do medidor**: SNI SSL  
**Unidade**: por enlace SNI SSL  
**Notas de**: serviço de aplicações suporta dois tipos de ligações SSL: ligações SSL de indicação de nome de servidor (SNI) e ligações de SSL de endereço IP. O SSL baseado em SNI funciona em browsers modernos, enquanto que o SSL baseado em IP funciona em todos os browsers.  
  
**ID do medidor**: 60B42D72-DC1C-472C-9895-6C516277EDB4  
**Nome do medidor**: IP SSL  
**Unidade**: por IP com base em enlace SSL  
**Notas de**: serviço de aplicações suporta dois tipos de ligações SSL: ligações SSL de indicação de nome de servidor (SNI) e ligações de SSL de endereço IP. O SSL baseado em SNI funciona em browsers modernos, enquanto que o SSL baseado em IP funciona em todos os browsers.  
  
**ID do medidor**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Nome do medidor**: processo Web  
**Unidade**:  
**Notas de**: calculado por site do Active Directory por hora.  
  
**ID do medidor**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9  
**Nome do medidor**: largura de banda de saída externa  
**Unidade**: GB  
**Notas de**: pedido de entrada Total de bytes de resposta + pedido de saída total de bytes + FTP de entrada total do pedido bytes de resposta + web de entrada total implementar bytes de resposta do pedido.  
  

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsdocsmicrosoftcomazurebillingbilling-usage-rate-card-overviewazure-resource-usage-api-preview-currently-in-public-preview"></a>Como fazer a utilização do Azure Stack APIs comparam com o [API de utilização do Azure](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-usage-api-preview) (atualmente em pré-visualização pública)?
* A API de utilização do inquilino é consistente com a API do Azure, com uma exceção: os *showDetails* sinalizador não é atualmente suportado no Azure Stack.
* A API de utilização do fornecedor aplica-se apenas ao Azure Stack.
* Atualmente, o [RateCard API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview#azure-resource-ratecard-api-preview) ou seja disponíveis no Azure não estão disponível no Azure Stack.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>O que é a diferença entre o tempo de utilização e o tempo comunicado?
Relatórios de dados de utilização tem dois valores de hora principal:

* **Comunicado tempo**. A hora quando o evento de utilização introduziu o sistema de utilização
* **Tempo de utilização**. A hora quando consumo do recurso do Azure Stack

Poderá ver uma discrepância em valores de tempo de utilização e a hora comunicados para um evento de utilização específicos. O atraso pode ser tão longo quanto várias horas, em qualquer ambiente.

Atualmente, pode consultar apenas pelo *tempo comunicado*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>O que significam esses códigos de erro da API de utilização?
| **Código de estado HTTP** | **Código de erro** | **Descrição** |
| --- | --- | --- |
| Pedido 400/inválido |*NoApiVersion* |O *versão de api* parâmetro de consulta está em falta. |
| Pedido 400/inválido |*InvalidProperty* |Uma propriedade está em falta ou tem um valor inválido. A mensagem no código de erro no corpo da resposta identifica a propriedade em falta. |
| Pedido 400/inválido |*RequestEndTimeIsInFuture* |O valor para *ReportedEndTime* é no futuro. No futuro, a valores não são permitidos para este argumento. |
| Pedido 400/inválido |*SubscriberIdIsNotDirectTenant* |Uma chamada de API do fornecedor usou um ID de subscrição que não é um inquilino válido do chamador. |
| Pedido 400/inválido |*SubscriptionIdMissingInRequest* |O ID de subscrição do chamador está em falta. |
| Pedido 400/inválido |*InvalidAggregationGranularity* |Foi pedida uma granularidade de agregação inválida. Valores válidos são diariamente e por hora. |
| 503 |*ServiceUnavailable* |Ocorreu um erro recuperável porque o serviço está ocupado ou a chamada está a ser limitada. |

## <a name="next-steps"></a>Próximos Passos
[Cliente de faturação e estorno no Azure Stack](azure-stack-billing-and-chargeback.md)

[API de utilização de recursos do fornecedor](azure-stack-provider-resource-api.md)

[API de utilização de recursos de inquilino](azure-stack-tenant-resource-usage-api.md)
