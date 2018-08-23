---
title: Como utilizar o GPU para o Azure Machine Learning | Documentos da Microsoft
description: Este artigo descreve como utilizar unidades de processamento gráfico (GPU) para treinar redes neurais profundas no Azure Machine Learning Workbench.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: f3b6c4f6af14615511400650662fe7a350c172ba
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "42057408"
---
# <a name="how-to-use-gpu-in-azure-machine-learning"></a>Como utilizar a GPU no Azure Machine Learning
Unidade de processamento gráfico (GPU) é amplamente utilizada para processar tarefas computacionalmente intensivas que, normalmente, podem acontecer quando determinados modelos de rede neural profunda de treinamento. Ao utilizar GPUs, pode reduzir significativamente o tempo de treinamento dos modelos. Neste documento, irá aprender a configurar para utilizar do Azure ML Workbench [DSVM (máquina de Virtual de ciência de dados)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) equipados com GPUs como destino de execução. 

## <a name="prerequisites"></a>Pré-requisitos
- Para seguir este guia de procedimentos, precisa primeiro [instalar o Azure ML Workbench](../service/quickstart-installation.md).
- Tem de ter acesso a computadores equipados com NVidia GPUs.
    - Pode executar os scripts diretamente na máquina local (Windows ou macOS) com GPUs.
    - Também pode executar scripts num contentor do Docker num computador com Linux com GPUs....

## <a name="execute-in-local-environment-with-gpus"></a>Executar num _local_ ambiente com GPUs
Pode instalar o Azure ML Workbench num computador equipado com GPUs e executar contra _local_ ambiente. Isto pode ser:
- Uma máquina física Windows ou macOS.
- Uma VM do Windows (Máquina Virtual) como uma DSVM do Windows aprovisionado com o modelo de VMs de série NC do Azure.

Neste caso, não há nenhuma configuração especial necessária no Azure ML Workbench. Certifique-se de que tem todos os controladores necessários, kits de ferramentas e ativadas para GPU aprendizagem bibliotecas instaladas localmente. Simplesmente execute contra _local_ ambiente em que o tempo de execução do Python pode acessar diretamente o hardware GPU local.

1. Abra AML Bancada de trabalho. Aceda a **arquivo** e **linha de comandos aberta**. 
2. Linha de comandos, instale o framework de aprendizagem profunda ativadas para GPU, como o Microsoft Cognitive Toolkit, TensorFlow e etc. Por exemplo:

```batch
REM install latest TensorFlow with GPU support
C:\MyProj> pip install tensorflow-gpu

REM install Microsoft Cognitive Toolkit 2.5 with GPU support on Windows
C:\MyProj> pip install https://cntk.ai/PythonWheel/GPU/cntk_gpu-2.5.1-cp35-cp35m-win_amd64.whl
```

3. Escreva um código de Python que aproveita a bibliotecas de aprendizagem profunda.
4. Escolher _local_ como ambiente de computação e executar o código de Python.

```batch
REM execute Python script in local environment
C:\MyProj> az ml experiment submit -c local my-deep-learning-script.py
```

## <a name="execute-in-docker-environment-on-linux-vm-with-gpus"></a>Executar num _docker_ ambiente numa VM do Linux com GPUs
O Azure ML Workbench também suportam a execução no Docker numa VM Linux do Azure. Aqui tem uma ótima opção para executar tarefas computacionalmente intensivas num pedaço de hardware virtual potente e pagar apenas pela utilização por desativá-lo quando terminar. Enquanto em princípio, é possível usar GPUs em qualquer máquina virtual do Linux, a DSVM baseada em Ubuntu é fornecido com o necessários drivers CUDA e bibliotecas previamente instaladas, facilitando muito da configuração. Siga os passos abaixo:

### <a name="create-a-ubuntu-based-linux-data-science-virtual-machine-in-azure"></a>Criar uma máquina de Virtual de ciência de dados de Linux baseadas em Ubuntu no Azure
1. Abra o browser e aceda ao [portal do Azure](https://portal.azure.com)

2. Selecione **+ novo** no lado esquerdo do portal.

3. Procure "Dados Máquina Virtual de ciência para Linux (Ubuntu)" no marketplace.

4. Clique em **criar** para criar um DSVM em Ubuntu.

5. Preencha os **Noções básicas** formulário com as informações necessárias.
Ao selecionar a localização para a sua VM, tenha em atenção que as VMs de GPU só estão disponíveis em determinadas regiões do Azure, por exemplo, **Centro-Sul**. Ver [produtos disponíveis por região de computação](https://azure.microsoft.com/regions/services/).
Clique em OK para guardar a **Noções básicas** informações.

6. Escolha o tamanho da máquina virtual. Selecione um dos tamanhos com um prefixo NC VMs, que estão equipados com chips de GPU da NVidia.  Clique em **ver tudo** para ver a lista completa, conforme necessário. Saiba mais sobre [as VMs do Azure de equipado com GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

7. Concluir as restantes definições e rever as informações de compra. Clique em compra, para criar a VM. Tome nota do endereço IP atribuída à máquina virtual. 

### <a name="create-a-new-project-in-azure-ml-workbench"></a>Criar um novo projeto no Azure ML Workbench 
Pode utilizar o _MNIST de classificação de mensagens em fila usando o TensorFlow_ exemplo, ou o _MNIST de classificar o conjunto de dados com o CNTK_ exemplo.

### <a name="create-a-new-compute-target"></a>Criar um novo destino de computação
Inicie a linha de comandos do Azure ML Workbench. Introduza o seguinte comando. Substitua o texto de marcador de posição do exemplo abaixo com seus próprios valores para o nome, endereço IP, nome de utilizador e palavra-passe. 

```batch
C:\MyProj> az ml computetarget attach remotedocker --name "my_dsvm" --address "my_dsvm_ip_address" --username "my_name" --password "my_password" 
```

### <a name="configure-azure-ml-workbench-to-access-gpu"></a>Configurar o Azure ML Workbench para acesso GPU
Volte ao projeto e open **File View**e acessos a **atualizar** botão. Agora, pode ver dois novos ficheiros de configuração `my_dsvm.compute` e `my_dsvm.runconfig`.
 
Abra o `my_dsvm.compute`. Alteração da `baseDockerImage` para `microsoft/mmlspark:plus-gpu-0.7.9` e adicione uma nova linha `nvidiaDocker: true`. Portanto, o ficheiro deve ter estas duas linhas:
 
```yaml
...
baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9
nvidiaDocker: true
```
 
Agora, abra `my_dsvm.runconfig`, altere `Framework` partir da `PySpark` para `Python`. Se não vir esta linha, adicioná-lo, uma vez que o valor predefinido seria `PySpark`.

```yaml
"Framework": "Python"
```
### <a name="reference-deep-learning-framework"></a>Arquitetura de aprendizagem profunda de referência 
Abra `conda_dependencies.yml` de ficheiros e certifique-se de que está a utilizar a versão GPU da pacotes de Python de estrutura de aprendizagem profunda. Os projetos de exemplo listam versões de CPU, por isso terá de efetuar esta alteração.

Exemplo para o TensorFlow: 
```
name: project_environment
dependencies:
  - python=3.5.2
  # latest TensorFlow library with GPU support
  - tensorflow-gpu
```

Exemplo de ferramentas cognitivas da Microsoft:
```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip: 
    # use the Linux build of Microsoft Cognitive Toolkit 2.5 with GPU support
    - https://cntk.ai/PythonWheel/GPU/cntk_gpu-2.5.1-cp35-cp35m-win_amd64.whl
```

### <a name="execute"></a>Executar
Agora está pronto para executar os seus scripts de Python. Pode executá-los dentro do Azure ML Workbench com o `my_dsvm` contexto ou carregá-los a partir da linha de comando:
 
```batch
C:\myProj> az ml experiment submit -c my_dsvm my_tensorflow_or_cntk_script.py
```
 
Para verificar que a GPU é utilizada, examine o resultado de execução para ver algo semelhante ao seguinte:

```
name: Tesla K80
major: 3 minor: 7 memoryClockRate (GHz) 0.8235
pciBusID 06c3:00:00.0
Total memory: 11.17GiB
Free memory: 11.11GiB
```

Parabéns! O script aproveitada apenas a potência da GPU por meio de um contentor de Docker!

## <a name="next-steps"></a>Passos Seguintes
Ver um exemplo da utilização de GPU para acelerar o treinamento de rede neural profunda na galeria do Azure ML.
