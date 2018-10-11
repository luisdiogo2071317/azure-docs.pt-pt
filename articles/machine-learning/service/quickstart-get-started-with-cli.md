---
title: Introdução ao serviço Azure Machine Learning com a extensão da CLI | Microsoft Docs
description: Neste início rápido, aprenderá a começar a utilizar o serviço Azure Machine Learning com a extensão da CLI do Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
author: rastala
ms.author: roastala
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 296bbc12cb326c416c87948db0ce6d7b5ff114da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972730"
---
# <a name="quickstart-get-started-with-azure-machine-learning-using-the-cli-extension"></a>Início Rápido: Introdução ao Azure Machine Learning com a extensão da CLI

Neste início rápido, vai utilizar um extensão da CLI de aprendizagem automática para começar a utilizar o [serviço Azure Machine Learning](overview-what-is-azure-ml.md) (Pré-visualização).

Com a CLI, aprenderá a:

1. Criar uma área de trabalho na sua subscrição do Azure. A área de trabalho é utilizada por um ou mais utilizadores para armazenar os recursos de computação, modelos, implementações e históricos de execuções na cloud.
1. Anexar um projeto à área de trabalho.   Um projeto é uma pasta local que contém os scripts e ficheiros de configuração necessários para resolver o problema da aprendizagem automática.  
1. Executar um script do Python no projeto para registar alguns valores em várias iterações.
1. Ver os valores registados no histórico de execuções da área de trabalho.

> [!NOTE]
> Para sua comodidade, os seguintes recursos do Azure são adicionados automaticamente à sua área de trabalho quando estiverem disponíveis a nível regional: [registo de contentor](https://azure.microsoft.com/services/container-registry/), [armazenamento](https://azure.microsoft.com/services/storage/), [estatísticas das aplicações](https://azure.microsoft.com/services/application-insights/) e [cofre de chaves](https://azure.microsoft.com/services/key-vault/).

Os recursos que criar podem ser utilizados como pré-requisitos para outros tutoriais e artigos de procedimentos do Azure Machine Learning.

Esta CLI foi criada a partir do <a href="http://aka.ms/aml-sdk" target="_blank">SDK</a> baseado em Python para o serviço Azure Machine Learning.

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que tem os seguintes pré-requisitos antes de iniciar os passos de início rápido:

+ Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
+ [Python 3.5 ou superior](https://www.python.org/) instalado
+ [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalada

## <a name="install-the-cli-extension"></a>Instalar a extensão da CLI

No computador, abra um editor de linha de comandos e instale [a extensão de aprendizagem automática na CLI do Azure](reference-azure-machine-learning-cli.md).  A instalação pode demorar vários minutos a concluir.

```azurecli
az extension add azureml-sdk
```

## <a name="install-the-sdk"></a>Instalar o SDK

[!INCLUDE [aml-install-sdk](../../../includes/aml-install-sdk.md)]

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure. Com a CLI do Azure, inicie sessão no Azure, especifique a subscrição e crie um grupo de recursos.

Numa janela da linha de comandos, inicie sessão com o comando da CLI do Azure, `az login`. Siga as instruções de início de sessão interativo:
    
   ```azurecli
   az login
   ```

Liste as subscrições do Azure disponíveis e especifique qual pretende utilizar:
   ```azurecli
   az account list --output table
   az account set --subscription <your-subscription-id>
   az account show
   ```
   em que \<ID-da-sua-subscrição\> é o valor do ID da subscrição que deseja utilizar. Não inclua os parêntesis angulares.

Crie um grupo de recursos para conter a sua área de trabalho.
Neste início rápido:
   + O nome do grupo de recursos é `docs-aml`.
   + A região é `eastus2`. 

   ```azurecli
   az group create -n docs-aml -l eastus2
   ```

## <a name="create-a-workspace-and-a-project-folder"></a>Criar uma área de trabalho e uma pasta de projeto

Na janela da linha de comandos, crie uma Área de Trabalho do Azure Machine Learning no grupo de recursos.


   Neste início rápido:
   + O nome da área de trabalho é `docs-ws`.
   + O nome do grupo de recursos é `docs-aml`

   ```azurecli
   az ml workspace create -n docs-ws -g docs-aml
   ```

Na janela da linha de comandos, crie uma pasta no computador local para o projeto do Azure Machine Learning.

   ```
   mkdir docs-prj
   cd docs-prj
   ```

## <a name="create-a-python-script"></a>Criar um script Python

[!INCLUDE [aml-create-script-pi](../../../includes/aml-create-script-pi.md)]

## <a name="run-the-script"></a>Executar o script

Anexe a pasta como um projeto à área de trabalho. O argumento `--history` especifica um nome para o ficheiro de histórico de execuções que captura as métricas de cada execução.

   ```azurecli
   az ml project attach --history my_history -w docs-ws -g docs-aml
   ```

Execute o script no computador local.

   ```azurecli
   az ml run submit -c local pi.py
   ```

   Este comando executa o código e produz uma ligação Web à consola. Copie e cole a ligação no browser.

Num browser, aceda ao URL. É apresentado um portal Web com os resultados da execução. Pode inspecionar os resultados dessa execução ou de execuções anteriores, caso existam.

O dashboard do portal é suportado apenas nos browsers Edge, Chrome e Firefox.

   ![ver histórico](./media/quickstart-get-started/web-results.png)

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Passos seguintes
Criou os recursos necessários para começar a experimentar e implementar modelos. Criou também um projeto, executou um script e explorou o histórico de execuções desse script.

Para obter uma experiência aprofundada do fluxo de trabalho, siga o tutorial do Azure Machine Learning sobre a compilação, preparação e implementação de um modelo.

> [!div class="nextstepaction"]
> [Tutorial: compilar, preparar e implementar](tutorial-train-models-with-aml.md)