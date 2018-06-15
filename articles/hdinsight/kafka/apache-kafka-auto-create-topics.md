---
title: Permitir a criação automática de tópico no Apache Kafka - Azure HDInsight | Microsoft Docs
description: Saiba como configurar o Apache Kafka no HDInsight para criar automaticamente tópicos. Pode configurar Kafka definindo auto.create.topics.enable true através do Ambari ou durante a criação de clusters através do PowerShell ou Gestor de recursos de modelos.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/18/2018
ms.author: larryfr
ms.openlocfilehash: fa5dd7533259c794671cd16231fd3f530173bfa3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787228"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Como configurar o Apache Kafka no HDInsight para criar automaticamente tópicos

Por predefinição, Kafka no HDInsight não permitem a criação automática de tópico. Pode ativar a criação de tópico de automático para os clusters existentes com o Ambari. Também pode ativar a criação automática de tópico quando criar um novo cluster de Kafka através de um modelo Azure Resource Manager.

## <a name="ambari-web-ui"></a>IU da Web do Ambari

Para permitir a criação de tópico automática num cluster existente através da IU de Web do Ambari, utilize os seguintes passos:

1. Do [portal do Azure](https://portal.azure.com), selecione o cluster Kafka.

2. Do __descrição geral do Cluster__, selecione __Cluster dashboard__. 

    ![Imagem do portal com o dashboard do cluster selecionado](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. Em seguida, selecione __dashboard de cluster do HDInsight__. Quando lhe for pedido, autenticar com as credenciais de início de sessão (administrador) para o cluster.

    ![Imagem da entrada de dashboard do cluster de HDInsight](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. Selecione o serviço de Kafka na lista à esquerda da página.

    ![Lista de serviço](./media/apache-kafka-auto-create-topics/service-list.png)

4. Selecione o documento no meio da página.

    ![Separador de configuração de serviço](./media/apache-kafka-auto-create-topics/service-config.png)

5. No campo de filtro, introduza um valor de `auto.create`. 

    ![Imagem do campo de filtro](./media/apache-kafka-auto-create-topics/filter.png)

    Este procedimento filtra a lista de propriedades e apresenta o `auto.create.topics.enable` definição.

6. Altere o valor de `auto.create.topics.enable` para `true`e, em seguida, selecione guardar. Adicionar uma nota e, em seguida, selecione guardar novamente.

    ![Imagem da entrada de auto.create.topics.enable](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. Selecione o serviço de Kafka, selecione __reiniciar__e, em seguida, selecione __reiniciar todos os afetados__. Quando lhe for pedido, selecione __confirmar reinicie todos os__.

    ![Imagem da seleção de reinício](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]
> Também pode definir valores de Ambari através da API de REST do Ambari. Isto é, geralmente, mais difícil, tal como fez para várias chamadas de REST para obter a configuração atual, modifique-lo, etc. Para obter mais informações, consulte o [gerir clusters do HDInsight utilizando a API de REST do Ambari](../hdinsight-hadoop-manage-ambari-rest-api.md) documento.

## <a name="resource-manager-templates"></a>Modelos do Resource Manager

Quando criar um cluster de Kafka através de um modelo Azure Resource Manager, pode definir diretamente `auto.create.topics.enable` adicionando-a num `kafka-broker`. O fragmento JSON seguinte demonstra como definir este valor para `true`:

```json
"clusterDefinition": {
    "kind": "kafka",
    "configurations": {
    "gateway": {
        "restAuthCredential.isEnabled": true,
        "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
        "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
    },
    "kafka-broker": {
        "auto.create.topics.enable": "true"
    }
}
```

## <a name="next-steps"></a>Próximos Passos

Neste documento, aprendeu a permitir a criação de tópico automática para Kafka no HDInsight. Para obter mais informações sobre como trabalhar com Kafka, consulte as seguintes ligações:

* [Analisar os registos do Kafka](apache-kafka-log-analytics-operations-management.md)
* [Replicar dados entre clusters do Kafka](apache-kafka-mirroring.md)