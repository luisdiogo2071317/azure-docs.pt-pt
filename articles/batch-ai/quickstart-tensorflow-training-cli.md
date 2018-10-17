---
title: Início Rápido do Azure - Preparação de aprendizagem aprofundada - CLI do Azure | Microsoft Docs
description: Início Rápido - Aprenda rapidamente a preparar uma rede neural de aprendizagem aprofundada do TensorFlow num único GPU com o Batch AI através da CLI do Azure
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 09/03/2018
ms.author: danlep
ms.openlocfilehash: 99d864a5d519ce56a559bea4db7fe89a113e47b9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157927"
---
# <a name="quickstart-train-a-deep-learning-model-with-batch-ai"></a>Início Rápido: Preparar um modelo de aprendizagem aprofundada com o Batch AI

Este início rápido mostra como preparar um modelo de aprendizagem aprofundada de exemplo numa máquina virtual ativada para GPU gerida pelo Batch AI. O Batch AI é um serviço gerido que permite aos cientistas de dados e investigadores de IA preparar modelos de machine learning e IA em escala em clusters de máquinas virtuais do Azure. 

Neste exemplo, vai utilizar a CLI do Azure para configurar o Batch AI para preparar uma rede neural do [TensorFlow](https://www.tensorflow.org/) de exemplo na [base de dados MNIST](http://yann.lecun.com/exdb/mnist/) de dígitos escritos à mão. Depois de concluir este início rápido, irá compreender os principais conceitos de utilização do Batch AI para preparar um modelo de machine learning ou IA e estará pronto para preparar diferentes modelos em grande escala.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este início rápido requer a execução da versão 2.0.38 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

Este início rápido pressupõe que está a executar comandos numa shell de Bash no Cloud Shell ou no seu computador local. Se já tiver concluído o início rápido para [criar um cluster do Batch AI com a CLI do Azure](quickstart-create-cluster-cli.md), ignore os primeiros dois passos para criar um grupo de recursos e um cluster do Batch AI.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando `az group create`. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eualeste2*. Certifique-se de que escolhe a localização E.U.A. Leste 2 ou outra localização onde o serviço Batch AI está disponível. 

```azurecli-interactive 
az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-a-batch-ai-cluster"></a>Criar um cluster do Batch AI

Primeiro, utilize o comando `az batchai workspace create` para criar uma *área de trabalho* do Batch AI. Precisa de uma área de trabalho para organizar os clusters do Batch AI e outros recursos.

```azurecli-interactive
az batchai workspace create \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Para criar um cluster do Batch AI, utilize o comando `az batchai cluster create`. O exemplo seguinte cria um cluster de um nó com as seguintes propriedades:

* Utiliza o tamanho de VM NC6, que tem um GPU NVIDIA Tesla K80. O Azure disponibiliza vários tamanhos de VM com diferentes GPUs NVIDIA.
* Executa uma imagem do Ubuntu Server predefinida para alojar aplicações baseadas em contentores. Pode executar a maioria das cargas de trabalho de preparação nesta distribuição. 
* Adiciona uma conta de utilizador com o nome *myusername* e gera chaves SSH, caso ainda não existam na localização de chaves predefinida (*~/.ssh*) no seu ambiente local.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --vm-size Standard_NC6 \
    --target 1 \
    --user-name myusername \
    --generate-ssh-keys
```

A saída do comando mostra as propriedades do cluster. Demora alguns minutos a criar e iniciar o nó. Para ver o estado do cluster, execute o comando `az batchai cluster show`.

```azurecli-interactive
az batchai cluster show \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --output table
```

No início da criação do cluster, o resultado é semelhante ao seguinte, que mostra que o cluster está `resizing`:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```

Efetue os seguintes passos para carregar o script de preparação e criar a tarefa de preparação enquanto o estado do cluster muda. O cluster está pronto para executar a tarefa de preparação quando o estado for `steady` e o único nó estiver `Idle`.

## <a name="upload-training-script"></a>Carregar o script de preparação

Utilize o comando `az storage account create` para criar uma conta de armazenamento para armazenar o script e a saída da preparação.

```azurecli-interactive
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus2 \
    --sku Standard_LRS
```

Crie uma partilha de ficheiros do Azure chamada `myshare` na conta com o comando `az storage share create`:

```azurecli-interactive
az storage share create \
    --name myshare \
    --account-name mystorageaccount
```

Utilize o comando `az storage directory create` para criar diretórios na partilha de ficheiros do Azure. Crie o diretório `scripts` para o script de preparação e `logs` para a saída de preparação:

```azurecli-interactive
# Create /scripts directory in file share
az storage directory create \
    --name scripts \
    --share-name myshare \
    --account-name mystorageaccount

# Create /logs directory in file share 
az storage directory create \
    --name logs \
    --share-name myshare \
    --account-name mystorageaccount
```

Na shell de Bash, crie um diretório de trabalho local e transfira o exemplo [convolutional.py](https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py) do TensorFlow. O script Python prepara uma rede neural convolucional no conjunto de imagens MNIST de 60 000 dígitos escritos à mão, de 0 a 9. Em seguida, testa o modelo num conjunto de exemplos de teste.

```bash
wget https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py
```

Carregue o script para o diretório `scripts` na partilha com o comando `az storage file upload`.

```azurecli-interactive
az storage file upload \
    --share-name myshare \
    --path scripts \
    --source convolutional.py \
    --account-name mystorageaccount
```

## <a name="submit-training-job"></a>Submeter tarefa de preparação

Primeiro, crie uma *experimentação* do Batch AI na sua área de trabalho com o comando `az batchai experiment create`. Uma experimentação é um contentor lógico para tarefas relacionadas com o Batch AI.

```azurecli-interactive
az batchai experiment create \
    --name myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

No diretório de trabalho, crie um ficheiro de configuração da tarefa de preparação `job.json` com o seguinte conteúdo. Transmita este ficheiro de configuração quando submeter a tarefa de preparação.

Este ficheiro `job.json` inclui definições para localizar o ficheiro de script Python e executá-lo num contentor do TensorFlow no nó de GPU. Também especifica onde guardar os ficheiros de saída da tarefa no armazenamento do Azure. `<AZURE_BATCHAI_STORAGE_ACCOUNT>` indica que o nome da conta de armazenamento será especificado durante a submissão da tarefa.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 1,
        "tensorFlowSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/scripts/convolutional.py"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/logs",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Utilize o comando `az batchai job create` para submeter a tarefa no nó ao transmitir o ficheiro de configuração `job.json` e o nome da conta de armazenamento:

```azurecli-interactive
az batchai job create \
    --name myjob \
    --cluster mycluster \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --config-file job.json \
    --storage-account-name mystorageaccount
```

O comando é devolvido com as propriedades da tarefa e, em seguida, demora alguns minutos a concluir. Para monitorizar o progresso desta tarefa, utilize o comando `az batchai job file stream` para transmitir o ficheiro `stdout-wk-0.txt` a partir do diretório de saída padrão no nó. O script de preparação gera este ficheiro depois de a tarefa começar a ser executada.  

```azurecli-interactive
az batchai job file stream \
    --job myjob \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --file-name stdout-wk-0.txt
```

Exemplo de saída:

```
File found with URL "https://mystorageaccount.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/myResourceGroup/workspaces/myworkspace/experiments/myexperiment/jobs/myjob/<JOB_ID>/stdouterr/stdout-wk-0.txt?sv=2016-05-31&sr=f&sig=Kih9baozMao8Ugos%2FVG%2BcsVsSeY1O%2FTocCNvLQhwtx4%3D&se=2018-06-20T22%3A07%3A30Z&sp=rl". Start streaming
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Step 0 (epoch 0.00), 14.9 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 9.7 ms
Minibatch loss: 3.240, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.7%
Step 200 (epoch 0.23), 8.3 ms
Minibatch loss: 3.335, learning rate: 0.010000
Minibatch error: 7.8%
Validation error: 4.5%
Step 300 (epoch 0.35), 8.3 ms
Minibatch loss: 3.157, learning rate: 0.010000
Minibatch error: 3.1%
...
Step 8500 (epoch 9.89), 8.3 ms
Minibatch loss: 1.605, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.9%
Test error: 0.8%
```

A transmissão é interrompida quando a tarefa fica concluída. O script de exemplo prepara mais de 10 *épocas* ou transmite o conjunto de dados de preparação. Neste exemplo, depois de 10 épocas, o modelo preparado tem um erro de teste de apenas 0,8%.

## <a name="get-job-output"></a>Obter a saída da tarefa

O Batch AI cria uma estrutura de pastas exclusiva na conta de armazenamento para a saída de cada tarefa. Defina a variável de ambiente JOB_OUTPUT_PATH com este caminho. Em seguida, liste os ficheiros de saída no armazenamento com o comando `az storage file list`:

```azurecli-interactive
export JOB_OUTPUT_PATH=$(az batchai job show --name myjob --experiment myexperiment --workspace myworkspace --resource-group myResourceGroup --query jobOutputDirectoryPathSegment --output tsv)

az storage file list \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr \
    --output table
```

A saída é semelhante a:

```
Name               Content Length  Type    Last Modified
---------------  ----------------  ------  ---------------
execution.log               14866  file
stderr-wk-0.txt              1527  file
stdout-wk-0.txt             11027  file
```

Utilize o comando `az storage file download` para transferir um ou mais ficheiros para o diretório de trabalho local. Por exemplo:

```azurecli-interactive
az storage file download \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr/stdout-wk-0.txt
```

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser continuar com os tutoriais e exemplos do Batch AI, utilize a área de trabalho, o cluster e a conta de armazenamento do Batch AI criados neste início rápido. 

É cobrado o cluster do Batch AI enquanto os nós estiverem em execução. Se quiser manter a configuração do cluster quando não existirem tarefas em execução, redimensione o cluster para 0 nós. 

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 0
```

Mais tarde, redimensione-o para 1 ou mais nós para executar as suas tarefas. Quando já não precisar de um cluster, elimine-o com o comando `az batchai cluster delete`:

```azurecli-interactive
az batchai cluster delete \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Quando já não for necessário, pode utilizar o comando `az group delete` para remover o grupo de recursos dos recursos do Batch AI e de armazenamento. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes
Neste início rápido, aprendeu a utilizar o Batch AI para preparar um modelo de aprendizagem aprofundada do TensorFlow de exemplo numa VM de GPU com a CLI do Azure. Para saber mais sobre como distribuir a preparação de modelos num cluster de GPU maior, avance para o tutorial do Batch AI.

> [!div class="nextstepaction"]
> [Tutorial de preparação distribuída](./tutorial-horovod-tensorflow.md)