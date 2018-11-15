---
title: Ingestão de dados do Azure Data Explorer
description: Saiba mais sobre as diferentes formas, pode ingerir (carregar) de dados no Explorador de dados do Azure
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6c7d4d8d4a16e0679722f9de007870a7ec7554b0
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636004"
---
# <a name="azure-data-explorer-data-ingestion"></a>Ingestão de dados do Azure Data Explorer

Ingestão de dados é o processo usado para carregar os registos de dados a partir de uma ou mais origens para criar ou atualizar uma tabela no Explorador de dados do Azure. Uma vez ingeridos, os dados ficam disponíveis para consulta. O diagrama abaixo mostra o fluxo de ponto-a-ponto para trabalhar no Explorador de dados do Azure, incluindo a ingestão de dados.

![Fluxo de dados](media/ingest-data-overview/data-flow.png)

O serviço de gestão de dados do Explorador de dados do Azure, que é responsável por ingestão de dados, fornece as seguintes funcionalidades:

1. **Extração de dados**: extrair dados de fontes externas (os Hubs de eventos) ou pedidos de ingestão de leitura da fila do Azure.

1. **Criação de batches**: fluxo de dados para a mesma base de dados e tabela para otimizar o débito de ingestão do Batch.

1. **Validação**: preliminar conversão de formato e validação se necessário.

1. **Manipulação de dados**: esquema correspondente, organizar, a indexação, a codificação e a compressão dos dados.

1. **Ponto de persistência no fluxo de ingestão**: Gerir a carga de ingestão das repetições de motor e identificador após falhas transitórias.

1. **Consolidar os dados de ingestão**: torna os dados disponíveis para consulta.

## <a name="ingestion-methods"></a>Métodos de ingestão

O Explorador de dados do Azure suporta vários métodos de ingestão, cada um com seus próprios cenários de destino, as vantagens e desvantagens. O Explorador de dados do Azure oferece os pipelines e dos conectores para serviços comuns, ingestão programática utilizar SDKs e acesso direto para o mecanismo para fins de exploração.

### <a name="ingestion-using-pipelines"></a>Com pipelines de ingestão

O Explorador de dados do Azure suporta, atualmente, o pipeline de Hub de eventos, que pode ser gerido com o Assistente de gestão no portal do Azure. Para obter mais informações, consulte [início rápido: ingerir dados de Hub de eventos no Explorador de dados do Azure](ingest-data-event-hub.md).

### <a name="ingestion-using-connectors-and-plugins"></a>Utilizar conectores e plug-ins de ingestão
O Explorador de dados do Azure suporta atualmente o plug-in do Logstash. Para obter mais informações, consulte [Plug-in do Logstash saída para o Explorador de dados do Azure](https://github.com/Azure/logstash-output-kusto/blob/master/README.md).

### <a name="programmatic-ingestion"></a>Ingestão programática

O Explorador de dados do Azure disponibiliza SDKs que podem ser utilizadas para ingestão de dados e de consultas. Ingestão programático é otimizada para reduzir os custos de ingestão (COGs), minimizando as transações de armazenamento durante e após o processo de ingestão.

**SDKs disponíveis e projetos de código aberto**:

Kusto oferece o cliente SDK que pode ser utilizado para ingerir e consultar dados com:

* [Python SDK](/azure/kusto/api/python/kusto-python-client-library)

* [SDK do .NET](/azure/kusto/api/netfx/about-the-sdk)

* [SDK Java](/azure/kusto/api/java/kusto-java-client-library)

* [Nó de SDK](/azure/kusto/api/node/kusto-node-client-library)

* [API REST](/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Técnicas de ingestão programática**:

* Ingestão de dados através do serviço de gestão de dados do Azure Data Explorer (ingestão de alto débito e fiável):

  * [**Ingestão de lote** ](/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (fornecido pelo SDK): o cliente carrega os dados para o armazenamento de Blobs do Azure (designado pelo serviço de gestão de dados do Explorador de dados do Azure) e envia uma notificação para uma fila do Azure. Essa é a técnica recomendada para ingestão de dados de grande volume, fiáveis e barato.

* Ingestão de dados diretamente para o mecanismo de Explorador de dados do Azure (mais adequado para a exploração e a criação de protótipos):

  * **Ingestão de inline**: controlo de comando (.ingest inline) que contém os dados em banda destina-se a fins de teste ad hoc.

  * **Ingestão de consulta**: comandos de controle (.set, .set ou acrescentar, .set ou substituir) que aponta para os resultados da consulta é utilizado para a geração de relatórios ou pequenas tabelas temporárias.

  * **Ingestão de armazenamento**: o comando de controlo (.ingest em) com os dados armazenados externamente (por exemplo, armazenamento de Blobs do Azure) permite a ingestão em massa eficiente de dados.

**Latência de métodos diferentes**:

| Método | Latência |
| --- | --- |
| **Ingestão de inline** | Imediato |
| **Ingestão de consulta** | Hora de consulta + tempo de processamento |
| **Ingestão de armazenamento** | Tempo de transferência + tempo de processamento |
| **Ingestão em fila** | Hora de criação de batches + tempo de processamento |
| |

Tempo de processamento depende do tamanho de dados, normalmente, menos do que alguns segundos. Criação de batches tempo a predefinição é 5 minutos.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>Escolher o método mais apropriado de ingestão

Antes de começar a ingestão de dados, deve se fazer as perguntas seguintes.

* Onde residem os meus dados? 
* O que é o formato de dados e pode ser alterado? 
* Quais são os campos necessários para ser consultado? 
* O que é o volume de dados esperado e a velocidade? 
* Quantos tipos de eventos são esperados (refletida como o número de tabelas)? 
* A frequência com que o esquema de eventos é esperado para alterar? 
* Quantos nós irão gerar os dados? 
* O que é a origem do sistema operacional? 
* Quais são os requisitos de latência? 
* Um dos pipelines de ingestão gerido existente sirva? 

Para organizações com uma infraestrutura existente, com base num serviço de mensagens, como o Hub de eventos, através de um conector é provavelmente a solução mais apropriada. Ingestão em fila é adequada para grandes volumes de dados.

## <a name="supported-data-formats"></a>Formatos de dados suportados

Para a ingestão de todos os métodos diferentes de ingestão de consulta, os dados têm de ser formatados em um dos formatos de dados suportados, de modo a que o Explorador de dados do Azure pode analisá-lo.

* CSV, TSV, PSV, SCSV, SOH
* Avro (separados por linha, com várias linha), de JSON
* ZIP e GZIP 

> [!NOTE]
> Quando está a ser ingeridos dados, os tipos de dados são inferidos com base nas colunas da tabela de destino. Se um registo está incompleto ou um campo não pode ser analisado como o tipo de dados necessários, as colunas da tabela correspondente serão preenchidas com valores nulos.

## <a name="ingestion-recommendations-and-limitations"></a>Recomendações de ingestão e limitações
* A política de retenção em vigor a partir de dados ingeridos é derivada de política de retenção da base de dados. Ver [política de retenção](/azure/kusto/concepts/retentionpolicy) para obter detalhes. Ingestão de dados requer **ingestor de tabela** ou **ingestor de base de dados** permissões.
* Ingestão suporta um tamanho de ficheiro máximo de 5GB. Recomenda-se para a ingestão de ficheiros entre 100MB e 1GB.

## <a name="schema-mapping"></a>Mapeamento de esquema

Mapeamento de esquema ajuda determinística vincular os campos de dados de origem a colunas da tabela de destino.

* [Mapeamento de CSV](/azure/kusto/management/mappings?branch=master#csv-mapping) (opcional) funciona com todos os ordinal com base em formatos e pode ser passado como o parâmetro de comando de ingestão ou [previamente criado na tabela](/azure/kusto/management/tables?branch=master#create-ingestion-mapping) e referenciado do parâmetro de comando de ingestão.
* [Mapeamento de JSON](/azure/kusto/management/mappings?branch=master#json-mapping) (obrigatório) e [Avro mapeamento](/azure/kusto/management/mappings?branch=master#avro-mapping) (obrigatório) pode ser passado como o parâmetro de comando de ingestão ou [previamente criado na tabela](/azure/kusto/management/tables#create-ingestion-mapping) e referenciado do parâmetro de comando de ingestão.

## <a name="next-steps"></a>Passos Seguintes

[Início Rápido: ingerir dados do Hub de Eventos para o Azure Data Explorer](ingest-data-event-hub.md)

[Início Rápido: ingerir dados através da biblioteca Python do Azure Data Explorer](python-ingest-data.md)

[Início rápido: Ingerir dados com a biblioteca de nó de Explorador de dados do Azure](node-ingest-data.md)

