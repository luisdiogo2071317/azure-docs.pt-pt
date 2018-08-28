---
title: Instalar ou atualizar o Mono no HDInsight - Azure
description: Saiba como utilizar uma versão específica do Mono com o cluster do HDInsight. Mono é utilizada para executar aplicações de .NET em clusters do HDInsight baseado em Linux.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: jasonh
ms.custom: hdinsightactive
ms.openlocfilehash: db460c6ebe934fa9ca9b6b42d517f39acbecf0c9
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105318"
---
# <a name="install-or-update-mono-on-hdinsight"></a>Instalar ou atualizar o Mono no HDInsight

Saiba como instalar uma versão específica do [Mono](https://www.mono-project.com) no HDInsight 3.4 ou superior.

Mono está instalado no HDInsight 3.4 ou superior e é utilizado para executar aplicações de .NET. Para obter informações sobre a versão do Mono incluído em cada versão do HDInsight, consulte [controlo de versões de componente de HDInsight](hdinsight-component-versioning.md). Para instalar uma versão diferente no seu cluster, utilize a ação de script neste documento. 

## <a name="how-it-works"></a>Como funciona

Este script aceita o parâmetro seguinte:

* __Número de versão mono__: A versão do Mono para instalar. A versão tem de estar disponível a partir [ https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/ ](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

O script instala os seguintes pacotes Mono:

* __mono-complete__

* __ca-certificates-mono__

## <a name="the-script"></a>O script

__Localização do script__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Requisitos de__:

* O script deve ser aplicado a __nós principais__ e __nós de trabalho__.

## <a name="to-use-the-script"></a>Para utilizar o script

Para obter informações sobre como utilizar este script com o HDInsight, consulte a [HDInsight baseado em Linux personalizar clusters com ação de script](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) documento. Pode usar o script através do portal do Azure, Azure PowerShell ou a CLI do Azure.

Ao seguir o documento de ação de script, utilize o URI seguinte:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

Para especificar a versão do Mono que está instalada, utilize o número de versão no __parâmetros__ campo. Por exemplo, introduza `5.4` instalar Mono 5.4.

> [!NOTE]
> Quando configurar o HDInsight com este script, marque o script como __persistentes__. Esta definição permite que o HDInsight aplicar o script para nós de trabalho adicionados por meio de operações de dimensionamento.

## <a name="next-steps"></a>Passos Seguintes

Aprendeu como atualizar ou instalar uma versão específica do Mono no HDInsight. Para obter mais informações sobre a utilização de aplicações .NET com Mono no HDInsight, consulte os seguintes documentos:

* [Use o .NET para transmissão em fluxo MapReduce no HDInsight](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Use o .NET com o Hive e Pig no HDInsight](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)
* [Desenvolver soluções em c# com o Storm no HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md)
* [Migrar soluções .NET para o HDInsight baseado em Linux](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Para obter mais informações sobre como utilizar as ações de script, consulte [HDInsight baseado em Linux personalizar clusters com ação de script](hdinsight-hadoop-customize-cluster-linux.md)
