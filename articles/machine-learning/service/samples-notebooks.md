---
title: Blocos de notas Jupyter do exemplo
titleSuffix: Azure Machine Learning service
description: Encontre e utilize blocos de notas do Jupyter exemplo para explorar o serviço Azure Machine Learning em Python.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: f7affc60e3504b9b43d7663a4b7799c52fef79e7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082786"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Utilizar blocos de notas do Jupyter para explorar o serviço Azure Machine Learning


Para sua comodidade, desenvolvemos uma série de blocos de notas do Jupyter Python que pode utilizar para explorar o serviço Azure Machine Learning. 

Saiba como utilizar o serviço com a documentação sobre este site e utilizar estes blocos de notas para personalizá-las à sua situação. 

## <a name="prerequisite"></a>Pré-requisito

Concluir o [guia de início rápido do Azure Machine Learning Python](quickstart-get-started.md) para criar uma área de trabalho e iniciar os blocos de notas do Azure.

## <a name="try-azure-notebooks-free-jupyter-notebooks-in-the-cloud"></a>Experimente os blocos de notas do Azure: Gratuita blocos de notas Jupyter na cloud

É fácil começar com blocos de notas do Azure! O [do Azure Machine Learning SDK para Python](https://aka.ms/aml-sdk) já está instalado e configurado para si nos blocos de notas do Azure. A instalação e as futuras atualizações são geridas automaticamente por meio de serviços do Azure.
  
+ Para executar o **principais blocos de notas do tutoriais**:
  1. Aceda a [blocos de notas do Azure](https://notebooks.azure.com/).
    
  1. Encontrar o **tutoriais** pasta na **introdução** biblioteca que criou durante o início rápido de pré-requisitos.
    
  1. Abra o bloco de notas que pretende executar.
    
+ Para executar **outros blocos de notas**:

  1. [Importar os blocos de notas do exemplo](https://aka.ms/aml-clone-azure-notebooks) em blocos de notas do Azure.

  1. Adicione um ficheiro de configuração da área de trabalho para a biblioteca com qualquer um dos seguintes métodos:
     + Cópia a **config** ficheiro a partir do **introdução ao** clonar a biblioteca para a nova biblioteca.

     + Criar uma nova área de trabalho com o código na [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
    
  1. Abra o bloco de notas que pretende executar.     


## <a name="use-a-data-science-virtual-machine-dsvm"></a>Utilizar uma máquina de Virtual de ciência de dados (DSVM)

O [do Azure Machine Learning SDK para Python](https://aka.ms/aml-sdk) e o servidor já instalados e configurados para numa DSVM do. Utilize estes passos a executar os blocos de notas.

1. [Criar uma DSVM](how-to-configure-environment.md#dsvm).

1. Clone o [repositório do GitHub](https://aka.ms/aml-notebooks).

1. Adicione um ficheiro de configuração da área de trabalho para a biblioteca com qualquer um dos seguintes métodos:
    * Copiar o **aml_config\config.json** ficheiro que criou com o guia de introdução de pré-requisitos para o diretório clonado.

    * Criar uma nova área de trabalho com o código na [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Inicie o servidor de blocos de notas a partir do diretório clonado.

## <a name="use-your-own-jupyter-notebook-server"></a>Utilizar o seu próprio servidor de bloco de notas do Jupyter

Utilize estes passos para criar um servidor de bloco de notas Jupyter local no seu computador.

1. Certifique-se de que concluiu o guia de introdução dos pré-requisitos, na qual instalou os SDKs do Azure Machine Learning.

1. Clone o [repositório do GitHub](https://aka.ms/aml-notebooks).

1. Adicione um ficheiro de configuração da área de trabalho para a biblioteca com qualquer um dos seguintes métodos:
    * Copiar o **aml_config\config.json** ficheiro que criou com o guia de introdução de pré-requisitos para o diretório clonado.
    
    * Criar uma nova área de trabalho com o código na [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Inicie o servidor de blocos de notas a partir do diretório clonado.

1. Vá para a pasta que contém o bloco de notas.

1. Abra o bloco de notas.

<a name="auto"></a>

## <a name="automated-ml-setup"></a>Configuração automatizada de ML 

**Estes passos aplicam-se apenas aos blocos de notas no `automated-machine-learning` pasta.**

Embora seja possível usar qualquer uma das opções acima, também pode instalar o ambiente e criar uma área de trabalho ao mesmo tempo, com as instruções seguintes. 

1. Instale [Mini-conda](https://conda.io/miniconda.html). Escolha 3.7 ou superior. Siga as instruções para instalar. 
   >[!NOTE]
   >Pode usar um conda existente como há muito tempo, dado que é a versão 4.4.10 ou posterior. Utilize `conda -V` para apresentar a versão. Pode atualizar uma versão de conda com o comando: `conda update conda`. Não é necessário para instalar a mini-conda especificamente.

1. Transferir os blocos de notas de exemplo da [Github](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning
) como um zip e extraia o conteúdo para um diretório local. Os blocos de notas de aprendizagem automatizada estão no `how-to-use-azureml/automated-machine-learning` pasta.

1. Configure um novo ambiente de Conda. 
   1. Abra uma linha de Conda no seu computador local.
   
   1. Navegue para os ficheiros extraídos no seu computador local.
   
   1. Abra o `automated-machine-learning` pasta.
   
   1. Execute `automl_setup.cmd` na linha de comandos da conda para Windows, ou o `.sh` ficheiro para o seu sistema operativo. Pode demorar cerca de 10 minutos para executar.

      O script de configuração:
      + Cria um novo ambiente de conda
      + Instala os pacotes necessários
      + Configura o widget
      + Inicia um bloco de notas do jupyter
      
      O script aceita o nome do ambiente de conda como um parâmetro opcional. O nome de ambiente de conda predefinido é `azure_automl`. O comando exato depende do sistema operativo. 
      
      Assim que o script tiver concluído, verá uma home page de bloco de notas do Jupyter no seu browser.

1. Navegue para o caminho onde guardou os blocos de notas. 

1. Abra a pasta de automatizada-machine-learning, em seguida, abra o `configuration.ipynb` bloco de notas. 

1. Execute as células no bloco de notas para registar o fornecedor de recursos de serviços do Machine Learning e criar uma área de trabalho.

Agora está pronto para abrir e executar os blocos de notas guardados no seu computador local.


## <a name="next-steps"></a>Passos Seguintes

Explorar o [repositório de blocos de notas do GitHub para o serviço Azure Machine Learning](https://aka.ms/aml-notebooks)

Experimente estes tutoriais:
+ [Preparar e implementar um modelo de classificação de imagem com MNIST](tutorial-train-models-with-aml.md)

+ [Preparar dados e utilizar automatizada de machine learning para preparar um modelo de regressão com o conjunto de dados de táxis NYC](tutorial-data-prep.md)
