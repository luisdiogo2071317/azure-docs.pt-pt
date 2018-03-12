 ---
Título: Perguntas mais frequentes relacionadas com a API de utilização | Descrição da Microsoft Docs: medidores de lista de pilha do Azure, comparar com a API de utilização do Azure, tempo de utilização e comunicadas hora, códigos de erro.
serviços: documentationcenter de pilha do azure: ' autor: Gestor mattbriggs: femila editor: '

ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071 ms.service: azure-stack ms.workload: na ms.tgt_pltfrm: na ms.devlang: na ms.topic: article ms.date: 03/09/2018 ms.author: mabrigg ms.reviewer: alfredop

---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Perguntas mais frequentes sobre a utilização de pilha de Azure API do
Este artigo responde a algumas perguntas mais frequentes sobre a API de utilização de pilha do Azure.

## <a name="what-meter-ids-can-i-see"></a>O IDs de medidor posso ver?
Utilização é comunicada para os seguintes fornecedores de recursos:

| **Fornecedor de recursos** | **ID de medidor** | **Nome do medidor** | **unidade** | **Informações adicionais** |
| --- | --- | --- | --- | --- |
| **Rede** |F271A8A388C44D93956A063E1D2FA80B |Utilização de endereços IP estáticos |Endereços IP| Contagem de endereços IP utilizados. Se chamar a API de utilização com uma granularidade diária, o medidor devolve o endereço IP, multiplicado pelo número de horas. |
| |9E2739BA86744796B465F64674B822BA |Utilização de endereços IP dinâmico |Endereços IP| Contagem de endereços IP utilizados. Se chamar a API de utilização com uma granularidade diária, o medidor devolve o endereço IP, multiplicado pelo número de horas. |
| **Armazenamento** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*horas |Capacidade total consumida pelos tabelas. |
| |B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*horas |Capacidade total consumida pelos blobs de páginas. |
| |B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*horas |Capacidade total consumida na fila. |
| |09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*horas |Capacidade total consumida pelos blobs de blocos. |
| |B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |Contagem de pedido no 10, 000's |Pedidos de serviço tabela (em 10 000s). |
| |50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Dados de entrada em GB |Entrada de dados do serviço tabela em GB. |
| |1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Saída em GB |Saída de dados do serviço tabela em GB |
| |43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |Contagem de pedidos nas 10, 000's |Pedidos de serviço blob (em 10 000s). |
| |9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Dados de entrada em GB |Entrada de dados do serviço blob em GB. |
| |3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Saída em GB |Saída de dados do serviço blob em GB. |
| |EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |Contagem de pedidos nas 10, 000's |Pedidos de serviço de fila (em 10 000s). |
| |E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Dados de entrada em GB |Entrada de dados do serviço fila em GB. |
| |DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Saída em GB |Saída de dados do serviço fila em GB |
| **RP do SQL Server**            | CBCFEF9A-B91F-4597-A4D3-01FE334BED82 | DatabaseSizeHourSqlMeter   | MB\*horas   | Capacidade total de base de dados durante a criação. Se chamar a API de utilização com uma granularidade diária, o medidor devolve MB multiplicada pelo número de horas. |
| **MySql RP**          | E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3 | DatabaseSizeHourMySqlMeter | MB\*horas    | Capacidade total de base de dados durante a criação. Se chamar a API de utilização com uma granularidade diária, o medidor devolve MB multiplicada pelo número de horas. |
| **Computação** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Horas de tamanho base VM |Horas de núcleo virtual | Número de núcleos virtuais multiplicado pelo horas que a VM foi executada. |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Horas de tamanho VM do Windows |Horas de núcleo virtual | Número de núcleos virtuais multiplicado pelo horas que a VM foi executada. |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |Horas de tamanho VM |Horas VM |Captura VM Base e Windows. Não ajuste para núcleos. |
| **Cofre de Chaves** |EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Transações de Cofre de chaves | Contagem de pedido no 10, 000's| Número de pedidos de REST API recebidos pelo plane de dados do Cofre de chaves. |
| |2C354225-B2FE-42E5-AD89-14F0EA302C87 |Transações de chaves avançado | 10 mil transações|     Transações da chave RSA 3K/4K, ECC. (pré-visualização). |
| **Serviço de aplicações** | 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA | Serviço de Aplicações | Horas de núcleo virtual | Número de núcleos virtuais utilizada para executar o serviço de aplicações. Nota: A Microsoft utiliza este medidor para cobram o serviço de aplicações na pilha do Azure. Fornecedores de serviços em nuvem pode utilizar as outras App Service medidores (abaixo) para calcular utilização para os seus inquilinos. |
|  | 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE | Pedidos de funções | 10 pedidos | Número total de execuções de pedido (por 10 execuções). Execuções são contadas sempre que uma função é executada em resposta a um evento ou é acionada por um enlace. |
|  | D1D04836-075C-4F27-BF65-0A1130EC60ED | Funções - computação | GB-s | Consumo de recursos medido em segundos de gigabyte (GB/s). **Consumo de recursos de observados** é calculado multiplicando o tamanho de memória médio em GB do tempo em milissegundos, que demora a executar a função. Memória utilizada por uma função é medida por arredondamento até o MB mais próximo de 128, até ao tamanho máximo de memória de 1,536 MB, com o tempo de execução calculado por arredondamento até o mais próximo 1 ms. O tempo de execução mínimo e a memória para uma execução única função é 100 ms e 128 mb, respetivamente. |
|  | 957E9F36-2C14-45A1-B6A1-1723EF71A01D | Horas do serviço de aplicações partilhado | 1 hora | Por utilização da hora de partições horizontais plano do App Service. Planos são limitados numa base por aplicação. |
|  | 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9 | Horas do serviço de aplicações gratuito | 1 hora | Por utilização de hora do plano do livre App Service. Planos são limitados numa base por aplicação. |
|  | 88039D51-A206-3A89-E9DE-C5117E2D10A6 | Horas breves do serviço de aplicações Standard | 1 hora | Calculado com base no tamanho e número de instâncias. |
|  | 83A2A13E-4788-78DD-5D55-2831B68ED825 | Horas medianas do serviço de aplicações Standard | 1 hora | Calculado com base no tamanho e número de instâncias. |
|  | 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6 | Horas prolongadas do serviço de aplicações Standard | 1 hora | Calculado com base no tamanho e número de instâncias. |
|  | *Camadas de trabalho personalizado* | Camadas de trabalho personalizado | Horas | ID de medidor determinista é criado com base no SKU e o nome de camada de trabalho personalizado. Este ID de medidor é exclusivo para cada camada de trabalho personalizado. |
|  | 264ACB47-AD38-47F8-ADD3-47F01DC4F473 | SSL SNI | Por enlace SNI SSL | Serviço de aplicações suportam dois tipos de ligações de SSL: ligações de SSL indicação de nome de servidor (SNI) e ligações de SSL de endereços IP. O SSL baseado em SNI funciona em browsers modernos, enquanto que o SSL baseado em IP funciona em todos os browsers. |
|  | 60B42D72-DC1C-472C-9895-6C516277EDB4 | SSL IP | Por IP com base em enlace SSL | Serviço de aplicações suportam dois tipos de ligações de SSL: ligações de SSL indicação de nome de servidor (SNI) e ligações de SSL de endereços IP. O SSL baseado em SNI funciona em browsers modernos, enquanto que o SSL baseado em IP funciona em todos os browsers. |
|  | 73215A6C-FA54-4284-B9C1-7E8EC871CC5B | Processo Web |  | Calculado por site do Active Directory por hora. |
|  | 5887D39B-0253-4E12-83C7-03E1A93DFFD9 | Largura de banda de saída externo | GB | Total bytes de resposta de pedidos recebidos + total pedidos a enviar bytes + total FTP recebido pedidos bytes da resposta + total web recebido implementar bytes de resposta do pedido. |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Como fazer a utilização de pilha do Azure APIs comparam com a [utilização do Azure API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (atualmente em pré-visualização pública)?
* A API de utilização do inquilino é consistente com a API do Azure, com uma exceção: o *showDetails* sinalizador atualmente não é suportado na pilha do Azure.
* A API de utilização de fornecedor só se aplica a pilha do Azure.
* Atualmente, o [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) que está disponível no Azure não está disponível na pilha do Azure.

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
