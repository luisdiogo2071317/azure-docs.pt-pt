---
title: Início Rápido do Azure - Criar um cluster do Batch AI - CLI do Azure | Microsoft Docs
description: Início Rápido - Criar um cluster do Batch AI preparar modelos de machine learning e IA - CLI do Azure
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
ms.openlocfilehash: 0d4ba7edfb22a6710222c854ceb2bf86284d2d77
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057803"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-cli"></a>Início Rápido: Criar um cluster para tarefas de preparação do Batch AI com a CLI do Azure

Este início rápido mostra como utilizar a CLI do Azure para criar um cluster do Batch AI que pode utilizar para preparar modelos de machine learning e IA. O Batch AI é um serviço gerido que permite aos cientistas de dados e investigadores de IA preparar modelos de machine learning e IA em escala em clusters de máquinas virtuais do Azure.

Inicialmente, o cluster tem um único nó de GPU. Depois de concluir este início rápido, terá um cluster que pode aumentar verticalmente e utilizar para preparar os seus modelos. Submeta tarefas de preparação para o cluster com o Batch AI, ferramentas do [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) ou o [Visual Studio Tools para IA](https://github.com/Microsoft/vs-tools-for-ai).

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este início rápido requer a execução da versão 2.0.38 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

Este início rápido pressupõe que está a executar comandos numa shell de Bash no Cloud Shell ou no seu computador local.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando `az group create`. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eualeste2*. Certifique-se de que escolhe uma localização como E.U.A. Leste 2 em que o serviço Batch AI esteja disponível.

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

Para criar um cluster do Batch AI, utilize o comando `az batchai cluster create`. O exemplo seguinte cria um cluster com as seguintes propriedades:

* Contém um único nó no tamanho de VM NC6, que tem um GPU NVIDIA Tesla K80. 
* Executa uma imagem do Ubuntu Server predefinida criada para alojar aplicações baseadas em contentores, que pode utilizar para a maioria das cargas de trabalho de preparação. 
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

No início da criação do cluster, o resultado é semelhante ao seguinte, que mostra que o cluster está no estado `resizing`:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```
O cluster está pronto a utilizar quando o estado for `steady` e o único nó estiver `Idle`.

## <a name="list-cluster-nodes"></a>Listar nós de cluster 

Se precisar de ligar aos nós de cluster (neste caso, um único nó) para instalar aplicações ou efetuar manutenção, obtenha as informações de ligação ao executar o comando `az batchai cluster node list`:


```azurecli-interactive
az batchai cluster node list \
    --cluster mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup 
 ```

O resultado JSON é semelhante a:

```JSON
[
  {
    "ipAddress": "40.68.254.143",
    "nodeId": "tvm-1816144089_1-20180626t233430z",
    "port": 50000.0
  }
]
```
Utilize estas informações para fazer uma ligação SSH ao nó. Por exemplo, substitua o endereço IP correto do nó no comando seguinte:

```bash
ssh myusername@40.68.254.143 -p 50000
``` 
Saia da sessão SSH para continuar.

## <a name="resize-the-cluster"></a>Redimensionar o cluster

Quando utilizar o seu cluster para executar uma tarefa de preparação, poderá precisar de mais recursos de computação. Por exemplo, para aumentar o tamanho para 2 nós para uma tarefa de preparação distribuída, execute o comando [batch ai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize):

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 2
```

Demora alguns minutos a redimensionar o cluster.

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser continuar com os tutoriais e exemplos do Batch AI, utilize a área de trabalho do Batch AI que foi criada neste início rápido. 

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
    --resource-group myResourceGroup \
```

Quando já não for necessário, pode utilizar o comando `az group delete` para remover o grupo de recursos dos recursos do Batch AI. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a criar um cluster do Batch AI com a CLI do Azure. Para saber mais sobre como utilizar um cluster do Batch AI para preparar um modelo, avance para o início rápido para preparar um modelo de aprendizagem aprofundada.

> [!div class="nextstepaction"]
> [Preparar um modelo de aprendizagem aprofundada](./quickstart-tensorflow-training-cli.md)
