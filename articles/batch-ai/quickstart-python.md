---
title: Início Rápido do Azure - Preparação CNTK com o Batch AI - Python | Microsoft Docs
description: Aprenda rapidamente a executar uma tarefa de preparação CNTK com o Batch AI através do SDK Python
services: batch-ai
documentationcenter: na
author: lliimsft
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: Python
ms.topic: quickstart
ms.date: 10/06/2017
ms.author: lili
ms.openlocfilehash: da5c1181f9c4d311bdeabe837435ae4e0eb3dc1a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
ms.locfileid: "31513253"
---
# <a name="run-a-cntk-training-job-using-the-azure-python-sdk"></a>Executar uma tarefa de preparação CNTK com o SDK Python

Este início rápido detalha a utilização do SDK Python do Azure para executar uma tarefa Microsoft Cognitive Toolkit (CNTK) com o serviço Batch AI.

Neste exemplo, vai utilizar a base de dados MNIST de imagens manuscritas para preparar uma rede neural convolucional (CNN) num cluster GPU de nó único.

## <a name="prerequisites"></a>Pré-requisitos

* Subscrição do Azure - se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

* SDK Python do Azure - veja as [instruções de instalação](/python/azure/python-sdk-azure-install)

* Conta de armazenamento do Azure - veja [Como criar uma conta de armazenamento do Azure](../storage/common/storage-create-storage-account.md)

* Credenciais do principal de serviço do Azure Active Directory - veja [Como criar um principal de serviço com a CLI](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

* Registe o fornecedor de recursos do Batch AI uma vez para a sua subscrição, utilizando o Azure Cloud Shell ou a CLI do Azure. Um registo de fornecedor pode demorar até 15 minutos.

```azurecli
az provider register -n Microsoft.BatchAI
```

## <a name="configure-credentials"></a>Configurar as credenciais
Adicione o seguinte código ao seu ficheiro de script e substitua `FILL-IN-HERE` com os valores apropriados:

```Python
# credentials used for authentication
aad_client_id = 'FILL-IN-HERE'
aad_secret = 'FILL-IN-HERE'
aad_tenant = 'FILL-IN-HERE'
subscription_id = 'FILL-IN-HERE'

# credentials used for storage
storage_account_name = 'FILL-IN-HERE'
storage_account_key = 'FILL-IN-HERE'

# specify the credentials used to remote login your GPU node
admin_user_name = 'FILL-IN-HERE'
admin_user_password = 'FILL-IN-HERE'
```

Tenha em atenção que se a colocação das credenciais no código de origem não for uma boa prática e é feito aqui para tornar o início rápido mais simples.
Em vez disso, considere utilizar as variáveis de ambiente ou um ficheiro de configuração diferente.

## <a name="create-batch-ai-client"></a>Criar o cliente do Batch AI

O código seguinte cria um objeto de credenciais principal de serviço e o cliente do Batch AI:

```Python
from azure.common.credentials import ServicePrincipalCredentials
import azure.mgmt.batchai as batchai
import azure.mgmt.batchai.models as models

creds = ServicePrincipalCredentials(
        client_id=aad_client_id, secret=aad_secret, tenant=aad_tenant)

batchai_client = batchai.BatchAIManagementClient(
    credentials=creds, subscription_id=subscription_id)
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Os clusters e as tarefas do Batch AI são recursos do Azure e têm de ser colocados num grupo de recursos do Azure. O fragmento seguinte cria um grupo de recursos:

```Python
from azure.mgmt.resource import ResourceManagementClient

resource_group_name = 'myresourcegroup'
resource_management_client = ResourceManagementClient(
        credentials=creds, subscription_id=subscription_id)
resource = resource_management_client.resource_groups.create_or_update(
        resource_group_name, {'location': 'eastus'})
```


## <a name="prepare-azure-file-share"></a>Preparar a partilha de ficheiros do Azure
Para efeitos de ilustração, este início rápido utiliza uma partilha de Ficheiros do Azure para alojar os dados e os scripts de preparação da tarefa de formação.

1. Criar uma partilha de ficheiros com o nome `batchaiquickstart`.

```Python
from azure.storage.file import FileService
azure_file_share_name = 'batchaiquickstart'
service = FileService(storage_account_name, storage_account_key)
service.create_share(azure_file_share_name, fail_on_exist=False)
```

2. Crie um diretório na partilha com o nome `mnistcntksample`

```Python
mnist_dataset_directory = 'mnistcntksample'
service.create_directory(azure_file_share_name, mnist_dataset_directory,
                         fail_on_exist=False)
```
3. Transferir o [pacote exemplo](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) e deszipe para o diretório atual. O seguinte código carrega os ficheiros necessários para a partilha de Ficheiros do Azure:

```Python
for f in ['Train-28x28_cntk_text.txt', 'Test-28x28_cntk_text.txt',
          'ConvNet_MNIST.py']:
     service.create_file_from_path(
             azure_file_share_name, mnist_dataset_directory, f, f)
```

## <a name="create-gpu-cluster"></a>Criar cluster de GPU

Crie um cluster do Batch AI. Neste exemplo, o cluster consiste num único nó de VM STANDARD_NC6. Este tamanho da VM tem um GPU NVIDIA K80. Monte a partilha de ficheiros numa pasta com o nome `azurefileshare`. O caminho completo desta pasta do nó de computação para a GPU é `$AZ_BATCHAI_MOUNT_ROOT/azurefileshare`.

```Python
cluster_name = 'mycluster'

relative_mount_point = 'azurefileshare'

parameters = models.ClusterCreateParameters(
    # Location where the cluster will physically be deployed
    location='eastus',
    # VM size. Use NC or NV series for GPU
    vm_size='STANDARD_NC6',
    # Configure the ssh users
    user_account_settings=models.UserAccountSettings(
        admin_user_name=admin_user_name,
        admin_user_password=admin_user_password),
    # Number of VMs in the cluster
    scale_settings=models.ScaleSettings(
        manual=models.ManualScaleSettings(target_node_count=1)
    ),
    # Configure each node in the cluster
    node_setup=models.NodeSetup(
        # Mount shared volumes to the host
        mount_volumes=models.MountVolumes(
            azure_file_shares=[
                models.AzureFileShareReference(
                    account_name=storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=storage_account_key),
                    azure_file_url='https://{0}/{1}'.format(
                        service.primary_endpoint, azure_file_share_name),
                    relative_mount_path=relative_mount_point)],
        ),
    ),
)
batchai_client.clusters.create(resource_group_name, cluster_name,
                               parameters).result()
```

## <a name="get-cluster-status"></a>Obter estado do cluster

Monitorize o estado do cluster com o seguinte comando:

```Python
cluster = batchai_client.clusters.get(resource_group_name, cluster_name)
print('Cluster state: {0} Target: {1}; Allocated: {2}; Idle: {3}; '
      'Unusable: {4}; Running: {5}; Preparing: {6}; Leaving: {7}'.format(
    cluster.allocation_state,
    cluster.scale_settings.manual.target_node_count,
    cluster.current_node_count,
    cluster.node_state_counts.idle_node_count,
    cluster.node_state_counts.unusable_node_count,
    cluster.node_state_counts.running_node_count,
    cluster.node_state_counts.preparing_node_count,
    cluster.node_state_counts.leaving_node_count))
```

O código anterior imprime as informações de alocação de cluster básico, como no exemplo seguinte:

```
Cluster state: AllocationState.steady Target: 1; Allocated: 1; Idle: 0; Unusable: 0; Running: 0; Preparing: 1; Leaving: 0
```

O cluster está pronto quando os nós forem alocados e a preparação estiver terminada (veja o atributo `nodeStateCounts`). Se ocorrer um problema, o atributo `errors` contém a descrição do erro.

## <a name="create-training-job"></a>Criar tarefa de preparação

Depois do cluster ser criado, configure e submeta a tarefa de aprendizagem:

```Python
job_name = 'myjob'

parameters = models.job_create_parameters.JobCreateParameters(
    # The job and cluster must be created in the same location
    location=cluster.location,
    # The cluster this job will run on
    cluster=models.ResourceId(id=cluster.id),
    # The number of VMs in the cluster to use
    node_count=1,
    # Write job's standard output and execution log to Azure File Share
    std_out_err_path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(
        relative_mount_point),
    # Configure location of the training script and MNIST dataset
    input_directories=[models.InputDirectory(
        id='SAMPLE',
        path='$AZ_BATCHAI_MOUNT_ROOT/{0}/{1}'.format(
            relative_mount_point, mnist_dataset_directory))],
    # Specify location where generated model will be stored
    output_directories=[models.OutputDirectory(
        id='MODEL',
        path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point),
        path_suffix="Models")],
    # Container configuration
    container_settings=models.ContainerSettings(
        image_source_registry=models.ImageSourceRegistry(
            image='microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0')),
    # Toolkit specific settings
    cntk_settings=models.CNTKsettings(
        python_script_file_path='$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py',
        command_line_args='$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL')
)

# Create the job
batchai_client.jobs.create(resource_group_name, job_name, parameters).result()
```

## <a name="monitor-job"></a>Monitorizar tarefa
Pode inspecionar o estado da tarefa com o seguinte código:

```Python
job = batchai_client.jobs.get(resource_group_name, job_name)

print('Job state: {0} '.format(job.execution_state.name))
```

O resultado é semelhante a: `Job state: running`.

`executionState` contém o estado atual de execução da tarefa:
* `queued`: a tarefa está a aguardar que os nós do cluster fiquem disponíveis
* `running`: a tarefa está em execução
* `succeeded` (ou `failed`): a tarefa está concluída e `executionInfo` contém detalhes sobre o resultado

## <a name="list-stdout-and-stderr-output"></a>Listar o resultado de stdout e stderr
Utilize o seguinte código para listar os ficheiros de registo, stdout e stderr gerados:

```Python
files = batchai_client.jobs.list_output_files(
    resource_group_name, job_name,
    models.JobsListOutputFilesOptions(outputdirectoryid="stdouterr"))

for file in (f for f in files if f.download_url):
    print('file: {0}, download url: {1}'.format(file.name, file.download_url))
```

## <a name="list-generated-model-files"></a>Listar ficheiros de modelo gerados
Utilize o seguinte código para listar os ficheiros de modelo gerados:
```Python
files = batchai_client.jobs.list_output_files(
    resource_group_name, job_name,
    models.JobsListOutputFilesOptions(outputdirectoryid="MODEL"))

for file in (f for f in files if f.download_url):
    print('file: {0}, download url: {1}'.format(file.name, file.download_url))
```

## <a name="delete-resources"></a>Eliminar recursos

Utilize o seguinte código para eliminar a tarefa:
```Python
batchai_client.jobs.delete(resource_group_name, job_name)
```

Utilize o seguinte código para eliminar o cluster:
```Python
batchai_client.clusters.delete(resource_group_name, cluster_name)
```

Utilize o seguinte código para eliminar todos os recursos alocados:
```Python
resource_management_client.resource_groups.delete('myresourcegroup')
```
## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a executar uma tarefa de preparação CNTK num cluster do Batch AI com o SDK Python. Para obter mais informações sobre como utilizar o Batch AI com ferramentas diferentes, veja as [receitas de preparação](https://github.com/Azure/BatchAI).
