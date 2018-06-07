---
title: Partilha de ficheiros do Azure para conjuntos do Azure Batch | Microsoft Docs
description: Como montar uma partilha de ficheiros do Azure a partir de nós de computação num conjunto Linux ou do Windows no Azure Batch.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/24/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 88d7c0d033d7b517a396df27468de8be7ae20be9
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34811740"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Utilizar uma partilha de ficheiros do Azure com um conjunto do Batch

[Ficheiros do Azure](../storage/files/storage-files-introduction.md) oferece completamente geridos partilhas de ficheiros na nuvem que estão acessíveis através do protocolo Server Message Block (SMB). Este artigo fornece exemplos de código e informações para montar e utilizar uma partilha de ficheiros do Azure em nós de computação do conjunto. Os exemplos de código utilizam o .NET do Batch e Python SDKs, mas pode efetuar operações semelhantes utilizando outras ferramentas e os SDKs do Batch.

O batch fornece suporte nativo de API para ler e escrever dados utilizando o blobs Storage do Azure. No entanto, em alguns casos, que poderá aceder a uma partilha de ficheiros do Azure do seu conjunto de nós de computação. Por exemplo, ter uma carga de trabalho de legado que depende de uma partilha de ficheiros SMB ou as suas tarefas tiverem aceder a dados partilhados ou produzir resultados partilhado. 

## <a name="considerations-for-use-with-batch"></a>Considerações para utilização com o Batch

* Considere utilizar uma partilha de ficheiros do Azure quando tiver agrupamentos que executam um número relativamente baixo de tarefas paralelas. Reveja o [metas de desempenho e dimensionamento](../storage/files/storage-files-scale-targets.md) para determinar se os ficheiros do Azure (que utiliza uma conta de armazenamento do Azure) deve ser utilizados, o tamanho do conjunto esperado e número de ficheiros de recurso. 

* Partilhas de ficheiros do Azure são [económico](https://azure.microsoft.com/pricing/details/storage/files/) e pode ser configurada com dados de replicação noutra região, por isso, são globalmente redundante. 

* Podem montar uma partilha de ficheiros do Azure em simultâneo a partir de um computador local.

* Consulte também o geral [considerações de planeamento](../storage/files/storage-files-planning.md) partilhas de ficheiros para o Azure.


## <a name="create-a-file-share"></a>Criar uma partilha de ficheiros

[Criar uma partilha de ficheiros](../storage/files/storage-how-to-create-file-share.md) numa conta do storage que está ligada à sua conta do Batch ou numa conta de armazenamento separada.

## <a name="mount-a-share-on-a-windows-pool"></a>Montar uma partilha num agrupamento de Windows

Esta secção fornece os passos e exemplos de código para montar e utilizar um ficheiro do Azure partilham um conjunto de nós do Windows. Para obter informações adicionais, consulte o [documentação](../storage/files/storage-how-to-use-files-windows.md) para montar um Azure partilha de ficheiros no Windows. 

No lote, terá de montar a partilha de cada vez que uma tarefa é executada num nó do Windows. Atualmente, não é possível manter a ligação de rede entre tarefas em nós do Windows.

Por exemplo, incluir uma `net use` comandos para montar a partilha de ficheiros como parte de cada linha de comandos da tarefa. Para montar a partilha de ficheiros, são necessárias as seguintes credenciais:

* **Nome de utilizador**: AZURE\\\<storageaccountname\>, por exemplo, AZURE\\*mystorageaccountname*
* **Palavra-passe**: < StorageAccountKeyWhichEnds no = = >, por exemplo, *XXXXXXXXXXXXXXXXXXXXX = =*

O seguinte comando monta uma partilha de ficheiros *myfileshare* na conta do storage *mystorageaccountname* como o *s:* unidade:

```
net use S: \\mystorageaccountname.file.core.windows.net\myfileshare /user:AZURE\mystorageaccountname XXXXXXXXXXXXXXXXXXXXX==
```

De simplicidade, os exemplos aqui passaram as credenciais diretamente no texto. Na prática, recomendamos vivamente que gerir as credenciais utilizando variáveis de ambiente, certificados ou uma solução, tais como o Cofre de chaves do Azure.

Para simplificar a operação de montagem, opcionalmente, manter as credenciais em nós. Em seguida, pode montar a partilha sem credenciais. Execute os seguintes dois passos:

1. Execute o `cmdkey` utilitário de linha de comandos utilizando uma tarefa de início na configuração do agrupamento. Este problema persistir, as credenciais em cada nó do Windows. A linha de comandos da tarefa de início é semelhante a:

  ```
  cmd /c "cmdkey /add:mystorageaccountname.file.core.windows.net /user:AZURE\mystorageaccountname /pass:XXXXXXXXXXXXXXXXXXXXX=="

  ```

2. Montar a partilha em cada nó como parte de cada tarefa utilizando `net use`. Por exemplo, a seguinte linha de comandos de tarefas montar a partilha de ficheiros como o *s:* unidade. Isto poderia ser seguido de um comando ou script que faça referência a partilha. Credenciais em cache são utilizadas na chamada para `net use`. Este passo parte do princípio de que está a utilizar a mesma identidade de utilizador para as tarefas que utilizou na tarefa de início no conjunto, o que não é adequada para todos os cenários.

  ```
  cmd /c "net use S: \\mystorageaccountname.file.core.windows.net\myfileshare" 
  ```

### <a name="c-example"></a>Exemplo do c#
O exemplo do c# seguinte mostra como manter as credenciais de um conjunto de Windows com uma tarefa de início. O nome de serviço do ficheiro de armazenamento e credenciais de armazenamento são transmitidas como constantes definidas. Aqui, a tarefa de início é executado sob uma conta de auto-utilizador (não-administrador) padrão com âmbito de agrupamento.

```csharp
...
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: PoolId,
    targetDedicatedComputeNodes: PoolNodeCount,
    virtualMachineSize: PoolVMSize,
    virtualMachineConfiguration: virtualMachineConfiguration);

// Start task to store credentials to mount file share
string startTaskCommandLine = String.Format("cmd /c \"cmdkey /add:{0} /user:AZURE\\{1} /pass:{2}\"", StorageFileService, StorageAccountName, StorageAccountKey);

pool.StartTask = new StartTask
{
    CommandLine = startTaskCommandLine,
    UserIdentity = new UserIdentity(new AutoUserSpecification(
        elevationLevel: ElevationLevel.NonAdmin, 
        scope: AutoUserScope.Pool))
};

pool.Commit();
```

Depois de armazenar as credenciais, utilize as linhas de comandos de tarefas para montar a partilha e a partilha de referência na leitura ou operações de escrita. Como obter um exemplo básico, a linha de comandos de tarefas no seguinte fragmento utiliza o `dir` comando para listar ficheiros na partilha de ficheiros. Certifique-se de que executa cada tarefa com o mesmo [a identidade do utilizador](batch-user-accounts.md) utilizado para executar a tarefa de início no conjunto. 

```csharp
...
string taskId = "myTask";
string taskCommandLine = String.Format("cmd /c \"net use {0} {1} & dir {2}\"", ShareMountPoint, StorageFileShare, ShareMountPoint);

CloudTask task = new CloudTask(taskId, taskCommandLine);
task.UserIdentity = new UserIdentity(new AutoUserSpecification(
    elevationLevel: ElevationLevel.NonAdmin,
    scope: AutoUserScope.Pool));
tasks.Add(task);
```

## <a name="mount-a-share-on-a-linux-pool"></a>Montar uma partilha num agrupamento de Linux

Partilhas de ficheiros do Azure podem ser montadas nas distribuições do Linux utilizando o [cliente de kernel CIFS](https://wiki.samba.org/index.php/LinuxCIFS). O exemplo seguinte mostra como montar uma partilha de ficheiros num agrupamento Ubuntu 16.04 LTS de nós de computação. Se utilizar uma distribuição diferente do Linux, os passos gerais são semelhantes, mas o Gestor de pacotes apropriado para a distribuição. Para obter detalhes e exemplos adicionais, consulte [utilizar Azure Files com o Linux](../storage/files/storage-how-to-use-files-linux.md).

Primeiro, com uma identidade de utilizador administrador, instale o `cifs-utils` do pacote e criar o ponto de montagem (por exemplo, *mnt/MyAzureFileShare*) no sistema de ficheiros local. Uma pasta para um ponto de montagem pode ser criada em qualquer local no sistema de ficheiros, mas é Convenção comuns para criar esta sob o `/mnt` pasta. Lembre-se de que não crie um ponto de montagem diretamente em `/mnt` (no Ubuntu) ou `/mnt/resource` (em outras distribuições).

```
apt-get update && apt-get install cifs-utils && sudo mkdir -p /mnt/MyAzureFileShare
```

Em seguida, execute o `mount` comandos para montar a partilha de ficheiros, fornecer estas credenciais:

* **Nome de utilizador**: \<storageaccountname\>, por exemplo, *mystorageaccountname*
* **Palavra-passe**: < StorageAccountKeyWhichEnds no = = >, por exemplo, *XXXXXXXXXXXXXXXXXXXXX = =*

O seguinte comando monta uma partilha de ficheiros *myfileshare* na conta do storage *mystorageaccountname* em *mnt/MyAzureFileShare*: 

```
mount -t cifs //mystorageaccountname.file.core.windows.net/myfileshare /mnt/MyAzureFileShare -o vers=3.0,username=mystorageaccountname,password=XXXXXXXXXXXXXXXXXXXXX==,dir_mode=0777,file_mode=0777,serverino && ls /mnt/MyAzureFileShare
```

De simplicidade, os exemplos aqui passaram as credenciais diretamente no texto. Na prática, recomendamos vivamente que gerir as credenciais utilizando variáveis de ambiente, certificados ou uma solução, tais como o Cofre de chaves do Azure.

Num agrupamento de Linux, pode combinar todos estes passos numa tarefa de início único ou executá-los num script. Execute a tarefa de início como um utilizador de administrador no conjunto. Defina a tarefa de início de espera para concluir com êxito antes de executar tarefas adicionais no conjunto que façam referência a partilha.

### <a name="python-example"></a>Exemplo de Python

O exemplo de Python seguinte mostra como configurar um conjunto de Ubuntu para montar a partilha de uma tarefa de início. O ponto de montagem, ponto final de partilha de ficheiro e as credenciais de armazenamento são transmitidas como constantes definidas. A tarefa de início é executado sob uma conta de utilizador automática de administrador com âmbito de agrupamento.

```python
pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration = batchmodels.VirtualMachineConfiguration(
        image_reference = batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="16.04.0-LTS",
            version="latest"),
        node_agent_sku_id = "batch.node.ubuntu 16.04"),
    vm_size=_POOL_VM_SIZE,
    target_dedicated_nodes=_POOL_NODE_COUNT,
    start_task=batchmodels.StartTask(
        command_line="/bin/bash -c \"apt-get update && apt-get install cifs-utils && mkdir -p {} && mount -t cifs {} {} -o vers=3.0,username={},password={},dir_mode=0777,file_mode=0777,serverino\"".format(_COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_SHARE_ENDPOINT, _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_NAME, _STORAGE_ACCOUNT_KEY),
        wait_for_success=True,
        user_identity=batchmodels.UserIdentity(
            auto_user=batchmodels.AutoUserSpecification(
                scope=batchmodels.AutoUserScope.pool,
                elevation_level=batchmodels.ElevationLevel.admin)),
    )
)
batch_service_client.pool.add(pool)
```

Depois de montar a partilha e definir uma tarefa, utilize a partilha as linhas de comandos de tarefas. Por exemplo, utiliza o seguinte comando básico `ls` para listar ficheiros na partilha de ficheiros.

```python
...
task = batch.models.TaskAddParameter(
    id='mytask',
    command_line="/bin/bash -c \"ls {}\"".format(_COMPUTE_NODE_MOUNT_POINT))

batch_service_client.task.add(job_id, task)
```


## <a name="next-steps"></a>Passos Seguintes

* Para outras opções ler e escrever dados no Batch, consulte o [descrição geral da funcionalidade do Batch](batch-api-basics.md) e [manter tarefas e saída](batch-task-output.md).

* Consulte também o [Batch Shipyard](https://github.com/Azure/batch-shipyard) toolkit, que inclui [receitas Shipyard](https://github.com/Azure/batch-shipyard/tree/master/recipes) para implementar sistemas de ficheiros para cargas de trabalho do Batch contentor.