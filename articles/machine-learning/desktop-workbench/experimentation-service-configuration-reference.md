---
title: Ficheiros de configuração do serviço de experimentação do Machine Learning do Azure
description: Este documento detalha as definições de configuração para o serviço de experimentação do Azure ML.
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ROBOTS: NOINDEX
ms.openlocfilehash: abce80528479ba180783dbab604d4c836ddb7f1e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950782"
---
# <a name="azure-machine-learning-experimentation-service-configuration-files"></a>Ficheiros de configuração do serviço de experimentação do Machine Learning do Azure

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Quando executa um script na bancada de trabalho do Azure Machine Learning (Azure ML), o comportamento da execução é controlado por ficheiros a **aml_config** pasta. Esta pasta está na raiz da pasta do projeto. É importante compreender o conteúdo desses arquivos de forma a alcançar o resultado desejado para a execução de uma forma ideal.

Seguem-se os arquivos relevantes nessa pasta:
- conda_dependencies.yml
- spark_dependencies.yml
- ficheiros de destino de computação
    - \<nome do destino de computação > .compute
- executar arquivos de configuração
    - \<Execute o nome de configuração > .runconfig

>[!NOTE]
>Normalmente possui um arquivo de destino de computação e execute o ficheiro de configuração para cada destino de computação que cria. No entanto, pode criar estes ficheiros de forma independente e ter vários arquivos de configuração de execução que aponta para o mesmo destino de computação.

## <a name="condadependenciesyml"></a>conda_dependencies.yml
Este ficheiro é um [o ficheiro de ambiente de conda](https://conda.io/docs/using/envs.html#create-environment-file-by-hand) que especifica a versão de runtime de Python e os pacotes que depende do seu código. Quando o Azure ML Workbench executa um script de um contentor de Docker ou o cluster do HDInsight, cria um [conda ambiente](https://conda.io/docs/using/envs.html) para o script seja executado. 

Nesse arquivo, especificar os pacotes de Python que seu script precisará para execução. Serviço de experimentação do Azure ML cria o ambiente de conda, de acordo com sua lista de dependências. Pacotes listados aqui tem de estar acessíveis pelo motor de execução por meio de canais, tais como:

* [continuum.io](https://anaconda.org/conda-forge/repo)
* [PyPI](https://pypi.python.org/pypi)
* um ponto final acessível publicamente (URL)
* ou um caminho de ficheiro local
* outras pessoas acessível pelo motor de execução

>[!NOTE]
>Quando em execução num cluster do HDInsight, Azure ML Workbench cria um ambiente de conda para sua execução específica. Isso permite que usuários diferentes para serem executadas em ambientes do python diferentes no mesmo cluster.  

Eis um exemplo de um típico **conda_dependencies.yml** ficheiro.
```yaml
name: project_environment
dependencies:
  # Python version
  - python=3.5.2
  
  # some conda packages
  - scikit-learn
  - cryptography
  
  # use pip to install some more packages
  - pip:
     # a package in PyPi
     - azure-storage
     
     # a package hosted in a public URL endpoint
     - https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
     
     # a wheel file available locally on disk (this only works if you are executing against local Docker target)
     - C:\temp\my_private_python_pkg.whl
```

O Azure ML Workbench utiliza o mesmo ambiente de conda sem a recriação, desde que o **conda_dependencies.yml** permanece o mesmo. Ele irá reconstruir o seu ambiente se alterar as suas dependências.

>[!NOTE]
>Se destinar execução contra _local_ contexto, de cálculo **conda_dependencies.yml** ficheiro é **não** utilizado. As dependências de pacote para o seu ambiente de Python do Azure ML Workbench local tem de ser instalado manualmente.

## <a name="sparkdependenciesyml"></a>spark_dependencies.yml
Este ficheiro Especifica o nome da aplicação Spark ao submeter um script PySpark e pacotes de Spark que precisam ser instalados. Também pode especificar um repositório público do Maven, bem como pacotes de Spark que podem ser encontrados nesses repositórios do Maven.

Segue-se um exemplo:

```yaml
configuration:
  # Spark application name
  "spark.app.name": "ClassifyingIris"
  
repositories:
  # Maven repository hosted in Azure CDN
  - "https://mmlspark.azureedge.net/maven"
  
  # Maven repository hosted in spark-packages.org
  - "https://spark-packages.org/packages"
  
packages:
  # MMLSpark package hosted in the Azure CDN Maven
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.5"
    
  # spark-sklearn packaged hosted in the spark-packages.org Maven
  - group: "databricks"
    artifact: "spark-sklearn"
    version: "0.2.0"
```

>[!NOTE]
>Ajuste os parâmetros, como o tamanho de função de trabalho e os núcleos de cluster deve passar à secção "configuração" no ficheiro spark_dependecies.yml 

>[!NOTE]
>Se está a executar o script num ambiente de Python, *spark_dependencies.yml* ficheiro é ignorado. É utilizado apenas se estiver a executar em relação a Spark (seja num Docker ou o Cluster de HDInsight).

## <a name="run-configuration"></a>Configuração de execução
Para especificar uma configuração de execução específica, terá de um arquivo de .compute e um arquivo de .runconfig. Normalmente, estes são gerados com um comando da CLI. Também pode clonar a sair aqueles, renomeá-los e editá-los.

```azurecli
# create a compute target pointing to a VM via SSH
$ az ml computetarget attach remotedocker -n <compute target name> -a <IP address or FQDN of VM> -u <username> -w <password>

# create a compute context pointing to an HDI cluster head-node via SSH
$ az ml computetarget attach cluster -n <compute target name> -a <IP address or FQDN of HDI cluster> -u <username> -w <password> 
```

Este comando cria um par de ficheiros com base no destino de computação especificado. Digamos que com o nome do destino de computação _foo_. Este comando gera _foo.compute_ e _foo.runconfig_ no seu **aml_config** pasta.

>[!NOTE]
> _local_ ou _docker_ nomes para os ficheiros de configuração de execução são arbitrários. O Azure ML Workbench adiciona que estes dois executam configurações quando cria um projeto em branco para sua comodidade. Pode mudar o nome "<run configuration name>.runconfig" ficheiros fornecidos com o modelo de projeto, ou criar novos com qualquer nome que pretende.

### <a name="compute-target-namecompute"></a>\<nome do destino de computação > .compute
_\<nome do destino de computação > .compute_ ficheiro Especifica informações de ligação e a configuração para o destino de computação. É uma lista de pares nome-valor. Seguem-se as configurações suportadas:

**tipo de**: tipo de ambiente de computação. Os valores suportados são:
  - local
  - remoto
  - Docker
  - remotedocker
  - cluster

**baseDockerImage**: imagem de Docker a utilizada para executar o script de Python/PySpark. O valor predefinido é _microsoft/mmlspark:plus-0.7.91_. Também suportamos uma outra imagem: _microsoft/mmlspark:plus-gpu-0.7.91_, que lhe dá acesso à GPU para a máquina de anfitrião (se GPU estiver presente).

**endereço**: O endereço IP ou FQDN (nome de domínio completamente qualificado) da máquina virtual ou o HDInsight nó principal do cluster.

**nome de utilizador**: O nome de utilizador SSH para aceder à máquina virtual ou o nó principal do HDInsight.

**palavra-passe**: A palavra-passe encriptada para a ligação de SSH.

**sharedVolumes**: sinalizador para sinalizar que motor de execução deve utilizar o Docker partilhado o recurso de volume para enviar ficheiros do projeto e volta. Ter este sinalizador ativado pode acelerar a execução, uma vez que o Docker pode acessar projetos diretamente sem a necessidade de copiá-los. É melhor definir _false_ se o motor de Docker está a ser executado no Windows desde o volume de partilha para o Docker no Windows pode ser instável. Defina-o como _true_ se estiver em execução no macOS ou Linux.

**nvidiaDocker**: este sinalizador, quando definido como _verdadeira_, informa o serviço de experimentação do Azure ML para utilizar _nvidia docker_ comando, em vez de regular _docker_comando, para iniciar a imagem do Docker. O _nvidia docker_ mecanismo permite que o contentor do Docker para o hardware GPU de acesso. A definição é necessária se quiser executar a execução de GPU no contentor do Docker. Suporta apenas o anfitrião Linux _nvidia docker_. Por exemplo, a DSVM baseada em Linux no Azure é fornecido com _nvidia docker_. _NVIDIA docker_ a partir de agora, não é suportada no Windows.

**nativeSharedDirectory**: Esta propriedade especifica o diretório base (por exemplo: _~/.azureml/share/_) onde os ficheiros podem ser guardados para serem compartilhados entre é executado no mesmo destino de computação. Se esta definição é utilizada numa execução num contentor do Docker _sharedVolumes_ tem de ser definido como true. Caso contrário, a execução falhará.

**userManagedEnvironment**: Esta propriedade especifica se este destino de computação é gerenciado pelo utilizador diretamente ou através do serviço de experimentação.  

**pythonLocation**: Esta propriedade especifica a localização do runtime de python para ser utilizado no destino de computação para executar o programa do utilizador. 

### <a name="run-configuration-namerunconfig"></a>\<Execute o nome de configuração > .runconfig
_\<Execute o nome de configuração > .runconfig_ Especifica o comportamento de execução de experimentação do Azure ML. Pode configurar o comportamento de execução, tais como o controlo de histórico de execuções ou o que destino utilizar juntamente com vários outros de computação. Os nomes dos ficheiros de configuração de execução são utilizados para preencher a lista pendente de contexto de execução do aplicativo de área de trabalho do Azure ML Workbench.

**ArgumentVector**: Esta secção especifica o script para ser executado como parte dessa execução e os parâmetros para o script. Por exemplo, se tiver o fragmento seguinte na sua "<run configuration name>.runconfig" ficheiro 

```
 "ArgumentVector":[
  - "myscript.py"
  - 234
  - "-v" 
 ] 
```
_"submeter a experimentação do ml de az foo.runconfig"_ executa automaticamente o comando com _myscript.py_ passando 234 como um parâmetro e conjuntos de ficheiros o-- sinalizador verboso.

**Destino**: este parâmetro é o nome da _.compute_ do ficheiro que o _runconfig_ referências de ficheiros. Em geral, aponte o _foo.compute_ ficheiro, mas pode editá-lo para apontar para um destino de computação diferentes.

**Variáveis de ambiente**: Esta secção permite que os usuários definam as variáveis de ambiente como parte das suas execuções. Utilizador pode especificar variáveis de ambiente através de pares de nome / valor no seguinte formato:
```
EnvironmentVariables:
  "EXAMPLE_ENV_VAR1": "Example Value1"
  "EXAMPLE_ENV_VAR2": "Example Value2"
```

Estas variáveis de ambiente podem ser acedidas no código do usuário. Por exemplo, este código de Python imprime a variável de ambiente com o nome "EXAMPLE_ENV_VAR"
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

**Framework**: Esta propriedade especifica se o Azure ML Workbench deve iniciar uma sessão do Spark para executar o script. O valor predefinido é _PySpark_. Defina-o como _Python_ se não estiver a executar o código do PySpark, que pode ajudar a iniciar a tarefa mais rápida com menos sobrecarga.

**CondaDependenciesFile**: Esta propriedade aponta para o ficheiro que especifica as dependências de ambiente de conda no *aml_config* pasta. Se definido como _nulo_, aponte para a predefinição **conda_dependencies.yml** ficheiro.

**SparkDependenciesFile**: Esta propriedade aponta para o ficheiro que especifica as dependências do Spark no **aml_config** pasta. Ele é definido como _nulo_ por predefinição e ele aponta para a predefinição **spark_dependencies.yml** ficheiro.

**PrepareEnvironment**: Esta propriedade, quando definida como _true_, indica ao serviço de experimentação para preparar o ambiente de conda com base nas dependências de conda especificadas como parte de sua execução inicial. Esta propriedade é eficaz apenas quando executa um ambiente do Docker. Esta definição não tem qualquer efeito se estiver a executar em relação a um _local_ ambiente. 

**TrackedRun**: este sinalizador sinaliza o serviço de experimentação ou não controlar a execução no Azure ML Workbench infraestrutura de histórico. O valor predefinido é _true_. 

**UseSampling**: _UseSampling_ Especifica se os conjuntos de dados de exemplo do Active Directory para origens de dados são utilizados para a execução. Se definido como _false_, origens de dados de ingestão e utilizar os dados completos de leitura do arquivo de dados. Se definido como _true_, exemplos de Active Directory são usados. Os utilizadores podem utilizar o **DataSourceSettings** para especificar quais conjuntos de dados de exemplo específicos a utilizar se pretende substituir o exemplo de Active Directory. 

**DataSourceSettings**: Esta secção de configuração especifica as definições da origem de dados. Nesta secção, o utilizador Especifica qual amostra de dados existente para uma determinada origem de dados é utilizada como parte da execução. 

A seguinte definição de configuração especifica esse exemplo com o nome "MySample" é utilizado para a origem de dados com o nome "MyDataSource"
```
DataSourceSettings:
    MyDataSource.dsource:
    Sampling:
    Sample: MySample
```

**DataSourceSubstitutions**: substituições de origem de dados podem ser utilizadas quando o usuário deseja mudar de uma origem de dados para outra sem alterar o seu código. Por exemplo, os usuários podem alternar de um ficheiro de objeto de amostragem para baixo, o local para o conjunto de dados original, maior armazenado em BLOBs do Azure, alterando a referência de origem de dados. Quando é utilizada uma substituição, o Azure ML Workbench executa suas origens de dados e os pacotes de preparação de dados ao referenciar a origem de dados substituto.

O exemplo seguinte substitui as referências de "mylocal.datasource" em origens de dados do Azure ML e pacotes de preparação de dados "myremote.dsource". 
 
```
DataSourceSubstitutions:
    mylocal.dsource: myremote.dsource
```

Com base na substituição acima, o código de exemplo seguintes agora lê a partir de "myremote.dsource" em vez de "mylocal.dsource" sem utilizadores de alterar o seu código.
```
df = datasource.load_datasource('mylocal.dsource')
```
## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre [configuração do serviço de experimentação](experimentation-service-configuration.md).
