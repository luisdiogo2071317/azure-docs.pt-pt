---
title: As cargas de trabalho de contentor do Azure batch | Microsoft Docs
description: Saiba como executar aplicações a partir de imagens do contentor do Azure batch.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 06/04/2018
ms.author: danlep
ms.openlocfilehash: 8ef9d5a8e5212f6715769eecf4fde92a6d0b9d44
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060522"
---
# <a name="run-container-applications-on-azure-batch"></a>Executar aplicações de contentor do Azure batch

O Azure Batch permite-lhe executar e dimensionar grandes quantidades de batch de computação de tarefas no Azure. Tarefas do batch podem executar diretamente em máquinas virtuais (nós) no conjunto do Batch, mas também pode configurar um conjunto do Batch para executar tarefas nos contentores compatível com o Docker em nós. Este artigo mostra como criar um conjunto de nós de computação que suportam as tarefas em execução do contentor e, em seguida, executar tarefas de contentor no conjunto. 

Deve estar familiarizado com conceitos de contentor e como criar um conjunto do Batch e a tarefa. Os exemplos de código utilizam o .NET do Batch e SDKs do Python. Também pode utilizar outros SDKs do Batch e ferramentas, incluindo o portal do Azure, para criar conjuntos do Batch preparados para o contentor e executar tarefas de contentor.

## <a name="why-use-containers"></a>Porquê utilizar contentores?

Utilizar contentores fornece uma forma fácil de executar tarefas de lote sem ter de gerir um ambiente e dependências para executar aplicações. Contentores implementar aplicações, como unidades de simples, portáteis, self-sufficient que podem ser executados em vários ambientes diferentes. Por exemplo, pode criar e testar um contentor localmente e carregue a imagem do contentor para um registo no Azure ou noutro local. O modelo de implementação do contentor assegura que o ambiente de tempo de execução da sua aplicação está sempre corretamente instalado e configurado onde quer que aloja a aplicação. Com base no contentor de tarefas no Batch também podem tirar partido das funcionalidades das tarefas de fora do contentor, incluindo pacotes de aplicações e gestão de ficheiros de recursos e ficheiros de saída. 

## <a name="prerequisites"></a>Pré-requisitos

* **Versões do SDK**: os SDKs do Batch suporte contentor imagens a partir das seguintes versões:
    * API de REST do batch versão 2017-09-01.6.0
    * SDK .NET do batch versão 8.0.0
    * SDK Python do batch versão 4.0
    * Batch Java SDK versão 3.0
    * SDK Node.js versão 3.0 do batch

* **Contas**: na sua subscrição do Azure, terá de criar uma conta do Batch e, opcionalmente, uma conta de armazenamento do Azure.

* **Uma imagem VM suportados**: contentores só são suportados em conjuntos criados com a configuração de Máquina Virtual, a partir de imagens de detalhado na secção seguinte, "suportada imagens da máquina virtual". Se fornecer uma imagem personalizada, consulte as considerações na secção seguinte e os requisitos na [utilizar uma imagem personalizada gerida para criar um conjunto de máquinas virtuais](batch-custom-images.md). 

### <a name="limitations"></a>Limitações

* O batch fornece suporte RDMA apenas para contentores em execução em conjuntos do Linux.

## <a name="supported-virtual-machine-images"></a>Imagens de máquina virtual suportadas

Utilize um dos seguintes suportados Windows ou imagens de Linux para criar um conjunto de VM nós de computação para cargas de trabalho do contentor. Para obter mais informações sobre imagens do Marketplace que são compatíveis com o Batch, consulte [lista de imagens da máquina virtual](batch-linux-nodes.md#list-of-virtual-machine-images). 

### <a name="windows-images"></a>Imagens do Windows

Para cargas de trabalho do Windows contentor, atualmente, o Batch suporta o **Datacenter do Windows Server 2016 com contentores** imagem no Azure Marketplace. Imagens de contentor de Docker só são suportadas no Windows.

Também pode criar imagens personalizadas de VMs em execução Docker no Windows.

### <a name="linux-images"></a>Imagens de Linux

Para cargas de trabalho do Linux contentor, o Batch atualmente suporta as seguintes imagens de Linux publicadas pelo Microsoft Azure Batch no Azure Marketplace:

* **CentOS para conjuntos de contentor do Azure Batch**

* **CentOS (com controladores RDMA) para conjuntos de contentor do Azure Batch**

* **Ubuntu Server para conjuntos de contentor do Azure Batch**

* **Ubuntu Server (com controladores RDMA) para conjuntos de contentor do Azure Batch**

Estas imagens são apenas suportadas para utilização em conjuntos do Azure Batch. Se a funcionalidade:

* Um pré-instaladas [Moby](https://github.com/moby/moby) tempo de execução do contentor 

* Controladores de NVIDIA GPU pré-instaladas, para simplificar a implementação em VMs do Azure N série

* Imagens com ou sem pré-instaladas controladores RDMA; Estes controladores permite que os nós de agrupamento aceder à rede Azure RDMA quando implementados em tamanhos de VM com capacidade RDMA  

Também pode criar imagens personalizadas de VMs em execução Docker das distribuições de Linux que é compatível com o Batch. Se optar por fornecer a sua imagem personalizada do Linux, consulte as instruções no [utilizar uma imagem personalizada gerida para criar um conjunto de máquinas virtuais](batch-custom-images.md).

Para obter suporte de Docker sobre uma imagem personalizada, instalar [Docker Comunidade Edition (CE)](https://www.docker.com/community-edition) ou [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Considerações adicionais para utilizar uma imagem personalizada do Linux:

* Para tirar partido do desempenho GPU dos tamanhos de série N do Azure ao utilizar uma imagem personalizada, de pré-instalação controladores NVIDIA. Além disso, tem de instalar o utilitário de motor de Docker para NVIDIA GPUs, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

* Para aceder à rede Azure RDMA, utilize um tamanho VM com capacidade RDMA. Os controladores RDMA necessários estão instalados no imagens Ubuntu suportadas pelo Batch e CentOS HPC. Para executar cargas de trabalho MPI, poderá ser necessária configuração adicional. Consulte [utilização com capacidade RDMA ou preparados para a GPU instâncias num conjunto do Batch](batch-pool-compute-intensive-sizes.md).


## <a name="container-configuration-for-batch-pool"></a>Configuração do contentor para o conjunto do Batch

Para ativar um conjunto do Batch executar cargas de trabalho do contentor, tem de especificar [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) definições no agrupamento de [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) objeto. (Este artigo fornece ligações para a referência de API .NET do Batch. As definições correspondentes de [Batch Python](/python/api/azure.batch) API.)

Pode criar um conjunto de contentor preparados com ou sem imagens prefetched contentor, conforme mostrado nos exemplos seguintes. O processo de extração (ou obtenção prévia) permite-lhe pré-carregar imagens de contentor do Docker Hub ou outro registo de contentor na Internet. Para melhor desempenho, utilize um [registo de contentor do Azure](../container-registry/container-registry-intro.md) na mesma região que a conta do Batch.

A vantagem de prefetching imagens do contentor é que quando tarefas primeiro iniciar a execução não têm de aguardar que a imagem do contentor transferir. A configuração do contentor obtém as imagens de contentor para as VMs quando o conjunto for criado. Tarefas que são executadas no conjunto, em seguida, podem referenciar a lista de imagens de contentor e opções de execução do contentor.


### <a name="pool-without-prefetched-container-images"></a>Conjunto sem imagens prefetched contentor

Para configurar um conjunto de ativado o contentor de mensagens em fila sem imagens prefetched contentor, defina `ContainerConfiguration` e `VirtualMachineConfiguration` objetos conforme mostrado no exemplo seguinte do Python. Este exemplo utiliza o Ubuntu Server para a imagem de conjuntos de contentor do Azure Batch no Marketplace.


```python
image_ref_to_use = batch.models.ImageReference(
        publisher='microsoft-azure-batch',
        offer='ubuntu-server-container',
        sku='16-04-lts',
        version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            container_configuration=container_conf,
            node_agent_sku_id='batch.node.ubuntu 16.04'),
        vm_size='STANDARD_D1_V2',
        target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Prefetch imagens para a configuração do contentor

Para prefetch imagens do contentor no conjunto, adicionar a lista de imagens de contentor (`container_image_names`, no Python) para o `ContainerConfiguration`. 

O exemplo de Python básico seguinte mostra como prefetch uma imagem de contentor Ubuntu padrão do [Docker Hub](https://hub.docker.com).

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


O seguinte exemplo de exemplo c# parte do princípio de que pretende prefetch uma imagem de TensorFlow de [Docker Hub](https://hub.docker.com). Este exemplo inclui uma tarefa de início que é executado no anfitrião VM em nós do conjunto. Poderá executar uma tarefa de início no anfitrião, por exemplo, para um servidor de ficheiros que pode ser acedido a partir os contentores de montagem.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Prefetch imagens a partir de um registo de contentor privada

Também pode prefetch imagens contentor através da autenticação para um servidor de registo do contentor privada. No exemplo seguinte, o `ContainerConfiguration` e `VirtualMachineConfiguration` objetos prefetch uma imagem de TensorFlow privada a partir de um registo de contentor do Azure privado. A referência da imagem é o mesmo do exemplo anterior.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```


## <a name="container-settings-for-the-task"></a>Definições de contentor para a tarefa

Para executar tarefas de contentor em nós de computação, tem de especificar definições específicas do contentor, tais como executar as opções, imagens a utilizar e registo de tarefas.

Utilize o `ContainerSettings` propriedade das classes de tarefas para configurar definições específicas do contentor. Estas definições são definidas pelo [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) classe.

Se executar tarefas nas imagens de contentor, a [tarefas nuvem](/dotnet/api/microsoft.azure.batch.cloudtask) e [tarefa do Gestor de tarefas](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) exigir definições de contentor. No entanto, o [iniciar a tarefa](/dotnet/api/microsoft.azure.batch.starttask), [tarefa de preparação](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), e [tarefa de libertação](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) não necessitam de definições de contentor (ou seja, podem ser executados no contexto de um contentor ou diretamente no nó).

A linha de comandos para uma tarefa de contentor do Azure Batch executa num diretório de trabalho no contentor que é muito semelhante ao ambiente do que batch configura uma tarefa (fora do contentor) regular:

* Todos os diretórios em modo recursivo abaixo o `AZ_BATCH_NODE_ROOT_DIR` (a raiz de diretórios do Azure Batch no nó) estão mapeadas para o contentor
* Todas as variáveis de ambiente de tarefas estão mapeadas para o contentor
* O diretório de trabalho de aplicações está definido igual para uma tarefa regular, pelo que pode utilizar funcionalidades como os pacotes de aplicações e ficheiros de recursos

Porque o lote altera o diretório de trabalho predefinido no seu contentor, a tarefa é executada numa localização diferente do ponto de entrada de contentor típico (por exemplo, `c:\` por predefinição no contentor de Windows, ou `/` no Linux). Certifique-se de que o ponto de entrada de linha de comandos ou contentor tarefas Especifica um caminho absoluto, se já não está configurado dessa forma.

O seguinte fragmento de Python mostra uma linha de comandos básica com um contentor de Ubuntu solicitados do Hub de Docker. As opções de contentor executar são argumentos adicionais para o `docker create` comando que a tarefa é executada. Aqui, o `--rm` opção remove o contentor depois de concluída a tarefa.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='ubuntu', 
    container_run_options='--rm')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='/bin/echo hello',
    container_settings=task_container_settings
)

```

O exemplo do c# seguinte mostra as definições de contentor básico de uma tarefa de cloud:

```csharp
// Simple container task command

string cmdLine = "c:\myApp.exe";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu",
    containerRunOptions: "--rm --read-only"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## <a name="next-steps"></a>Passos Seguintes

* Consulte também o [Batch Shipyard](https://github.com/Azure/batch-shipyard) toolkit para facilitar a implementação de cargas de trabalho de contentor do Azure batch através de [receitas Shipyard](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* Para obter mais informações sobre como instalar e utilizar o Docker CE no Linux, consulte o [Docker](https://docs.docker.com/engine/installation/) documentação.

* Para obter mais informações sobre como utilizar imagens personalizadas, consulte [utilizar uma imagem personalizada gerida para criar um conjunto de máquinas virtuais ](batch-custom-images.md).

* Saiba mais sobre o [Moby projeto](https://mobyproject.org/), uma estrutura para a criação de sistemas baseados no contentor.