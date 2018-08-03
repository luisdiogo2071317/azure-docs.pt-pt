---
title: Utilizar a funcionalidade de recolha de dados de modelo no Azure Machine Learning Workbench | Documentos da Microsoft
description: Este artigo fala sobre como utilizar a funcionalidade de recolha de dados de modelo no Azure Machine Learning Workbench
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 5c1a884ebe6216c4e8099f2ada2182ccff68b63e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449786"
---
# <a name="collect-model-data-by-using-data-collection"></a>Recolher dados do modelo ao utilizar a recolha de dados

Pode utilizar a funcionalidade de recolha de dados de modelo no Azure Machine Learning para arquivar as entradas de modelo e as previsões de um serviço web.

## <a name="install-the-data-collection-package"></a>Instalar o pacote de recolha de dados
Pode instalar a biblioteca de recolha de dados nativamente no Linux e Windows.

### <a name="windows"></a>Windows
No Windows, instale o módulo de recoletor de dados com o seguinte comando:

    pip install azureml.datacollector

### <a name="linux"></a>Linux
No Linux, instale primeiro a biblioteca libxml + +. Execute o seguinte comando, que tem de ser emitido em sudo:

    sudo apt-get install libxml++2.6-2v5

Em seguida, execute o seguinte comando:

    pip install azureml.datacollector

## <a name="set-environment-variables"></a>Definir variáveis de ambiente

Recolha de dados de modelo depende de duas variáveis de ambiente. AML_MODEL_DC_STORAGE_ENABLED tem de ser definido como **true** (em minúsculas) e AML_MODEL_DC_STORAGE deve ser definido para a cadeia de ligação para a conta de armazenamento do Azure onde pretende armazenar os dados.

Estas variáveis de ambiente já estão definidas para quando o serviço web está em execução num cluster no Azure. Ao executar localmente que deve defini-los por conta própria. Se estiver a utilizar o Docker, utilize o parâmetro -e do docker a executar o comando para passar as variáveis de ambiente.

## <a name="collect-data"></a>Recolher dados

Para utilizar a recolha de dados de modelo, efetue as seguintes alterações ao seu ficheiro de classificação:

1. Adicione o seguinte código na parte superior do ficheiro:
   
    ```python
    from azureml.datacollector import ModelDataCollector
    ```

1. Adicione as seguintes linhas de código para o `init()` função:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1. Adicione as seguintes linhas de código para o `run(input_df)` função:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

    Certifique-se de que as variáveis `input_df` e `pred` (valor de previsão de `model.predict()`) são inicializados antes de chamar o `collect()` função nos mesmos.

1. Utilize o `az ml service create realtime` comando com o `--collect-model-data true` comutador para criar um serviço web em tempo real. Este passo certifica-se de que os dados do modelo são recolhidos quando o serviço é executado.

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
1. Para testar a recolha de dados, execute o `az ml service run realtime` comando:

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="view-the-collected-data"></a>Ver os dados recolhidos
Para ver os dados recolhidos no armazenamento de BLOBs:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Selecione **todos os serviços**.
1. Na caixa de pesquisa, escreva **contas de armazenamento** e selecione a tecla Enter.
1. Do **contas de armazenamento** painel de pesquisa, selecione a **conta de armazenamento** recursos. Para determinar a sua conta de armazenamento, utilize os seguintes passos:

    a. Aceda ao Azure Machine Learning Workbench, selecione o projeto que está a trabalhar e abra uma linha de comandos do **ficheiro** menu.
    
    b. Introduza `az ml env show -v` e verifique a *storage_account* valor. Este é o nome da sua conta de armazenamento.

1. Selecione **contentores** no recurso chamado menu do painel e, em seguida, o contentor **modeldata**. Para ver os dados começarem a ser propagados para a conta de armazenamento, poderá ter de aguardar até dez minutos após o primeiro pedido de serviço da web. Os dados fluem para os blobs com o caminho de contentor seguinte:

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

Dados de blobs do Azure podem ser consumidos de diversas formas, por meio de software da Microsoft e ferramentas open source. Eis alguns exemplos:
- O Azure Machine Learning Workbench: Abra o ficheiro. csv no Azure Machine Learning Workbench, adicionando o ficheiro. csv como uma origem de dados.
- Excel: Abra os ficheiros. csv diários como uma folha de cálculo.
- [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/): Crie gráficos com os dados extraídos a partir dos dados. csv nos blobs.
- [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): criar um quadro de dados com uma grande quantidade de dados. csv.
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): tabela de dados do. csv de carga num Hive e execute consultas SQL diretamente nos blob.

