---
title: Início Rápido do Azure – Preparação CNTK com o Batch AI - CLI do Azure | Microsoft Docs
description: Aprenda rapidamente a executar uma tarefa de preparação CNTK com o Batch AI com a CLI do Azure
services: batch-ai
documentationcenter: na
author: AlexanderYukhanov
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 06/14/2018
ms.author: Alexander.Yukhanov
ms.openlocfilehash: eb00c1d4ec74b5268a1497b11087030ab6a86e5a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294078"
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Executar uma tarefa de preparação CNTK com a CLI do Azure

A CLI 2.0 do Azure permite-lhe criar e gerir recursos do Batch AI - criar/eliminar servidores de ficheiros e clusters do Batch AI e submeter/terminar/eliminar/monitorizar trabalhos de preparação.

Este início rápido mostra-lhe como criar um cluster de GPU e executar um trabalho de preparação com o Microsoft Cognitive Toolkit (CNTK).

O script de preparação [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) está disponível na página do GitHub do Batch AI. Este script prepara uma rede neural convolucional na base de dados MNIST de dígitos escritos à mão.

O exemplo do CNTK oficial foi modificado para aceitar a localização do conjunto de dados da preparação e a localização do diretório de saída através de argumentos da linha de comandos.

## <a name="quickstart-overview"></a>Descrição geral do início rápido

* Criar um cluster de GPU com um único nó (com o tamanho de VM `Standard_NC6`) e o nome `nc6`;
* Criar uma conta de armazenamento para armazenar a entrada e a saída do trabalho;
* Criar uma Partilha de Ficheiros do Azure com duas pastas, `logs` e `scripts`, para armazenar a saída da tarefa e os scripts de preparação;
* Criar um Contentor de Blobs do Azure, `data`, para armazenar os dados da preparação;
* Implementar o script de preparação e os dados de preparação na partilha de ficheiros e no contentor criados;
* Configurar o trabalho para montar a Partilha de Ficheiros do Azure e o Contentor de Blobs do Azure no nó do cluster e disponibilizá-los como sistema de ficheiros normais em `$AZ_BATCHAI_JOB_MOUNT_ROOT/logs`, `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts` e `$AZ_BATCHAI_JOB_MOUNT_ROOT/data`.
`AZ_BATCHAI_JOB_MOUNT_ROOT` é uma variável de ambiente que o Batch AI define para o trabalho.
* Monitorizar a execução do trabalho ao transmitir em fluxo a respetiva saída padrão;
* Após a conclusão do trabalho, inspecionar a respetiva saída e os modelos gerados;
* No final, eliminar todos os recursos alocados.

## <a name="prerequisites"></a>Pré-requisitos

* Subscrição do Azure - se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
* Acesso à CLI 2.0 do Azure com a versão 0.3 ou superior do módulo do batchai. Pode utilizar a CLI 2.0 do Azure que está disponível no [Azure Cloud Shell](../cloud-shell/overview.md) ou seguir [estas instruções](/cli/azure/install-azure-cli?view=azure-cli-latest) para instalá-la localmente.

  Se estiver a utilizar o Cloud Shell, altere o diretório de trabalho para `/usr/$USER/clouddrive`, porque o diretório raiz não tem espaço vazio:

  ```azurecli
  cd /usr/$USER/clouddrive
  ```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Os grupos de recursos do Azure são contentores lógicos para implementar e gerir os recursos do Azure. O comando seguinte cria um grupo de recursos novo, `batchai.quickstart`, na localização E.U.A. Leste:

```azurecli
az group create -n batchai.quickstart -l eastus
```
## <a name="create-batch-ai-workspace"></a>Criar área de trabalho do Batch AI

O comando seguinte cria uma área de trabalho do Batch AI no grupo de recursos. Uma área de trabalho do Batch AI é uma coleção de nível superior de todos os tipos de recursos do Batch AI:

```azurecli
az batchai workspace create -g batchai.quickstart -n quickstart
```

## <a name="create-gpu-cluster"></a>Criar cluster de GPU

O comando seguinte cria um cluster de GPU de nó único (o tamanho da VM é Standard_NC6) na área de trabalho, ao utilizar a Máquina Virtual de Ciência de Dados (DSVM) de Ubuntu como a imagem do sistema operativo:

```azurecli
az batchai cluster create -n nc6 -g batchai.quickstart -w quickstart -s Standard_NC6 -i UbuntuDSVM -t 1 --generate-ssh-keys
```

A DSVM do Ubuntu permite-lhe executar qualquer trabalho de preparação nos contentores do Docker e executar as estruturas de aprendizagem profunda mais populares diretamente na VM.

A opção `--generate-ssh-keys` diz à CLI do Azure para gerar chaves ssh públicas e privadas, se ainda não as tiver. Para aceder aos nós do cluster, pode utilizar o nome de utilizador atual e a chave ssh gerada.

Se utilizar o Cloud Shell, é recomendável criar uma cópia de segurança da pasta ~/.ssh num armazenamento permanente.

Exemplo de saída:
```json
{
  "allocationState": "steady",
  "allocationStateTransitionTime": "2018-04-11T21:17:26.345000+00:00",
  "creationTime": "2018-04-11T20:12:10.758000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/clusters/nc6",
  "location": "eastus",
  "name": "nc6",
  "nodeSetup": null,
  "nodeStateCounts": {
    "additionalProperties": {},
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T20:12:11.445000+00:00",
  "resourceGroup": "batchai.quickstart",
  "scaleSettings": {
    "additionalProperties": {},
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": null,
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "additionalProperties": {},
    "adminUserName": "myuser",
    "adminUserPassword": null,
    "adminUserSshPublicKey": "<YOUR SSH PUBLIC KEY HERE>"
  },
  "virtualMachineConfiguration": {
    "additionalProperties": {},
    "imageReference": {
      "additionalProperties": {},
      "offer": "linux-data-science-vm-ubuntu",
      "publisher": "microsoft-ads",
      "sku": "linuxdsvmubuntu",
      "version": "latest",
      "virtualMachineImageId": null
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
}
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

O comando seguinte cria uma conta de armazenamento no mesmo grupo de recursos que serve para criar o cluster do Batch AI. Utilize a conta de armazenamento para armazenar a entrada e a saída da tarefa. Atualize o comando com um nome de conta de armazenamento exclusivo.

```azurecli
az storage account create -n <storage account name> --sku Standard_LRS -g batchai.quickstart
```


## <a name="deploy-data"></a>Implementar dados

### <a name="download-the-training-script-and-training-data"></a>Transferir o script de preparação e os dados de preparação

* Transfira e extraia uma base de dados MNIST pré-processada a partir [desta localização](https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D) para a pasta atual.

Para GNU/Linux ou Cloud Shell:

```azurecli
wget "https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D" -O mnist_dataset.zip
unzip mnist_dataset.zip
```

Tenha em atenção que poderá ter de instalar `unzip` se a sua distribuição de GNU/Linux não o tiver.

* Transfira o script de exemplo [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) para a pasta atual:

Para GNU/Linux ou Cloud Shell:

```azurecli
wget https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py
```

### <a name="create-azure-file-share-and-deploy-the-training-script"></a>Criar a partilha de ficheiros do Azure e implementar o script de preparação

Os seguintes comandos criam as Partilhas de ficheiros do Azure `scripts` e `logs` e copiam o script de preparação para a pasta `cntk` na partilha `scripts`:

```azurecli
az storage share create -n scripts --account-name <storage account name>
az storage share create -n logs --account-name <storage account name>
az storage directory create -n cntk -s scripts --account-name <storage account name>
az storage file upload -s scripts --source ConvNet_MNIST.py --path cntk --account-name <storage account name> 
```

### <a name="create-a-blob-container-and-deploy-training-data"></a>Criar um contentor de blobs e implementar os dados de preparação

Os seguintes comandos criam o um contentor de blobs do Azure denominado `data` e copiam os dados de preparação para a pasta `mnist_cntk`:

```azurecli
az storage container create -n data --account-name <storage account name>
az storage blob upload-batch -s . --pattern '*28x28_cntk*' --destination data --destination-path mnist_cntk --account-name <storage account name>
```

## <a name="submit-training-job"></a>Submeter tarefa de preparação

### <a name="create-a-batch-ai-experiment"></a>Criar uma experimentação do Batch AI

Uma experimentação é um contentor lógico para tarefas relacionadas com o Batch AI. Utilize o seguinte comando para criar uma experimentação na sua área de trabalho:

```azurecli
az batchai experiment create -g batchai.quickstart -w quickstart -n quickstart
```

### <a name="prepare-job-configuration-file"></a>Preparar o ficheiro de configuração da tarefa

Crie o ficheiro de configuração da tarefa de preparação `job.json` com o seguinte conteúdo. Atualize pelo nome da sua conta de armazenamento.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-03-01/cntk.json",
    "properties": {
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs"
        }],
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<YOUR_STORAGE_ACCOUNT>.file.core.windows.net/logs",
                    "relativeMountPath": "logs"
                },
                {
                    "azureFileUrl": "https://<YOUR_STORAGE_ACCOUNT>.file.core.windows.net/scripts",
                    "relativeMountPath": "scripts"
                }
            ],
            "azureBlobFileSystems": [
                {
                    "accountName": "<YOUR_STORAGE_ACCOUNT>",
                    "containerName": "data",
                    "relativeMountPath": "data"
                }
            ]
        }
    }
}
```

Este ficheiro de configuração especifica:

* `nodeCount` - número de nós exigidos pela tarefa (1 neste início rápido)
* `cntkSettings` - especifica o caminho do script de preparação e os argumentos da linha de comandos. Os argumentos da linha de comandos incluem o caminho para os dados de preparação e o caminho de destino para armazenar os modelos gerados. `AZ_BATCHAI_OUTPUT_MODEL` é uma variável de ambiente que o Batch AI define com base na configuração do diretório de saída (ver abaixo)
* `stdOutErrPathPrefix` - o caminho no qual o Batch AI cria diretórios que contêm a saída e os registos do trabalho
* `outputDirectories` - a coleção de diretórios de saída que o Batch AI vai criar. Para cada diretório, o Batch AI cria uma variável de ambiente com o nome `AZ_BATCHAI_OUTPUT_<id>`, em que `<id>` é o identificador do diretório
* `mountVolumes` - uma lista de sistemas de ficheiros que vão ser montados durante a execução do trabalho. Os sistemas de ficheiros são montados em `AZ_BATCHAI_JOB_MOUNT_ROOT/<relativeMountPath>`. `AZ_BATCHAI_JOB_MOUNT_ROOT` é uma variável de ambiente que o Batch AI define
* `<AZURE_BATCHAI_STORAGE_ACCOUNT>` - o nome da conta de armazenamento especificado durante a submissão da tarefa através da variável de ambiente `--storage-account-name parameter` ou `AZURE_BATCHAI_STORAGE_ACCOUNT` no seu computador.

### <a name="submit-the-job"></a>Submeter o Trabalho

```azurecli
az batchai job create -n cntk_python_1 -c nc6 -g batchai.quickstart -w quickstart -e quickstart  -f job.json --storage-account-name <storage account name>
```

Exemplo de saída:
```
{
  "caffe2Settings": null,
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/clusters/nc6",
    "resourceGroup": "batchai.quickstart"
  },
  "cntkSettings": {
    "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py"
  },
  "constraints": {
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": null,
  "creationTime": "2018-06-14T22:22:57.543000+00:00",
  "customMpiSettings": null,
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": {
    "endTime": null,
    "errors": null,
    "exitCode": null,
    "startTime": "2018-06-14T22:22:59.838000+00:00"
  },
  "executionState": "running",
  "executionStateTransitionTime": "2018-06-14T22:22:59.838000+00:00",
  "horovodSettings": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1",
  "inputDirectories": null,
  "jobOutputDirectoryPathSegment": "00000000-0000-0000-0000-000000000000/batchai.quickstart/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1/f2d6ff09-7549-4e1a-8cd8-ec839f042a61",
  "jobPreparation": null,
  "mountVolumes": {
    "azureBlobFileSystems": [
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "containerName": "data",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "mountOptions": null,
        "relativeMountPath": "data"
      }
    ],
    "azureFileShares": [
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "azureFileUrl": "https://<YOUR STORAGE ACCOUNT NAME>.file.core.windows.net/logs",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "logs"
      },
      {
        "accountName": "<YOUR STORAGE ACCOUNT NAME>",
        "azureFileUrl": "https://<YOUR STORAGE ACCOUNT NAME>.file.core.windows.net/scripts",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "scripts"
      }
    ],
    "fileServers": null,
    "unmanagedFileSystems": null
  },
  "name": "cntk_python_1",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
      "pathSuffix": null
    }
  ],
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-06-14T22:22:58.625000+00:00",
  "pyTorchSettings": null,
  "resourceGroup": "danlep0614b",
  "schedulingPriority": "normal",
  "secrets": null,
  "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
  "tensorFlowSettings": null,
  "toolType": "cntk",
  "type": "Microsoft.BatchAI/workspaces/experiments/jobs"
}

```

## <a name="monitor-job-execution"></a>Monitorizar a execução da tarefa

O script de preparação comunica o progresso da preparação no ficheiro `stderr.txt` no diretório de saída standard. Utilize o seguinte comando para monitorizar o progresso:

```azurecli
az batchai job file stream -j cntk_python_1 -g batchai.quickstart -w quickstart -e quickstart -f stderr.txt
```

Exemplo de saída:
```
File found with URL "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>/stdouterr/stderr.txt?sv=2016-05-31&sr=f&sig=n86JK9YowV%2BPQ%2BkBzmqr0eud%2FlpRB%2FVu%2FFlcKZx192k%3D&se=2018-04-11T23%3A05%3A54Z&sp=rl". Start streaming
Selected GPU[0] Tesla K80 as the process wide default device.
-------------------------------------------------------------------
Build info:

        Built time: Jan 31 2018 15:03:41
        Last modified date: Tue Jan 30 03:26:13 2018
        Build type: release
        Build target: GPU
        With 1bit-SGD: no
        With ASGD: yes
        Math lib: mkl
        CUDA version: 9.0.0
        CUDNN version: 7.0.4
        Build Branch: HEAD
        Build SHA1: a70455c7abe76596853f8e6a77a4d6de1e3ba76e
        MPI distribution: Open MPI
        MPI version: 1.10.7
-------------------------------------------------------------------
Training 98778 parameters in 10 parameter tensors.

Learning rate per 1 samples: 0.001
Momentum per 1 samples: 0.0
Finished Epoch[1 of 40]: [Training] loss = 0.405960 * 60000, metric = 13.01% * 60000 21.741s (2759.8 samples/s);
Finished Epoch[2 of 40]: [Training] loss = 0.106030 * 60000, metric = 3.09% * 60000 3.638s (16492.6 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.078542 * 60000, metric = 2.32% * 60000 3.477s (17256.3 samples/s);
...
Final Results: Minibatch[1-11]: errs = 0.62% * 10000
```

A transmissão em fluxo é interrompida quando o trabalho é concluído (com êxito ou falha).

## <a name="inspect-generated-model-files"></a>Inspecionar os ficheiros do modelo gerados

A tarefa armazena os ficheiros de modelo gerado no diretório de saída com o atributo `id` igual a `MODEL`. Liste ficheiros de modelo e obtenha URLs de transferência com o seguinte comando:

```azurecli
az batchai job file list -j cntk_python_1 -w quickstart -e quickstart -g batchai.quickstart -d MODEL
```

Exemplo de saída:
```
[
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:51+00:00",
    "name": "ConvNet_MNIST_0.dnn"
  },
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:55+00:00",
    "name": "ConvNet_MNIST_1.dnn"
  },
...

```

Em alternativa, utilize o portal do Azure ou o Explorador de Armazenamento do Azure para inspecionar os ficheiros gerados. Para distinguir o resultado dos diferentes trabalhos, o Batch AI cria uma estrutura de pastas exclusiva para cada um. Utilize o atributo `jobOutputDirectoryPathSegment` da tarefa submetida para localizar o caminho para a pasta que contém a saída:

```azurecli
az batchai job show -n cntk_python_1 -g batchai.quickstart -w quickstart -e quickstart --query jobOutputDirectoryPathSegment
```

Exemplo de saída:
```
"00000000-0000-0000-0000-000000000000/batchai.quickstart/workspaces/quickstart/experiments/quickstart/jobs/cntk_python_1/<JOB's UUID>"
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for preciso, elimine o grupo de recursos e todos os recursos alocados com o seguinte comando:

```azurecli
az group delete -n batchai.quickstart -y
```
