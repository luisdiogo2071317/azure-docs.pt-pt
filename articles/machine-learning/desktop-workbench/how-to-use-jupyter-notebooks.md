---
title: Como utilizar o Jupyter Notebooks no Azure Machine Learning Workbench | Documentos da Microsoft
description: Um guia para utilizar a funcionalidade de bloco de notas Jupyter do Azure Machine Learning Workbench
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: 712cdaa65487620b2f8af4a0ad57c01c24b9a965
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649961"
---
# <a name="use-jupyter-notebooks-in-azure-machine-learning-workbench"></a>Utilizar blocos de notas do Jupyter no Azure Machine Learning Workbench

O Azure Machine Learning Workbench oferece suporte a experimentação de ciência de dados interativa por meio da integração com blocos de notas do Jupyter. Este artigo descreve como fazer uso eficiente esse recurso para aumentar a velocidade e qualidade da sua experimentação de ciência de dados interativos.

## <a name="prerequisites"></a>Pré-requisitos
- [Criar contas do Azure Machine Learning e instalar o Azure Machine Learning Workbench](../service/quickstart-installation.md).
- Estar familiarizado com o [bloco de notas do Jupyter](http://jupyter.org/). Este artigo não é sobre como utilizar o Jupyter de aprendizado.

## <a name="jupyter-notebook-architecture"></a>Arquitetura de bloco de notas do Jupyter
Num alto nível, a arquitetura de bloco de notas do Jupyter inclui três componentes. Cada uma pode executar em ambientes de computação diferentes:

- **Cliente**: recebe a intervenção do utilizador e apresenta composta de saída.
- **Servidor**: servidor Web que aloja os ficheiros de bloco de notas (.ipynb ficheiros).
- **Kernel**: ambiente de tempo de execução, no qual a execução do notebook células acontece.

Para obter mais informações, consulte oficial [Jupyter documentação](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). O diagrama seguinte ilustra como essa arquitetura de cliente, o servidor e o kernel é mapeada para os componentes no Azure Machine Learning:

![Arquitetura de bloco de notas do Jupyter](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-machine-learning-workbench-notebooks"></a>Kernels em blocos de notas do Azure Machine Learning Workbench
Pode aceder a diferentes kernels no Azure Machine Learning Workbench, definindo as configurações de execução e de computação destinos no `aml_config` pasta do projeto. Adicionar um novo destino de computação ao emitir o `az ml computetarget attach` comando é o equivalente da adição de um novo kernel.

>[!NOTE]
>Revisão [configurar o serviço do Azure Machine Learning experimentação](experimentation-service-configuration.md) para obter mais detalhes sobre executado configurações e destinos de computação.

### <a name="kernel-naming-convention"></a>Convenção de nomenclatura de kernel
O Azure Machine Learning Workbench gera personalizados kernels do Jupyter. Estes kernels são nomeados  *\<nome do projeto > \<execute o nome de configuração >*. Por exemplo, se tiver uma configuração de execução com o nome _docker-python_ num projeto chamado _myIris_, o Azure Machine Learning disponibilizam um kernel com o nome *myIris docker-python.* Definir o kernel em execução no bloco de notas Jupyter **Kernel** menu, na **kernel de alteração** submenu. O nome do kernel em execução é apresentada na extremidade direita da barra de menus.
 
Atualmente, o Azure Machine Learning Workbench suporta os seguintes tipos de kernels.

### <a name="local-python-kernel"></a>Kernel de Python local
Este kernel Python suporta a execução em máquinas locais. Ele está integrado com o suporte de histórico de execuções do Azure Machine Learning. O nome do kernel é normalmente *my_project_name local.*

>[!NOTE]
>Não utilize o kernel de Python 3. É um kernel autônoma fornecido pelo Jupyter por predefinição e não está integrado com recursos do Azure Machine Learning. Por exemplo, o `%azureml` funções mágicas do Jupyter devolvem erros de "não encontrados". 

### <a name="python-kernel-in-docker-local-or-remote"></a>Kernel de Python no Docker (locais ou remotos)
Este kernel Python é executado num contentor do Docker no seu computador local ou numa máquina de virtual de Linux remota (VM). O nome do kernel é normalmente *my_project docker.* Associada `docker.runconfig` ficheiro tem o `Framework` campo definido como `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>Kernel do PySpark no Docker (locais ou remotos)
Este kernel do PySpark executa scripts num contexto de Spark em execução num contentor do Docker, no seu computador local ou numa VM do Linux remota. O nome de kernel é normalmente *my_project docker.* Associada `docker.runconfig` ficheiro tem o `Framework` campo definido como `PySpark`.

### <a name="pyspark-kernel-in-an-azure-hdinsight-cluster"></a>Kernel do PySpark num cluster HDInsight do Azure
Este kernel é executado no Azure HDInsight cluster remoto que anexado como um destino de computação para o seu projeto. O nome de kernel é normalmente *my_project my_hdi.* 

>[!IMPORTANT]
>Na `.compute` destino de computação de ficheiro para o HDI, tem de alterar o `yarnDeployMode` campo `client` (o valor predefinido é `cluster`) para utilizar este kernel. 

## <a name="start-a-jupyter-server-from-azure-machine-learning-workbench"></a>Iniciar um servidor de Jupyter do Azure Machine Learning Workbench
Do Azure Machine Learning Workbench, pode acessar os blocos de notas através da **blocos de notas** separador. O _Classifying Iris_ projeto de exemplo inclui um `iris.ipynb` bloco de notas de exemplo.

![Separador de blocos de notas](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Quando abre um bloco de notas no Azure Machine Learning Workbench, será apresentado no separador seu próprio documento **modo de pré-visualização**. Esta é uma vista só de leitura que não requer um servidor em execução do Jupyter e kernel.

![Pré-visualização do bloco de notas](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Selecionar o **iniciar o servidor de bloco de notas** botão inicia o servidor do Jupyter e muda para o bloco de notas **modo de edição**. A interface de usuário familiar do bloco de notas do Jupyter aparece embedded na bancada de trabalho. Agora pode definir um kernel do **Kernel** menu e iniciar a sessão de bloco de notas interativo. 

>[!NOTE]
>Com kernels não local, pode demorar um minuto ou dois para começar a se estiver a utilizá-lo pela primeira vez. Pode executar o `az ml experiment prepare` comando na janela da CLI para preparar o destino de computação, de modo kernel muito mais rapidamente começa depois do destino de computação é preparado.

![Modo de edição](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Esta é uma experiência totalmente interativa do bloco de notas do Jupyter. Todas as operações de bloco de notas regular de atalhos de teclado que são suportados nessa janela, exceto por algumas operações de arquivo que pode ser feito por meio da bancada de trabalho **blocos de notas** separador e **ficheiro** separador.

## <a name="start-a-jupyter-server-from-the-command-line"></a>Iniciar um servidor de Jupyter a partir da linha de comandos
Também pode iniciar uma sessão de bloco de notas emitindo `az ml notebook start` na janela da linha de comandos:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
Copy and paste this URL into your browser when you connect for the first time, to login with a token: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
Browser predefinido abre automaticamente com o servidor de Jupyter apontando para o diretório de raiz do projeto. Também pode utilizar o URL e o token apresentado na janela da CLI para abrir outras janelas do browser localmente. 

![Dashboard do projeto](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Agora, pode selecionar um `.ipynb` ficheiro do bloco de notas, abra-o, defina o kernel (se não tiver sido definida) e iniciar a sessão interativa.

![Dashboard do projeto](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Usar comandos mágicas para gerenciar experimentações

Pode usar [mágica comandos](http://ipython.readthedocs.io/en/stable/interactive/magics.html) dentro de seu células de bloco de notas para monitorizar o histórico de execução e guardar saídas, tais como modelos ou conjuntos de dados.

Para controlar as execuções de célula do bloco de notas individuais, utilize o `%azureml history on` mágica de comando. Depois de ativar o histórico, cada execução de célula é apresentado como uma entrada no histórico de execuções:

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Para desativar o controlo de célula executar, utilize o `%azureml history off` mágica de comando.

Pode utilizar o `%azureml upload` mágica de comando para salvar os arquivos de modelo e dados de sua execução. Os objetos guardados são apresentados como saídas no histórico de execução:

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>As saídas tem de ser guardadas para uma pasta chamada *saídas.*

## <a name="next-steps"></a>Passos Seguintes
- Para saber como utilizar o bloco de notas do Jupyter, veja a [documentação oficial do Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Para obter uma compreensão mais aprofundada do ambiente de execução da experimentação do Azure Machine Learning, consulte [configurar o serviço do Azure Machine Learning experimentação](experimentation-service-configuration.md).

