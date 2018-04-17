---
title: Atualizar cluster do HDInsight para uma versão mais recente-Azure | Microsoft Docs
description: Saiba como cluster de HDInsight atualizar para uma versão mais recente.
services: hdinsight
documentationcenter: ''
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: d6cd0dd8b326f5340e5014bba4babc6dd945744d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a>Atualize o cluster do HDInsight para uma versão mais recente
Para tirar partido das funcionalidades mais recentes do HDInsight, recomendamos que os clusters do HDInsight sejam atualizados para a versão mais recente. Siga o abaixo diretrizes para atualizar o HDInsight versões de cluster.

> [!NOTE]
> Clusters do HDInsight versão 3.2 e 3.3 estão próximos da data de retirada. Para obter informações sobre a versão suportada do HDInsight, consulte [as versões de componentes do HDInsight](hdinsight-component-versioning.md#supported-hdinsight-versions).
>
>

## <a name="upgrade-tasks"></a>Tarefas de atualização
Segue-se o fluxo de trabalho para atualizar o Cluster do HDInsight.

![Diagrama de fluxo de trabalho de atualização](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. Leia cada secção deste documento para compreender as alterações que poderão ser necessárias quando atualizar o cluster do HDInsight.
2. Crie um cluster como um ambiente de garantia de teste/qualidade. Para obter mais informações sobre como criar um cluster, consulte [Saiba como criar clusters do HDInsight baseado em Linux](hdinsight-hadoop-provision-linux-clusters.md)
3. Copie as tarefas existentes, origens de dados e sinks para o novo ambiente. Consulte [copiar dados para ambiente de teste](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) para obter mais detalhes.
4. Execute testes de validação para se certificar de que as tarefas funcionam como esperado no novo cluster.


Depois de verificar que tudo funciona conforme esperado, agende o período de indisponibilidade para a migração. Durante este período de indisponibilidade, efetue as seguintes ações:

1.  Cópia de segurança de todos os dados transitórios armazenados localmente em nós de cluster. Por exemplo, se tiver dados armazenados diretamente num nó principal.
2.  Elimine o cluster existente.
3.  Crie um cluster na mesma sub-rede da VNET com mais recente (ou suportado) versão HDI utilizando o mesmo arquivo de dados predefinida que o cluster anterior utilizado. Isto permite que o novo cluster continuar a trabalhar contra os dados de produção existentes.
4.  Importe dados transitórios que uma cópia de segurança.
5.  Iniciar tarefas/continuar a processar utilizando o novo cluster.

## <a name="next-steps"></a>Próximos Passos
* [Saiba como criar clusters do HDInsight baseado em Linux](hdinsight-hadoop-provision-linux-clusters.md)
* [Ligar ao HDInsight através de SSH](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Gerir um cluster baseado em Linux com o Ambari](hdinsight-hadoop-manage-ambari.md)

