---
title: Como eliminar um cluster do HDInsight - Azure
description: Informações sobre as várias formas que pode eliminar um cluster do HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: dff9420eb0f91652cc134a37d1b248e2e5b2b681
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004540"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>Eliminar um cluster do HDInsight com o seu browser, o PowerShell ou a CLI clássica do Azure

A faturação do cluster do HDInsight tem início quando o cluster é criado e termina quando é eliminado. A faturação é rateada por minuto, pelo que deve sempre eliminar o cluster quando deixar de ser utilizado. Neste documento, saiba como eliminar um cluster com o portal do Azure, Azure PowerShell e CLI clássica do Azure.

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

## <a name="azure-classic-cli"></a>CLI clássica do Azure

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

A partir de uma linha de comandos, utilize o seguinte para eliminar o cluster:

    azure hdinsight cluster delete CLUSTERNAME

Substitua **CLUSTERNAME** pelo nome do cluster do HDInsight.
