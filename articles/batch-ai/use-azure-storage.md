---
title: Armazenar o Batch AI da tarefa entrada e saída com o armazenamento do Azure | Documentos da Microsoft
description: Como utilizar o armazenamento do Azure com o Batch AI para armazenamento de cloud rápida e fácil de ficheiros de entrada e saídos
services: batch-ai
documentationcenter: ''
author: kevwang1
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 1e9a4c6355c60b18bb78aae362c1e2f142e2d864
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408005"
---
# <a name="store-batch-ai-job-input-and-output-with-azure-storage"></a>Store entrada da tarefa de IA do Batch e de saída com o armazenamento do Azure

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Este guia descreve como utilizar o armazenamento do Azure para armazenar ficheiros de entrada e saídos, ao executar uma tarefa. Armazenamento do Azure é uma das várias opções de armazenamento suportadas pelo Batch AI. O batch AI integra-se com o armazenamento do Azure ao montar a sistemas de armazenamento do Azure para um Batch AI tarefa ou cluster de sistema de ficheiros, permitindo o acesso totalmente integrado aos ficheiros armazenados na cloud. 

## <a name="introduction-to-azure-storage"></a>Introdução ao Armazenamento do Azure

O Armazenamento do Azure é a solução de armazenamento da cloud da Microsoft. Suporta o batch AI montar partilhas de ficheiros do Azure para tarefas de IA do Batch ou clusters e contentores de Blobs do Azure, permitindo que os ficheiros para ser acedido a partir de uma tarefa como se estivessem no sistema de ficheiros nativo. O batch AI monta contentores de Blobs do Azure com [blobfuse](https://github.com/Azure/azure-storage-fuse)e partilhas através do protocolo SMB de ficheiros do Azure. Para obter mais informações sobre o armazenamento do Azure, consulte [introdução ao armazenamento do Azure](../storage/common/storage-introduction.md).

## <a name="store-datasets-and-input-scripts-in-azure-storage"></a>Store conjuntos de dados e scripts de entrada no armazenamento do Azure

Ao escolher o armazenamento do Azure para o seu ambiente do Batch AI, recomendamos que armazene os ficheiros de entrada (por exemplo, os conjuntos de dados) num contentor de BLOBs, que tem um débito mais elevado, e armazenar a saída de treinamento na partilha de ficheiros, que suporta a transmissão em fluxo (permitindo a leitura dos registos de saída, enquanto a tarefa está em execução em simultâneo). 

Antes de poder utilizar o armazenamento do Azure, tem [criar uma conta de armazenamento do Azure](../storage/common/storage-quickstart-create-account.md). O batch AI suporta volumes de montagem de ambos os para fins gerais v1 (GPv1) e para fins gerais v2 contas de armazenamento do Azure (GPv2). A conta de armazenamento do Azure pode conter vários contentores de BLOBs ou instâncias de partilha de ficheiros. Considere os requisitos de desempenho e custo ao escolher o tipo de conta de armazenamento para criar. Para mais informações, veja [Visão geral de conta de armazenamento do Azure](../storage/common/storage-account-overview.md). 

Para criar um contentor de BLOBs e para carregar o conjunto de dados para um contentor de Blobs do Azure, escolha um dos seguintes métodos:
- [Portal do Azure](../storage/blobs/storage-quickstart-blobs-portal.md) para carregar com uma GUI baseada na web. Para carregar um pequeno número de ficheiros, o portal do Azure fornece a operação mais simples.
- [CLI de armazenamento do Azure](../storage/blobs/storage-quickstart-blobs-cli.md) para carregar através da linha de comandos (carregamento de diretório suporta). Para carregar diretórios de ficheiros, utilize `az storage blob upload-batch`.
- [outras técnicas](../storage/common/storage-moving-data.md), incluindo a utilização de SDKs do application.

Da mesma forma, para criar uma partilha de ficheiros do Azure, escolha um dos seguintes métodos:
- [Portal do Azure](../storage/files/storage-how-to-use-files-portal.md)
- [Armazenamento do Azure CLI](../storage/files/storage-how-to-use-files-cli.md)
- [outras técnicas](../storage/common/storage-moving-data.md)

### <a name="auto-storage-with-batch-ai"></a>Armazenamento automático com o Batch AI

Em alternativa, pode criar uma conta de armazenamento do Azure com uma partilha de ficheiros do Azure e o contentor de BLOBs (e montar automaticamente estes volumes para um cluster do Batch AI) ao utilizar o `--use-auto-storage` parâmetro com `az batchai cluster create`. Para obter mais informações, consulte [aqui](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#auto-storage-account).

## <a name="mount-azure-storage"></a>Armazenamento do Azure de montagem 

### <a name="mount-volumes-to-a-job"></a>Volumes de montagem para uma tarefa

Montar um volume de armazenamento do Azure permite que ela seja acessada por meio do sistema de ficheiros criado para cada tarefa. Por conseguinte, uma tarefa pode ler e escrever ficheiros de forma totalmente integrada para armazenamento na nuvem como se estivessem ficheiros locais.

Para montar um volume de armazenamento do Azure para uma tarefa criada com a CLI do Azure, utilize o `mountVolumes` propriedade em seu `job.json` durante a execução de ficheiros `az batchai job create`. Por exemplo, veja a [receita de distribuídas do Tensorflow GPU](https://github.com/Azure/BatchAI/blob/master/recipes/TensorFlow/TensorFlow-GPU-Distributed/job.json). O esquema para `mountVolumes` é:
```json
{
    "mountVolumes": {
        "azureFileShares": [{
            "azureFileUrl": "https://<STORAGE_ACCOUNT_NAME>.file.core.windows.net/<FILE_SHARE_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }],
        "azureBlobFileSystems": [{
            "accountName": "<STORAGE_ACCOUNT_NAME>",
            "containerName": "<BLOB_CONTAINER_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }]
    }
}
```

`azureFileShares` e `azureBlobFileSystems` são as duas matrizes de objetos que representam os volumes de montagem. Descrições dos marcadores de posição:

- < RELATIVE_MOUNT_PATH > - o volume será montado nesse caminho. Por exemplo, se `relativeMountPath` é `jobs`, o volume ficará localizado em `$AZ_BATCHAI_JOB_MOUNT_ROOT/jobs`)
- < STORAGE_ACCOUNT_NAME > - o nome da conta de armazenamento do Azure que contém a partilha de ficheiros ou o contentor de BLOBs
- < FILE_SHARE_NAME > - o nome da partilha de ficheiros
- < BLOB_CONTAINER_NAME > - o nome do contentor de BLOBs

Para montar os volumes de armazenamento do Azure com os SDKs do Azure Batch AI, defina o `mount_volumes` (Python) ou `MountVolumes` (c#, Java) propriedade no `JobCreateParameters`. Tem de fornecer credenciais da conta de armazenamento ao montar volumes com os SDKs do Azure Batch AI. Ver os esquemas de volumes de montagem [Python](https://docs.microsoft.com/python/api/azure-mgmt-batchai/azure.mgmt.batchai.models.MountVolumes?view=azure-python), [c#](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.batchai.models.mountvolumes?view=azure-dotnet), e [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.management.batchai._mount_volumes?view=azure-java-stable).

### <a name="mount-volumes-to-a-cluster"></a>Volumes de montagem para um cluster

O batch AI também suporta a montagem de volumes de armazenamento do Azure para um cluster do Batch AI. Quando um volume está montado para um cluster, todas as tarefas em execução nesse cluster podem utilizar volumes montados para esse cluster. Embora a montagem de nível de tarefa fornece a maior flexibilidade (permitindo que cada tarefa ter diferentes volumes montados), a montagem de nível de cluster pode ser suficiente em cenários simples.

Para montar um volume de armazenamento do Azure para um cluster que criou com a CLI do Azure, utilize o `mountVolumes` propriedade em seu `cluster.json` durante a execução de ficheiros `az batchai cluster create`. O esquema para `mountVolumes` quando montar um cluster é igual ao montar a uma tarefa. 

Da mesma forma, pode utilizar o `mount_volumes` (Python) ou `MountVolumes` (c#, Java) propriedade no `ClusterCreateParameters` ao montar com os SDKs do Azure Batch AI. 

## <a name="access-mounted-filesystem-in-a-job"></a>Sistema de ficheiros de acesso montado numa tarefa

### <a name="azbatchaijobmountroot-environment-variable"></a>Variável de ambiente de AZ_BATCHAI_JOB_MOUNT_ROOT $

Dentro do ambiente de execução da tarefa, o diretório que contém os sistemas de armazenamento montado pode ser acessado com o `$AZ_BATCHAI_JOB_MOUNT_ROOT` variável de ambiente (se usar a montagem de ao nível da tarefa). Se usar a montagem de nível de cluster, esta variável de ambiente é `$AZ_BATCHAI_MOUNT_ROOT`. Os exemplos seguintes partem do princípio de que utiliza a montagem de ao nível da tarefa.

Para fornecer o caminho de dados num volume montado, utilize a variável de ambiente `$AZ_BATCHAI_JOB_MOUNT_ROOT` juntamente com o caminho montado. Por exemplo, se o script de formação `train.py` foi carregado para ficheiros do Azure partilha montada no caminho de montagem relativo `scripts`, o arquivo estará disponível em `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py`.

Se o seu script de treinamento requer conhecimento de um caminho, deve passá-lo como um argumento da linha de comandos. Por exemplo, se tiver armazenado os dados numa pasta chamada `train_data` no contentor de Blobs do Azure montada no caminho `data`, poderia passar `--data-dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/data/train_data` como um argumento da linha de comandos ao seu script. Da mesma forma, tem de modificar o script para aceitar os argumentos da linha de comandos.

### <a name="abbreviate-input-paths"></a>Abreviar os caminhos de entrada

Para abreviar os caminhos de entrada como uma variável de ambiente, utilize o `inputDirectories` propriedade de seu `job.json` ficheiro (ou `models.JobCreateParameters.input_directories` se utilizar o SDK de IA do Batch). O esquema de `inputDirectories` é:

```json
{
    "inputDirectories": [{
        "id": "<ID>",
        "path": "<PATH>" 
    }]
}
```

Cada caminho especificado será colocado numa variável de ambiente chamada `$AZ_BATCHAI_INPUT_<ID>`. Através deste método podem simplificar os caminhos de arquivos ou diretórios de entrada. Por exemplo, para abreviar o caminho para um script de preparação: se `"id"` é `"SCRIPT"` e `"path"` é `"$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py"`, em seguida, o que o caminho está disponível em `$AZ_BATCHAI_INPUT_SCRIPT` num ambiente de execução da tarefa.

Para obter mais informações, consulte [aqui](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#input-directories).

### <a name="abbreviate-output-paths"></a>Abreviar os caminhos de saída

Para abreviar os caminhos de saída como uma variável de ambiente, utilize o `outputDirectories` propriedade de seu `job.json` ficheiro (ou `models.JobCreateParameters.output_directories` se utilizar o SDK de IA do Batch). Através deste método podem simplificar os caminhos para ficheiros de saída. O esquema de `outputDirectories` é:

```json
{
    "outputDirectories": [{
        "id": "<ID>",
        "pathPrefix": "<PATH_PREFIX>",
        "pathSuffix": "<PATH_SUFFIX>"
    }]
}
```

Cada caminho especificado será colocado numa variável de ambiente chamada `$AZ_BATCHAI_OUTPUT_<ID>`. O `pathPrefix` determina o volume montado para armazenar a saída (por exemplo, `"$AZ_BATCHAI_JOB_MOUNT_ROOT/output"`). O `pathSuffix` determina o nome da pasta de saída (por exemplo, `"logs"`, `"checkpoints"`). O caminho real da saída é uma concatenação das `pathPrefix`, `jobOutputDirectoryPathSegment` (gerado automaticamente para cada trabalho) e `pathSuffix`.

Para obter mais informações, consulte [aqui](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#output-directories).

## <a name="retrieve-job-output-from-azure-storage"></a>Obter Resultado da tarefa do armazenamento do Azure

### <a name="use-azure-portal"></a>Utilizar o portal do Azure

O portal do Azure é um meio cómodo para ver a saída de tarefas, utilizando um Explorador de ficheiros de GUI. No entanto, se pretender ver o resultado de Stdout e Stderr ou de um caminho no `outputDirectories`, os ficheiros são colocados num caminho de geradas automaticamente no seu volume de armazenamento do Azure. Veja a tabela abaixo para obter mais informações.

### <a name="access-stdout-and-stderr-output"></a>Acesso Stdout e Stderr

Utilize o `stdOutErrPathPrefix` propriedade do `job.json` para informar o trabalho onde pretende colocar os registos de execução da tarefa e a saída de Stdout e Stderr. Por exemplo, se montar uma partilha de ficheiros no caminho de montagem relativo `outputs`, e especificar o `stdOutErrPathPrefix` ser `"$AZ_BATCHAI_JOB_MOUNT_ROOT/outputs"`, em seguida, o resultado da tarefa Stdout e Stderr estará disponível em `{subscription id}/{resource group}/workspaces/{workspace name}/experiments/{experiment name}/jobs/{job name}/{job uuid}/stdouterr` que volume montado. Este caminho gerado automaticamente é utilizado para evitar colisões de saída entre tarefas o mesmo nome.

Para obter mais informações, consulte [aqui](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#standard-and-error-output).

### <a name="list-the-output-files"></a>Listar os ficheiros de saída

Pode utilizar a CLI do Azure para listar os ficheiros de saída disponíveis de uma tarefa com o `az batchai job file list` comando. Por exemplo, para listar os ficheiros no diretório de saída padrão de uma tarefa, utilize `az batchai job file list -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME>`.

Para obter mais informações e exemplos, consulte [aqui](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

### <a name="stream-output-files"></a>Ficheiros de saída do Stream

Pode utilizar a CLI do Azure para ficheiros de fluxo com o `az batchai job file stream` comando. Por exemplo, para ver:
- STDOUT: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stdout.txt`
- Stderr: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stderr.txt`

Para obter mais informações e exemplos, consulte [aqui](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

## <a name="next-steps"></a>Passos Seguintes
- Para saber mais sobre os comandos da CLI para interagir com o armazenamento do Azure, veja a [documentação da CLI do Azure Batch AI](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md).
- Para obter mais exemplos de utilização do Batch AI, incluindo o armazenamento de montar e ler ficheiros de saída, consulte a [receitas de bloco de notas do Jupyter para o Batch AI](https://github.com/Azure/BatchAI).
- Explorar outras opções de montagem do armazenamento, incluindo [montar um servidor NFS](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs) e [montar seu próprio cluster NFS, cifs ou GlusterFS](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs)