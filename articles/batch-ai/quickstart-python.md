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
ms.openlocfilehash: f535c9adf4926f29ae9cade6382debedab73937d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="run-a-cntk-training-job-using-the-azure-python-sdk"></a>Executar uma tarefa de preparação CNTK com o SDK Python

Este início rápido detalha a utilização do SDK Python do Azure para executar uma tarefa Microsoft Cognitive Toolkit (CNTK) com o serviço Batch AI. 

Neste exemplo, vai utilizar a base de dados MNIST de imagens manuscritas para preparar uma rede neural convolucional (CNN) num cluster GPU de nó único. 

## <a name="prerequisites"></a>Pré-requisitos

* Subscrição do Azure - se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

* SDK Python do Azure - veja as [instruções de instalação](/python/azure/python-sdk-azure-install)

* Conta de armazenamento do Azure - veja [Como criar uma conta de armazenamento do Azure](../storage/common/storage-create-storage-account.md)

* Credenciais do principal de serviço do Azure Active Directory - veja [Como criar um principal de serviço com a CLI](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

* Registe os fornecedores de recursos do Batch AI uma vez para a sua subscrição, utilizando o Azure Cloud Shell ou a CLI do Azure. Um registo de fornecedor pode demorar até 15 minutos.

  ```azurecli
  az provider register -n Microsoft.BatchAI
  az provider register -n Microsoft.Batch
  ```


## <a name="configure-credentials"></a>Configurar as credenciais
Crie estes parâmetros no seu script Python, substituindo os seus próprios valores:

```Python
# credentials used for authentication
client_id = 'my_aad_client_id'
secret = 'my_aad_secret_key'
token_uri = 'my_aad_token_uri'
subscription_id = 'my_subscription_id'

# credentials used for storage
storage_account_name = 'my_storage_account_name'
storage_account_key = 'my_storage_account_key'

# specify the credentials used to remote login your GPU node
admin_user_name = 'my_admin_user_name'
admin_user_password = 'my_admin_user_password'
```

Como melhor prática, todas as credenciais devem ser armazenadas num ficheiro de configuração separado, que não é mostrado neste exemplo. Consulte as [receitas](https://github.com/Azure/BatchAI/tree/master/recipes) para implementar um ficheiro de configuração. 

## <a name="authenticate-with-batch-ai"></a>Autenticar com o Batch AI

Para conseguir aceder ao Azure Batch AI, tem de autenticar com o Azure Active Directory. Segue-se código para autenticar com o serviço (criar um objeto `BatchAIManagementClient`), utilizando as credenciais do principal de serviço e o ID de subscrição:

```Python
from azure.common.credentials import ServicePrincipalCredentials
import azure.mgmt.batchai as batchai
import azure.mgmt.batchai.models as models

creds = ServicePrincipalCredentials(
        client_id=client_id, secret=secret, token_uri=token_uri)

batchai_client = batchai.BatchAIManagementClient(credentials=creds,
                                         subscription_id=subscription_id
)
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
Para efeitos de ilustração, este início rápido utiliza uma partilha de ficheiros do Azure para alojar os dados e os scripts de preparação da tarefa de aprendizagem. 

1. Crie uma partilha de ficheiros com o nome *batchaiquickstart*.

  ```Python
  from azure.storage.file import FileService 
 
  azure_file_share_name = 'batchaiquickstart' 
 
  service = FileService(storage_account_name, storage_account_key) 
 
  service.create_share(azure_file_share_name, fail_on_exist=False)
  ``` 
 
2. Crie um diretório na partilha com o nome *mnistcntksample* 

  ```Python
  mnist_dataset_directory = 'mnistcntksample' 
 
  service.create_directory(azure_file_share_name, mnist_dataset_directory, fail_on_exist=False) 
  ```
3. Transfira o [pacote de exemplo](https://batchaisamples.blob.core.windows.net/samples/BatchAIQuickStart.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=b&sig=hrAZfbZC%2BQ%2FKccFQZ7OC4b%2FXSzCF5Myi4Cj%2BW3sVZDo%3D) e deszipe-o. Carregue o conteúdo para o diretório onde irá executar o script Python.

  ```Python
  for f in ['Train-28x28_cntk_text.txt', 'Test-28x28_cntk_text.txt', 'ConvNet_MNIST.py']:     
     service.create_file_from_path(
             azure_file_share_name, mnist_dataset_directory, f, f) 
  ```

## <a name="create-gpu-cluster"></a>Criar cluster de GPU

Crie um cluster do Batch AI. Neste exemplo, o cluster consiste num único nó de VM STANDARD_NC6. Este tamanho da VM tem um GPU NVIDIA K80. Monte a partilha de ficheiros numa pasta com o nome *azurefileshare*. O caminho completo desta pasta no nó de computação do GPU é $AZ_BATCHAI_MOUNT_ROOT/azurefileshare.

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
         azure_file_url='https://{0}.file.core.windows.net/{1}'.format(
               storage_account_name, azure_file_share_name),
                  relative_mount_path = relative_mount_point)],
         ), 
    ), 
) 
batchai_client.clusters.create(resource_group_name, cluster_name, parameters).result() 
```

## <a name="get-cluster-status"></a>Obter estado do cluster

Monitorize o estado do cluster com o seguinte comando: 

```Python
cluster = batchai_client.clusters.get(resource_group_name, cluster_name)
print('Cluster state: {0} Target: {1}; Allocated: {2}; Idle: {3}; '
      'Unusable: {4}; Running: {5}; Preparing: {6}; leaving: {7}'.format(
    cluster.allocation_state,
    cluster.scale_settings.manual.target_node_count,
    cluster.current_node_count,
    cluster.node_state_counts.idle_node_count,
    cluster.node_state_counts.unusable_node_count,
    cluster.node_state_counts.running_node_count,
    cluster.node_state_counts.preparing_node_count,
    cluster.node_state_counts.leaving_node_count)) 
```

O código anterior imprime as informações de alocação de cluster básico, como as seguintes:

```Shell
Cluster state: AllocationState.steady Target: 1; Allocated: 1; Idle: 0; Unusable: 0; Running: 0; Preparing: 1; Leaving 0
 
```  

O cluster está pronto quando os nós forem alocados e a preparação estiver terminada (veja o atributo `nodeStateCounts`). Se ocorrer um problema, o atributo `errors` contém a descrição do erro.

## <a name="create-training-job"></a>Criar tarefa de preparação

Depois de o cluster estar pronto, configure e submeta a tarefa de aprendizagem. 

```Python
job_name = 'myjob' 
 
parameters = models.job_create_parameters.JobCreateParameters( 
 
     # Location where the job will run
     # Ideally this should be co-located with the cluster.
     location='eastus', 
 
     # The cluster this job will run on
     cluster=models.ResourceId(cluster.id), 
 
     # The number of VMs in the cluster to use
     node_count=1, 
 
     # Override the path where the std out and std err files will be written to.
     # In this case we will write these out to an Azure Files share
     std_out_err_path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point), 
 
     input_directories=[models.InputDirectory(
         id='SAMPLE',
         path='$AZ_BATCHAI_MOUNT_ROOT/{0}/{1}'.format(relative_mount_point, mnist_dataset_directory))], 
 
     # Specify directories where files will get written to 
     output_directories=[models.OutputDirectory(
        id='MODEL',
        path_prefix='$AZ_BATCHAI_MOUNT_ROOT/{0}'.format(relative_mount_point),
        path_suffix="Models")], 
 
     # Container configuration
     container_settings=models.ContainerSettings(
        models.ImageSourceRegistry(image='microsoft/cntk:2.1-gpu-python3.5-cuda8.0-cudnn6.0')), 
 
     # Toolkit specific settings
     cntk_settings = models.CNTKsettings(
        python_script_file_path='$AZ_BATCHAI_INPUT_SAMPLE/ConvNet_MNIST.py',
        command_line_args='$AZ_BATCHAI_INPUT_SAMPLE $AZ_BATCHAI_OUTPUT_MODEL')
 ) 
 
# Create the job 
batchai_client.jobs.create(resource_group_name, job_name, parameters).result() 
```

## <a name="monitor-job"></a>Monitorizar tarefa
Pode inspecionar o estado da tarefa com o seguinte comando: 
 
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
Utilize o comando seguinte para listar as ligações aos ficheiros de registo stdout e stderr:

```Python
files = batchai_client.jobs.list_output_files(resource_group_name, job_name, models.JobsListOutputFilesOptions("stdouterr")) 
 
for file in list(files):
     print('file: {0}, download url: {1}'.format(file.name, file.download_url)) 
```
## <a name="delete-resources"></a>Eliminar recursos

Utilize o seguinte comando para eliminar a tarefa:
```Python
batchai_client.jobs.delete(resource_group_name, job_name) 
```

Utilize o seguinte comando para eliminar o cluster:
```Python
batchai_client.clusters.delete(resource_group_name, cluster_name)
```
## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a executar uma tarefa de preparação CNTK num cluster do Batch AI com o SDK Python. Para obter mais informações sobre como utilizar o Batch AI com ferramentas diferentes, veja as [receitas de preparação](https://github.com/Azure/BatchAI).
