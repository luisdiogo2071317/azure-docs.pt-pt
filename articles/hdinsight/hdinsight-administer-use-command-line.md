---
title: Gerir clusters do Hadoop com a CLI do Azure - Azure HDInsight
description: Saiba como utilizar a Interface de linha de comandos do Azure para gerir clusters do Hadoop no HDInsight do Azure. A CLI do Azure funciona no Windows, Mac e Linux.
services: hdinsight
editor: jasonwhowell
author: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: dea0f004c4283bf594e46097092a52dedabb9f4b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599025"
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Gira clusters Hadoop no HDInsight com a CLI do Azure
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Saiba como utilizar o [Interface de linha de comandos do Azure](../cli-install-nodejs.md) para gerir clusters do Hadoop no HDInsight do Azure. A CLI do Azure está implementada no Node.js. Pode ser utilizado em qualquer plataforma que suporte Node.js, incluindo Windows, Mac e Linux. Atualmente não suporta o HDInsight [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure).

Este artigo abrange apenas a utilização da CLI do Azure com o HDInsight. Para obter um guia Geral sobre como utilizar a CLI do Azure, veja [instalar e configurar a CLI do Azure][azure-command-line-tools].

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este artigo, tem de ter o seguinte:

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **CLI do Azure** -Veja [Instalar e configurar a CLI do Azure](../cli-install-nodejs.md) para obter informações de instalação e configuração.
* **Ligar ao Azure**, com o seguinte comando:

    ```cli
    azure login
    ```
  
    Para mais informações sobre a autenticação através de uma conta escolar ou profissional, consulte [Ligar a uma subscrição do Azure a partir da CLI do Azure](/cli/azure/authenticate-azure-cli).
* **Mude para o modo Azure Resource Manager**, utilizando o seguinte comando:
  
    ```cli
    azure config mode arm
    ```

Para obter ajuda, utilize o **-h** mudar.  Por exemplo:

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>Criar clusters com a CLI
Ver [criar clusters no HDInsight com a CLI do Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Listar e mostrar detalhes do cluster
Utilize os seguintes comandos para listar e mostrar detalhes do cluster:

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![Vista de linha de comandos de lista de clusters][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Eliminar clusters
Utilize o seguinte comando para eliminar um cluster:

```cli
azure hdinsight cluster delete <Cluster Name>
```

Também pode eliminar um cluster ao eliminar o grupo de recursos que contém o cluster. Tenha em atenção, esta ação irá eliminar todos os recursos no grupo, incluindo a conta de armazenamento predefinida.

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>Dimensionar clusters
Para alterar o tamanho de cluster do Hadoop:

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>Ativar/desativar o acesso HTTP para um cluster

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Ativar/desativar o acesso do RDP para um cluster

```cli
azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
azure hdinsight cluster disable-rdp-access [options] <Cluster Name>
```

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como efetuar diferentes tarefas administrativas de cluster HDInsight. Para obter mais informações, consulte os artigos seguintes:

* [Administrar o HDInsight com o Portal do Azure][hdinsight-admin-portal]
* [Administrar o HDInsight com o Azure PowerShell][hdinsight-admin-powershell]
* [Get started with Azure HDInsight (Introdução ao Azure HDInsight)][hdinsight-get-started]
* [Como utilizar a CLI do Azure][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Listar e Mostrar clusters"
