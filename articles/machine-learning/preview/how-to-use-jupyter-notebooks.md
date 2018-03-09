---
title: Como utilizar os Jupyter Notebooks no Azure Machine Learning Workbench | Microsoft Docs
description: Um guia para utilizar a funcionalidade de notas do Jupyter do Workbench do Azure Machine Learning
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: c21b7096f689efedacd6e7d55d83912d35dff803
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="use-jupyter-notebooks-in-azure-machine-learning-workbench"></a>Utilize blocos de notas do Jupyter no Azure Machine Learning Workbench

Azure Workbench de aprendizagem máquina suporta experimentação de ciência de dados interativa através da integração com blocos de notas do Jupyter. Este artigo descreve como tornar a utilização eficaz desta funcionalidade para aumentar a velocidade e da qualidade da sua experimentação de ciência de dados interativa.

## <a name="prerequisites"></a>Pré-requisitos
- [Criar contas do Azure Machine Learning e instalar o Azure Machine Learning Workbench](quickstart-installation.md).
- Deve estar familiarizado com o [bloco de notas do Jupyter](http://jupyter.org/). Este artigo não é sobre aprender a utilizar os Jupyter.

## <a name="jupyter-notebook-architecture"></a>Arquitetura de notas do Jupyter
Um nível elevado, a arquitetura de notas do Jupyter inclui três componentes. Cada pode ser executados em ambientes de computação diferentes:

- **Cliente**: recebe intervenção do utilizador e apresenta composta de saída.
- **Servidor**: servidor Web que aloja os ficheiros de bloco de notas (.ipynb ficheiros).
- **Kernel**: em que a execução do bloco de notas acontece células de ambiente de tempo de execução.

Para obter mais informações, consulte oficial [Jupyter documentação](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). O diagrama seguinte ilustra a forma como esta arquitetura de cliente, o servidor e o kernel mapeia para os componentes no Azure Machine Learning:

![Arquitetura de notas do Jupyter](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-machine-learning-workbench-notebooks"></a>Kernels em blocos de notas do Workbench do Azure Machine Learning
Pode aceder a diferentes kernels no Azure Machine Learning Workbench por definir configurações de execução e computação destinos no `aml_config` pasta no seu projeto. A adição de um destino de cálculo nova ao emitir o `az ml computetarget attach` comando é o equivalente a adicionar um novo kernel.

>[!NOTE]
>Reveja [configurar o serviço do Azure Machine Learning experimentação](experimentation-service-configuration.md) para obter mais detalhes sobre executado configurações e destinos de computação.

### <a name="kernel-naming-convention"></a>Convenção de nomenclatura de kernel
Workbench de aprendizagem máquina Azure gera personalizados Jupyter kernels. Estes kernels são denominados  *\<nome do projeto > \<executar o nome de configuração >*. Por exemplo, se tiver uma configuração de execução com o nome _docker python_ num projeto com o nome _myIris_, do Azure Machine Learning torna disponível com o nome de um kernel *myIris docker-python.* Definir o kernel em execução no bloco de notas do Jupyter **Kernel** menu, no **kernel alteração** o submenu. O nome de kernel em execução aparece na extremidade direita da barra de menus.
 
Atualmente, o Azure Machine Learning Workbench suporta os seguintes tipos de kernels.

### <a name="local-python-kernel"></a>Local kernel do Python
Este kernel Python suporta a execução em máquinas locais. Está integrado com o suporte de histórico de execuções do Azure Machine Learning. O nome do kernel está normalmente *my_project_name local.*

>[!NOTE]
>Não utilize o kernel do Python 3. Este é um kernel autónomo fornecida pelo Jupyter por predefinição e não estiver integrado com capacidades do Azure Machine Learning. Por exemplo, o `%azureml` funções mágicos do Jupyter devolvam erros de "não encontrados". 

### <a name="python-kernel-in-docker-local-or-remote"></a>Kernel Python no Docker (locais ou remotos)
Este kernel Python é executado num contentor Docker no seu computador local ou num computador remoto Linux virtual (VM). O nome do kernel está normalmente *my_project docker.* Associada `docker.runconfig` ficheiro tem o `Framework` campo definido como `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>Kernel do PySpark no Docker (locais ou remotos)
Este kernel do PySpark executa scripts num contexto de Spark no interior de um contentor de Docker, no seu computador local ou numa VM com Linux remoto. O nome de kernel está normalmente *my_project docker.* Associada `docker.runconfig` ficheiro tem o `Framework` campo definido como `PySpark`.

### <a name="pyspark-kernel-in-an-azure-hdinsight-cluster"></a>Kernel do PySpark num cluster do Azure HDInsight
Este kernel que é executado no Azure HDInsight cluster remoto que ligado como um destino de computação para o seu projeto. O nome de kernel está normalmente *my_project my_hdi.* 

>[!IMPORTANT]
>No `.compute` ficheiro para o HDI computação destino, tem de alterar o `yarnDeployMode` campo para `client` (o valor predefinido é `cluster`) para utilizar este kernel. 

## <a name="start-a-jupyter-server-from-azure-machine-learning-workbench"></a>Iniciar um servidor de Jupyter do Workbench do Azure Machine Learning
Pode aceder a blocos de notas através do Workbench do Azure Machine Learning, o **blocos de notas** separador. O _classificar Iris_ projeto de exemplo inclui uma `iris.ipynb` bloco de notas do exemplo.

![separador de blocos de notas](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Quando abre um bloco de notas no Azure Machine Learning Workbench, é apresentado no seu próprio separador documento **modo de pré-visualização**. Esta é uma vista só de leitura que não requer um servidor de Jupyter em execução e de kernel.

![pré-visualização do bloco de notas](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Selecionar o **iniciar o bloco de notas do servidor** botão inicia o servidor do Jupyter e muda o bloco de notas para **modo de edição**. A interface de utilizador do bloco de notas do Jupyter familiar aparece incorporada no Workbench. Agora pode definir um kernel do **Kernel** menu e iniciar a sessão interativa de bloco de notas. 

>[!NOTE]
>Com kernels não local, pode demorar um minuto ou dois ao iniciar se estiver a utilizá-la pela primeira vez. Pode executar o `az ml experiment prepare` comando a partir da janela do CLI para preparar o destino de computação, de modo kernel muito mais rapidamente inicia após o cálculo de destino está preparado.

![Modo de edição](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Esta é uma experiência de notas do Jupyter totalmente interativa. Todas as operações normais do bloco de notas e atalhos de teclado são suportados por esta janela, exceto para algumas operações de ficheiro que pode ser efetuado através do Workbench **blocos de notas** separador e **ficheiro** separador.

## <a name="start-a-jupyter-server-from-the-command-line"></a>Iniciar um servidor de Jupyter na linha de comandos
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
O browser predefinido abre automaticamente com o servidor de Jupyter apontar para o diretório de raiz do projeto. Também pode utilizar o URL e o token apresentado na janela da CLI para abrir outras janelas do browser localmente. 

![dashboard do projeto](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Agora, pode selecionar um `.ipynb` ficheiro do bloco de notas, abra-o, definir do kernel (se ainda não foi definido) e iniciar a sessão interativa.

![dashboard do projeto](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="use-magic-commands-to-manage-experiments"></a>Utilizar comandos mágicos para gerir as experimentações

Pode utilizar [mágica comandos](http://ipython.readthedocs.io/en/stable/interactive/magics.html) nas células bloco de notas para controlar o histórico de execução e guardar saídas, tais como modelos ou conjuntos de dados.

Para controlar as execuções de célula do bloco de notas individuais, utilize o `%azureml history on` mágica comando. Depois de ativar o histórico, cada execução de célula é apresentado como uma entrada no histórico de execução:

```
%azureml history on
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()
logger.log("Cell","Load Data")
```

Para desativar o controlo de célula executar, utilize o `%azureml history off` mágica comando.

Pode utilizar o `%azureml upload` mágica comando para guardar os ficheiros de modelo e os dados da sua execução. Objetos guardados aparecem como saídas na vista do histórico de execução:

```
modelpath = os.path.join("outputs","model.pkl")
with open(modelpath,"wb") as f:
    pickle.dump(model,f)
%azureml upload outputs/model.pkl
```

>[!NOTE]
>As saídas tem de ser guardadas para uma pasta denominada *produz.*

## <a name="next-steps"></a>Passos Seguintes
- Para saber como utilizar o bloco de notas do Jupyter, consulte o [documentação oficial do Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Para obter uma compreensão mais aprofundada sobre o ambiente de execução de experimentação do Azure Machine Learning, consulte [configurar o serviço do Azure Machine Learning experimentação](experimentation-service-configuration.md).

