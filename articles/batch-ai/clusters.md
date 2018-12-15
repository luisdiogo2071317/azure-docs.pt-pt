---
title: Trabalhar com clusters do Azure Batch AI | Documentos da Microsoft
description: Como escolher uma configuração de cluster do Batch AI e criar e gerir um cluster de IA
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 9af8ce84805e48dd3c91dd7fb4fcf0b136fbfc60
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410096"
---
# <a name="work-with-batch-ai-clusters"></a>Trabalhar com clusters de IA do Batch 

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Este artigo explica como trabalhar com clusters no Azure Batch AI. Ele introduz o conceito de clusters, os tipos de configurações que são possíveis e exemplos. A maioria dos exemplos para criar e gerir um cluster neste artigo usa a CLI do Azure. No entanto, pode utilizar outras ferramentas, incluindo o portal do Azure e os SDKs do Azure Batch AI para trabalhar com clusters.

Um cluster do Batch AI é um dos vários recursos no serviço. Consulte a [descrição geral dos recursos do Batch AI](resource-concepts.md) para compreender o escopo de clusters no serviço.

## <a name="introduction-to-clusters"></a>Introdução aos clusters

Um cluster do Batch AI contém os recursos de computação para a execução de machine learning e tarefas de formação em IA. Todos os nós num cluster têm o mesmo tamanho VM e a imagem do SO. O batch AI oferece muitas opções para criar clusters que são personalizadas para necessidades diferentes. Normalmente, configura um cluster diferente para cada categoria de poder necessário para concluir um projeto de processamento. Pode dimensionar o número de nós num cluster verticalmente com base na procura e orçamento. Clusters podem ser aprovisionados e compartilhados entre uma equipe ou indivíduos podem aprovisionar cada seu próprio cluster. 

Cada cluster existe num recurso do Batch AI chamado um *área de trabalho*. Antes do aprovisionamento qualquer cluster, tem de ter uma área de trabalho do Batch AI configurar. Por exemplo, se utilizar a CLI do Azure, utilize o [criar área de trabalho do az batchai](/cli/azure/batchai/workspace?view=azure-cli-latest#az-batchai-workspace-create) comando para configurar uma área de trabalho. 

Depois de criar um cluster, pode submeter tarefas um por vez numa fila. O batch AI trata, em seguida, o processo de alocação de recursos para as tarefas em execução no cluster. 

## <a name="cluster-configuration-options"></a>Opções de configuração de cluster

Ao planejar um cluster, primeiro determine os requisitos de computação. O batch AI oferece opções de configuração flexíveis para que pode adaptar um cluster às suas necessidades. Seguem-se as opções de majors a considerar ao aprovisionar um cluster:

* **Tamanho da VM** -escolha qualquer [tamanho VM](../virtual-machines/linux/sizes.md) que está disponível numa [suportada região](https://azure.microsoft.com/global-infrastructure/services/) para os nós de cluster. Cada cluster só pode conter um tamanho de VM, portanto, se as suas tarefas necessitam de vários tipos de VMs, precisa de aprovisionar um cluster separado para cada tipo de requisito de computação. Para preparar o machine learning ou modelos de IA desenvolvidos com estruturas que tiram partido das GPUs, consulte a [tamanhos de VM com otimização de GPU](../virtual-machines/linux/sizes-gpu.md) no Azure.
  
* **Prioridade VM** -Batch AI oferece dedicadas ou com baixa prioridade VMs para um cluster. VMs dedicadas estão reservadas para utilização no cluster. A opção de baixa prioridade aloca capacidade não utilizada de VM do Azure em economias significativas em troca a possibilidade de tarefas que está a ser suplantadas se Azure recupera as VMs. Tarefas que são executadas durante mais de 24 horas numa VM de baixa prioridade também forem suplantadas automaticamente. Se o orçamento é uma preocupação, considere a utilização de VMs de baixa prioridade para as tarefas de experimentação curto. Em seguida, mude para VMs dedicadas, quando está na altura de executar tarefas de mais tempo.

* **Número de nós** -Batch AI oferece opções de manual e o dimensionamento automático para o número de nós do cluster. Com a opção manual, controla a um cluster ou reduza verticalmente, para que possa gerir os seus custos de computação. A opção de dimensionamento automático garante que o cluster downscales sempre quando não estiver a utilizar, portanto, estará a minimizar os custos de computação. 

  Se optar por dimensionar o cluster manualmente, em seguida, define o destino inicial durante a criação do cluster. O destino é o número de nós que o Batch AI atribui inicialmente. Mais tarde, pode redimensionar manualmente o número de nós.  

  Se escolher a opção de dimensionamento automático, define o número máximo e mínimo de nós de destino durante a criação do cluster. O destino pode variar entre 0 e o número máximo de nós suportado pelo seu [quota de núcleos do Batch AI](quota-limits.md). O destino de 0 permite que mantenha a configuração do cluster sem a ser cobrado por quaisquer custos de computação. Se solicitar um destino maior do que o suporta de limite de quota, em seguida, o aprovisionamento irá falhar. 

* **Imagem de VM** -IA do Batch por predefinição Aprovisiona as VMs do cluster com uma imagem de Ubuntu Server padrão que suporta cargas de trabalho do contentor. Pode escolher outra imagem pré-configurada do Linux no Azure Marketplace, como um [máquina de Virtual de ciência de dados](../machine-learning/data-science-virtual-machine/overview.md). 

* **Armazenamento** -Batch AI fornece uma opção de armazenamento automático, o que pode escolher ao criar um cluster com a CLI do Azure. Esta opção cria automaticamente um contentor de BLOBs e de partilha de ficheiros do Azure com uma conta de armazenamento nova. Estes recursos de armazenamento estão montados para cada um de nós do cluster durante o tempo de execução, permitindo que os ficheiros para ser acedido a partir de um caminho local. Os nomes de conta de armazenamento, nome da partilha de ficheiros, nome do contentor de BLOBs e caminhos de montagem que todas têm valores predefinidos, que também podem ser personalizados com parâmetros adicionais durante a criação do cluster. 

  Se não existem opções de armazenamento são definidas, terá de criar os recursos de armazenamento em separado e defini-las ao submeter tarefas. Esta opção é útil se o cluster é gerenciado ao nível da organização, mas o armazenamento é gerenciado ao nível do utilizador. Para obter mais informações sobre como carregar ficheiros para o armazenamento do Azure e acessá-los durante o tempo de execução, consulte [Store Batch AI entrada e saída com o armazenamento do Azure da tarefa](use-azure-storage.md).

* **Acesso de utilizador** - Batch AI permite-lhe gerar pública e privada SSH arquivos de chaves ao criar um cluster ou fornecer as suas próprias chaves SSH para que possa SSH em nós individuais. Também pode definir um nome de utilizador (definido como o utilizador atual por padrão) e a palavra-passe. Estas credenciais podem ser utilizadas para aceder a nós durante a execução para poder ver várias métricas ou obter mais informações para as tarefas.

* **Tarefas de configuração** -Batch AI permite-lhe definir argumentos da linha de comandos para ser executado em cada nó após a alocação. Também pode definir o caminho onde o ficheiro de saída deve ter sessão iniciado. Utilize esta opção quando tem passos adicionais de aprovisionamento para além da imagem base.

* **Configuração adicional** -existem vários cenários menos comuns, que podem exigir configurações mais especializadas. Neste caso, um [o ficheiro de configuração de cluster](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#using-cluster-configuration-file) podem ser anexados com o comando da CLI do Azure para criar um cluster. 

## <a name="create-the-cluster"></a>Criar o cluster

Depois de decidir sobre as opções de configuração de cluster, utilize a CLI do Azure, portal do Azure ou APIs de IA do Batch para criar o cluster. Por exemplo, para criar um cluster com a CLI do Azure, pode seguir a [az batchai cluster criar](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-create) documentação para criar o comando exato que dá-lhe as configurações que precisa. 

O que é mais básicas de configuração, as seguintes cláusulas de comando um Standard_NC6 cluster com um nó e o acesso SSH. Por predefinição, este cluster contém VMs dedicadas com o padrão mais recente imagem de Ubuntu Server.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --target 1 \
    --generate-ssh-keys
```

As seguintes cláusulas de exemplo, um Standard_NC6 de cluster que pode ser dimensionada automaticamente de 0 para 4 nós e usa nós de baixa prioridade e uma conta de armazenamento automático. Esta configuração é uma boa configuração se precisar de um cluster de baixo custo e fácil de gerenciar. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --vm-priority lowpriority \
    --max 4 \
    --min 0 \
    --use-auto-storage 
```

O exemplo seguinte aprovisiona um cluster de Standard_NC6 que inclui uma imagem de máquina de Virtual de ciência de dados, armazenamento personalizado e montagem opções, credenciais SSH personalizadas, uma tarefa de instalação que instala o *deszipe* pacote e um cluster ficheiro de configuração para configuração adicional. Esta configuração é um exemplo de um cluster que é mais adaptado às suas necessidades.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --image UbuntuDSVM \ 
    --config-file cluster.json \
    --setup-task 'apt install unzip -y'
    --storage-account-name <STORAGE ACCOUNT NAME> \
    --nfs-name nfsmount \
    --afs-name afsmount \
    --bfs-name bfsmount \
    --user-name adminuser \
    --ssh-key id_rsa.pub \
    --password secretpassword 
```

## <a name="monitor-the-cluster"></a>Monitorizar o cluster

O [lista de clusters do az batchai](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-list), [show do az batchai cluster](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-show), e [lista de nós do az batchai cluster](/cli/azure/batchai/cluster/node?view=azure-cli-latest#az-batchai-cluster-node-list) comandos podem ser usados para monitorar várias informações para cada um dos clusters.

### <a name="list-all-clusters"></a>Lista de todos os clusters

A seguinte alls de lista comando dos clusters numa área de trabalho.

```azurecli-interactive
az batchai cluster list \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
```

### <a name="show-information-about-a-cluster"></a>Mostrar informações sobre um cluster

O comando seguinte mostra as informações completas sobre um cluster específico num formato de tabela.

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --output table
```

Se o cluster foi aprovisionado com a opção de armazenamento automática, desejará recuperar o nome de conta de armazenamento a utilizar ao carregar scripts e tarefas de formação. Utilize o seguinte comando:

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --query "nodeSetup.mountVolumes.azureFileShares[0].{storageAccountName:accountName}"
```

O resultado deve ser semelhante ao seguinte.

```JSON
{
  "storageAccountName": "baixxxxxxxxx"
}
```

### <a name="list-cluster-nodes"></a>Listar nós de cluster

Se precisar de ligar a nós do cluster, o comando seguinte obtém a lista de nós e as informações de ligação.  

```azurecli-interactive
az batchai cluster node list \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
 ```

O resultado para cada nó será semelhante ao seguinte:

```JSON
[
  {
    "ipAddress": "40.68.xxx.xxx",
    "nodeId": "tvm-xxxxxxxxxx-xxxxxxxx",
    "port": 50000.0
  }
]
```
Pode usar essas informações para fazer uma ligação SSH a um nó com um comando semelhante ao seguinte.

```bash
ssh myusername@40.68.xxx.xxx -p 50000
``` 

## <a name="submit-jobs-to-the-cluster"></a>Submeter tarefas para o cluster

Após o aprovisionamento do cluster, em seguida, pode submeter trabalhos para execução em nós. Consulte a [tarefa do az batchai](/cli/azure/batchai/job?view=azure-cli-latest) comando formas diferentes de preparar, submeter e monitorizar tarefas com a CLI do Azure. 

## <a name="downscale-cluster-for-later-use"></a>Cluster downscale para utilização posterior

Quando tiver terminado de executar as suas tarefas, vai querer reduzir verticalmente o seu cluster. Recomenda-se para sempre downscale clusters quando não estão a ser utilizadas para economizar nos custos de computação. Downscaling um cluster para 0 nós permite-lhe parar as cobranças de faturas enquanto não precisa reaprovisionar os clusters no futuro, quando precisar upscale novamente. Se o dimensionamento automático foi selecionado na criação do cluster, o cluster automaticamente deve reduzir verticalmente para o mínimo de destino. Se o dimensionamento manual tiver sido selecionada, reduzir verticalmente o cluster com o seguinte comando.

```azurecli-interactive
az batchai cluster resize \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE> \
    --target 0
```

## <a name="delete-a-cluster"></a>Eliminar um cluster

Quando tiver terminado a utilização de um cluster, utilize o [delete do az batchai cluster](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-delete) comando eliminá-lo.

```azurecli-interactive
az batchai cluster delete \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE>
```

A eliminar o grupo de recursos automaticamente também elimina todos os clusters que foram aprovisionados no grupo de recursos.

```azurecli-interactive
az group delete --name <RESOURCE GROUP>
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais exemplos de criação de um cluster do Batch AI, consulte a [Portal](quickstart-create-cluster-portal.md) ou [CLI do Azure](quickstart-create-cluster-cli.md) início rápido, ou o [receitas de Batch AI](https://github.com/Azure/BatchAI/tree/master/recipes) no GitHub.
