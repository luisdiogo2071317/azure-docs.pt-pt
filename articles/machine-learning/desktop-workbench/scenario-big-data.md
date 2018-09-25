---
title: Previsão da carga de trabalho de servidor em terabytes de dados - Azure | Documentos da Microsoft
description: Como preparar um modelo de aprendizagem automática em grandes volumes de dados com o Azure Machine Learning Workbench.
services: machine-learning
documentationcenter: ''
author: daden
manager: mithal
editor: daden
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: daden
ROBOTS: NOINDEX
ms.openlocfilehash: 8f3bd4e62aa85c69a0bfafeacf13bc3e472136d5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964706"
---
# <a name="server-workload-forecasting-on-terabytes-of-data"></a>Previsão da carga de trabalho dos servidores em terabytes de dados

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Este artigo aborda como os cientistas de dados podem utilizar o Azure Machine Learning Workbench permite desenvolver soluções que requerem a utilização de grandes volumes de dados. Pode começar a partir de um exemplo de um conjunto de dados grandes, iterar por meio de preparação de dados, engenharia de funcionalidades e machine learning e, em seguida, expandir o processo para o conjunto completo de dados grandes. 

Irá aprender sobre as principais capacidades de Machine Learning Workbench seguintes:
* Fácil alternar entre destinos de computação. Pode configurar destinos de computação diferentes e utilizá-los a experimentação. Neste exemplo, vai utilizar uma DSVM do Ubuntu e um cluster do Azure HDInsight como os destinos de computação. Também pode configurar os destinos de computação, dependendo da disponibilidade de recursos. Em particular, depois de aumentar horizontalmente o cluster do Spark com mais nós de trabalho, pode utilizar os recursos através do Machine Learning Workbench para acelerar a execuções da experimentação.
* Execute o controlo de histórico. Pode utilizar o Machine Learning Workbench para controlar o desempenho de outras métricas relevantes e modelos de aprendizagem automática.
* Operacionalização de modelo de aprendizagem automática. Pode utilizar as ferramentas internas do Machine Learning Workbench para implementar a máquina preparada, modelo de aprendizagem como um serviço web no Azure Container Service. Também pode utilizar o serviço web para obter previsões de mini-batch por meio de chamadas de REST API. 
* Suporte para terabytes de dados.

> [!NOTE]
> Para obter exemplos de código e outros materiais relacionados a este exemplo, consulte [GitHub](https://github.com/Azure/MachineLearningSamples-BigData).
> 

## <a name="use-case-overview"></a>Descrição geral de caso de utilização

Previsão da carga de trabalho nos servidores é uma necessidade de negócio comuns para as empresas de tecnologia que gerir a sua própria infraestrutura. Para reduzir os custos de infraestrutura, serviços em execução nos servidores utilizada devem ser agrupados para serem executadas num número mais pequeno de máquinas. Devem receber mais máquinas para executar serviços em execução nos servidores usado em excesso. 

Neste cenário, se concentrar na previsão da carga de trabalho para cada máquina (ou servidor). Em particular, utilizar os dados de sessão em cada servidor para prever a classe de carga de trabalho do servidor no futuro. Classificar a carga de cada servidor em baixa, média e alta classes utilizando o classificador de floresta aleatório na [Apache Spark ML](https://spark.apache.org/docs/2.1.1/ml-guide.html). O machine learning, técnicas e de fluxo de trabalho neste exemplo pode ser facilmente estendido para outros problemas semelhantes. 


## <a name="prerequisites"></a>Pré-requisitos

As pré-requisitos para executar este exemplo são os seguintes:

* Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).
* Uma cópia instalada do [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). Para instalar o programa e criar uma área de trabalho, consulte a [guia de início rápido de instalação](quickstart-installation.md). Se tiver várias subscrições, pode [definir a subscrição pretendida para a subscrição de Active Directory atual](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az_account_set).
* Windows 10 (as instruções neste exemplo são geralmente as mesmas para sistemas de macOS).
* Uma ciência de dados Máquina Virtual (DSVM) para Linux (Ubuntu), preferencialmente na região E.U.A. leste, onde os dados localiza. Pode aprovisionar uma DSVM do Ubuntu seguindo [estas instruções](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Também pode ver [este guia de introdução](https://ms.portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu). Recomendamos que utilize uma máquina virtual com, pelo menos, 8 núcleos e 32 GB de memória. 

Siga os [instrução](../desktop-workbench/known-issues-and-troubleshooting-guide.md#remove-vm-execution-error-no-tty-present) para ativar o acesso de sudoer sem palavra-passe na VM para AML Bancada de trabalho.  Pode optar por utilizar [autenticação baseada em chave SSH para criar e utilizar a VM na bancada de trabalho de AML](experimentation-service-configuration.md#using-ssh-key-based-authentication-for-creating-and-using-compute-targets). Neste exemplo, podemos utilizar palavra-passe para aceder à VM.  Guarde a tabela seguinte com as informações DSVM para os passos seguintes:

 Nome do campo| Valor |  
 |------------|------|
Endereço IP da DSVM | xxx|
 Nome de utilizador  | xxx|
 Palavra-passe   | xxx|


 Pode optar por utilizar qualquer VM com [motor do Docker](https://docs.docker.com/engine/) instalado.

* Um Cluster do Spark do HDInsight, com a versão de Hortonworks Data Platform 3.6 e versão do Spark 2.1.x, preferencialmente na região E.U.A. leste, onde os dados localiza. Visite [criar um cluster do Apache Spark no Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters) para obter detalhes sobre como criar clusters do HDInsight. Recomendamos que utilize um cluster de função de trabalho de três, com cada função de trabalho ter 16 núcleos e 112 GB de memória. Ou pode simplesmente Escolher tipo de VM `D12 V2` para o nó principal, e `D14 V2` para o nó de trabalho. A implementação do cluster demora cerca de 20 minutos. Terá do nome do cluster, o nome de utilizador SSH e a palavra-passe para experimentar este exemplo. Guarde a tabela seguinte com as informações de cluster do HDInsight do Azure para os passos seguintes:

 Nome do campo| Valor |  
 |------------|------|
 Nome do cluster| xxx|
 Nome de utilizador  | xxx (sshuser por predefinição)|
 Palavra-passe   | xxx|


* Uma conta do Armazenamento do Azure. Pode seguir [estas instruções](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) para criar um. Além disso, crie dois contentores de blob privado com os nomes `fullmodel` e `onemonthmodel` nesta conta de armazenamento. A conta de armazenamento é utilizada para guardar resultados intermediários de computação e modelos de machine learning. Precisa da chave de acesso e o nome conta de armazenamento para experimentar este exemplo. Guarde a tabela seguinte com as informações de conta de armazenamento do Azure para os passos seguintes:

 Nome do campo| Valor |  
 |------------|------|
 Nome da conta de armazenamento| xxx|
 Chave de acesso  | xxx|


A DSVM do Ubuntu e o cluster de HDInsight do Azure que criou na lista de pré-requisitos são os destinos de computação. Destinos são o recurso de computação no contexto do Machine Learning Workbench, que poderá ser diferente do computador onde é executada a Bancada de trabalho de computação.   

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abra o Machine Learning Workbench.
2.  Sobre o **projetos** página, selecione a **+** inicie sessão e selecione **novo projeto**.
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto.
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva **previsão da carga de trabalho em Terabytes de dados**e selecione o modelo.
5.  Selecione **Criar**.

Pode criar um projeto da bancada de trabalho com um repositório de git previamente criada ao seguir [essa instrução](./tutorial-classifying-iris-part-1.md).  
Executar `git status` para inspecionar o estado dos ficheiros para a versão de controlo.

## <a name="data-description"></a>Descrição de dados

Os dados utilizados neste exemplo são os dados de carga de trabalho de servidor sintetizado. Ele está hospedado numa conta de armazenamento de Blobs do Azure que seja acessível publicamente na região E.U.A. Leste. Podem encontrar as informações de conta de armazenamento específicos a `dataFile` campo [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json) no formato de "wasb: / /<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>". Pode utilizar os dados diretamente a partir do armazenamento de Blobs. Se o armazenamento é utilizado por vários utilizadores em simultâneo, pode utilizar [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) para transferir os dados para o seu próprio armazenamento para uma melhor experiência de experimentação. 

O tamanho total dos dados é aproximadamente 1 TB. Cada arquivo é de cerca de 1 a 3 GB e está no formato de ficheiro CSV, sem cabeçalho. Cada linha de dados representa a carga de uma transação de um determinado servidor. As informações detalhadas do esquema de dados são o seguinte:

Número de coluna | Nome do campo| Tipo | Descrição |  
|------------|------|-------------|---------------|
1  | `SessionStart` | Datetime |    Hora de início de sessão
2  |`SessionEnd`    | Datetime | Hora de fim de sessão
3 |`ConcurrentConnectionCounts` | Número inteiro | Número de ligações simultâneas
4 | `MbytesTransferred` | Valor de duplo | Dados normalizados transferidos em megabytes
5 | `ServiceGrade` | Número inteiro |  Nível de serviço para a sessão
6 | `HTTP1` | Número inteiro|  Sessão utiliza HTTP1 ou HTTP2
7 |`ServerType` | Número inteiro   |Tipo de servidor
8 |`SubService_1_Load` | Valor de duplo |   Carga subservice 1
9 | `SubService_2_Load` | Valor de duplo |  Carga subservice 2
10 | `SubService_3_Load` | Valor de duplo |     Carga subservice 3
11 |`SubService_4_Load` | Valor de duplo |  Carga subservice 4
12 | `SubService_5_Load`| Valor de duplo |      Carga subservice 5
13 |`SecureBytes_Load`  | Valor de duplo | Carga de bytes seguro
14 |`TotalLoad` | Valor de duplo | Carga total no servidor
15 |`ClientIP` | Cadeia|    Endereço IP do cliente
16 |`ServerIP` | Cadeia|    Endereço IP do servidor



Tenha em atenção que os tipos de dados esperados estão listados na tabela anterior. Devido a valores em falta e problemas de dados incorreto, não é garantido que os tipos de dados, na verdade, são os esperados. Processamento de dados deve levar isso em consideração. 


## <a name="scenario-structure"></a>Estrutura do cenário

Os ficheiros neste exemplo são organizados da seguinte forma.

| Nome de ficheiro | Tipo | Descrição |
|-----------|------|-------------|
| `Code` | Pasta | A pasta contém todo o código no exemplo. |
| `Config` | Pasta | A pasta contém os ficheiros de configuração. |
| `Image` | Pasta | A pasta utilizada para guardar imagens para o arquivo Leiame. |
| `Model` | Pasta | A pasta utilizada para guardar os ficheiros de modelo transferidos a partir do armazenamento de Blobs. |
| `Code/etl.py` | Ficheiro de Python | O ficheiro de Python utilizado para a preparação de dados e de engenharia de funcionalidades. |
| `Code/train.py` | Ficheiro de Python | O ficheiro de Python utilizado para formar um modelo de classe de três várias-classificação.  |
| `Code/webservice.py` | Ficheiro de Python | O ficheiro de Python utilizado para operação.  |
| `Code/scoring_webservice.py` | Ficheiro de Python |  O ficheiro de Python utilizados para a transformação de dados e chamando o serviço web. |
| `Code/O16Npreprocessing.py` | Ficheiro de Python | O ficheiro de Python utilizado para processar previamente os dados para scoring_webservice.py.  |
| `Code/util.py` | Ficheiro de Python | O ficheiro de Python que contém o código para leitura e escrita de blobs do Azure.  
| `Config/storageconfig.json` | Ficheiro JSON | O ficheiro de configuração para o contentor de Blobs do Azure que armazena os resultados intermediários e o modelo para processamento e formação em dados de um mês. |
| `Config/fulldata_storageconfig.json` | Ficheiro JSON | O ficheiro de configuração para o contentor de Blobs do Azure que armazena os resultados intermediários e o modelo para processamento e treinamento num conjunto de dados completo.|
| `Config/webservice.json` | Ficheiro JSON | O ficheiro de configuração para scoring_webservice.py.|
| `Config/conda_dependencies.yml` | Ficheiro YAML | O ficheiro de dependências de Conda. |
| `Config/conda_dependencies_webservice.yml` | Ficheiro YAML | O ficheiro de dependências de Conda para o serviço web.|
| `Config/dsvm_spark_dependencies.yml` | Ficheiro YAML | O ficheiro de dependência do Spark para DSVM do Ubuntu. |
| `Config/hdi_spark_dependencies.yml` | Ficheiro YAML | O ficheiro de dependência do Spark para o cluster do HDInsight Spark. |
| `README.md` | Ficheiro de markdown | O arquivo Leiame do markdown. |
| `Code/download_model.py` | Ficheiro de Python | O ficheiro de Python utilizado para transferir os ficheiros de modelo do Azure blob para um disco local. |
| `Docs/DownloadModelsFromBlob.md` | Ficheiro de markdown | O ficheiro de markdown que contém instruções sobre como executar `Code/download_model.py`. |



### <a name="data-flow"></a>Fluxo de dados

O código na [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) carrega dados a partir do contentor publicamente acessível (`dataFile` campo [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json)). Ele inclui a preparação de dados e de engenharia de funcionalidades e guarda os resultados intermediários de computação e modelos para o seu próprio contentor privada. O código na [ `Code/train.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/train.py) carrega os resultados intermediários de computação do contêiner privado, prepara o modelo de classificação de Roc e escreve o modelo treinado do machine learning para o contentor privado. 

Deve utilizar um contentor para a experimentação no conjunto de dados de um mês e outro para experimentação no conjunto de dados completo. Uma vez que os dados e os modelos são guardados como ficheiro Parquet, cada arquivo é, na verdade, uma pasta no contentor, que contém vários blobs. O contentor resultante será semelhante ao seguinte:

| Nome de prefixo de blob | Tipo | Descrição |
|-----------|------|-------------|
| featureScaleModel | Parquet | Modelo de scaler padrão para funcionalidades numéricas. |
| stringIndexModel | Parquet | Modelo do indexador de cadeia de caracteres para funcionalidades não numéricos.|
| oneHotEncoderModel|Parquet | Modelo de acesso frequente de um codificador para funcionalidades categóricos. |
| mlModel | Parquet | Modelo de aprendizagem de máquina treinado. |
| informações| Ficheiro pickle de Python | Informações sobre os dados transformados, incluindo o início de treinamento, a fim de treinamento, duração, o período de tempo para treinar-testar a divisão e colunas para indexação e a codificação de acesso frequente de um.

Todos os arquivos e os blobs na tabela anterior são utilizados para operacionalização.


### <a name="model-development"></a>Desenvolvimento de modelos

#### <a name="architecture-diagram"></a>Diagrama da arquitetura


O diagrama seguinte mostra o fluxo de trabalho ponto-a-ponto da utilização de Machine Learning Workbench para desenvolver o modelo: ![arquitetura](media/scenario-big-data/architecture.PNG)

As secções seguintes, vamos mostrar o desenvolvimento de modelo ao utilizar a funcionalidade de destino de computação remota no Machine Learning Workbench. Vamos primeiro carregar uma pequena quantidade de dados de exemplo e execute o script [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) num DSVM em Ubuntu para rápida iteração. Podemos pode limitar ainda mais o trabalho que fazemos no [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) passando um argumento extra para iteração mais rápido. No final, usamos um cluster do HDInsight para treinar com dados completos.     

O [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) ficheiro é carregado e prepara os dados e executa a engenharia de funcionalidades. Ele aceita dois argumentos:
* Um ficheiro de configuração para o contentor de armazenamento de BLOBs, para armazenar os resultados intermediários de computação e modelos.
* Um argumento de configuração de depuração para iteração mais rápido.

O primeiro argumento, `configFilename`, é um ficheiro de configuração local onde armazenar as informações de armazenamento de BLOBs e especifique onde pretende carregar os dados. Por predefinição, é [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/storageconfig.json), e ele vai ser utilizado nos dados de um mês, executar. Incluímos também [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json), que tem de utilizar no conjunto de dados completo ser executado. O conteúdo na configuração é o seguinte: 

| Campo | Tipo | Descrição |
|-----------|------|-------------|
| storageAccount | Cadeia | Nome de conta de armazenamento do Azure |
| storageContainer | Cadeia | Contentor na conta de armazenamento do Azure para armazenar os resultados intermediários |
| storageKey | Cadeia |Chave de acesso da conta de armazenamento do Azure |
| dataFile|Cadeia | Ficheiros de origem de dados  |
| duração| Cadeia | Duração de dados nos arquivos de origem de dados|

Modificar ambos `Config/storageconfig.json` e `Config/fulldata_storageconfig.json` para configurar a conta de armazenamento, a chave de armazenamento e o contentor de BLOBs para armazenar os resultados intermediários. Por predefinição, é o contentor de BLOBs para os dados de um mês, executados `onemonthmodel`, e é o contentor de BLOBs para o conjunto de dados completo, execute `fullmodel`. Certifique-se de que criar esses dois contentores na sua conta de armazenamento. O `dataFile` campo na [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) configura quais dados são carregados no [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py). O `duration` campo configura o intervalo incluem os dados. Se a duração é definida como ONE_MONTH, os dados carregados devem ser apenas um ficheiro. csv entre os arquivos de sete dos dados para Junho de 2016. Se a duração é completo, o conjunto completo de dados (1 TB) é carregado. Não precisa alterar `dataFile` e `duration` nesses arquivos de configuração de dois.

O segundo argumento é a depuração. Defini-la como FILTER_IP permite uma iteração mais rápida. Utilize este parâmetro é útil quando deseja depurar seu script.

> [!NOTE]
> Em todos os seguintes comandos, substitua qualquer variável de argumento com o respetivo valor real.
> 


#### <a name="model-development-on-the-docker-of-ubuntu-dsvm"></a>Modelo de desenvolvimento sobre o Docker da DSVM do Ubuntu

#####  <a name="1-set-up-the-compute-target"></a>1. Configurar o destino de computação

Inicie a linha de comandos do Machine Learning Workbench selecionando **arquivo** > **linha de comandos aberta**. Em seguida, execute: 

```az ml computetarget attach remotedocker --name dockerdsvm --address $DSVMIPaddress  --username $user --password $password ```

Os seguintes dois ficheiros são criados na pasta aml_config do seu projeto:

-  dockerdsvm.Compute: este ficheiro contém as informações de ligação e a configuração para um destino de execução remota.
-  dockerdsvm.runconfig: este ficheiro é um conjunto de opções de execução usadas dentro do aplicativo da bancada de trabalho.

Procure dockerdsvm.runconfig e altere a configuração de um destes campos para o seguinte:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/dsvm_spark_dependencies.yml

Prepare o ambiente de projeto ao executar:

```az ml experiment prepare -c dockerdsvm```


Com `PrepareEnvironment` definido como true, o Machine Learning Workbench cria o ambiente de tempo de execução, sempre que submete um trabalho. `Config/conda_dependencies.yml` e `Config/dsvm_spark_dependencies.yml` contêm a personalização do ambiente de tempo de execução. Pode sempre modificar as dependências Conda, configuração de Spark e dependências do Spark ao editar esses dois arquivos YMAL. Neste exemplo, adicionamos `azure-storage` e `azure-ml-api-sdk` como pacotes de Python Extras no `Config/conda_dependencies.yml`. Também adicionámos `spark.default.parallelism`, `spark.executor.instances`, e `spark.executor.cores` no `Config/dsvm_spark_dependencies.yml`. 

#####  <a name="2-data-preparation-and-feature-engineering-on-dsvm-docker"></a>2. Preparação de dados e de engenharia de funcionalidades no DSVM Docker

Execute o script `etl.py` na DSVM do Docker. Utilize um parâmetro de depuração que filtra os dados carregados com endereços IP do servidor específico:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FILTER_IP```

Navegue para o painel de lado e selecione **execute** para ver o histórico de execuções de `etl.py`. Tenha em atenção que o tempo de execução é cerca de dois minutos. Se pretender alterar o código para incluir novos recursos, fornecer FILTER_IP como o segundo argumento fornece uma iteração mais rápida. Poderá ter de executar este passo de várias vezes ao lidar com seu próprios problemas, para explorar o conjunto de dados ou criar novos recursos de aprendizagem. 

Com a restrição personalizada aos dados que a carga e mais filtragens de quais os dados para processar, pode acelerar o processo de iteração no seu desenvolvimento de modelos. Como experimentar, deve salvar periodicamente as alterações no seu código para o repositório de Git. Tenha em atenção que usamos o seguinte código no `etl.py` para ativar o acesso ao contentor privado:

```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)

# attach the blob storage to the spark cluster or VM so that the storage can be accessed by the cluster or VM        
attach_storage_container(spark, storageAccount, storageKey)
```


Em seguida, execute o script `etl.py` no Docker DSVM sem o parâmetro de depuração FILTER_IP:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FALSE```

Navegue para o painel de lado e selecione **execute** para ver o histórico de execuções de `etl.py`. Tenha em atenção que o tempo de execução é cerca de quatro minutos. O resultado processado deste passo é guardado no contêiner e é carregado para treinamento em train.py. Além disso, os indexadores de cadeia de caracteres, pipelines de codificador e scalers padrão são guardados no contentor privada. Estes são utilizados em operacionalização. 


##### <a name="3-model-training-on-dsvm-docker"></a>3. Preparação de modelos no DSVM Docker

Execute o script `train.py` na DSVM do Docker:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/train.py ./Config/storageconfig.json```

Este passo carrega os resultados intermediários de computação da execução de `etl.py`e prepara um modelo de aprendizagem automática. Este passo demora cerca de dois minutos.

Quando tiver concluído com êxito a experimentação nos dados pequenos, pode continuar a executar a experimentação no conjunto de dados completo. Pode iniciar com o mesmo código e, em seguida, experimente com o argumento e alterações de destino de computação.  

####  <a name="model-development-on-the-hdinsight-cluster"></a>Desenvolvimento de modelos no cluster do HDInsight

##### <a name="1-create-the-compute-target-in-machine-learning-workbench-for-the-hdinsight-cluster"></a>1. Criar o destino de computação no Machine Learning Workbench para o cluster do HDInsight

```az ml computetarget attach cluster --name myhdi --address $clustername-ssh.azurehdinsight.net --username $username --password $password```

Os seguintes dois ficheiros são criados na pasta aml_config:
    
-  myhdi.Compute: este ficheiro contém informações de ligação e a configuração para um destino de execução remota.
-  myhdi.runconfig: este ficheiro é um conjunto de opções de execução usadas dentro do aplicativo da bancada de trabalho.


Procure myhdi.runconfig e altere a configuração de um destes campos para o seguinte:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/hdi_spark_dependencies.yml

Prepare o ambiente de projeto ao executar:

```az ml experiment prepare -c myhdi```

Este passo pode demorar até sete minutos.

##### <a name="2-data-preparation-and-feature-engineering-on-hdinsight-cluster"></a>2. Preparação de dados e de engenharia de funcionalidades em cluster do HDInsight

Execute o script `etl.py`, com dados completos no cluster do HDInsight:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/etl.py Config/fulldata_storageconfig.json FALSE```

Uma vez que esta tarefa uma duração de um tempo relativamente longo (cerca de duas horas), pode usar `-a` para desativar a transmissão em fluxo de saída. Quando a tarefa está concluída, na **histórico de execuções**, pode ver os registos de controlador e o controlador. Se tiver um cluster maior, pode reconfigurar as configurações no sempre `Config/hdi_spark_dependencies.yml` para utilizar mais instâncias ou núcleos. Por exemplo, se tiver um cluster de nó de trabalho de quatro, pode aumentar o valor de `spark.executor.instances` entre 5 a 7. Pode ver a saída deste passo no **fullmodel** contentor na sua conta de armazenamento. 


##### <a name="3-model-training-on-hdinsight-cluster"></a>3. Preparação de modelos no cluster do HDInsight

Execute o script `train.py` num cluster do HDInsight:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/train.py Config/fulldata_storageconfig.json```

Uma vez que esta tarefa uma duração de um tempo relativamente longo (cerca de 30 minutos), pode usar `-a` para desativar a transmissão em fluxo de saída.

#### <a name="run-history-exploration"></a>Exploração de histórico de execução

Histórico de execução é uma funcionalidade que permite o controlo da sua experimentação no Machine Learning Workbench. Por padrão, ele controla a duração da experimentação. No nosso exemplo específico, quando movemos para o conjunto de dados completo para `Code/etl.py` a experimentação, percebemos que duração aumenta significativamente. Também pode iniciar métricas específicas para fins de controlo. Para ativar o controlo de métrico, adicione as seguintes linhas de código ao cabeçalho do seu ficheiro de Python:
```python
# import logger
from azureml.logging import get_azureml_logger

# initialize logger
run_logger = get_azureml_logger()
```
Eis um exemplo para controlar uma métrica específica:

```python
run_logger.log("Test Accuracy", testAccuracy)
```

Na barra lateral direita da bancada de trabalho, navegue até **execuções** para ver o histórico de execução para cada ficheiro de Python. Também pode aceder ao seu repositório do GitHub. Um novo ramo com o nome que começa por "AMLHistory", é criado para controlar a alteração que fez para o script em cada execução. 


### <a name="operationalize-the-model"></a>Operacionalizar o modelo

Nesta secção, operacionalizar o modelo que criou nos passos anteriores, como um serviço web. Também irá aprender a utilizar o serviço web para prever a carga de trabalho. Utilize interfaces de linha de comandos de operacionalização de linguagem de máquina (CLIs) para empacotar o código e as dependências como imagens do Docker e publicar o modelo como um serviço web em contentores.

Pode utilizar a linha de comandos no Machine Learning Workbench para executar a CLI.  Também pode executar a CLI no Ubuntu Linux ao seguir a [guia de instalação](./deployment-setup-configuration.md#using-the-cli). 

> [!NOTE]
> Em todos os comandos seguintes, substitua qualquer variável de argumento com o respetivo valor real. Demora cerca de 40 minutos para concluir esta secção.
> 

Escolha uma cadeia de caracteres exclusiva como o ambiente de operacionalização. Aqui, podemos usar a cadeia de caracteres "[exclusiva]" para representar a cadeia de caracteres que escolher.

1. Criar o ambiente de operacionalização e criar o grupo de recursos.

        az ml env setup -c -n [unique] --location eastus2 --cluster -z 5 --yes

   Tenha em atenção que pode utilizar o serviço de contentor como o ambiente utilizando `--cluster` no `az ml env setup` comando. Pode operacionalizar o modelo de machine learning no [Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-intro-kubernetes). Ele usa [Kubernetes](https://kubernetes.io/) para automatizar a implementação, dimensionamento e gestão de aplicações em contentores. Este comando demora cerca de 20 minutos para ser executado. Utilize o seguinte para verificar se a implementação foi concluída com êxito: 

        az ml env show -g [unique]rg -n [unique]

   Defina o ambiente de implantação como aquele que acabou de criar executando o seguinte:

        az ml env set -g [unique]rg -n [unique]

2. Criar e utilizar uma conta de gestão de modelos. Para criar uma conta de gestão de modelo, execute o seguinte:

        az ml account modelmanagement create --location  eastus2 -n [unique]acc -g [unique]rg --sku-instances 4 --sku-name S3 

   Utilize a gestão de modelos para operacionalização executando o seguinte:

        az ml account modelmanagement set  -n [unique]acc -g [unique]rg  

   Uma conta de gestão de modelo é utilizada para gerir os modelos e serviços da web. No portal do Azure, verá que uma nova conta de gestão de modelo foi criada. Pode ver os modelos, manifestos, imagens do Docker e serviços que são criados com esta conta de gestão de modelos.

3. Transferir e registe os modelos.

   Transferir os modelos na **fullmodel** contentor no seu computador local no diretório do código. Não transferir o ficheiro parquet de dados com o nome "vmlSource.parquet." Não é um ficheiro de modelo; é um resultado intermediário de computação. Também é possível reutilizar os arquivos de modelo incluídos no repositório Git. Para obter mais informações, consulte [GitHub](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Docs/DownloadModelsFromBlob.md). 

   Vá para o `Model` segue de modelos como pasta na CLI e registre-se:

        az ml model register -m  mlModel -n vmlModel -t fullmodel
        az ml model register -m  featureScaleModel -n featureScaleModel -t fullmodel
        az ml model register -m  oneHotEncoderModel -n  oneHotEncoderModel -t fullmodel
        az ml model register -m  stringIndexModel -n stringIndexModel -t fullmodel
        az ml model register -m  info -n info -t fullmodel

   A saída de cada comando fornece um ID de modelo, o que é necessário no próximo passo. Guarde-as num ficheiro de texto para utilização futura.

4. Crie um manifesto para o serviço web.

   Um manifesto inclui o código para o serviço web, modelos de todos os o machine learning e dependências de ambiente de tempo de execução. Vá para o `Code` pasta na CLI e execute o seguinte comando:

        az ml manifest create -n $webserviceName -f webservice.py -r spark-py -c ../Config/conda_dependencies_webservice.yml -i $modelID1 -i $modelID2 -i $modelID3 -i $modelID4 -i $modelID5

   A saída fornece um ID de manifesto para a próxima etapa. 

   Mantenha-se no `Code` directory e pode testar webservice.py ao executar o seguinte: 

        az ml experiment submit -t dockerdsvm -c dockerdsvm webservice.py

5. Crie uma imagem do Docker. 

        az ml image create -n [unique]image --manifest-id $manifestID

   A saída fornece um ID de imagem para a próxima etapa, esta imagem do docker é utilizada no serviço de contentor. 

6. Implemente o serviço web para o cluster do Container Service.

        az ml service create realtime -n [unique] --image-id $imageID --cpu 0.5 --memory 2G

   A saída fornece um ID de serviço. Terá de utilizá-lo para obter a chave de autorização e URL de serviço.

7. Chame o web service no código de Python para pontuar em lotes de mini.

   Utilize o seguinte comando para obter a chave de autorização:

         az ml service keys realtime -i $ServiceID 

   Utilize o seguinte comando para obter o URL de classificação de serviço:

        az ml service usage realtime -i $ServiceID

   Modificar o conteúdo `./Config/webservice.json` com o serviço correto de URL e autorização da chave de classificação. Mantenha o "Bearer" no arquivo original e substituir a parte "xxx". 
   
   Vá para o diretório de raiz do seu projeto e testar o serviço web para o mini-batch classificação usando o seguinte:

        az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/scoring_webservice.py ./Config/webservice.json

8. Dimensione o serviço web. 

   Para obter mais informações, consulte [como dimensionar a operacionalização do seu cluster do Azure Container Service](how-to-scale-clusters.md).
 

## <a name="next-steps"></a>Passos Seguintes

Este exemplo realça como utilizar o Machine Learning Workbench para preparar um modelo de machine learning em grandes volumes de dados e operacionalizar o modelo preparado. Em particular, aprendeu a configurar e usar destinos de computação diferentes e execute o histórico de controlar métricas e utilizar diferentes execuções.

Pode estender o código para explorar a validação cruzada e de hyper-parâmetro de otimização. Para saber mais sobre a validação cruzada e de hyper-parâmetro de otimização, veja [este recurso do GitHub](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning).  

Para saber mais sobre a previsão de séries temporais, veja [este recurso do GitHub](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting).