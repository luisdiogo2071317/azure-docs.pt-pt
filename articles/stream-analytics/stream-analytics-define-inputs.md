---
title: Dados de fluxo como entrada para o Azure Stream Analytics
description: Saiba mais sobre como configurar uma ligação de dados no Azure Stream Analytics. Entradas incluem um fluxo de dados de eventos e também referenciam a dados.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 1fc1791d75355cc30f2ef43fc17e39a868e2c756
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Dados de fluxo como entrada para o Stream Analytics

Do Stream Analytics tem integração de primeira classe com fluxos de dados do Azure como entradas de três tipos de recursos:
- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Hub IoT do Azure](https://azure.microsoft.com/services/iot-hub/) 
- [Armazenamento de Blobs do Azure](https://azure.microsoft.com/services/storage/blobs/) 

Estes recursos entrados podem em direto na mesma subscrição do Azure como a tarefa de Stream Analytics, ou a partir de uma subscrição diferente.

### <a name="compression"></a>Compressão
Stream Analytics suporta a compressão em todos os fluxo entrada as origens de dados. Os tipos de referência atualmente suportados são: None, GZip e Deflate compressão. Suporte de compressão não está disponível para dados de referência. Se o formato de entrada for dados Avro são comprimidos,-lo é processada de forma transparente. Não é necessário especificar o tipo de compressão com serialização Avro. 

## <a name="create-or-edit-inputs"></a>Criar ou editar entradas
Para criar novas entradas e lista ou editar entradas existentes na sua tarefa de transmissão em fluxo, pode utilizar o portal do Azure:
1. Abra o [portal do Azure](https://portal.azure.com) para localizar e selecione a tarefa de Stream Analytics.
2. Selecione o **entradas** opção sob o **definições** cabeçalho. 
4. O **entradas** página lista quaisquer entradas existentes. 
5. No **entradas** página, selecione **adicionar a entrada de fluxo** ou **Adicionar referência de entrada** para abrir a página de detalhes.
6. Selecione uma entrada existente para editar os detalhes e selecione **guardar** para editar uma entrada existente.
7. Selecione **teste** na página de detalhes de entrada para verificar se as opções de ligação são válidos e a funcionar. 
8. Clique com o botão direito no nome de uma entrada existente e selecione **dados de entrada de exemplo** conforme necessário para fins de teste ainda mais.


## <a name="stream-data-from-event-hubs"></a>Dados de fluxo dos Event Hubs

Os Hubs de eventos do Azure fornece altamente dimensionável ingestors de eventos de publicação-subscrição. Um hub de eventos pode recolher milhões de eventos por segundo, para que possa processar e analisar as quantidades enormes de dados produzidos pelos seus dispositivos e aplicações ligados. Stream Analytics e Hubs de eventos em conjunto lhe fornecem uma solução ponto a ponto para a análise em tempo real. Os Event Hubs permitem-lhe feed eventos no Azure em tempo real e tarefas do Stream Analytics podem processar esses eventos em tempo real. Por exemplo, pode enviar cliques web, as leituras dos sensores ou eventos de registo online para os Event Hubs. Em seguida, pode criar tarefas do Stream Analytics para utilizar os Hubs de eventos como os fluxos de dados de entrada para em tempo real de filtragem, Agregar e correlação.

O carimbo de predefinição de eventos provenientes dos Event Hubs no Stream Analytics é timestamp que o evento de eventos chegou hub, o que é `EventEnqueuedUtcTime`. Para processar os dados como uma transmissão em fluxo utilizando um timestamp nos eventos payload, tem de utilizar o [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) palavra-chave.

### <a name="consumer-groups"></a>Grupos de consumidores
Deve configurar cada hub de eventos do Stream Analytics de entrada para que o seu próprio grupo de consumidores. Quando uma tarefa contém uma associação automática ou quando tem várias entradas, alguns entrada pode ser lidos por mais do que um leitor downstream. Esta situação tem impacto sobre o número de leitores num grupo de consumidores único. Para evitar exceder o limite de Hubs de eventos de leitores de cinco por grupo de consumidores por partição, é uma melhor prática para designar um grupo de consumidores para cada tarefa de Stream Analytics. Também é um limite de 20 grupos de consumidores do hub de eventos. Para obter mais informações, consulte [guia de programação de Hubs de eventos](../event-hubs/event-hubs-programming-guide.md).

### <a name="stream-data-from-event-hubs"></a>Dados de fluxo dos Event Hubs
A tabela seguinte explica cada propriedade no **nova entrada** página no portal do Azure para a entrada de fluxo de dados de um Hub de eventos:

| Propriedade | Descrição |
| --- | --- |
| **Alias de entrada** |Um nome amigável que utilizar na consulta da tarefa para fazer referência a esta entrada. |
| **Subscrição** | Selecione a subscrição na qual o recurso de hub de eventos existe. | 
| **Espaço de nomes de Hub de eventos** | O espaço de nomes do Hub de eventos é um contentor para um conjunto de entidades de mensagens. Quando cria um novo hub de eventos, também criar o espaço de nomes. |
| **Nome do Hub de eventos** | O nome do hub de eventos para utilizar como entrada. |
| **Nome de política do Hub de eventos** | A política de acesso partilhado fornece acesso ao Hub de eventos. Cada política de acesso partilhado tem um nome, as permissões que define e chaves de acesso. Esta opção é preenchida automaticamente no, a menos que selecione a opção de fornecer as definições de Hub de eventos manualmente.|
| **Grupo de consumidores do Hub de eventos** (recomendado) | Recomenda-se vivamente a utilizar um grupo de consumidores distinta para cada tarefa de Stream Analytics. Esta cadeia identifica o grupo de consumidores para utilizar para a ingestão de dados do hub de eventos. Não se for especificado nenhum grupo de consumidores, a tarefa de Stream Analytics utiliza o grupo de consumidores $Default.  |
| **Formato de serialização de eventos** | O formato de serialização (JSON, CSV ou Avro) do fluxo de dados de entrada.  Certifique-se de que o formato JSON está alinhada com a especificação e não inclui 0 à esquerda para números decimais. |
| **Codificação** | UTF-8 atualmente é o único formato de codificação suportado. |
| **Tipo de compressão de evento** | O tipo de compressão utilizado para ler o fluxo de dados de entrada, tais como None (predefinição), GZip, ou Deflate. |

Quando os dados é proveniente de uma entrada de fluxo de Hub de eventos, terá acesso para os seguintes campos de metadados na sua consulta do Stream Analytics:

| Propriedade | Descrição |
| --- | --- |
| **EventProcessedUtcTime** |A data e hora em que o evento foi processado pelo Stream Analytics. |
| **EventEnqueuedUtcTime** |A data e hora em que o evento foi recebido pelo Event Hubs. |
| **PartitionId** |O ID de partição baseado em zero para o adaptador de entrada. |

Por exemplo, utilizando estes campos, pode escrever uma consulta semelhante ao seguinte exemplo:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> Quando utilizar o Hub de eventos como um ponto final para rotas de Hub IoT, pode aceder à utilização de medadata IoT Hub a [GetMetadataPropertyValue função](https://msdn.microsoft.com/library/azure/mt793845.aspx).
> 

## <a name="stream-data-from-iot-hub"></a>Dados de fluxo do IoT Hub
Iot Hub do Azure é altamente dimensionável de publicação-subscrição ingestor de eventos otimizado para cenários de IoT.

O carimbo de predefinição de eventos proveniente de um IoT Hub no Stream Analytics é timestamp que o evento chegou no IoT Hub, que é `EventEnqueuedUtcTime`. Para processar os dados como uma transmissão em fluxo utilizando um timestamp nos eventos payload, tem de utilizar o [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) palavra-chave.

> [!NOTE]
> Apenas as mensagens enviadas com um `DeviceClient` propriedade pode ser processada.
> 

### <a name="consumer-groups"></a>Grupos de consumidores
Deve configurar cada Stream Analytics IoT Hub para que o seu próprio grupo de consumidores de entrada. Quando uma tarefa contém uma associação automática ou quando tem várias entradas, alguns entrada pode ser lidos por mais do que um leitor downstream. Esta situação tem impacto sobre o número de leitores num grupo de consumidores único. Para evitar exceder o limite de IoT Hub do Azure de leitores de cinco por grupo de consumidores por partição, é uma melhor prática para designar um grupo de consumidores para cada tarefa de Stream Analytics.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Configurar um IoT Hub como um fluxo de dados de entrada
A tabela seguinte explica cada propriedade no **nova entrada** página no portal do Azure quando configurar um IoT Hub como um fluxo de entrada.

| Propriedade | Descrição |
| --- | --- |
| **Alias de entrada** | Um nome amigável que utilizar na consulta da tarefa para fazer referência a esta entrada.|
| **Subscrição** | Selecione a subscrição na qual o recurso do IoT Hub existe. | 
| **Hub IoT** | O nome do IoT Hub para utilizar como entrada. |
| **Endpoint** | O ponto final para o IoT Hub.|
| **Nome da política de acesso partilhado** | A política de acesso partilhado fornece acesso ao IoT Hub. Cada política de acesso partilhado tem um nome, as permissões que define e chaves de acesso. |
| **Chave de política de acesso partilhado** | A chave de acesso partilhado utilizada para autorizar o acesso ao IoT Hub.  Esta opção é preenchida automaticamente no, exceto se selecionar a opção para fornecer definições de Iot Hub manualmente. |
| **Grupo de consumidores** | É vivamente recomendado que utilize um grupo de consumidores diferente para cada tarefa de Stream Analytics. O grupo de consumidores é utilizado para a ingestão de dados do IoT Hub. Do Stream Analytics utiliza o grupo de consumidores $Default, a menos que especifique o contrário.  |
| **Formato de serialização de eventos** | O formato de serialização (JSON, CSV ou Avro) do fluxo de dados de entrada.  Certifique-se de que o formato JSON está alinhada com a especificação e não inclui 0 à esquerda para números decimais. |
| **Codificação** | UTF-8 atualmente é o único formato de codificação suportado. |
| **Tipo de compressão de evento** | O tipo de compressão utilizado para ler o fluxo de dados de entrada, tais como None (predefinição), GZip, ou Deflate. |


Quando utilizar dados de fluxo a partir de um IoT Hub, tem acesso para os seguintes campos de metadados na sua consulta do Stream Analytics:

| Propriedade | Descrição |
| --- | --- |
| **EventProcessedUtcTime** | A data e hora em que o evento foi processado. |
| **EventEnqueuedUtcTime** | A data e hora em que o evento foi recebido pelo IoT Hub. |
| **PartitionId** | O ID de partição baseado em zero para o adaptador de entrada. |
| **IoTHub.MessageId** | Um ID que é utilizado para correlacionar comunicação bidirecional no IoT Hub. |
| **IoTHub.CorrelationId** | Um ID que é utilizado em respostas de mensagem e comentários no IoT Hub. |
| **IoTHub.ConnectionDeviceId** | O ID de autenticação utilizado para enviar esta mensagem. Este valor é carimbo de data / nas mensagens de servicebound pelo IoT Hub. |
| **IoTHub.ConnectionDeviceGenerationId** | O ID de geração do dispositivo autenticado que foi utilizado para enviar esta mensagem. Este valor é carimbo de data / nas mensagens de servicebound pelo IoT Hub. |
| **IoTHub.EnqueuedTime** | A hora quando a mensagem foi recebida pelo IoT Hub. |
| **IoTHub.StreamId** | Uma propriedade de evento personalizado adicionada pelo dispositivo remetente. |


## <a name="stream-data-from-blob-storage"></a>Dados de fluxo do armazenamento de BLOBs
Para cenários com grandes quantidades de dados não estruturados para armazenar na nuvem, o Blob storage do Azure oferece uma solução económica e dimensionável. Dados no Blob storage, normalmente, são considerados dados inativos. No entanto, podem ser processados dados blob como um fluxo de dados pelo Stream Analytics. 

O processamento do registo é um cenário frequentemente utilizado para a utilização de entradas de armazenamento de Blobs com o Stream Analytics. Neste cenário, os ficheiros de dados de telemetria captou de um sistema e tem de ser analisados e processados para extrair dados significativos.

O carimbo de predefinição de eventos de armazenamento de BLOBs no Stream Analytics é timestamp que o blob foi modificado pela última vez, o que é `BlobLastModifiedUtcTime`. Para processar os dados como uma transmissão em fluxo utilizando um timestamp nos eventos payload, tem de utilizar o [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) palavra-chave.

Entradas de CSV formatado *requerem* uma linha de cabeçalho para definir os campos para o conjunto de dados e todos os campos de linha de cabeçalho tem de ser exclusiva.

Do Stream Analytics não suporta atualmente a anulação da serialização AVRO mensagens geradas pela captura de Hub de eventos ou endpoint personalizado do contentor de armazenamento do IoT Hub do Azure.

> [!NOTE]
> Do Stream Analytics não suporta a adição de conteúdo para um ficheiro de blob existente. Do Stream Analytics irá ver uma só vez cada ficheiro e as alterações que ocorrem no ficheiro depois da tarefa tem de ler os dados não são processadas. É melhor prática carregar em simultâneo todos os dados para um ficheiro de blob e, em seguida, adicionar eventos mais recentes adicionais para um ficheiro de blob diferente, os novos.
> 

### <a name="configure-blob-storage-as-a-stream-input"></a>Configurar o armazenamento de Blob como uma transmissão em fluxo de entrada 

A tabela seguinte explica cada propriedade no **nova entrada** página no portal do Azure quando configurar o armazenamento de Blob como uma transmissão em fluxo de entrada.

| Propriedade | Descrição |
| --- | --- |
| **Alias de entrada** | Um nome amigável que utilizar na consulta da tarefa para fazer referência a esta entrada. |
| **Subscrição** | Selecione a subscrição na qual o recurso do IoT Hub existe. | 
| **Conta de armazenamento** | O nome da conta do storage onde estão localizados os ficheiros de blob. |
| **Chave de conta de armazenamento** | A chave secreta associada à conta de armazenamento. Esta opção é preenchida automaticamente no, a menos que selecione a opção de fornecer as definições de armazenamento de BLOBs manualmente. |
| **contentor** | O contentor do blob de entrada. Contentores fornecem um agrupamento lógico blobs armazenados no Microsoft serviço Blob do Azure. Quando carregar um blob para o serviço de armazenamento de Blobs do Azure, tem de especificar um contentor para esse blob. Pode escolher uma **utilização existente** contentor ou **criar nova** ter um novo contentor criado.|
| **Padrão de caminho** (opcional) | O caminho do ficheiro utilizado para localizar os blobs no contentor especificado. No caminho, pode especificar uma ou mais instâncias das três variáveis seguintes: `{date}`, `{time}`, ou `{partition}`<br/><br/>Exemplo 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Exemplo 2: `cluster1/logs/{date}`<br/><br/>O `*` carácter não é um valor permitido para o prefixo do caminho. Só é válido <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">carateres de Blobs do Azure</a> são permitidos. |
| **Formato de data** (opcional) | Se utilizar a variável de data no caminho, o formato da data em que os ficheiros estão organizados. Exemplo: `YYYY/MM/DD` |
| **Formato de hora** (opcional) |  Se utilizar a variável de tempo no caminho, o formato de hora em que os ficheiros estão organizados. Atualmente é o único valor suportado `HH` durante horas. |
| **Formato de serialização de eventos** | O formato de serialização (JSON, CSV ou Avro) do fluxo de dados de entrada.  Certifique-se de que o formato JSON está alinhada com a especificação e não inclui 0 à esquerda para números decimais. |
| **Codificação** | Para CSV e JSON, UTF-8 atualmente é o único formato de codificação suportado. |
| **Compressão** | O tipo de compressão utilizado para ler o fluxo de dados de entrada, tais como None (predefinição), GZip, ou Deflate. |

Quando os dados provém de uma origem de armazenamento de BLOBs, terá acesso para os seguintes campos de metadados na sua consulta do Stream Analytics:

| Propriedade | Descrição |
| --- | --- |
| **BlobName** |O nome do blob de entrada que provém do evento. |
| **EventProcessedUtcTime** |A data e hora em que o evento foi processado pelo Stream Analytics. |
| **BlobLastModifiedUtcTime** |A data e hora em que o blob foi modificado pela última vez. |
| **PartitionId** |O ID de partição baseado em zero para o adaptador de entrada. |

Por exemplo, utilizando estes campos, pode escrever uma consulta semelhante ao seguinte exemplo:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Início rápido: Criar uma tarefa de Stream Analytics, utilizando o portal do Azure](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
