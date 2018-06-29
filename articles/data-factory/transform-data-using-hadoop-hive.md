---
title: Transformar dados utilizando a atividade do Hive do Hadoop no Azure Data Factory | Microsoft Docs
description: Saiba como pode utilizar a atividade do ramo de registo de um Azure data factory para executar consultas do Hive num cluster do HDInsight no-a pedido/seu próprio.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: fc7c2c49de582a413b49d31c4b4e062d81e5e6ae
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051329"
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Transformar dados utilizando a atividade do Hive do Hadoop no Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-hive-activity.md)
> * [Versão atual](transform-data-using-hadoop-hive.md)

A atividade do ramo de registo do HDInsight numa fábrica de dados [pipeline](concepts-pipelines-activities.md) executa consultas do Hive no [os seus próprios](compute-linked-services.md#azure-hdinsight-linked-service) ou [a pedido](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) cluster do HDInsight. Este artigo baseia-se a [atividades de transformação de dados](transform-data.md) artigo, que apresenta uma descrição geral de transformação de dados e as atividades de transformação suportados.

Se estiver familiarizado com o Azure Data Factory, leia [introdução ao Azure Data Factory](introduction.md) e efetue a [Tutorial: transformar dados](tutorial-transform-data-spark-powershell.md) antes de ler este artigo. 

## <a name="syntax"></a>Sintaxe

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }   
}
```
## <a name="syntax-details"></a>Detalhes de sintaxe
| Propriedade            | Descrição                              | Necessário |
| ------------------- | ---------------------------------------- | -------- |
| nome                | Nome da atividade                     | Sim      |
| descrição         | Texto que descreve o que é utilizada a atividade para | Não       |
| tipo                | Para a atividade do ramo de registo, o tipo de atividade é HDinsightHive | Sim      |
| linkedServiceName   | Referência para o cluster do HDInsight registado como um serviço ligado no Factory de dados. Para saber mais sobre este serviço ligado, consulte [serviços ligados de computação](compute-linked-services.md) artigo. | Sim      |
| scriptLinkedService | Referência a um serviço ligado do Storage do Azure utilizados para armazenar o script de ramo de registo para ser executada. Se não especificar este serviço ligado, o serviço ligado de armazenamento de Azure definido no serviço ligado do HDInsight é utilizado. | Não       |
| scriptPath          | Forneça o caminho para o ficheiro de script armazenado no Storage do Azure que referida pelo scriptLinkedService. O nome de ficheiro é maiúsculas e minúsculas. | Sim      |
| getDebugInfo        | Especifica se os ficheiros de registo são copiados para o armazenamento do Azure utilizados pelo cluster do HDInsight (ou) especificado pelo scriptLinkedService. Valores permitidos: None, sempre ou falha. Valor predefinido: nenhuma. | Não       |
| argumentos           | Especifica uma matriz de argumentos para uma tarefa do Hadoop. Os argumentos são transmitidos como argumentos da linha de comandos para cada tarefa. | Não       |
| Define             | Especifique parâmetros como pares chave-valor para referenciar dentro do script de ramo de registo. | Não       |

## <a name="next-steps"></a>Passos Seguintes
Consulte os artigos seguintes que explicam como transformar dados de outras formas: 

* [Atividade U-SQL](transform-data-using-data-lake-analytics.md)
* [Atividade do PIg](transform-data-using-hadoop-pig.md)
* [Atividade de MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Atividade de transmissão em fluxo do Hadoop](transform-data-using-hadoop-streaming.md)
* [Atividade do Spark](transform-data-using-spark.md)
* [Atividade personalizada do .NET](transform-data-using-dotnet-custom-activity.md)
* [Atividade de execução de lote de aprendizagem máquina](transform-data-using-machine-learning.md)
* [Atividade de procedimento armazenado](transform-data-using-stored-procedure.md)

