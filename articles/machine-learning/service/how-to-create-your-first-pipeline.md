---
title: Criar e executar pipelines de ML
titleSuffix: Azure Machine Learning service
description: Criar e executar uma pipeline com o SDK do Azure Machine Learning de aprendizagem para Python.  Pipelines são utilizados para criar e gerir os fluxos de trabalho que fases de aprendizagem automática em conjunto inserir manualmente (ML), como a preparação de dados, preparação de modelos, implementação do modelo e inferência.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 3038d8978ed3705e9e18be58080afd532b9d72d6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100560"
---
# <a name="create-and-run-a-machine-learning-pipeline-using-azure-machine-learning-sdk"></a>Criar e executar um pipeline de machine learning com o Azure Machine Learning SDK

Neste artigo, irá aprender a criar, publicar, executar e controlar um [pipeline de machine learning](concept-ml-pipelines.md) utilizando o [do Azure Machine Learning SDK](https://aka.ms/aml-sdk).  Esses pipelines ajudam a criar e gerir os fluxos de trabalho que reunir várias fases de aprendizado de máquina. Cada fase de um pipeline, por exemplo, a preparação de dados e preparação de modelos, pode incluir um ou mais passos.

Os pipelines que cria são visíveis para os membros do seu serviço Azure Machine Learning [área de trabalho](how-to-manage-workspace.md). 

Pipelines usar destinos de computação remoto para computação e o armazenamento dos dados intermediários e finais associados a esse pipeline.  Pipelines podem ler e suporte de dados de escrita de e para [armazenamento do Azure](https://docs.microsoft.com/azure/storage/) localizações.


## <a name="prerequisites"></a>Pré-requisitos

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

* [Configurar o ambiente de desenvolvimento](how-to-configure-environment.md) para instalar o SDK do Azure Machine Learning.

* Criar uma [área de trabalho do Azure Machine Learning](how-to-configure-environment.md#workspace) para conter todos os seus recursos do pipeline. 

 ```python
 ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
 ```

## <a name="set-up-machine-learning-resources"></a>Configurar recursos de aprendizado de máquina

Crie os recursos necessários para executar um pipeline:

* Configure um arquivo de dados usado para acessar os dados necessários nos passos de pipeline.

* Configurar um `DataReference` objeto para apontar para dados que residem ou estão acessível a um arquivo de dados.

* Configurar o [destinos de computação](concept-azure-machine-learning-architecture.md#compute-target) em que executará as etapas do pipeline.

### <a name="set-up-a-datastore"></a>Configurar um arquivo de dados
Um arquivo de dados armazena os dados para o pipeline aceder.  Cada área de trabalho tem um arquivo de dados padrão. Pode registrar os arquivos de dados adicionais. 

Quando cria a área de trabalho, uma [armazenamento de ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) e uma [armazenamento de BLOBs](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) estão anexados à área de trabalho, por predefinição.  O armazenamento de ficheiros do Azure é o "padrão arquivo de dados" para uma área de trabalho, mas também pode utilizar o armazenamento de BLOBs como um arquivo de dados.  Saiba mais sobre [opções de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

Carregar ficheiros de dados ou diretórios para o arquivo de dados para que sejam acessíveis a partir de seus pipelines.  Este exemplo utiliza a versão de armazenamento de Blobs do arquivo de dados:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

Um pipeline consiste num ou mais passos.  Um passo é uma unidade executada num destino de computação.  Passos podem consumir origens de dados e produzir dados de "intermediários". Um passo pode criar dados como um modelo, um diretório com o modelo e os arquivos dependentes ou dados temporários.  Estes dados, em seguida, estão disponíveis para outros passos mais tarde no pipeline.

### <a name="configure-data-reference"></a>Configurar a referência de dados

Acabou de criar uma origem de dados que pode ser referenciada num pipeline como entrada para um passo. Uma origem de dados num pipeline é representada por um [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) objeto. O `DataReference` objeto aponta para dados que residem ou estão acessível a partir de um arquivo de dados.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Dados intermédios (ou de saída de um passo) é representada por um [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) objeto. `output_data1` é produzido como a saída de um passo e utilizado como a entrada de um ou mais passos futuras.  `PipelineData` Apresenta uma dependência de dados entre passos e cria uma ordem de execução implícita no pipeline.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

### <a name="set-up-compute"></a>Configurar computação

No Azure Machine Learning, computação (ou destino de computação) refere-se a computadores ou clusters que executará as etapas de computacionais no seu pipeline de machine learning. Por exemplo, pode criar uma computação do Azure Machine Learning para executar os passos.

```python
compute_name = "aml-compute"
 if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                min_nodes = 1, 
                                                                max_nodes = 4)
     # create the compute target
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # Can poll for a minimum number of nodes and for a specific timeout. 
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

## <a name="construct-your-pipeline-steps"></a>Construir suas etapas de pipeline

Agora, está pronto para definir um passo de pipeline. Existem muitos passos internos disponível através do SDK do Azure Machine Learning. As mais básicas destes passos é uma `PythonScriptStep` que executa um script de Python num destino de computação especificado.

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Depois de definir suas etapas, criar o pipeline com alguns ou todos esses passos.

>[!NOTE]
>Nenhum ficheiro ou a dados são carregados para o serviço Azure Machine Learning ao definir os passos ou criar o pipeline.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

## <a name="submit-the-pipeline"></a>Submeter o pipeline

Quando submete o pipeline, as dependências são verificadas para cada passo e um instantâneo da pasta especificado como o diretório de origem é carregado para o serviço Azure Machine Learning.  Se não for especificado nenhum diretório de origem, o atual diretório de local é carregado.

```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
```

Quando executa primeiro um pipeline:

* O instantâneo de projeto é transferido para o destino de computação associado a área de trabalho de armazenamento de Blobs do.
* Uma imagem do docker é criada a correspondente a cada etapa no pipeline.
* A imagem do docker para cada passo é transferida para o destino de computação do container registry.
* Se um `DataReference` objeto foi especificado um passo, o arquivo de dados está montado. Se não é suportada a montagem, os dados em vez disso, são copiados para o destino de computação.
* O passo é executado no destino de computação especificado na definição do passo. 
* Artefactos, tais como registos, stdout e stderr, métricas e especificado pelo passo de saída são criados. Estes artefactos, em seguida, são carregados e mantidos no armazenamento de dados do usuário padrão.

![executar uma experimentação como um pipeline](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

## <a name="publish-a-pipeline"></a>Publicar um pipeline

Pode publicar um pipeline para executá-lo mais tarde com entradas diferentes. Para o ponto final REST de um pipeline já publicado para aceitar parâmetros, o pipeline tem de ser parametrizado antes da publicação. 

1. Para criar um parâmetro de pipeline, utilize um [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) objeto com um valor predefinido.

 ```python
 pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
 ```

2. Adicionar isso `PipelineParameter` objeto como um parâmetro para qualquer uma das etapas no pipeline da seguinte forma:

 ```python
 compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
 ```

3. Publica este pipeline, que aceita um parâmetro, quando invocado.

```python
published_pipeline1 = pipeline1.publish(
    name="My_Published_Pipeline", 
    description="My Published Pipeline Description")
```

## <a name="run-a-published-pipeline"></a>Executar um pipeline publicado

Todos os pipelines de publicação tem um ponto de final REST para invocar a execução do pipeline de sistemas externos, como os clientes não-Python. Este ponto final fornece uma forma para "gerida repetibilidade" no lote de classificação e cenários de reparametrização.

Para invocar a execução do pipeline anterior, tem um token de cabeçalho de autenticação do Azure Active Directory conforme descrito no [AzureCliAuthentication classe](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py)

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```
## <a name="view-results"></a>Ver resultados

Ver a lista de todos os seus pipelines e os respetivos detalhes de execução:
1. Inicie sessão no [portal do Azure](https://portal.azure.com/).  

1. [Ver a sua área de trabalho](how-to-manage-workspace.md#view-a-workspace) para encontrar a lista de pipelines.
 ![lista de pipelines de aprendizagem](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Selecione um pipeline específico para ver os resultados de execução.

## <a name="next-steps"></a>Passos Seguintes
- Uso [estes blocos de notas do Jupyter no GitHub](https://aka.ms/aml-pipeline-readme) para explorar ainda mais pipelines do machine learning.
- Ler a ajuda de referência do SDK para o [azureml pipelines-núcleo](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) pacote e o [passos de pipelines azureml](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) pacote.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
