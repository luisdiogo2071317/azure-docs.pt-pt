---
title: Configurar o serviço de experimentação do Azure Machine Learning | Documentos da Microsoft
description: Este artigo fornece uma visão geral do serviço de experimentação do Azure Machine Learning com instruções sobre como configurá-lo.
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
ms.openlocfilehash: e79817ffad139e0a3bcb0ba32b9bc6e5666319d0
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646767"
---
# <a name="configuring-azure-machine-learning-experimentation-service"></a>Configurar o serviço de experimentação do Azure Machine Learning

## <a name="overview"></a>Descrição geral
Serviço de experimentação do Azure Machine Learning permite que os cientistas de dados executar as suas experimentações com a execução do Azure Machine Learning e executar capacidades de gestão. Ele fornece uma estrutura para a experimentação agile com iterações rápidas. O Azure Machine Learning Workbench permite-lhe começar com local é executado na sua máquina e também uma forma fácil para aumentar verticalmente e horizontalmente para outros ambientes, tais como VMs de ciência de dados remoto com GPU ou Clusters de HDInsight com o Spark.

Serviço de experimentação foi concebido para fornecer consistentes, isoladas e reproduzíveis execuções de suas experimentações. Ele ajuda a gerenciar seus destinos de computação, ambientes de execução, e executar configurações. Ao utilizar as capacidades de gerenciamento de execução e a execução do Azure Machine Learning Workbench, pode mover facilmente entre ambientes diferentes. 

Pode executar um script de Python ou PySpark num projeto do Workbench localmente ou em escala na cloud. 

Pode executar os scripts em: 

* Ambiente de Python (3.5.2) no seu computador local instalado pela Bancada de trabalho
* Ambiente de Python de Conda dentro do contentor do Docker num computador local
* Num ambiente de Python que é o proprietário e gerir num computador Linux remoto
* Ambiente de Python de Conda dentro do contentor do Docker num computador Linux remoto. Por exemplo, um ([DSVM baseada em Ubuntu no Azure]https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight para o Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) no Azure

>[!IMPORTANT]
>Serviço de experimentação do Azure Machine Learning suporta atualmente o Python 3.5.2 e Spark 2.1.11 como versões de runtime do Python e do Spark, respectivamente. 


### <a name="key-concepts-in-experimentation-service"></a>Principais conceitos do serviço de experimentação
É importante compreender os seguintes conceitos na execução de experimentação do Azure Machine Learning. As seções seguintes, vamos discutir como usar esses conceitos em detalhes. 

#### <a name="compute-target"></a>Destino de computação
R _destino de computação_ Especifica onde executar o seu programa, como o Docker sua área de trabalho, remoto numa VM ou um cluster. Um destino de computação tem de ser endereçável e acessível por si. Workbench dá-lhe a capacidade de criar destinos de computação e geri-los com a aplicação Workbench e a CLI. 

_anexar AZ ml computetarget_ comando na CLI permite-lhe criar um destino de computação que pode usar em suas execuções.

Destinos de computação suportados são:
* Ambiente de Python (3.5.2) local no seu computador instalado pela Bancada de trabalho.
* Docker local no seu computador
* Gerido por usuário, ambiente de Python em VMs do Linux Ubuntu remoto. Por exemplo, um [DSVM baseada em Ubuntu no Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* Docker remoto em VMs do Linux Ubuntu. Por exemplo, um [DSVM baseada em Ubuntu no Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight para o cluster do Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) no Azure

Serviço experimentação atualmente suporta a versão Python 3.5.2 e 2.1.11 como Python do Spark e do Spark versões de tempo de execução, respectivamente. 

>[!IMPORTANT]
> Execução de VMs do Windows são do Docker **não** suportado como destinos de computação remota.

#### <a name="execution-environment"></a>Ambiente de execução
O _ambiente de execução_ define a configuração de tempo de execução e as dependências necessárias para executar o programa na bancada de trabalho.

Gerir o ambiente de execução local usando suas ferramentas preferidas e gestores de pacotes, se estiver a executar o tempo de execução do padrão do Workbench. 

Conda é utilizado para gerir local Docker e remotas execuções do Docker, bem como as execuções com base no HDInsight. Para estes destinos de computação, a configuração do ambiente de execução é gerenciada por meio **Conda_dependencies.yml** e **Spark_dependencies.yml ficheiros**. Estes ficheiros estão a **aml_config** pasta dentro de seu projeto.

**Tempos de execução para ambientes de execução suportados são:**
* Python 3.5.2
* Spark 2.1.11

### <a name="run-configuration"></a>Configuração de execução
Para além do ambiente de execução e de destino de computação, o Azure Machine Learning oferece uma estrutura para definir e alterar *executar configurações*. Execuções diferentes da sua experimentação podem exigir uma configuração diferente como parte da experimentação iterativa. Pode ser varrimento intervalos de parâmetros diferentes, através de diferentes origens de dados e otimizar os parâmetros do spark. Serviço experimentação fornece uma estrutura para gerenciar as configurações de execução.

Em execução _anexar az ml computetarget_ comando produz dois arquivos no seu **aml_config** pasta do projeto: ".compute" e um ".runconfig" seguir essa convenção: _< your_ computetarget_name > .compute_ e _< your_computetarget_name > .runconfig_. O ficheiro de .runconfig é criado automaticamente para sua comodidade quando cria um destino de computação. Pode criar e gerir outras configurações de execução usando _az ml runconfigurations_ comando na CLI. Também pode criar e editá-los no seu sistema de ficheiros.

Configuração de execução na bancada de trabalho também permite-lhe especificar variáveis de ambiente. Pode especificar variáveis de ambiente e utilizá-los em seu código, adicionando a seguinte secção em seu arquivo .runconfig. 

```
EnvironmentVariables:
    "EXAMPLE_ENV_VAR1": "Example Value1"
    "EXAMPLE_ENV_VAR2": "Example Value2"
```

Estas variáveis de ambiente podem ser acedidas em seu código. Por exemplo, este fragmento de código phyton imprime a variável de ambiente com o nome "EXAMPLE_ENV_VAR1"
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

_**A figura seguinte mostra o fluxo de alto nível de experiência inicial executar.**_
![](media/experimentation-service-configuration/experiment-execution-flow.png)

## <a name="experiment-execution-scenarios"></a>Cenários de execução da experimentação
Nesta secção, vamos nos aprofundar em cenários de execução e saiba mais sobre como do Azure Machine Learning é executado experimentações, especificamente com uma experimentação localmente, numa VM remota e num Cluster do HDInsight. Esta secção é um passo a passo a partir de criação de um destino de computação para executar as suas experimentações.

>[!NOTE]
>No restante deste artigo, estamos a utilizar os comandos da CLI (interface de linha de comandos) para mostrar os conceitos e as capacidades. Funcionalidades descritas aqui também podem ser usadas da bancada de trabalho.

## <a name="launching-the-cli"></a>Iniciar a CLI
Uma forma fácil de iniciar a CLI está abrindo um projeto no Workbench e navegar até **ficheiro--> Abrir linha de comandos**.

![](media/experimentation-service-configuration/opening-cli.png)

Este comando inicia uma janela de terminal em que pode introduzir os comandos para executar scripts na pasta do projeto atual. Esta janela de terminal está configurada com o ambiente de Python 3.5.2, que é instalado pela Bancada de trabalho.

>[!NOTE]
> Quando executa qualquer _az ml_ comando na janela de comando, terá de ser autenticado no Azure. CLI utiliza uma cache de autenticação independente, em seguida, o aplicativo de desktop e então iniciar sessão no Workbench não significa que são autenticados no ambiente da CLI. Para se autenticar, utilize o seguinte os passos. Token de autenticação é colocado em cache localmente durante um período de tempo para que apenas terá de repetir essas etapas, quando o token expirar. Quando o token expira ou se estiver a ver erros de autenticação, execute os seguintes comandos:

```
# to authenticate 
$ az login

# to list subscriptions
$ az account list -o table

# to set current subscription to a particular subscription ID 
$ az account set -s <subscription_id>

# to verify your current Azure subscription
$ az account show
```

>[!NOTE] 
>Quando executa _az ml_ comando dentro de uma pasta de projeto, certifique-se de que o projeto pertence a uma conta de experimentação do Azure Machine Learning no _atual_ subscrição do Azure. Caso contrário, poderá encontrar erros de execução.


## <a name="running-scripts-and-experiments"></a>Executar scripts e experimentações
Com a Bancada de trabalho, pode executar o Python e scripts PySpark sobre várias computação destinos com o _submeter a experimentação do ml de az_ comando. Este comando necessita de uma definição de configuração de execução. 

Workbench cria um ficheiro de runconfig correspondente, quando criar um destino de computação, mas pode criar as configurações de execução adicionais usando _az ml runconfiguration criar_ comando. Pode editar manualmente os ficheiros de configuração de execução.

As configurações de execução aparecem como parte da experimentação experiência de execução na bancada de trabalho. 

>[!NOTE]
>Pode saber mais sobre o ficheiro de configuração de execução no [referência de configuração de execução de experimentação](experimentation-service-configuration-reference.md) secção.

## <a name="running-a-script-locally-on-workbench-installed-runtime"></a>Executar o script localmente num tempo de execução do Workbench instalado
Bancada de trabalho permite-lhe executar os seus scripts diretamente contra o tempo de execução do Workbench instalado o Python 3.5.2. Este tempo de execução padrão é instalado em tempo de configuração da bancada de trabalho e inclui as bibliotecas do Azure Machine Learning e as dependências. Resultados da execução e artefactos para execuções locais ainda são guardados no serviço de histórico de execução na cloud.

Ao contrário das execuções baseadas no Docker, esta configuração é _não_ geridos pelo Conda. Precisa de aprovisionar manualmente as dependências de pacote para o seu ambiente de Python da bancada de trabalho local.

Pode executar o seguinte comando para executar o script localmente no ambiente de Workbench instalado o Python. 

```
$az ml experiment submit -c local myscript.py
```

Pode encontrar o caminho para o ambiente de Python padrão, escrevendo o seguinte comando na janela da CLI da bancada de trabalho.
```
$ conda env list
```

>[!NOTE]
>Executar localmente PySpark diretamente no local Spark ambientes está atualmente **não** suportado. Workbench oferece suporte a scripts PySpark em execução no local Docker. O Azure Machine Learning imagem base do Docker é fornecido com o Spark 2.1.11 pré-instalado. 

_**Descrição geral de execução local para um script de Python:**_
![](media/experimentation-service-configuration/local-native-run.png)

## <a name="running-a-script-on-local-docker"></a>Executar o script num local Docker
Também pode executar seus projetos no contentor do Docker no seu computador local através do serviço de experimentação. Bancada de trabalho fornece uma imagem base do Docker que vem com bibliotecas do Azure Machine Learning e também como tempo de execução do Spark 2.1.11 para facilitar o local Spark execuções. Docker tem de já estar em execução na máquina local.

Para executar o script de Python ou PySpark no local Docker, pode executar os seguintes comandos na CLI.

```
$az ml experiment submit -c docker myscript.py
```
ou
```
az ml experiment submit --run-configuration docker myscript.py
```

O ambiente de execução do local Docker é preparado com a imagem do Docker de base do Azure Machine Learning. Bancada de trabalho de baixa esta imagem durante a execução pela primeira vez e sobreposições-lo com pacotes especificados no ficheiro conda_dependencies.yml. Esta operação torna inicial execução mais lentamente, mas as execuções subsequentes são consideravelmente mais rapidamente graças à Bancada de trabalho de reutilizar a camadas em cache. 

>[!IMPORTANT]
>Precisa executar _experimentação do ml de az preparar docker - c_ comando pela primeira vez para preparar a imagem do Docker para a sua primeira passagem. Também pode definir o **PrepareEnvironment** parâmetro como true no seu ficheiro docker.runconfig. Esta ação prepara automaticamente o seu ambiente como parte da sua execução de execução.  

>[!NOTE]
>Se executar um script PySpark no Spark, spark_dependencies.yml também é usado para além de conda_dependencies.yml.

Execução dos scripts numa imagem do Docker dá-lhe as seguintes vantagens:

1. Ele garante que o script pode ser executado de forma fiável em outros ambientes de execução. Em execução num contentor do Docker ajuda a identificar e evitar quaisquer referências de locais que podem afetar a portabilidade. 

2. Ele permite que teste rapidamente o código nos tempos de execução e estruturas complexas para instalar e configurar, como o Apache Spark, sem ter de instalá-los por conta própria.


_**Descrição geral de execução local do Docker para um script de Python:**_
![](media/experimentation-service-configuration/local-docker-run.png)

## <a name="running-a-script-on-a-remote-docker"></a>Executar um script num Docker remoto
Em alguns casos, os recursos disponíveis no seu computador local podem não ser suficiente para preparar o modelo pretendido. Nesta situação, o serviço de experimentação permite uma forma fácil de executar os seus scripts Python ou PySpark em mais poderosas VMs com a execução remota de Docker. 

VM remota deve cumprir os requisitos seguintes:
* VM remota tem de estar a executar o Linux Ubuntu e deve estar acessível através de SSH. 
* VM remota tem de ter o Docker em execução.

>[!IMPORTANT]
> Execução de VMs do Windows é o Docker **não** suportado como destinos de computação remota


Pode utilizar o seguinte comando para criar os dois a definição de destino de computação e executar a configuração para execuções baseadas no Docker remotas.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```

Depois de configurar o destino de computação, pode utilizar o seguinte comando para executar o script.
```
$ az ml experiment submit -c remotevm myscript.py
```
>[!NOTE]
>Tenha em mente dessa execução ambiente estiver configurado com as especificações no conda_dependencies.yml. spark_dependencies.yml também é utilizado se não for especificado no ficheiro de .runconfig PySpark framework. 

O processo de criação de Docker para as VMs remotos é exatamente o mesmo como o processo do local Docker é executado para que deve esperar uma experiência de execução similar.

>[!TIP]
>Se preferir evitar a latência introduzida pela criação da imagem de Docker para a sua primeira execução, pode utilizar o comando seguinte para preparar o destino de computação antes de executar o script. experimentação do ml de AZ preparar remotedocker - c

_**Descrição geral da execução de vm remota para um script de Python:**_
![](media/experimentation-service-configuration/remote-vm-run.png)

## <a name="running-a-script-on-a-remote-vm-targeting-user-managed-environments"></a>Executar um script numa VM remota direcionamento ambientes gerida pelo utilizador
Serviço de experimentação também suporta a executar o script num ambiente de Python do usuário dentro de uma máquina virtual do Ubuntu remota. Isto permite-lhe gerir o seu próprio ambiente de execução e continuar a utilizar capacidades do Azure Machine Learning. 

Siga os passos seguintes para executar o script no seu próprio ambiente.
* Prepare o ambiente de Python numa VM do Ubuntu remota ou uma DSVM do instalar as suas dependências.
* Instale os requisitos do Azure Machine Learning com o seguinte comando.

```
pip install -I --index-url https://azuremldownloads.azureedge.net/python-repository/preview --extra-index-url https://pypi.python.org/simple azureml-requirements
```

>[!TIP]
>Em alguns casos, terá de executar este comando no modo de sudo dependendo de seus privilégios. 
```
sudo pip install -I --index-url https://azuremldownloads.azureedge.net/python-repository/preview --extra-index-url https://pypi.python.org/simple azureml-requirements
```
 
* Utilize o seguinte comando para criar a definição de destino de computação e a configuração de execução para execuções de utilizador gerido em execuções de VM remotas.
```
az ml computetarget attach remote --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```
>[!NOTE]
>Isso definirá o parâmetro de "userManagedEnvironment" no ficheiro de configuração .compute como true.

* Definir localização do seu tempo de execução do Python executável em seu arquivo .compute. Deve consultar o caminho completo do seu executável de python. 
```
pythonLocation: python3
```

Depois de configurar o destino de computação, pode utilizar o seguinte comando para executar o script.
```
$ az ml experiment submit -c remotevm myscript.py
```

>[!NOTE]
> Quando estiver executando numa DSVM, deve utilizar os seguintes comandos

Se quiser executar diretamente no ambiente de global python da DSVM, execute este comando.
```
sudo /anaconda/envs/py35/bin/pip install <package>
```


## <a name="running-a-script-on-an-hdinsight-cluster"></a>Executar um script num cluster do HDInsight
HDInsight é uma plataforma popular para análise de macrodados do Apache Spark de suporte. Bancada de trabalho permite que a experimentação de macrodados com clusters do HDInsight Spark. 

>[!NOTE]
>O cluster do HDInsight tem de utilizar o Blob do Azure como armazenamento principal. A utilização do armazenamento do Azure Data Lake ainda não é suportada.

Pode criar um destino de computação e executar a configuração para um cluster do HDInsight Spark com o seguinte comando:

```
$ az ml computetarget attach cluster --name "myhdi" --address "<FQDN or IP address>" --username "sshuser" --password "sshpassword"  
```

>[!NOTE]
>Se utilizar FQDN em vez de um endereço IP e o nome do cluster do HDI Spark _foo_, o ponto de final SSH está no nó de controlador com o nome _foo-ssh.azurehdinsight.net_. Não se esqueça das **-ssh** sufixo no nome do servidor ao utilizar o FQDN para _– endereço_ parâmetro.


Depois de ter o contexto de cálculo, pode executar o seguinte comando para executar o script PySpark.

```
$ az ml experiment submit -c myhdi myscript.py
```

Workbench prepara e gerencia o ambiente de execução num cluster do HDInsight com Conda. Configuração seja gerida pela _conda_dependencies.yml_ e _spark_dependencies.yml_ ficheiros de configuração. 

Precisa de acesso SSH para o cluster do HDInsight para executar experimentações neste modo. 

>[!NOTE]
>Configuração suportada é a que clusters do HDInsight Spark Linux em execução (Ubuntu com 3.5.2 de Python/PySpark e o Spark 2.1.11).

_**Descrição geral do HDInsight com base em execução para um script PySpark**_
![](media/experimentation-service-configuration/hdinsight-run.png)


## <a name="running-a-script-on-gpu"></a>Executar um script no GPU
Para executar seus scripts em GPU, pode seguir as orientações neste artigo:[como utilizar a GPU no Azure Machine Learning](how-to-use-gpu.md)

## <a name="using-ssh-key-based-authentication-for-creating-and-using-compute-targets"></a>Utilizar a autenticação baseada em chave SSH para criar e utilizar os destinos de computação
O Azure Machine Learning Workbench permite-lhe criar e usar destinos de computação com a autenticação baseada em chave SSH, além do esquema de nome de utilizador/palavra-passe. Pode utilizar esta capacidade quando utilizar remotedocker ou cluster como o destino de computação. Quando utiliza este esquema, o Workbench cria um par de chaves públicas/privadas e relata a chave pública. Acrescentar a chave pública para os ficheiros de ~/.ssh/authorized_keys para seu nome de utilizador. O Azure Machine Learning Workbench, em seguida, utiliza ssh autenticação baseada em chave para aceder e em execução neste destino de computação. Uma vez que a chave privada para o destino de computação é guardada no arquivo de chave para a área de trabalho, outros usuários da área de trabalho podem utilizar o destino de computação da mesma forma, fornecendo o nome de utilizador fornecida para criar o destino de computação.  

Siga estes passos para utilizar esta funcionalidade. 

- Crie um destino de computação através de um dos seguintes comandos.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
```
ou
```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" -k
```
- Acrescente a chave pública gerada pela Bancada de trabalho para o arquivo de ~/.ssh/authorized_keys no destino da computação anexado. 

>[!IMPORTANT]
>Tem de iniciar sessão no destino de computação com o mesmo nome de utilizador que utilizou para criar o destino de computação. 

- Agora pode preparar e utilizar o destino de computação com a autenticação baseada em chave SSH.

```
az ml experiment prepare -c remotevm
```

## <a name="next-steps"></a>Passos Seguintes
* [Criar e instalar o Azure Machine Learning](../service/quickstart-installation.md)
* [Gestão de modelos](model-management-overview.md)
