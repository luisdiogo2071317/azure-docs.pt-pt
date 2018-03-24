---
title: Transformar dados com o bloco de notas do Databricks - Azure | Microsoft Docs
description: Saiba como processar ou dados de transformação através da execução de um bloco de notas Databricks.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: d4a57e45d5ddf55906fcf575df39135a227418ec
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Transformar dados através da execução de um bloco de notas Databricks

A atividade de bloco de notas do Azure Databricks num [pipeline do Data Factory](concepts-pipelines-activities.md) executa um bloco de notas Databricks na sua área de trabalho Databricks do Azure. Este artigo baseia-se a [atividades de transformação de dados](transform-data.md) artigo, que apresenta uma descrição geral de transformação de dados e as atividades de transformação suportados. Databricks do Azure é uma plataforma gerida para executar o Apache Spark.

## <a name="databricks-notebook-activity-definition"></a>Definição de atividade do bloco de notas Databricks

Segue-se a definição de JSON de exemplo de uma atividade de bloco de notas Databricks:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            }
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Propriedades da atividade Databricks bloco de notas

A tabela seguinte descreve as propriedades JSON utilizadas na definição de JSON:

|Propriedade|Descrição|Necessário|
|---|---|---|
|nome|Nome da atividade no pipeline.|Sim|
|descrição|Texto que descreve o que faz a atividade.|Não|
|tipo|Para a atividade de bloco de notas Databricks, o tipo de atividade é DatabricksNotebook.|Sim|
|linkedServiceName|Nome do serviço ligado Databricks no qual o bloco de notas Databricks é executada. Para saber mais sobre este serviço ligado, consulte [serviços ligados de computação](compute-linked-services.md) artigo.|Sim|
|notebookPath|O caminho absoluto do bloco de notas para ser executada na área de trabalho Databricks. Este caminho tem de começar com uma barra.|Sim|
|baseParameters|Uma matriz de pares chave-valor. Base parâmetros podem ser utilizados para cada execução da atividade. Se o bloco de notas assume um parâmetro que não for especificado, será utilizado o valor predefinido do bloco de notas. Encontrar mais informações sobre parâmetros [Databricks blocos de notas](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Não|
