---
title: Destinos de computação para a preparação de modelos
titleSuffix: Azure Machine Learning service
description: Configure os ambientes de treinamento (destinos de computação) para a preparação de modelos de aprendizagem automática. Pode alternar facilmente entre ambientes de treinamento. Começar a formação localmente. Se precisar de aumentar horizontalmente, mude para um destino de computação com base na cloud.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 9037f6d7602f186bc30e55acbc050280bca134ee
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794469"
---
# <a name="set-up-compute-targets-for-model-training"></a>Configurar destinos de computação de preparação de modelos

Com o serviço Azure Machine Learning, pode treinar o modelo em recursos de computação diferentes. Estes recursos, chamados de computação __destinos de computação__, pode ser local ou na cloud. Neste artigo, ficará a conhecer os destinos de computação suportados e como utilizá-los.

Um destino de computação é um recurso onde executar seu script de treinamento ou que aloja o modelo quando for implementada como um serviço web. Pode criar e gerir um destino de computação com o SDK do Azure Machine Learning, o portal do Azure ou a CLI do Azure. Se tiver de destinos de computação criados por meio de outro serviço (por exemplo, um cluster do Azure HDInsight), pode utilizar esses destinos, anexando-los para sua área de trabalho do serviço do Azure Machine Learning.

Existem três amplas categorias de destinos de computação que suporta o Azure Machine Learning:

* __Local__: Seu computador local ou uma baseado na nuvem máquina virtual (VM) que usa como um ambiente de desenvolvimento e experimentação. 
* __Gerido computação__: Computação do Azure Machine Learning é uma oferta de computação que é gerenciada pelo serviço Azure Machine Learning. A oferta permite-lhe criar facilmente uma computação única ou vários nó de treinamento, teste e inferência de batch.
* __Ligado a computação__: Também pode trazer a sua própria computação em nuvem do Azure e ligá-los para o Azure Machine Learning. Leia mais sobre computação tipos suportados e como usá-las nas secções seguintes.


## <a name="supported-compute-targets"></a>Destinos de computação suportados

O serviço do Azure Machine Learning tem suporte diferentes em vários destinos de computação. Inicia um ciclo de vida de desenvolvimento do modelo típico com desenvolvimento e experimentação numa pequena quantidade de dados. Nesta fase, recomendamos que utilize um ambiente local, como o seu computador local ou uma VM com base na cloud. À medida que aumenta verticalmente o seu treinamento em conjuntos de dados maiores, ou treinamento distribuído, usar um Azure Machine Learning computação ambiente para criar um único ou com vários nós do cluster que é dimensionado automaticamente sempre que enviar uma execução. Também pode anexar seus próprios recursos de computação, embora o suporte para vários cenários podem variar como descrito na tabela a seguir:

|Destino de computação| Aceleração por GPU | Automatizada<br/> a otimização de hiper-parâmetros | Automatizada</br> machine learning | Amigável de pipeline|
|----|:----:|:----:|:----:|:----:|
|[Computador local](#local)| Talvez | &nbsp; | ✓ | &nbsp; |
|[Computação do Azure Machine Learning](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[VM remota](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | ✓ | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[O Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ _O Azure Databricks e o Azure Data Lake Analytics só podem ser utilizado num pipeline._<br/>
> Para obter mais informações sobre pipelines, consulte [Pipelines no Azure Machine Learning](concept-ml-pipelines.md).
>
> O ambiente de computação do Azure Machine Learning tem de ser criado dentro de uma área de trabalho. Não é possível anexar instâncias existentes para uma área de trabalho.
>
> Outros destinos de computação tem de ser criados fora do Azure Machine Learning e, em seguida, ligados à sua área de trabalho.
>
> Quando preparar um modelo, alguns de computação destinos dependem de imagens de contentor do Docker. A imagem de base de GPU deve ser utilizada apenas em serviços do Microsoft Azure. Para a preparação de modelos, os serviços incluem:
> * Computação do Azure Machine Learning
> * Serviço Kubernetes do Azure
> * Máquina de Virtual de ciência de dados de Windows (DSVM)

## <a name="workflow"></a>Fluxo de trabalho

O fluxo de trabalho para desenvolver e implantar um modelo com o Azure Machine Learning segue estes passos:

1. Desenvolva os scripts do machine learning treinamento em Python.
1. Criar e configurar o destino de computação ou anexar um destino de computação existente.
1. Submeta os scripts de treinamento para o destino de computação.
1. Inspecione os resultados para encontrar o melhor modelo.
1. Registe o modelo no registo do modelo.
1. Implemente o modelo.

> [!NOTE]
> O script de treinamento não está vinculado a um destino de computação específicos. Pode preparar inicialmente no seu computador local e depois mudar de destinos de computação sem ter de reescrever o script de treinamento.
> 
> Quando associa um destino de computação com a sua área de trabalho através da criação de uma gerida de computação ou anexando uma computação existente, forneça um nome para a computação. O nome deve ter entre dois e 16 carateres de comprimento.

Para mudar a partir do destino de computação para outra, é necessário um [execute a configuração](concept-azure-machine-learning-architecture.md#run-configuration). A configuração de execução define como executar o script no destino de computação.

## <a name="training-scripts"></a>Scripts de preparação

Quando iniciar uma execução de treinamento, ele cria um instantâneo do diretório que contém os scripts de treinamento e envia-os para o destino de computação. Para obter mais informações, consulte [instantâneos](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Computador local

Quando preparar localmente, utilize o SDK para enviar a operação de treinamento. Pode preparar usando um ambiente de utilizador gerido ou pelo sistema.

### <a name="user-managed-environment"></a>Ambiente gerenciado por utilizador

Num ambiente gerido pelo utilizador, certifique-se de que todos os pacotes necessários estão disponíveis no ambiente de Python em que executou o script. O fragmento de código seguinte é um exemplo de como configurar o treinamento para um ambiente de utilizador gerido:

```python
from azureml.core.runconfig import RunConfiguration

# Edit a run configuration property on the fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# Choose a specific Python environment by pointing to a Python path. For example:
# run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Ambiente de sistema gerido

Ambientes gerenciados pelo sistema dependem de conda para gerir as dependências. Conda cria um ficheiro denominado **conda_dependencies.yml** que contém uma lista de dependências. Pode pedir o sistema para criar um novo ambiente de conda e executar os seus scripts lá. Ambientes gerenciados pelo sistema podem ser reutilizados mais tarde, desde que o ficheiro de conda_dependencies.yml é alterado. 

A configuração inicial, cópia de segurança de um novo ambiente pode demorar vários minutos a concluir, com base no tamanho das dependências necessárias. O fragmento de código seguinte demonstra como criar um ambiente de gerenciados pelo sistema de mensagens em fila que depende de scikit-Saiba mais:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify the conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Computação do Azure Machine Learning

Computação do Azure Machine Learning é uma infraestrutura de computação gerida que permite ao utilizador criar facilmente uma única ou vários nó de computação. A computação é criada na sua região de área de trabalho como um recurso que pode ser compartilhado com outros utilizadores na sua área de trabalho. A computação verticalmente automaticamente quando for submetida uma tarefa e pode ser colocada numa rede virtual do Azure. A computação é executado num ambiente em contentores e empacota suas dependências de modelo num contentor do Docker.

Pode usar a computação do Azure Machine Learning para distribuir o processo de treinamento em clusters de nós de computação de CPU ou de GPU na cloud. Para obter mais informações sobre os tamanhos VM que incluem as GPUs, consulte [tamanhos de máquinas de virtuais com otimização de GPU](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

> [!NOTE]
> Computação do Azure Machine Learning tem limites predefinidos, como o número de núcleos que podem ser alocados. Para obter mais informações, consulte [quotas de pedido e gerir recursos do Azure](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

Pode criar um ambiente de computação do Azure Machine Learning a pedido quando agendar uma execução ou como um recurso persistente.

### <a name="run-based-creation"></a>Criação baseados em execução

Pode criar um ambiente de computação do Azure Machine Learning como um destino de computação no tempo de execução. A computação é criada automaticamente para a execução e se aumentar verticalmente para o número de **max_nodes** que especificou na sua configuração de execução. A computação é eliminada automaticamente depois da execução for concluída.

> [!IMPORTANT]
> Criação de um ambiente de computação do Azure Machine Learning baseados em execução está atualmente em pré-visualização. Não utilize a criação com base em execução, se utilizar a otimização de hiper-parâmetros automatizado ou automatizada de aprendizagem automática. Para utilizar a otimização de hiper-parâmetros ou aprendizagem automática, crie o ambiente de computação do Azure Machine Learning antes de submeter uma execução.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

# First, list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# Create a new runconfig object
run_config = RunConfiguration()

# Signal that you want to use AmlCompute to execute the script
run_config.target = "amlcompute"

# AmlCompute is created in the same region as your workspace
# Set the VM size for AmlCompute from the list of supported_vmsizes
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Computação persistente: Básica

Um ambiente de computação do Azure Machine Learning persistente pode ser reutilizado em tarefas. A computação pode ser compartilhada com outros utilizadores na área de trabalho e é mantida entre tarefas.

Para criar um recurso de ambiente de computação do Azure Machine Learning persistente, tem de especificar o **vm_size** e **max_nodes** propriedades. O Azure Machine Learning utiliza, em seguida, predefinições inteligentes para as outras propriedades. A computação é dimensionado automaticamente para zero nós quando não é utilizado e cria dedicada VMs para executarem as tarefas conforme necessário. 

* **vm_size**: A família VM de nós criados pela computação do Azure Machine Learning.
* **max_nodes**: O número máximo de nós para dimensionamento automático até ao executar uma tarefa na computação do Azure Machine Learning.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Computação persistente: Avançado

Também pode configurar várias propriedades avançadas ao criar um ambiente de computação do Azure Machine Learning. As propriedades permitem-lhe criar um cluster persistente de tamanho fixo, ou dentro de uma rede virtual do Azure existente na sua subscrição.

Juntamente com o **vm_size** e **max_nodes** propriedades, também pode utilizar as seguintes propriedades:

* **min_nodes**: O número mínimo de nós para reduzir verticalmente para quando executar uma tarefa num ambiente de computação do Azure Machine Learning. O mínimo padrão é zero (0) nós.
* **vm_priority**: O tipo de VM para utilizar quando criar um recurso de ambiente de computação do Azure Machine Learning. Escolha entre **dedicado** (predefinição) e **lowpriority**. VMs de baixa prioridade utilizam o excesso de capacidade no Azure. Estas VMs são mais baratas, mas podem ser pre-empted execuções quando estas VMs são utilizadas.
* **idle_seconds_before_scaledown**: A quantidade de tempo de inatividade de espera após a conclusão de uma execução e antes da dimensionamento automático até ao número de **min_nodes**. O tempo de inatividade de predefinição é 120 segundos.
* **vnet_resourcegroup_name**: O grupo de recursos do __existente__ rede virtual. O ambiente de computação do Azure Machine Learning é criado dentro desta rede virtual.
* **vnet_name**: O nome da rede virtual. A rede virtual tem de estar na mesma região que a sua área de trabalho do Azure Machine Learning.
* **subnet_name**: O nome da sub-rede na rede virtual. Os recursos de ambiente de computação do Azure Machine Learning são atribuídos endereços IP deste intervalo de sub-rede.

> [!TIP]
> Quando cria um recurso de ambiente de computação do Azure Machine Learning persistente, é possível atualizar as propriedades, como o número de **min_nodes** ou **max_nodes**. Para atualizar uma propriedade, chame o `update()` função para a propriedade.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist 
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           vm_priority='lowpriority',
                                                           min_nodes=2,
                                                           max_nodes=4,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```


## <a id="vm"></a>VM remota

O Azure Machine Learning também suporta a colocarem os seus próprios recursos de computação e ligá-la à sua área de trabalho. Um esse tipo de recurso é uma VM remota arbitrária, desde que esteja acessível a partir do serviço Azure Machine Learning. O recurso pode ser uma VM do Azure, um servidor remoto na sua organização ou no local. Especificamente, dado o endereço IP e as credenciais (nome de utilizador e palavra-passe ou chave SSH), pode usar qualquer VM acessível para execuções de remotas.
Pode utilizar um ambiente de sistema criado conda, um ambiente de Python existente ou um contentor do Docker. Quando executar através de um contentor do Docker, tem de ter o motor do Docker em execução na VM. A funcionalidade VM remota é especialmente útil quando desejar um baseado na nuvem experimentação do ambiente de desenvolvimento e que é mais flexível do que o seu computador local.

> [!TIP]
> Utilize a DSVM como VM do Azure à escolha para este cenário. Esta VM é uma ciência de dados pré-configurado e ambiente de desenvolvimento de IA do Azure a VM oferece uma opção organizada de ferramentas e estruturas para o ciclo de vida completo de machine learning de desenvolvimento. Para obter mais informações sobre como utilizar a DSVM com o Azure Machine Learning, consulte [configurar um ambiente de desenvolvimento](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

> [!WARNING]
> O Azure Machine Learning suporta apenas máquinas virtuais que executar o Ubuntu. Quando criar uma VM ou escolha uma VM existente, tem de selecionar uma VM que utiliza o Ubuntu.

Os passos seguintes utilizam o SDK para configurar uma DSVM do como um destino de treinamento:

1. Para anexar uma máquina virtual existente como um destino de computação, tem de fornecer o nome de domínio completamente qualificado (FQDN), o nome de utilizador e a palavra-passe para a máquina virtual. No exemplo, substitua \<fqdn > com o FQDN público da VM ou o endereço IP público. Substitua \<nome de utilizador > e \<palavra-passe > com o nome de utilizador SSH e palavra-passe para a VM.

    ```python
    from azureml.core.compute import RemoteCompute, ComputeTarget
    
    # Create the compute config
    attach_config = RemoteCompute.attach_configuration(address = "ipaddress",
                                                       ssh_port=22,
                                                       username='<username>',
                                                       password="<password>")

    # If you use SSH instead of a password, use this code:
    #                                                  ssh_port=22,
    #                                                  username='<username>',
    #                                                  password=None,
    #                                                  private_key_file="path-to-file",
    #                                                  private_key_passphrase="passphrase")

    # Attach the compute target
    compute = ComputeTarget.attach(ws, "attach-dsvm", attach_config)

    compute.wait_for_completion(show_output=True)
    ```

1. Crie uma configuração para o destino de computação DSVM. Docker e conda são utilizados para criar e configurar o ambiente de treinamento em DSVM.

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load into memory the cpu-dsvm.runconfig file created in the previous attach operation
    run_config = RunConfiguration(framework = "python")

    # Set the compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use the CPU base image
    # To use GPU in DSVM, you must also use the GPU base Docker image "azureml.core.runconfig.DEFAULT_GPU_IMAGE"
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask the system to provision a new conda environment based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when they're used for the first time
    run_config.prepare_environment = True

    # Specify the CondaDependencies object
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>O Azure Databricks

O Azure Databricks é um ambiente baseado em Apache Spark na cloud do Azure. O ambiente pode ser utilizado como um destino de computação quando preparar modelos com um pipeline do Azure Machine Learning.

> [!IMPORTANT]
> Um destino de computação apenas pode ser usado num pipeline de Machine Learning do Azure Databricks.
>
> Tem de criar uma área de trabalho do Azure Databricks antes de o utilizar para preparar o seu modelo. Para criar esses recursos, veja [executar uma tarefa do Spark no Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Para anexar o Azure Databricks, como um destino de computação, tem de utilizar o SDK do Azure Machine Learning e forneça as seguintes informações:

* __Nome de computação__: O nome a atribuir a este recurso de computação.
* __Nome de área de trabalho do Databricks__: O nome da área de trabalho do Azure Databricks.
* __Token de acesso__: O token de acesso utilizado para autenticar para o Azure Databricks. Para gerar um token de acesso, consulte [autenticação](https://docs.azuredatabricks.net/api/latest/authentication.html).

O código a seguir demonstra como anexar o Azure Databricks, como um destino de computação:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get("AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get("AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create the attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group = databricks_resource_group,
                                                           workspace_name = databricks_workspace_name,
                                                           access_token = databricks_access_token)
    databricks_compute = ComputeTarget.attach(
             ws,
             databricks_compute_name,
             attach_config
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

O Azure Data Lake Analytics é uma plataforma de análise de macrodados na cloud do Azure. A plataforma pode ser utilizada como um destino de computação quando preparar modelos com um pipeline do Azure Machine Learning.

> [!IMPORTANT]
> Um Azure Data Lake Analytics só pode ser utilizado o destino de computação num pipeline de Machine Learning.
>
> Tem de criar uma conta do Azure Data Lake Analytics antes de o utilizar para preparar o seu modelo. Para criar este recurso, veja [introdução ao Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Para anexar como um destino de computação do Data Lake Analytics, tem de utilizar o SDK do Azure Machine Learning e forneça as seguintes informações:

* __Nome de computação__: O nome a atribuir a este recurso de computação.
* __Grupo de recursos__: O grupo de recursos que contém a conta do Data Lake Analytics.
* __Nome da conta__: O nome da conta do Data Lake Analytics.

O código a seguir demonstra como anexar como um destino de computação do Data Lake Analytics:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get("AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get("AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    
    # Create the attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach the ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> O Azure Machine Learning pipelines só funcionam com dados armazenados no arquivo de dados predefinido da conta do Data Lake Analytics. Se os dados necessários num armazenamento de não-padrão, pode usar um [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) operação para copiar os dados antes de preparar o modelo.

## <a id="hdinsight"></a>O Azure HDInsight 

O Azure HDInsight é uma plataforma popular para análise de macrodados. A plataforma fornece Apache Spark, que pode ser utilizado para preparar o seu modelo.

> [!IMPORTANT]
> Tem de criar o cluster de HDInsight antes de o utilizar para preparar o seu modelo. Para criar Spark num cluster do HDInsight, veja [criar um Cluster do Spark no HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Quando criar o cluster, tem de especificar um nome de utilizador SSH e a palavra-passe. Tome nota destes valores, sempre que precisar para utilizar o HDInsight como um destino de computação.
>
> Depois do cluster for criado, ele tem o FQDN \<clustername >. azurehdinsight.net, onde \<clustername > é o nome que indicou para o cluster. É necessário o endereço FQDN (ou o endereço IP público do cluster) para utilizar o cluster como um destino de computação.

Para configurar o HDInsight como um destino de computação, tem de fornecer o FQDN, nome de utilizador e palavra-passe para o cluster do HDInsight. O exemplo seguinte utiliza o SDK para anexar um cluster a sua área de trabalho. No exemplo, substitua \<fqdn > com o FQDN público do cluster ou o endereço IP público. Substitua \<nome de utilizador > e \<palavra-passe > com o nome de utilizador SSH e palavra-passe para o cluster.

> [!NOTE]
> Para localizar o FQDN para o seu cluster, vá ao portal do Azure e selecione o cluster do HDInsight. Sob __descrição geral__, pode ver o FQDN no __URL__ entrada. Para obter o FQDN, remova a https:\/prefixo desde o início da entrada.

![Obtenha o FQDN para um cluster do HDInsight no portal do Azure](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute, ComputeTarget

try:
    # Attach an HDInsight cluster as a compute target
    attach_config = HDInsightCompute.attach_configuration(address = "fqdn-or-ipaddress",
                                                          ssh_port = 22,
                                                          username = "username",
                                                          password = None, #if using ssh key
                                                          private_key_file = "path-to-key-file",
                                                          private_key_phrase = "key-phrase")
    compute = ComputeTarget.attach(ws, "myhdi", attach_config)
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure the HDInsight run
# Load the runconfig object from the myhdi.runconfig file generated in the previous attach operation
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# Ask the system to prepare the conda environment automatically when it's used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-runs"></a>Submeter execuções de preparação

Existem duas formas de submeter uma execução de treinamento:

* Submeter um treinamento execute com um `ScriptRunConfig` objeto.
* Submeter um treinamento execute com um `Pipeline` objeto.

> [!IMPORTANT]
> O Azure Databricks e o Azure Datalake Analytics computação destinos só podem ser utilizados num pipeline.
>
> O destino de computação local não pode ser utilizado num pipeline.

### <a name="scriptrunconfig-object"></a>Objeto de ScriptRunConfig

O padrão de código para submeter um treinamento ser executado com o `ScriptRunConfig` objeto é o mesmo para todos os tipos de destinos de computação:

1. Criar um `ScriptRunConfig` objeto ao utilizar a configuração de execução para o destino de computação.
1. Submeta a execução.
1. Aguarde a execução concluir.

O exemplo seguinte utiliza a configuração para o destino de computação local gerenciados pelo sistema criado anteriormente:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="pipeline-object"></a>Objeto de pipeline

O padrão de código para submeter um treinamento ser executado com um `Pipeline` objeto é o mesmo para todos os tipos de destinos de computação:

1. Adicione um passo para o `Pipeline` objeto para o recurso de computação.
1. Ao utilizar o pipeline de envio de uma execução.
1. Aguarde a execução concluir.

O exemplo seguinte utiliza o destino de computação do Azure Databricks criado anteriormente:

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

# List of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Para obter mais informações sobre o machine learning pipelines do, veja [Pipelines e o Azure Machine Learning](concept-ml-pipelines.md).

Por exemplo os blocos de notas do Jupyter que demonstram como preparar um modelo através da utilização de um pipeline, veja [ https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline ](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="access-computes-in-the-azure-portal"></a>Acesso computa no portal do Azure

Pode acessar os destinos de computação que estão associados a sua área de trabalho no portal do Azure. 

### <a name="view-compute-targets"></a>Destinos de computação do Vista

Para ver os destinos de computação para a área de trabalho, utilize os seguintes passos:

1. Navegue para o [portal do Azure](https://portal.azure.com) e abra a área de trabalho.
1. Sob __aplicativos__, selecione __computação__.

    ![Destinos de computação do Vista](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Criar um destino de computação

Siga os passos anteriores para ver a lista de destinos de computação. Em seguida, utilize estes passos para criar um destino de computação:

1. Selecione o sinal de adição (+) para adicionar um destino de computação.

    ![Adicionar um destino de computação](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Introduza um nome para o destino de computação.
1. Selecione **computação do Machine Learning** como o tipo de computação a utilizar para __treinamento__.

    > [!IMPORTANT]
    > Só pode criar um ambiente de computação do Azure Machine Learning como o recurso de computação geridos para treinamento.

1. Preencha o formulário. Fornecer valores para as propriedades necessárias, especialmente **família de VM**e o **número máximo de nós** usar para acelerar a computação. 
1. Selecione __Criar__.
1. Ver o estado da operação de criação ao selecionar o destino de computação a partir da lista:

    ![Selecione um destino de computação para ver o estado da operação criar](./media/how-to-set-up-training-targets/View_list.png)

1. , Em seguida, ver os detalhes para o destino de computação:

    ![Ver os detalhes do computador de destino](./media/how-to-set-up-training-targets/compute-target-details.png)

Agora pode enviar uma execução contra os destinos de computador, tal como descrito anteriormente.


### <a name="reuse-existing-compute-targets"></a>Reutilizar os destinos de computação existente

Siga as etapas descritas anteriormente para ver a lista de destinos de computação. Em seguida, siga estas etapas para reutilizar um destino de computação:

1. Selecione o sinal de adição (+) para adicionar um destino de computação.
1. Introduza um nome para o destino de computação.
1. Selecione o tipo de computação para anexar para __treinamento__:

    > [!IMPORTANT]
    > Nem todos de computação tipos podem ser anexados a partir do portal do Azure.
    > Os tipos de computação que podem ser anexados atualmente para treinamento incluem:
    >
    > * VM remota
    > * Azure Databricks
    > * Azure Data Lake Analytics
    > * Azure HDInsight

1. Preencha o formulário e forneça valores para as propriedades necessárias.

    > [!NOTE]
    > A Microsoft recomenda que utilize chaves SSH, que são mais seguras do que as palavras-passe. As palavras-passe são vulneráveis a ataques de força bruta. Chaves SSH baseiam-se em assinaturas criptográficas. Para obter informações sobre como criar chaves SSH para utilização com máquinas virtuais do Azure, veja os documentos seguintes:
    >
    > * [Criar e utilizar chaves SSH no Linux ou macOS](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Criar e utilizar chaves SSH no Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Selecione __anexar__.
1. Ver o estado da operação de anexar ao selecionar o destino de computação da lista.

Agora pode enviar uma execução em relação a esses destinos de computação conforme descrito anteriormente.

## <a name="notebook-examples"></a>Exemplos de bloco de notas

Para obter exemplos, consulte os blocos de notas nas seguintes localizações:

* [procedimentos-to-use-azureml/treinamento](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [tutoriais/img-classificação-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Referência do SDK do Machine Learning do Azure](https://aka.ms/aml-sdk)
* [Tutorial: Preparar um modelo](tutorial-train-models-with-aml.md)
* [Onde pretende implementar modelos](how-to-deploy-and-where.md)
* [Criar machine learning pipelines com o serviço Azure Machine Learning](concept-ml-pipelines.md)
