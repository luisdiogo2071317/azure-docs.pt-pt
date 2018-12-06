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
ms.date: 12/04/2018
ms.openlocfilehash: 45a5e4c895a0c7a8f76bb34aa5aaf22fa31f4333
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864864"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Selecione e utilize um destino de computação para preparar o seu modelo

Com o serviço Azure Machine Learning, pode treinar o modelo em recursos de computação diferentes. Estes recursos, chamados de computação __destinos de computação__, pode ser local ou na cloud. Neste documento, aprenderá sobre os destinos de computação suportados e como utilizá-los.

Um destino de computação é um recurso onde é executado o script de treinamento, ou seu modelo será alojado quando implementado como um serviço web. Pode criar e gerir um destino de computação com o SDK do Azure Machine Learning, o portal do Azure ou a CLI do Azure. Se tiver de destinos de computação que foram criados por meio de outro serviço (por exemplo, um cluster do HDInsight), pode utilizá-los ao anexá-las para a área de trabalho do serviço do Azure Machine Learning.

Existem três amplas categorias de destinos de computação que suporta o Azure Machine Learning:

* __Local__: seu computador local ou uma VM com base na cloud que utiliza como um ambiente de dev/experimentação. 

* __Gerido computação__: computação do Azure Machine Learning é uma oferta de computação que é gerenciada pelo serviço Azure Machine Learning. Permite-lhe criar facilmente computação única ou várias node para o treinamento, teste e inferência de batch.

* __Ligado a computação__: também pode trazer a sua própria computação em nuvem do Azure e ligá-los para o Azure Machine Learning. Leia mais abaixo em tipos de computação suportados e como utilizá-los.


## <a name="supported-compute-targets"></a>Destinos de computação suportados

O serviço do Azure Machine Learning tem suporte diferentes em vários destinos de computação. Inicia um ciclo de vida de desenvolvimento do modelo típico com dev/experimentação numa pequena quantidade de dados. Nesta fase, recomendamos que utilize um ambiente local. Por exemplo, seu computador local ou uma VM com base na cloud. À medida que aumentar verticalmente o seu treinamento em conjuntos de dados maiores ou fazer o treinamento distribuído, recomendamos que utilize a computação do Azure Machine Learning para criar um cluster único ou vários node que é dimensionado automaticamente sempre que submete uma execução. Também pode anexar seus próprios recursos de computação, embora o suporte para vários cenários podem variar como detalhadas abaixo:

|Destino de computação| Aceleração por GPU | A otimização de hiper-parâmetros automatizado | Aprendizagem automática automatizada | Amigável de pipeline|
|----|:----:|:----:|:----:|:----:|
|[Computador local](#local)| Talvez | &nbsp; | ✓ | &nbsp; |
|[Computação do Azure Machine Learning](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[VM remota](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure Data Lake Analytics](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[O Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ O Azure Databricks e o Azure Data Lake Analytics podem __apenas__ ser usado num pipeline. Para obter mais informações sobre pipelines, consulte a [Pipelines no Azure Machine Learning](concept-ml-pipelines.md) documento.

> [!IMPORTANT]
> Computação do Azure Machine Learning tem de ser criada de dentro de uma área de trabalho. Não é possível anexar instâncias existentes para uma área de trabalho.
>
> Outros destinos de computação tem de ser criados fora do Azure Machine Learning e, em seguida, ligados à sua área de trabalho.

> [!NOTE]
> Alguns de computação destinos dependem de imagens de contentor do Docker ao preparar um modelo. A imagem de base de GPU deve ser utilizada apenas em serviços do Microsoft Azure. Para a preparação de modelos, estes serviços são:
>
> * Computação do Azure Machine Learning
> * Serviço Kubernetes do Azure
> * A Máquina Virtual de ciência de dados.

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

> [!TIP]
> Sempre que associa um destino de computação com a sua área de trabalho, criando gerido, computação ou anexar computação existente, tem de fornecer um nome para a computação. Isso deve ser entre 2 e 16 carateres de comprimento.

Mudar de destino de computação para outra envolve a criação de um [execute a configuração](concept-azure-machine-learning-architecture.md#run-configuration). A configuração de execução define como executar o script no destino de computação.

## <a name="training-scripts"></a>Scripts de preparação

Quando iniciar uma execução de formação, um instantâneo do diretório que contém os scripts de treinamento é criado e enviado para o destino de computação. Para obter mais informações, consulte [instantâneos](concept-azure-machine-learning-architecture.md#snapshot).

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

## <a id="amlcompute"></a>Computação do Azure Machine Learning

Computação do Azure Machine Learning é uma infraestrutura de computação gerida que permite ao utilizador criar facilmente computação único - para várias - node. Ele é criado __dentro de sua região de área de trabalho__ e é um recurso que pode ser compartilhado com outros utilizadores na sua área de trabalho. Dimensionamento automaticamente quando for submetida uma tarefa e pode ser colocada numa rede Virtual do Azure. Ele é executado num __ambiente em contentores__, a empacotar as dependências do seu modelo num contentor do Docker.

Pode usar a computação do Azure Machine Learning para distribuir o processo de treinamento em clusters de nós de computação de CPU ou de GPU na cloud. Para obter mais informações sobre os tamanhos VM que incluem as GPUs, consulte a [tamanhos de máquinas virtuais com otimização de GPU](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu) documentação.

> [!NOTE]
> Computação do Azure Machine Learning tem limites predefinidos no coisas como o número de núcleos que podem ser atribuídas. Para obter mais informações, consulte a [quotas de pedido e gerir recursos do Azure](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas) documento.

Pode criar a computação do Azure Machine Learning sob demanda quando agendar uma execução, ou como um recurso persistente.

### <a name="run-based-creation"></a>Criação baseados em execução

Pode criar a computação do Azure Machine Learning como um destino de computação em tempo de execução. Neste caso, a computação é criada automaticamente para a execução, dimensiona até max_nodes que especificou na sua configuração de execução e, em seguida, é __eliminados automaticamente__ após a execução for concluída.

Esta funcionalidade está atualmente em estado de pré-visualização e não irá funcionar com as tarefas de otimização de hiper-parâmetros ou Machine Learning automatizada.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

#Let us first list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use AmlCompute to execute script.
run_config.target = "amlcompute"

# AmlCompute will be created in the same region as workspace. Set vm size for AmlCompute from the list returned above
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Computação persistente (básico)

Um persistente computação do Azure Machine Learning podem ser reutilizada em várias tarefas. Ele pode ser compartilhado com outros utilizadores na área de trabalho e é mantido entre tarefas.

Para criar um recurso de computação do Azure Machine Learning persistente, tem de especificar o `vm_size` e `max_nodes` parâmetros. O Azure Machine Learning utiliza, em seguida, predefinições inteligentes para o restante dos parâmetros.  Por exemplo, a computação está definida para dimensionar automaticamente para zero nós quando não utilizado e para criar VMs dedicadas para executarem as tarefas conforme necessário. 

* **vm_size**: família de VM de nós criados pela computação do Azure Machine Learning.
* **max_nodes**: número máximo de nós para dimensionamento automático e executar uma tarefa na computação do Azure Machine Learning.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Computação persistente (avançado)

Também pode configurar várias propriedades avançadas durante a criação de computação do Azure Machine Learning.  Estas propriedades permitem-lhe criar um cluster persistente de tamanho fixo, ou dentro de uma rede Virtual do Azure existente na sua subscrição.

Para além `vm_size` e `max_nodes`, pode utilizar as seguintes propriedades:

* **min_nodes**: nós mínimo (0 nós predefinida) para reduzir verticalmente e executar uma tarefa na computação do Azure Machine Learning.
* **vm_priority**: escolher entre "dedicada" (predefinição) e "lowpriority' VMs durante a criação de computação do Azure Machine Learning. As VMs de prioridade baixa utilizar excesso de capacidade do Azure e são, portanto, mais baratas, mas o risco de ficar a ser suplantadas.
* **idle_seconds_before_scaledown**: tempo de inatividade (padrão de 120 segundos) de espera após a conclusão de execução antes de dimensionamento automático para min_nodes.
* **vnet_resourcegroup_name**: grupo de recursos do __existente__ rede virtual. Computação do Azure Machine Learning é criado dentro desta rede virtual.
* **vnet_name**: nome da rede virtual. A rede virtual tem de estar na mesma região que a sua área de trabalho do Azure Machine Learning.
* **subnet_name**: nome da sub-rede na rede virtual. Recursos de computação do Machine Learning do Azure serão atribuídos a endereços IP deste intervalo de sub-rede.

> [!TIP]
> Quando cria um recurso de computação do Azure Machine Learning persistente tem também a capacidade de atualizar as respetivas propriedades, como o min_nodes ou o max_nodes. Basta chamar o `update()` função para o mesmo.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
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

O Azure Machine Learning também suporta a colocarem os seus próprios recursos de computação e ligá-la à sua área de trabalho. Um esse tipo de recurso é uma VM remota arbitrária, desde que esteja acessível a partir do serviço Azure Machine Learning. Pode ser uma VM do Azure ou um servidor remoto na sua organização ou no local. Especificamente, tendo em conta o endereço IP e credenciais (nome de utilizador/palavra-passe ou chave SSH), pode usar qualquer VM acessível para execuções de remotas.
Pode utilizar um ambiente de sistema criado conda, um ambiente de Python já existente ou um contentor do Docker. Execução com o contentor de Docker exige que tenha de motor do Docker em execução na VM. Esta funcionalidade é especialmente útil quando pretender que um ambiente de dev/experimentação mais flexível, com base na cloud que o seu computador local.

> [!TIP]
> Recomendamos utilizar a máquina de Virtual de ciência de dados como VM do Azure à escolha para este cenário. É uma ciência de dados pré-configurado e IA ambiente de desenvolvimento no Azure com uma opção organizada de ferramentas e estruturas para o ciclo de vida completo de desenvolvimento de ML. Para obter mais informações sobre como utilizar a máquina de Virtual de ciência de dados com o Azure Machine Learning, consulte a [configurar um ambiente de desenvolvimento](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm) documento.

> [!WARNING]
> O Azure Machine Learning suporta apenas máquinas virtuais com o Ubuntu. Quando criar uma máquina virtual ou selecione um já existente, tem de selecionar um que utiliza o Ubuntu.

Os passos seguintes utilizam o SDK para configurar uma Máquina Virtual de ciência de dados (DSVM) como um destino de treinamento:

1. Para anexar uma máquina virtual existente como um destino de computação, tem de fornecer o nome de domínio completamente qualificado, o nome de início de sessão e a palavra-passe para a máquina virtual.  No exemplo, substitua ```<fqdn>``` com o nome de domínio completamente qualificado público da VM ou o endereço IP público. Substitua ```<username>``` e ```<password>``` com o utilizador SSH e a palavra-passe para a VM:

    ```python
    from azureml.core.compute import RemoteCompute

    dsvm_compute = RemoteCompute.attach(ws,
                                    name="attach-dsvm",
                                    username='<username>',
                                    address="<fqdn>",
                                    ssh_port=22,
                                    password="<password>")

    dsvm_compute.wait_for_completion(show_output=True)

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

## <a id="hdinsight"></a>O Azure HDInsight 

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

1. Introduza um nome para o destino de computação
1. Selecione **computação do Machine Learning** como o tipo de computação a utilizar para __treinamento__

    > [!IMPORTANT]
    > Pode criar apenas a computação do Azure Machine Learning como a computação gerida para formação

1. Preencha o formulário necessária, especialmente a família de VM e o número máximo de nós a utilizar para configurar a computação 
1. Selecione __Criar__
1. Pode ver o estado da operação de criação ao selecionar o destino de computação da lista

    ![Ver lista de computação](./media/how-to-set-up-training-targets/View_list.png)

1. Em seguida, irá ver os detalhes para o destino de computação.

    ![Ver detalhes](./media/how-to-set-up-training-targets/compute-target-details.png)

1. Agora pode enviar uma execução em relação a esses destinos conforme detalhado acima


### <a name="reuse-existing-compute-in-your-workspace"></a>Reutilização de computação existente na sua área de trabalho

Siga os passos acima para ver a lista de destinos de computação e, em seguida, utilize os seguintes passos para reutilizar o destino de computação:

1. Clique nas **+** iniciar para adicionar um destino de computação
2. Introduza um nome para o destino de computação
3. Selecione o tipo de computação para anexar para __treinamento__

    > [!IMPORTANT]
    > Nem todos de computação tipos podem ser anexados utilizando o portal.
    > Atualmente, os tipos que podem ser anexados para treinamento são:
    > 
    > * VM remota
    > * Databricks
    > * Data Lake Analytics
    > * HDInsight

1. Preencha o formulário necessário

    > [!NOTE]
    > A Microsoft recomenda que utilize chaves SSH, conforme forem mais seguras do que as palavras-passe. As senhas são vulneráveis a ataques de força bruta, enquanto as chaves SSH se baseiam em assinaturas criptográficas. Para obter informações sobre como criar chaves SSH para utilização com máquinas virtuais do Azure, veja os documentos seguintes:
    >
    > * [Criar e utilizar chaves SSH no Linux ou macOS]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Criar e utilizar chaves SSH no Windows]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Selecione anexar
1. Pode ver o estado da operação de anexar ao selecionar o destino de computação a partir da lista
1. Agora pode enviar uma execução em relação a esses destinos conforme detalhado acima

## <a name="examples"></a>Exemplos
Consulte os blocos de notas nas seguintes localizações:
* [procedimentos-to-use-azureml/treinamento](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)

* [tutoriais/img-classificação-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Referência do SDK do Machine Learning do Azure](https://aka.ms/aml-sdk)
* [Tutorial: preparar um modelo](tutorial-train-models-with-aml.md)
* [Onde pretende implementar modelos](how-to-deploy-and-where.md)
* [Criar machine learning pipelines com o serviço Azure Machine Learning](concept-ml-pipelines.md)
