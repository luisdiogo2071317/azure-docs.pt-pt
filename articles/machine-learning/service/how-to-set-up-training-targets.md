---
title: Configurar destinos de computação de preparação de modelos com o serviço Azure Machine Learning | Documentos da Microsoft
description: Saiba como selecionar e configurar os ambientes de treinamento (destinos de computação) usados para treinar seus modelos de machine learning. O serviço Azure Machine Learning permite-lhe facilmente ambientes de treinamento do comutador. Comece a formação localmente e, se precisar de aumentar horizontalmente, mude para um destino de computação com base na cloud.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 7eacc475145dac61db1717f1860e22cedd022262
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231452"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Selecione e utilize um destino de computação para preparar o seu modelo

Com o serviço Azure Machine Learning, pode preparar o seu modelo em ambientes diferentes. Estes ambientes, chamados __destinos de computação__, pode ser local ou na cloud. Neste documento, ficará a conhecer os destinos de computação suportados e como utilizá-los.

Um destino de computação é o recurso que executa o script de treinamento ou que aloja o seu modelo quando for implementada como um serviço web. Eles podem ser criados e geridos com o SDK do Azure Machine Learning ou a CLI. Se tiver de destinos de computação que foram criados por outro processo (por exemplo, o portal do Azure ou a CLI do Azure), pode utilizá-los ao anexá-las para a área de trabalho do serviço do Azure Machine Learning.

Pode começar com execuções locais no seu computador e, em seguida, aumentar verticalmente e horizontalmente para outros ambientes, tais como máquinas de virtuais de ciência de dados remotas com GPU ou do Azure Batch AI. 

>[!NOTE]
> Código neste artigo foi testado com o Azure Machine Learning SDK versão 0.168 

## <a name="supported-compute-targets"></a>Destinos de computação suportados

Serviço de Machine Learning do Azure suporta os seguintes destinos de computação:

|Destino de computação| Aceleração por GPU | A otimização de hiper-parâmetros automatizado | Seleção de modelo automatizada | Pode ser utilizado nos pipelines|
|----|:----:|:----:|:----:|:----:|
|[Computador local](#local)| Talvez | &nbsp; | ✓ | &nbsp; |
|[Máquina de Virtual de ciência de dados (DSVM)](#dsvm) | ✓ | ✓ | ✓ | ✓ |
|[O Azure Batch AI](#batch)| ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[O Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>* O azure Databricks e o Azure Data Lake Analytics podem __apenas__ ser usado num pipeline. Para obter mais informações sobre pipelines, consulte a [Pipelines no Azure Machine Learning](concept-ml-pipelines.md) documento.

__[O Azure Container Instances (ACI)](#aci)__  também pode ser utilizado para formar modelos. É uma sem servidor oferta na cloud que é barata e fácil de criar e trabalhar com. ACI não suporta a aceleração da GPU, parâmetro hyper automatizada de otimização, ou automatizada seleção do modelo. Além disso, não pode ser utilizado num pipeline.

Os principais diferenciais entre os destinos de computação são:
* __Aceleração por GPU__: GPUs estão disponíveis com a máquina de Virtual de ciência de dados e o Azure Batch AI. Poderá ter acesso a uma GPU no seu computador local, consoante o hardware, drivers e estruturas que são instaladas.
* __Automatizada de otimização de hiper-parâmetros__: Azure Machine Learning automatizada hiper-parâmetros otimização ajuda-o a encontrar os hiperparâmetros melhor para seu modelo.
* __Automatizada de seleção de modelo__: serviço Azure Machine Learning pode recomendar inteligentemente seleção de algoritmo e hiper-parâmetros ao criar um modelo. Seleção de modelo automatizada ajuda-o a convergir mais rapidamente do que tentar manualmente diferentes combinações para um modelo de alta qualidade. Para obter mais informações, consulte a [Tutorial: preparar automaticamente um modelo de classificação com o Azure automatizada Machine Learning](tutorial-auto-train-models.md) documento.
* __Pipelines__: serviço Azure Machine Learning permite-lhe combinar diferentes tarefas, como o treinamento e a implementação num pipeline. Pipelines podem ser executado em paralelo ou na sequência e fornecem um mecanismo de automação fiável. Para obter mais informações, consulte a [Criar pipelines de aprendizado de máquina com o serviço Azure Machine Learning](concept-ml-pipelines.md) documento.

Pode utilizar o SDK do Azure Machine Learning, a CLI do Azure ou o portal do Azure para criar destinos de computação. Também pode usar destinos de computação existentes ao adicionar (anexar)-os para a área de trabalho.

> [!IMPORTANT]
> Não é possível anexar uma instância de contentores do Azure existente para a área de trabalho. Em vez disso, tem de criar uma nova instância.
>
> Não é possível criar o Azure HDInsight, o Azure Databricks ou o Azure Data Lake Store dentro de uma área de trabalho. Em vez disso, tem de criar o recurso e, em seguida, anexá-lo à área de trabalho.

## <a name="workflow"></a>Fluxo de trabalho

O fluxo de trabalho para desenvolver e implantar um modelo com o Azure Machine Learning segue estes passos:

1. Desenvolva aprendizagem, treinamento scripts em Python.
1. Criar e configurar ou anexar um destino de computação existente.
1. Submeta os scripts de treinamento para o destino de computação.
1. Inspecione os resultados para encontrar o melhor modelo.
1. Registe o modelo no registo do modelo.
1. Implemente o modelo.

> [!IMPORTANT]
> O script de treinamento não está vinculado a um destino de computação específicos. Pode preparar inicialmente no seu computador local e depois mudar de destinos de computação sem ter de reescrever o script de treinamento.

Mudar de destino de computação para outra envolve a criação de um [execute a configuração](concept-azure-machine-learning-architecture.md#run-configuration). A configuração de execução define como executar o script no destino de computação.

## <a name="training-scripts"></a>Scripts de preparação

Quando iniciar uma execução de treinamento, o diretório que contém os scripts de treinamento é submetido. Um instantâneo é criado e enviado para o destino de computação. Para obter mais informações, consulte [instantâneos](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Computador local

Quando o treinamento localmente, utilize o SDK para enviar a operação de treinamento. Pode preparar usando um ambiente de utilizador gerido ou pelo sistema.

### <a name="user-managed-environment"></a>Ambiente gerenciado por utilizador

Num ambiente gerido pelo utilizador, é responsável por assegurar que todos os pacotes necessários estão disponíveis no ambiente do Python, que optar por executar o script. O fragmento de código seguinte é um exemplo de configuração de treinamento para um ambiente de utilizador gerido:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

Para um bloco de notas do Jupyter que demonstra o treinamento num ambiente gerenciado por utilizador, consulte [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).
  
### <a name="system-managed-environment"></a>Ambiente de sistema gerido

Ambientes gerenciados pelo sistema dependem de conda para gerir as dependências. Conda cria um arquivo chamado `conda_dependencies.yml` que contém uma lista de dependências. Em seguida, pode pedir o sistema para criar um novo ambiente de conda e executar seus scripts no mesmo. Ambientes gerenciados pelo sistema podem ser reutilizados mais tarde, contanto que o `conda_dependencies.yml` ficheiros permanece inalterado. 

A configuração inicial, cópia de segurança de um novo ambiente pode demorar vários minutos a concluir, consoante o tamanho das dependências necessárias. O fragmento de código seguinte demonstra a criação de um ambiente de sistema gerido que depende de scikit-Saiba mais:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Para um bloco de notas do Jupyter que demonstra o treinamento num ambiente gerenciados pelo sistema, consulte [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).

## <a id="dsvm"></a>Máquina de Virtual de ciência de dados

Seu computador local não pode ter a computação ou de recursos GPU necessários para preparar o modelo. Nesta situação, pode aumentar ou destinos, como um máquinas virtuais de ciência de dados (DSVM) de computação de aumento horizontal o processo de treinamento, adicionando adicionais.

> [!WARNING]
> O Azure Machine Learning suporta apenas máquinas virtuais com o Ubuntu. Quando criar uma máquina virtual ou selecione um já existente, tem de selecionar um que utiliza o Ubuntu.

Os passos seguintes utilizam o SDK para configurar uma Máquina Virtual de ciência de dados (DSVM) como um destino de treinamento:

1. Criar ou anexar uma Máquina Virtual
    
    * Para criar uma novo DSVM, primeiro verificar se tiver uma DSVM com o mesmo nome, se não criar uma nova VM:
    
        ```python
        from azureml.core.compute import DsvmCompute
        from azureml.core.compute_target import ComputeTargetException

        compute_target_name = 'mydsvm'

        try:
            dsvm_compute = DsvmCompute(workspace = ws, name = compute_target_name)
            print('found existing:', dsvm_compute.name)
        except ComputeTargetException:
            print('creating new.')
            dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
            dsvm_compute = DsvmCompute.create(ws, name = compute_target_name, provisioning_configuration = dsvm_config)
            dsvm_compute.wait_for_completion(show_output = True)
        ```
    * Para anexar uma máquina virtual existente como um destino de computação, tem de fornecer o nome de domínio completamente qualificado, o nome de início de sessão e a palavra-passe para a máquina virtual.  No exemplo, substitua ```<fqdn>``` com o nome de domínio completamente qualificado público da VM ou o endereço IP público. Substitua ```<username>``` e ```<password>``` com o utilizador SSH e a palavra-passe para a VM:

        ```python
        from azureml.core.compute import RemoteCompute

        dsvm_compute = RemoteCompute.attach(ws,
                                        name="attach-dsvm",
                                        username='<username>',
                                        address="<fqdn>",
                                        ssh_port=22,
                                        password="<password>")

        dsvm_compute.wait_for_completion(show_output=True)
    
   It takes around 5 minutes to create the DSVM instance.

1. Create a configuration for the DSVM compute target. Docker and conda are used to create and configure the training environment on DSVM:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image
    # If you want to use GPU in DSVM, you must also use GPU base Docker image azureml.core.runconfig.DEFAULT_GPU_IMAGE
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

1. Para eliminar os recursos de computação quando tiver terminado, utilize o seguinte código:

    ```python
    dsvm_compute.delete()
    ```

Para um bloco de notas do Jupyter que demonstra o treinamento numa máquina de Virtual de ciência de dados, consulte [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb).

## <a id="batch"></a>O Azure Batch AI

Se ele demora muito tempo para preparar o seu modelo, pode utilizar o Azure Batch AI para distribuir o treinamento em clusters de recursos de computação na cloud. O batch AI também pode ser configurado para permitir que um recurso GPU.

O exemplo seguinte procura de um cluster existente do Batch AI por nome. Se houver uma, é criado:

```python
from azureml.core.compute import BatchAiCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
batchai_cluster_name = os.environ.get("BATCHAI_CLUSTER_NAME", ws.name + "gpu")
cluster_min_nodes = os.environ.get("BATCHAI_CLUSTER_MIN_NODES", 1)
cluster_max_nodes = os.environ.get("BATCHAI_CLUSTER_MAX_NODES", 3)
vm_size = os.environ.get("BATCHAI_CLUSTER_SKU", "STANDARD_NC6")
autoscale_enabled = os.environ.get("BATCHAI_CLUSTER_AUTOSCALE_ENABLED", True)


if batchai_cluster_name in ws.compute_targets():
    compute_target = ws.compute_targets()[batchai_cluster_name]
    if compute_target and type(compute_target) is BatchAiCompute:
        print('found compute target. just use it. ' + batchai_cluster_name)
else:
    print('creating a new compute target...')
    provisioning_config = BatchAiCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                vm_priority = 'lowpriority', # optional
                                                                autoscale_enabled = autoscale_enabled,
                                                                cluster_min_nodes = cluster_min_nodes, 
                                                                cluster_max_nodes = cluster_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current BatchAI cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

Para anexar um cluster do Batch AI existente como um destino de computação, tem de fornecer o ID de recurso do Azure. Para obter o ID de recurso no portal do Azure, utilize os seguintes passos:
1. Procure `Batch AI` serviço sob **todos os serviços**
1. Clique no nome da área de trabalho em que o seu cluster pertence
1. Selecione o cluster
1. Clique em **propriedades**
1. Copiar o **ID**

O exemplo seguinte utiliza o SDK para anexar um cluster a sua área de trabalho. No exemplo, substitua `<name>` com qualquer nome para a computação. O nome tem de corresponder ao nome do cluster. Substitua `<resource-id>` com o recurso do Azure ID detalhados acima:

```python
from azureml.core.compute import BatchAiCompute
BatchAiCompute.attach(workspace=ws,
                      name=<name>,
                      resource_id=<resource-id>)
```

Também pode verificar o estado de cluster e de trabalho de IA do Batch com os comandos da CLI do Azure seguintes:

- Verifique o estado do cluster. Pode ver quantos nós estão em execução usando `az batchai cluster list`.
- Verifique o estado da tarefa. Pode ver o número de tarefas estão executando utilizando `az batchai job list`.

Demora cerca de 5 minutos para criar o cluster de IA do Batch.

Para um bloco de notas do Jupyter que demonstra o treinamento num cluster do Batch AI, veja [ https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb).

## <a name='aci'></a>Instância de contentor do Azure (ACI)

O Azure Container Instances são contentores isolados que têm um tempo de inicialização mais rápido e não exigem que o usuário gerir máquinas virtuais. O serviço Azure Machine Learning utiliza contentores de Linux que estão disponíveis nos westus, eastus, westeurope, northeurope, westus2 e southeastasia regiões. Para obter mais informações, consulte [disponibilidade das regiões](https://docs.microsoft.com/azure/container-instances/container-instances-quotas#region-availability). 

O exemplo seguinte mostra como utilizar o SDK para criar um destino de computação do ACI e utilizá-la para preparar um modelo: 

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use ACI to run script.
run_config.target = "containerinstance"

# ACI container group is only supported in certain regions, which can be different than the region the Workspace is in.
run_config.container_instance.region = 'eastus'

# set the ACI CPU and Memory 
run_config.container_instance.cpu_cores = 1
run_config.container_instance.memory_gb = 2

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

# use conda_dependencies.yml to create a conda environment in the Docker image
run_config.environment.python.user_managed_dependencies = False

# auto-prepare the Docker image when used for the first time (if it is not already prepared)
run_config.auto_prepare_environment = True

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Pode demorar entre alguns segundos para alguns minutos para criar um destino de computação do ACI.

Para um bloco de notas do Jupyter que demonstra o treinamento na instância de contentor do Azure, consulte [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb).

## <a id="databricks"></a>O Azure Databricks

O Azure Databricks é um ambiente baseado em Apache Spark na cloud do Azure. Ele pode ser usado como um destino de computação ao treinar modela com um pipeline do Azure Machine Learning.

> [!IMPORTANT]
> Um destino de computação apenas pode ser usado num pipeline de Machine Learning do Azure Databricks.
>
> Tem de criar uma área de trabalho do Azure Databricks antes de o utilizar para preparar o seu modelo. Para criar esses recursos, consulte a [executar uma tarefa do Spark no Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) documento.

Para anexar o Azure Databricks, como um destino de computação, tem de utilizar o SDK do Azure Machine Learning e forneça as seguintes informações:

* __Nome de computação__: O nome que pretende atribuir a este recurso de computação.
* __ID de recurso__: O ID de recurso da área de trabalho do Azure Databricks. O texto seguinte é um exemplo de formato para este valor:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.Databricks/workspaces/<databricks-workspace-name>
    ```

    > [!TIP]
    > Para obter o ID de recurso, utilize o seguinte comando da CLI do Azure. Substitua `<databricks-ws>` com o nome da sua área de trabalho do Databricks:
    > ```azurecli-interactive
    > az resource list --name <databricks-ws> --query [].id
    > ```

* __Token de acesso__: O token de acesso utilizado para autenticar para o Azure Databricks. Para gerar um token de acesso, consulte a [autenticação](https://docs.azuredatabricks.net/api/latest/authentication.html) documento.

O código a seguir demonstra como anexar o Azure Databricks, como um destino de computação:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_resource_id = os.environ.get("AML_DATABRICKS_RESOURCE_ID", "<databricks_resource_id>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_resource_id {}'.format(databricks_resource_id))
    print('databricks_access_token {}'.format(databricks_access_token))
    databricks_compute = DatabricksCompute.attach(
             workspace=ws,
             name=databricks_compute_name,
             resource_id=databricks_resource_id,
             access_token=databricks_access_token
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

O Azure Data Lake Analytics é uma plataforma de análise de macrodados na cloud do Azure. Ele pode ser usado como um destino de computação ao treinar modela com um pipeline do Azure Machine Learning.

> [!IMPORTANT]
> Um Azure Data Lake Analytics só pode ser utilizado o destino de computação num pipeline de Machine Learning.
>
> Tem de criar uma conta do Azure Data Lake Analytics antes de o utilizar para preparar o seu modelo. Para criar este recurso, consulte a [introdução ao Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) documento.

Para anexar como um destino de computação do Data Lake Analytics, tem de utilizar o SDK do Azure Machine Learning e forneça as seguintes informações:

* __Nome de computação__: O nome que pretende atribuir a este recurso de computação.
* __ID de recurso__: O ID de recurso da conta do Data Lake Analytics. O texto seguinte é um exemplo de formato para este valor:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.DataLakeAnalytics/accounts/<datalakeanalytics-name>
    ```

    > [!TIP]
    > Para obter o ID de recurso, utilize o seguinte comando da CLI do Azure. Substitua `<datalakeanalytics>` com o nome do nome da sua conta do Data Lake Analytics:
    > ```azurecli-interactive
    > az resource list --name <datalakeanalytics> --query [].id
    > ```

O código a seguir demonstra como anexar como um destino de computação do Data Lake Analytics:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_id = os.environ.get("AML_ADLA_RESOURCE_ID", "<adla_resource_id>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_id))
    adla_compute = AdlaCompute.attach(
             workspace=ws,
             name=adla_compute_name,
             resource_id=adla_resource_id
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> O Azure Machine Learning pipelines só podem trabalhar com dados armazenados no arquivo de dados predefinido da conta do Data Lake Analytics. Se os dados tiver de com seja de um arquivo de não-padrão, pode utilizar um [ `DataTransferStep` ](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) para copiar os dados antes de treinamento.

## <a id="hdinsight"></a>Anexar um cluster do HDInsight 

HDInsight é uma plataforma popular para análise de macrodados. Ele fornece o Apache Spark, que pode ser utilizado para preparar o seu modelo.

> [!IMPORTANT]
> Tem de criar o cluster de HDInsight antes de o utilizar para preparar o seu modelo. Para criar Spark num cluster do HDInsight, consulte a [criar um Cluster do Spark no HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql) documento.
>
> Ao criar o cluster, tem de especificar um nome de utilizador SSH e a palavra-passe. Tenha em atenção estes valores, conforme seja necessário quando utilizar o HDInsight como um destino de computação.
>
> Depois do cluster ter sido criado, ele tem um nome de domínio completamente qualificado (FQDN) de `<clustername>.azurehdinsight.net`, onde `<clustername>` é o nome que indicou para o cluster. Tem este endereço (ou o endereço IP público do cluster) para utilizá-lo como um destino de computação

Para configurar o HDInsight como um destino de computação, tem de fornecer o nome de domínio completamente qualificado, o nome de início de sessão do cluster e a palavra-passe para o cluster do HDInsight. O exemplo seguinte utiliza o SDK para anexar um cluster a sua área de trabalho. No exemplo, substitua `<fqdn>` com o nome de domínio completamente qualificado público do cluster ou o endereço IP público. Substitua `<username>` e `<password>` com o utilizador SSH e a palavra-passe para o cluster:

> [!NOTE]
> Para localizar o FQDN para o seu cluster, visite o portal do Azure e selecione o cluster do HDInsight. Do __descrição geral__ secção, o FQDN é parte a __URL__ entrada. Basta remover o `https://` desde o início do valor.
>
> ![Captura de ecrã de descrição geral do cluster do HDInsight com a entrada de URL realçada](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute

try:
    # Attaches a HDInsight cluster as a compute target.
    HDInsightCompute.attach(ws,name = "myhdi",
                            address = "<fqdn>",
                            username = "<username>",
                            password = "<password>")
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Submeter treinamento executar

Existem duas formas de submeter uma execução de treinamento:

* Submeter um `ScriptRunConfig` objeto.
* Submeter um `Pipeline` objeto.

> [!IMPORTANT]
> O Azure Databricks e o Azure Datalake Analytics computação destinos só podem ser utilizados num pipeline.
> O destino de computação local não pode ser utilizado num Pipeline.

### <a name="submit-using-scriptrunconfig"></a>Submeter através `ScriptRunConfig`

O padrão de código para submeter um treinamento é executado com `ScriptRunConfig` é o mesmo, independentemente do destino de computação:

* Criar um `ScriptRunConfig` objeto usando a configuração de execução para o destino de computação.
* Submeta a execução.
* Aguarde a execução concluir.

O exemplo seguinte utiliza a configuração para o destino de computação local gerenciados pelo sistema que criou anteriormente neste documento:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```

Para um bloco de notas do Jupyter que demonstra o treinamento com o Spark no HDInsight, consulte [ https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb ](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb).

### <a name="submit-using-a-pipeline"></a>Submeter através de um pipeline

O código padrão para submeter um treinamento é executado com um pipeline é o mesmo, independentemente do destino de computação:

* Adicione um passo para o pipeline para o recurso de computação.
* Submeta uma execução com o pipeline.
* Aguarde a execução concluir.

O exemplo seguinte utiliza o destino de computação do Azure Databricks criado anteriormente neste documento:

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# list of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Para obter mais informações sobre o machine learning pipelines do, consulte a [Pipelines e o Azure Machine Learning](concept-ml-pipelines.md) documento.

Por exemplo os blocos de notas do Jupyter que demonstram o treinamento com um pipeline, veja [ https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline ](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Ver e configurar a computação no portal do Azure

Pode ver o que a computação de destinos estão associados a sua área de trabalho do portal do Azure. Para obter a lista, utilize os seguintes passos:

1. Visite o [portal do Azure](https://portal.azure.com) e navegue para a área de trabalho.
2. Clique no __computação__ ligação sob o __aplicativos__ secção.

    ![Separador de computação do Vista](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Criar um destino de computação

Siga os passos acima para ver a lista de destinos de computação e, em seguida, utilize os seguintes passos para criar um destino de computação:

1. Clique nas __+__ iniciar para adicionar um destino de computação.

    ![Adicionar computação ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Introduza um nome para o destino de computação.
1. Selecione o tipo de computação para anexar para __treinamento__. 

    > [!IMPORTANT]
    > Nem todos de computação tipos podem ser criados com o portal do Azure. Atualmente, os tipos que podem ser criados para treinamento são:
    > 
    > * Máquina Virtual
    > * Batch AI

1. Selecione __criar novo__ e preencha o formulário necessária. 
1. Selecione __Criar__
1. Pode ver o estado da operação de criação ao selecionar o destino de computação a partir da lista.

    ![Veja a lista de computação](./media/how-to-set-up-training-targets/View_list.png) , em seguida, verá os detalhes para o destino de computação.
    ![Ver detalhes](./media/how-to-set-up-training-targets/vm_view.PNG)
1. Agora pode enviar uma execução em relação a esses destinos conforme detalhado acima.

### <a name="reuse-existing-compute-in-your-workspace"></a>Reutilização de computação existente na sua área de trabalho

Siga os passos acima para ver a lista de destinos de computação e, em seguida, utilize os seguintes passos para reutilizar o destino de computação:

1. Clique nas **+** iniciar para adicionar um destino de computação.
2. Introduza um nome para o destino de computação.
3. Selecione o tipo de computação para anexar para treinamento.

    > [!IMPORTANT]
    > Nem todos de computação tipos podem ser anexados utilizando o portal.
    > Atualmente, os tipos que podem ser anexados para treinamento são:
    > 
    > * Máquina Virtual
    > * Batch AI

1. Selecione "Utilizar existente".
    - Ao anexar clusters de IA do Batch, selecione o destino de computação na lista pendente, selecione a área de trabalho de IA do Batch e o Cluster de IA do Batch e, em seguida, clique em **criar**.
    - Ao anexar uma Máquina Virtual, introduza o endereço IP, a combinação de nome de utilizador/palavra-passe, chaves pública/privada e a porta e clique em criar.

    > [!NOTE]
    > A Microsoft recomenda que utilize chaves SSH, conforme forem mais seguras do que as palavras-passe. As senhas são vulneráveis a ataques de força bruta, enquanto as chaves SSH se baseiam em assinaturas criptográficas. Para obter informações sobre como criar chaves SSH para utilização com máquinas virtuais do Azure, veja os documentos seguintes:
    >
    > * [Criar e utilizar chaves SSH no Linux ou macOS]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Criar e utilizar chaves SSH no Windows]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

5. Pode ver o estado do Estado de aprovisionamento ao selecionar o destino de computação a partir da lista.
6. Agora pode enviar uma execução em relação a esses destinos.

## <a name="examples"></a>Exemplos
Os seguintes blocos de notas demonstram os conceitos deste artigo:
* [01.Getting-Started/02.Train-on-local/02.Train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)
* [01.Getting-Started/04.Train-on-remote-VM/04.Train-on-remote-VM.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm)
* [01.Getting-Started/03.Train-on-aci/03.Train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci)
* [01.Getting-Started/05.Train-in-spark/05.Train-in-spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark)
* [Tutorials/01.Train-models.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb)

Obtenha estes blocos de notas: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Referência do SDK do Machine Learning do Azure](https://aka.ms/aml-sdk)
* [Tutorial: preparar um modelo](tutorial-train-models-with-aml.md)
* [Onde pretende implementar modelos](how-to-deploy-and-where.md)
* [Criar machine learning pipelines com o serviço Azure Machine Learning](concept-ml-pipelines.md)
