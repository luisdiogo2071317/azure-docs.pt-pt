---
title: Como eliminar um cluster do HDInsight - Azure
description: Informações sobre as várias formas que pode eliminar um cluster do HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: jasonh
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 737cc120877a9d0f06a1f6d209bcf9a233aa7d19
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091314"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Eliminar um cluster do HDInsight com o seu browser, o PowerShell ou a CLI do Azure

A faturação do cluster do HDInsight tem início quando o cluster é criado e termina quando é eliminado. A faturação é rateada por minuto, pelo que deve sempre eliminar o cluster quando deixar de ser utilizado. Neste documento, saiba como eliminar um cluster com o portal do Azure, Azure PowerShell e a CLI 1.0 do Azure.

> [!IMPORTANT]
> Eliminar um cluster do HDInsight não elimina as contas de armazenamento do Azure ou o Data Lake Store associada ao cluster. É possível reutilizar os dados armazenados nesses serviços no futuro.

## <a name="azure-portal"></a>Portal do Azure

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) e selecione o cluster do HDInsight. Se o seu cluster do HDInsight não está afixado ao dashboard, pode procurá-lo por nome com o campo de pesquisa.
   
    ![pesquisa de portal](./media/hdinsight-delete-cluster/navbar.png)

2. As definições do cluster, selecione o **eliminar** ícone. Quando lhe for pedido, selecione **Sim** para eliminar o cluster.
   
    ![ícone Eliminar](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

A partir de uma linha de comandos do PowerShell, utilize o seguinte comando para eliminar o cluster:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Substitua **CLUSTERNAME** pelo nome do cluster do HDInsight.

## <a name="azure-cli-10"></a>CLI do Azure 1.0

A partir de uma linha de comandos, utilize o seguinte para eliminar o cluster:

    azure hdinsight cluster delete CLUSTERNAME

Substitua **CLUSTERNAME** pelo nome do cluster do HDInsight.

> [!NOTE]
> CLI 2.0 do Azure não suporta a eliminar clusters do HDInsight neste momento (23 de Outubro de 2017).
