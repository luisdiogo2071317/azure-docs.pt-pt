---
title: Gerir o Azure Data Lake Analytics utilizando a Interface de linha de comandos do Azure | Microsoft Docs
description: Saiba como gerir contas de Data Lake Analytics, origens de dados, tarefas e os utilizadores com a CLI do Azure
services: data-lake-analytics
documentationcenter: ''
author: SnehaGunda
manager: Kfile
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/29/2018
ms.author: sngun
ms.openlocfilehash: 9504e1fcbb0128f538cb6e959cf7586f24827836
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Gerir o Azure Data Lake Analytics utilizando a Interface de linha de comandos do Azure (CLI)

[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Saiba como gerir contas de Azure Data Lake Analytics, origens de dados, os utilizadores e tarefas utilizando a CLI do Azure. Para ver tópicos de gestão, utilizando outras ferramentas, clique em Selecionar o separador acima.


**Pré-requisitos**

Antes de começar este tutorial, tem de ter os seguintes recursos:

* Uma subscrição do Azure. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

* CLI do Azure. Consulte [instalar e configurar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

   * Transfira e instale o **pré-lançamento** [Ferramentas CLI do Azure](https://github.com/MicrosoftBigData/AzureDataLake/releases) para concluir esta demonstração.

* Autenticar utilizando o `az login` de comandos e selecione a subscrição que pretende utilizar. Para mais informações sobre a autenticação através de uma conta escolar ou profissional, consulte [Ligar a uma subscrição do Azure a partir da CLI do Azure](/cli/azure/authenticate-azure-cli).

   ```azurecli
   az login
   az account set --subscription <subscription id>
   ```

   Agora pode aceder aos comandos de Data Lake Analytics e Data Lake Store. Execute o seguinte comando para listar os comandos de Data Lake Store e do Data Lake Analytics:

   ```azurecli
   az dls -h
   az dla -h
   ```

## <a name="manage-accounts"></a>Gerir contas

Antes de executar as tarefas de Data Lake Analytics, tem de ter uma conta de Data Lake Analytics. Ao contrário do Azure HDInsight, que não paga por uma conta de análise quando não está em execução uma tarefa. Apenas paga a hora quando se encontra em execução uma tarefa.  Para obter mais informações, consulte [descrição geral do Azure Data Lake Analytics](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Criar contas

Execute o seguinte comando para criar uma conta de Data Lake 

   ```azurecli
   az dla account create --account "<Data Lake Analytics account name>" --location "<Location Name>" --resource-group "<Resource Group Name>" --default-data-lake-store "<Data Lake Store account name>"
   ```

### <a name="update-accounts"></a>Atualizar contas

O seguinte comando atualiza as propriedades de uma conta existente do Data Lake Analytics

   ```azurecli
   az dla account update --account "<Data Lake Analytics Account Name>" --firewall-state "Enabled" --query-store-retention 7
   ```

### <a name="list-accounts"></a>Contas de lista

Contas de análise do Data Lake da lista dentro de um grupo de recurso específico

   ```azurecli
   az dla account list "<Resource group name>"
   ```

## <a name="get-details-of-an-account"></a>Obter os detalhes de uma conta

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

### <a name="delete-an-account"></a>Eliminar uma conta

   ```azurecli
   az dla account delete --account "<Data Lake Analytics account name>" --resource-group "<Resource group name>"
   ```

## <a name="manage-data-sources"></a>Gerir origens de dados

O Data Lake Analytics, atualmente, suporta as seguintes duas origens de dados:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Armazenamento do Azure](../storage/common/storage-introduction.md)

Quando cria uma conta de análise, tem de designar uma conta de armazenamento do Azure Data Lake para ser a conta do storage predefinida. A conta de armazenamento do Data Lake predefinida é utilizada para armazenar os registos de auditoria de tarefa e os metadados de tarefa. Depois de criar uma conta de análise, pode adicionar mais contas de armazenamento do Data Lake e/ou a conta de armazenamento do Azure. 

### <a name="find-the-default-data-lake-store-account"></a>Localizar a conta de Data Lake Store predefinida

Pode ver a conta de Data Lake Store predefinida utilizada ao executar o `az dla account show` comando. Nome da conta predefinida é listada sob a propriedade defaultDataLakeStoreAccount.

   ```azurecli
   az dla account show --account "<Data Lake Analytics account name>"
   ```

### <a name="add-additional-blob-storage-accounts"></a>Adicionar mais contas de armazenamento de BLOBs

   ```azurecli
   az dla account blob-storage add --access-key "<Azure Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Storage account name>"
   ```

> [!NOTE]
> São suportados apenas Blob storage nomes abreviados. Não utilize o FQDN, por exemplo "myblob.blob.core.windows.net".
> 

### <a name="add-additional-data-lake-store-accounts"></a>Adicionar mais contas de Data Lake Store

O seguinte comando atualiza a conta de Data Lake Analytics especificada com uma conta de Data Lake Store adicional:

   ```azurecli
   az dla account data-lake-store add --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Data Lake Store account name>"
   ```

### <a name="update-existing-data-source"></a>Atualizar a origem de dados existente

Para atualizar uma chave de conta de armazenamento de Blob existente:

   ```azurecli
   az dla account blob-storage update --access-key "<New Blob Storage Account Key>" --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

### <a name="list-data-sources"></a>Lista de origens de dados:

Para listar as contas de Data Lake Store:

   ```azurecli
   az dla account data-lake-store list --account "<Data Lake Analytics account name>"
   ```

Para listar a conta de armazenamento de BLOBs:

   ```azurecli
   az dla account blob-storage list --account "<Data Lake Analytics account name>"
   ```

![Origem de dados do Data Lake Analytics lista](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Elimine as origens de dados:
Para eliminar uma conta de Data Lake Store:

   ```azurecli
   az dla account data-lake-store delete --account "<Data Lake Analytics account name>" --data-lake-store-account-name "<Azure Data Lake Store account name>"
   ```

Para eliminar uma conta de armazenamento de BLOBs:

   ```azurecli
   az dla account blob-storage delete --account "<Data Lake Analytics account name>" --storage-account-name "<Data Lake Store account name>"
   ```

## <a name="manage-jobs"></a>Gerir tarefas
Tem de ter uma conta de Data Lake Analytics antes de poder criar uma tarefa.  Para obter mais informações, consulte [contas de gerir a análise do Data Lake](#manage-accounts).

### <a name="list-jobs"></a>Lista de tarefas

   ```azurecli
   az dla job list --account "<Data Lake Analytics account name>"
   ```

   ![Origem de dados do Data Lake Analytics lista](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Obter os detalhes da tarefa

   ```azurecli
   az dla job show --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

### <a name="submit-jobs"></a>Submeter tarefas

> [!NOTE]
> A prioridade de predefinido de uma tarefa é 1000 e o grau de predefinição de paralelismo de uma tarefa é 1.
> 
   ```azurecli
   az dla job submit --account "<Data Lake Analytics account name>" --job-name "<Name of your job>" --script "<Script to submit>"
   ```

### <a name="cancel-jobs"></a>Cancelar tarefas
Utilize o comando de lista para localizar o id da tarefa e, em seguida, Cancelar para cancelar a tarefa.

   ```azurecli
   az dla job cancel --account "<Data Lake Analytics account name>" --job-identity "<Job Id>"
   ```

## <a name="pipelines-and-recurrences"></a>Pipelines e recorrências

**Obter informações sobre pipelines e recorrências**

Utilize os comandos `az dla job pipeline` para ver as informações de pipeline de tarefas previamente submetidas.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Utilize os comandos `az dla job recurrence` para ver as informações de recorrência para tarefas previamente submetidas.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```


## <a name="use-azure-resource-manager-groups"></a>Utilizar grupos do Azure Resource Manager
As aplicações são geralmente constituídas por diversos componentes, por exemplo, uma aplicação Web, base de dados, servidor de base de dados, armazenamento e serviços de terceiros. O Azure Resource Manager permite-lhe trabalhar com os recursos na sua aplicação como um grupo, referido como um grupo de recursos do Azure. Pode implementar, atualizar, monitorizar ou eliminar todos os recursos para a sua aplicação numa operação única e coordenada. Utiliza um modelo para a implementação e esse modelo pode funcionar para ambientes diferentes, como de teste e produção. Pode clarificar a faturação da sua organização ao visualizar os custos agregados do grupo inteiro. Para obter mais informações, veja [Descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 

Um serviço do Data Lake Analytics pode incluir os seguintes componentes:

* Conta do Azure Data Lake Analytics
* Conta de armazenamento do Azure Data Lake predefinida necessária
* Contas de armazenamento adicionais do Azure Data Lake
* Contas de armazenamento do Azure adicionais

Pode criar um grupo de Gestor de recursos para os tornar mais fácil de gerir todos estes componentes.

![Conta do Azure Data Lake Analytics e armazenamento](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Uma conta de Data Lake Analytics e as contas do storage dependente devem ser colocadas no mesmo centro de dados do Azure.
No entanto o grupo do Gestor de recursos pode estar localizado num centro de dados diferentes.  

## <a name="see-also"></a>Consulte também
* [Descrição geral do Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Introdução ao Data Lake Analytics com o portal do Azure](data-lake-analytics-get-started-portal.md)
* [Gerir o Azure Data Lake Analytics com o portal do Azure](data-lake-analytics-manage-use-portal.md)
* [Monitorizar e resolver problemas das tarefas de Azure Data Lake Analytics com o portal do Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

