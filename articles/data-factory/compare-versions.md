---
title: "Compare as versões do Azure Data Factory 1 e 2 | Microsoft Docs"
description: "Este artigo compara a versão 1 e versão 2 do Azure Data Factory."
services: data-factory
documentationcenter: 
author: kromerm
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/20/2017
ms.author: makromer
ms.openlocfilehash: f55348e9974de17c6d7e704e185f1ea9b21ff66e
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2017
---
# <a name="compare-azure-data-factory-versions-1-and-2"></a>Comparar as versões do Azure Data Factory 1 e 2
Este artigo compara a versão 2 (V2) com a versão 1 (V1) do Azure Data Factory. Para uma introdução ao V1 do serviço, consulte [Data Factory versão 1 - introdução](v1/data-factory-introduction.md) e para o V2, consulte [Data Factory versão 2 - introdução](introduction.md).

## <a name="feature-comparison"></a>Comparação de funcionalidades
A tabela seguinte compara as funcionalidades do V1 e do V2. 

| Funcionalidade | V1 | V2 | 
| ------- | --------- | --------- | 
| Conjuntos de dados | Uma vista com o nome dos dados que fazem referência aos dados que pretende utilizar nas suas atividades como entradas e saídas. Os conjuntos de dados identificam dados dentro de diferentes arquivos de dados, como tabelas, ficheiros, pastas e documentos. Por exemplo, um conjunto de dados de Blobs do Azure especifica o contentor de blobs e a pasta no armazenamento de Blobs a partir dos quais a atividade deve ler os dados.<br/><br/>A **Disponibilidade** define o modelo de fragmentação da janela de processamento para o conjunto de dados (por exemplo, hora a hora, diária, etc.). | Os conjuntos de dados são os mesmos no V2. No entanto, não terá de definir agendas de **disponibilidade** para conjuntos de dados. Pode definir um recurso de acionador que agende os pipelines a partir de um paradigma de programador de relógio. Para obter mais informações, veja [Acionadores](concepts-pipeline-execution-triggers.md#triggers) e [Conjuntos de dados](concepts-datasets-linked-services.md). | 
| Serviços ligados | Os serviços ligados são muito semelhantes às cadeias de ligação, que definem as informações de ligação necessárias para que o Data Factory se possa ligar a recursos externos. | Tal como no Data Factory V1, mas com uma nova propriedade **connectVia** para utilizar o ambiente de computação do Data Factory V2 Integration Runtime. Para obter mais informações, consulte [Runtimes de integração](concepts-integration-runtime.md) e [Propriedades de serviço ligado para o Armazenamento de Blobs do Azure](connector-azure-blob-storage.md#linked-service-properties). |
| Pipelines | Uma fábrica de dados pode ter um ou mais pipelines. Os pipelines são agrupamentos lógicos de atividades que, em conjunto, realizam uma tarefa. Utilizou StartTime, endTime e isPaused para agendar e executar pipelines. | Os pipelines ainda são grupos de atividades para a ser executados nos dados. No entanto, o agendamento de atividades no pipeline foi separado em novos recursos de acionador. Pode considerar os pipelines V2 no Data Factory como "unidades de fluxo de trabalho" que agenda em separado através de acionadores. <br/><br/>Os pipelines não dispõem de "janelas" da execução de tempo no Data Factory V2. Os conceitos startTime, endTime e isPaused do Data Factory V1 já não estão presentes no Data Factory V2. Para obter mais informações, veja o artigo [Execuções de pipelines e acionadores](concepts-pipeline-execution-triggers.md) e [Pipelines e atividades](concepts-pipelines-activities.md). |
| Atividades | As atividades definem ações a realizar nos seus dados, dentro de um pipeline. O movimento de dados (atividade de cópia) e as atividades de transformação de dados (tais como o Hive, Pig e MapReduce) são suportadas. | No Data Factory V2, as atividades ainda são ações definidas dentro de um pipeline. O V2 apresenta novas [atividades de fluxo de controlo](concepts-pipelines-activities.md#control-activities). Utilize estas atividades no fluxo de controlo (ciclo e ramificação). As atividades de movimento de dados e transformação de dados que eram suportadas no V1 são suportadas no V2. Pode definir as atividades de transformação sem utilizar os conjuntos de dados no V2. |
| Movimento de dados híbridos e distribuição de atividades | Anteriormente conhecido como [Data Management Gateway](v1/data-factory-data-management-gateway.md), suportava movimentar dados entre o local e a cloud. Agora, chama-se Integration Runtime.| O Data Management Gateway agora é referido como Integration Runtime Auto-alojado. Fornece a mesma capacidade que no V1. <br/><br/> O Azure-SSIS Integration Runtime no V2 também suporta a implementação e execução de pacotes do SQL Server Integration Services (SSIS) na cloud. Para obter mais informações, veja o artigo [Integration Runtime](concepts-integration-runtime.md).|
| Parâmetros | ND | Os parâmetros são pares chave-valor das definições de configuração só de leitura que estão definidas em pipelines. Pode passar os argumentos para os parâmetros quando está a executar manualmente o pipeline. Se estiver a utilizar um acionador de agendador, o acionador também pode passar valores para os parâmetros. As atividades dentro do pipeline consomem os valores dos parâmetros.  |
| Expressões | O Data Factory V1 permite-lhe utilizar funções e variáveis de sistema em consultas de seleção de dados e propriedades de atividade/conjuntos de dados. | No Data Factory V2, pode utilizar expressões em qualquer parte de um valor de cadeia de carateres JSON. Para mais informações, consulte [Expressões e Funções no V2](control-flow-expression-language-functions.md).|
| Execuções de pipeline | ND | Uma única instância de uma execução de pipeline. Por exemplo, imagine que tem um pipeline que é executado às 08:00, às 09: 00 e às 10:00. Neste caso, existem três execuções separadas do pipeline (execuções de pipeline). Cada execução de pipeline tem um ID exclusivo, que é um GUID que as define exclusivamente. Normalmente, as execuções de pipeline são instanciadas pela transmissão de argumentos a parâmetros definidos nos pipelines. |
| Execuções de Atividade | ND | Uma instância de uma execução de atividade dentro de um pipeline. | 
| Execuções de acionador | ND | Uma instância de uma execução do acionador. Para obter mais informações, consulte [Acionadores](concepts-pipeline-execution-triggers.md). |
| Agendamento | O agendamento baseia-se nas horas de início/fim do pipeline e na disponibilidade do conjunto de dados. | Acionador de agendador ou execução através de agendador externo. Para mais informações, consulte [Execuções de pipelines e acionadores](concepts-pipeline-execution-triggers.md). |

As secções seguintes fornecem mais informações da versão 2: 

## <a name="control-flow"></a>Fluxo de controlo  
Para suportar os diversos fluxos e padrões de integração nos armazéns de dados modernos, o Data Factory V2 disponibiliza um novo modelo de pipeline de dados flexível, que já não está associado a dados de séries de tempo. Alguns fluxos comuns agora possíveis e que não o eram antes incluem:   

### <a name="chaining-activities"></a>Encadeamento de atividades
Na versão 1, tinha que configurar a saída de uma atividade como a entrada de outra atividade para as encadear. No V2, pode encadear atividades numa sequência dentro de um pipeline. Pode utilizar a propriedade **dependsOn** numa definição de atividade para encadeá-la com uma atividade de origem. Para obter mais informações e um exemplo, consulte os artigos [Pipelines e atividades](concepts-pipelines-activities.md#multiple-activities-in-a-pipeline) e [Ramificar e encadear atividades](tutorial-control-flow.md). 

### <a name="branching-activities"></a>Ramificar atividades
Agora, no V2, pode ramificar atividades dentro de um pipeline. A [atividade condição-If](control-flow-if-condition-activity.md) disponibiliza a mesma funcionalidade que as instruções `if` fornecem nas linguagens de programação. Avalia um conjunto de atividades quando a condição é avaliada como `true` e outro conjunto de atividades quando é avaliada como `false`. Para obter um exemplo de ramificação de atividades, consulte o tutorial [Ramificar e encadear atividades](tutorial-control-flow.md).

### <a name="parameters"></a>Parâmetros 
Pode definir parâmetros ao nível do pipeline e passar argumentos quando é invocado o pipeline a pedido ou a partir de um acionador. As atividades podem consumir os argumentos transmitidos para o pipeline Para mais informações, consulte [Pipelines e acionadores](concepts-pipeline-execution-triggers.md). 

### <a name="custom-state-passing"></a>Transmissão de estado personalizado
Os resultados das atividades, incluindo o estado, podem ser consumidos por uma atividade subsequente no pipeline. Por exemplo, na definição de JSON de uma atividade, pode aceder à saída da atividade anterior com a seguinte sintaxe: `@activity('NameofPreviousActivity').output.value`. Esta funcionalidade permite-lhe criar fluxos de trabalho em que os valores podem passar por atividades.

### <a name="looping-containers"></a>Contentores de ciclo
A [atividade ForEach](control-flow-for-each-activity.md) define um fluxo de controlo de repetição no seu pipeline. Esta atividade é utilizada para iterar uma coleção e executa atividades especificadas em ciclo. A implementação de ciclo desta atividade é semelhante à estrutura de ciclo Foreach nas linguagens de programação. 

A atividade [Until](control-flow-until-activity.md) disponibiliza a mesma funcionalidade que uma estrutura de ciclos do-until fornecem nas linguagens de programação. Executa um conjunto de atividades num ciclo até que a condição associada às atividades seja avaliada como verdadeira. Pode especificar um valor de tempo limite para a atividade Until no Data Factory.  

### <a name="trigger-based-flows"></a>Fluxos baseados em acionadores
Os pipelines podem ser adicionados a pedido ou com base no tempo total de execução. O artigo [pipelines e acionadores](concepts-pipeline-execution-triggers.md) tem informações detalhadas sobre acionadores. 

### <a name="invoke-a-pipeline-from-another-pipeline"></a>Invocar um pipeline de outro pipeline
A [atividade Executar Pipeline](control-flow-execute-pipeline-activity.md) permite que um pipeline do Data Factory invoque outro pipeline.

### <a name="delta-flows"></a>Fluxos delta
Um caso de utilização-chave em padrões ETL são "cargas-delta" ou apenas carregar dados que foram alterados desde a última iteração de um pipeline. As novas funcionalidades na versão 2, tais como [atividade de pesquisa](control-flow-lookup-activity.md), agendamento flexível e fluxo de controlo permitem este caso de utilização de forma natural. Para um tutorial com instruções passo a passo, consulte [Tutorial: Cópia incremental](tutorial-incremental-copy-powershell.md).

### <a name="other-control-flow-activities"></a>Outras atividades de fluxo de controlo
Seguem-se mais algumas atividades de fluxo de controlo suportadas pelo Data Factory V2: 

Atividade de controlo | Descrição
---------------- | -----------
[Atividade ForEach](control-flow-for-each-activity.md) | A atividade ForEach define um fluxo de controlo de repetição no seu pipeline. Esta atividade é utilizada para iterar uma coleção e executa atividades especificadas em ciclo. A implementação de ciclo desta atividade é semelhante à estrutura de ciclo Foreach nas linguagens de programação.
[Atividade Web](control-flow-web-activity.md) | A atividade Web pode ser utilizada para chamar um ponto final REST personalizado a partir de um pipeline do Data Factory. Pode transmitir conjuntos de dados e serviços ligados aos quais a atividade tem acesso e que pode consumir. 
[Atividade de Pesquisa](control-flow-lookup-activity.md) | A atividade de Pesquisa pode ser utilizada para ler ou procurar registos/nomes de tabelas/valores em qualquer origem externa. Este resultado pode ser ainda referenciado por atividades subsequentes. 
[Atividade Obter Metadados](control-flow-get-metadata-activity.md) | A atividade Obter Metadados pode ser utilizada para obter os metadados de quaisquer dados no Azure Data Factory. 
[Atividade Aguardar](control-flow-wait-activity.md) | O pipeline aguarda (coloca em pausa) para o período de tempo especificado.

## <a name="deploy-ssis-packages-to-azure"></a>Implementar pacotes do SSIS no Azure 
Se quiser mover as cargas de trabalho do SQL Server Integration Services (SSIS) para a cloud, crie uma fábrica de dados da V2 e aprovisionar um Runtime de Integração (IR) Azure-SSIS. O IR Azure-SSIS é um cluster totalmente gerido de VMs do Azure (nós) dedicadas à execução dos pacotes do SSIS na cloud. Depois de aprovisionar o IR do Azure-SSIS, tem as mesmas ferramentas que tem estado a utilizar para implementar pacotes do SSIS num ambiente SSIS local. Por exemplo, pode utilizar o SQL Server Data Tools (SSDT) ou o SQL Server Management Studio (SSMS) para implementar pacotes do SSIS neste runtime no Azure. Para obter instruções passo a passo, veja o tutorial [Implementar pacotes de SQL Server Integration Services no Azure](tutorial-deploy-ssis-packages-azure.md). 

## <a name="flexible-scheduling"></a>Agendamento flexível
No Data Factory V2, não terá de definir agendas de disponibilidade para conjuntos de dados. Pode definir um recurso de acionador que agende os pipelines a partir de um paradigma de programador de relógio. Pode também passar parâmetros para os pipelines a partir de um acionador para um modelo de agendamento/execução flexível. Os pipelines não dispõem de "janelas" da execução de tempo no Data Factory V2. Os conceitos startTime, endTime e isPaused do Data Factory V1 já não estão presentes no Data Factory V2. Eis como criar e agendar um pipeline no Data Factory V2: [Execução e acionadores de pipelines](concepts-pipeline-execution-triggers.md).

## <a name="support-for-more-data-stores"></a>Suporte para mais arquivos de dados
O V2 suporta a cópia de dados de/para mais arquivos de dados que V1. Para obter uma lista dos arquivos de dados suportados, consulte os artigos seguintes:

- [V1 - arquivos de dados suportados](v1/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
- [V2 - arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats)

## <a name="support-for-on-demand-spark-cluster"></a>Suporte para cluster do Spark a pedido
V2 suporta a criação de um cluster do Spark do HDInsight a pedido. Para criar um cluster do Spark a pedido, especifique o tipo de cluster como Spark no seu HDInsight a pedido associado à definição de serviço. Em seguida, pode configurar a atividade do Spark no seu pipeline para utilizar este serviço ligado. No runtime, quando a atividade é executada, o serviço Data Factory cria automaticamente o cluster do Spark para si. Para obter mais informações, veja os artigos seguintes:

- [Atividade do Spark no V2](transform-data-using-spark.md)
- [Serviço ligado do Azure HDInsight a pedido](compute-linked-services.md#azure-hdinsight-on-demand-linked-service)

## <a name="custom-activities"></a>Atividades personalizadas
No V1, implementa códigos de Atividade DotNet (Personalizado) ao criar um projeto de Biblioteca de Classe .Net com uma classe que implemente o método Execute da interface IDotNetActivity. Portanto, o seu código personalizado precisa de ser escrito em .Net Framework 4.5.2 e ser executado em nós do Azure Batch Pool baseados em Windows. 

Na Atividade Personalizada V2, não precisa de implementar uma interface de .Net. Pode agora executar diretamente comandos de scripts e executar o seu próprio código personalizado compilado como um executável. 

Para mais informações, consulte [Diferença entre a atividade personalizada no V1 e V2](transform-data-using-dotnet-custom-activity.md#difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1).

## <a name="sdks"></a>SDKs
A Versão 2 do Data Factory fornece um melhor conjunto de SDKs que podem ser utilizados para criar, gerir e monitorizar pipelines.

- **.NET SDK**: o SDK .NET está atualizado para V2. 
- **PowerShell**: os cmdlets do PowerShell estão atualizados para V2. Os cmdlets de V2 têm **V2** no nome. Por exemplo: Get-AzureRmDataFactoryV2. 
- **Python SDK**: este SDK é novo no V2.
- **REST API**: a API REST está atualizada para V2. 

Os SDKs que são atualizados para a V2 não são compatíveis com versões anteriores com clientes de versão 1. 

## <a name="authoring-experience"></a>Experiência de criação
O Data Factory V1 permite-lhe criar pipelines com o Data Factory Editor no portal do Azure. Atualmente, o Data Factory V2 suporta apenas a forma via programação (.NET SDK, REST API, PowerShell, Python, etc.) para criar fábricas de dados. Ainda não existe suporte de interface de utilizador.  O Data Factory V1 também suporta o suporte de criação de conteúdos de SDK, REST e PowerShell.

## <a name="monitoring-experience"></a>Experiência de monitorização
No V2, também pode monitorizar fábricas de dados ao utilizar o [Azure Monitor](monitor-using-azure-monitor.md). Os novos cmdlets do PowerShell suportam a monitorização de [tempos de execução de integração](monitor-integration-runtime.md). Ambos o V1 e V2 suportam monitorização visual através da aplicação de monitorização que pode ser aberta a partir do portal do Azure.


## <a name="next-steps"></a>Passos seguintes
Saiba como criar uma fábrica de dados com as instruções passo a passo nos Inícios Rápidos [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST API](quickstart-create-data-factory-rest-api.md). 
