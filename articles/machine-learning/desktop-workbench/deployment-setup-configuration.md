---
title: Configuração de gestão de modelo do Azure Machine Learning e configuração | Documentos da Microsoft
description: Este documento descreve os passos e conceitos envolvidos na definir e configurar a gestão de modelos no Azure Machine Learning.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 12/6/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 84068376f35bc4df6672cffcc0ac3438b12edbb5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979680"
---
# <a name="model-management-setup"></a>Configuração de gestão de modelo

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Este documento apresenta-lhe começar a utilizar a gestão de modelos do Azure ML para implementar e gerir modelos como serviços da web de aprendizagem. 

Utilizar a gestão de modelos do Azure ML, pode com eficiência implementar e gerir modelos de Machine Learning, que são criados usando várias estruturas, incluindo SparkML, Keras, TensorFlow, o Microsoft Cognitive Toolkit ou Python. 

No final deste documento, será capaz de ter o seu ambiente de gestão de modelo configurado e pronto para implementar modelos de aprendizagem.

## <a name="what-you-need-to-get-started"></a>O que precisa para começar a utilizar
Para tirar o máximo proveito deste guia, deve ter acesso de contribuinte a uma subscrição do Azure ou um grupo de recursos que pode implementar os seus modelos para.
A CLI vem pré-instalada no Azure Machine Learning Workbench e, no [Azure DSVMs](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>Com a CLI
Para utilizar as interfaces de linha de comandos (CLI) do Workbench, clique em **arquivo** -> **linha de comandos aberta**. 

Num dados de Máquina Virtual de ciência, ligue e abra o prompt de comando. Tipo de `az ml -h` para ver as opções. Para obter mais detalhes sobre os comandos, utilize o-- sinalizador de ajuda.

Em todos os outros sistemas, terá de instalar a CLI.

>[!NOTE]
> Num bloco de notas do Jupyter numa DSVM do Linux, pode aceder a CLI do Azure e a CLI do Azure ML com o formato do comando abaixo.  **Isso é para um bloco de notas do Jupyter numa DSVM do Linux, especificamente**.  Estes comandos acedem o kernel de Python atual no bloco de notas (por exemplo, o conda `py35` ambiente)
>```
>import sys
>! {sys.executable} -m azure.cli login
>! {sys.executable} -m azure.cli ml -h
>```

### <a name="installing-or-updating-on-windows"></a>Instalar (ou atualizar) no Windows

Instalar o Python de https://www.python.org/. Certifique-se de que selecionou para instalar o pip.

Abra um prompt de comando usando executar como administrador e execute os seguintes comandos:

```cmd
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="installing-or-updating-on-linux"></a>Instalar (ou atualizar) no Linux
Execute o seguinte comando na linha de comando e siga as instruções:

```bash
sudo -i
pip install -r https://aka.ms/az-ml-o16n-cli-requirements-file
```

### <a name="configuring-docker-on-linux"></a>Configurar Docker no Linux
Para configurar o Docker no Linux para utilização por utilizadores não raiz, siga as instruções aqui: [os passos de pós-instalação para Linux](https://docs.docker.com/engine/installation/linux/linux-postinstall/)

>[!NOTE]
> Numa DSVM do Linux, pode executar o script abaixo para configurar corretamente o Docker. **Lembre-se de terminar sessão e voltar a iniciar sessão depois de executar o script.**
>```
>sudo /opt/microsoft/azureml/initial_setup.sh
>```

## <a name="deploying-your-model"></a>Implementar o seu modelo
Utilize a CLI para implementar modelos como serviços da web. Os serviços da web podem ser implementados localmente ou num cluster.

Começar com uma implementação local, validar que o modelo e o código funcionam, em seguida, implementar num cluster para efeitos de escala de produção.

Para começar, terá de configurar o ambiente de implantação. Configuração do ambiente é uma tarefa de tempo. Depois da configuração estiver concluída, é possível reutilizar o ambiente para implementações subsequentes. Consulte a secção seguinte para obter mais detalhes.

Quando concluir a configuração do ambiente:
- Lhe for pedido para iniciar sessão no Azure. Para iniciar sessão, utilize um browser para abrir a página https://aka.ms/devicelogin e introduza o código fornecido para autenticar.
- Durante o processo de autenticação, lhe for pedido para uma conta autenticar com o. Importante:-Selecione uma conta que tenha uma subscrição do Azure válida e permissões suficientes para criar recursos na conta. Quando o início de sessão estiver concluído, as informações de subscrição são apresentadas e lhe for perguntado se desejar continuar com a conta selecionada.

### <a name="environment-setup"></a>Configuração do ambiente
Para iniciar o processo de configuração, terá de registar os fornecedores de ambiente alguns ao introduzir os seguintes comandos:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
```
#### <a name="local-deployment"></a>Implementação de local
Para implementar e testar o seu serviço web no computador local, configure um ambiente local com o seguinte comando. O nome do grupo de recursos é opcional.

```azurecli
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Implementação do serviço local web requer a instalação do Docker no computador local. 
>

O comando de configuração do ambiente local cria os seguintes recursos na sua subscrição:
- Um recurso de grupo (se não for indicado, ou se o nome fornecido não existir)
- Uma conta de armazenamento
- Um Azure Container Registry (ACR)
- Uma conta do Application insights

Após a conclusão da configuração com êxito, defina o ambiente para ser utilizado com o seguinte comando:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Implementação de cluster
Utilize a implementação de Cluster para cenários de produção de grande escala. Ele configura um cluster de ACS com o Kubernetes como o orchestrator. O cluster de ACS pode ser dimensionado para processar um débito maior de suas chamadas de serviço da web.

Para implementar o seu serviço web num ambiente de produção, primeiro configure o ambiente com o seguinte comando:

```azurecli
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. eastus2] [-g [resource group]]
```

O comando de configuração do ambiente de cluster cria os seguintes recursos na sua subscrição:
- Um recurso de grupo (se não for indicado, ou se o nome fornecido não existir)
- Uma conta de armazenamento
- Um Azure Container Registry (ACR)
- Uma implementação de Kubernetes num cluster do Azure Container Service (ACS)
- Uma conta do Application insights

>[!IMPORTANT]
> Para criar com êxito um ambiente de cluster, terá de estar a ter acesso de Contribuidor na subscrição do Azure ou o grupo de recursos.

O grupo de recursos, a conta de armazenamento e o ACR são criadas rapidamente. A implementação do ACS pode demorar até 20 minutos. 

Para verificar o estado de um tipo de aprovisionamento de clusters em curso, utilize o seguinte comando:

```azurecli
az ml env show -n [environment name] -g [resource group]
```

Após a conclusão da configuração, tem de definir o ambiente para ser utilizado para esta implementação. Utilize o seguinte comando:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Depois do ambiente for criado, para implementações subsequentes, só tem de utilizar o comando set acima para reutilizá-la.
>

### <a name="create-a-model-management-account"></a>Criar uma conta de gestão de modelos
Uma conta de gestão é necessária para a implementação de modelos. Terá de fazer isto vez por subscrição e pode reutilizar a mesma conta em várias implementações.

Para criar uma nova conta, utilize o seguinte comando:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Para utilizar uma conta existente, utilize o seguinte comando:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

Como resultado desse processo, o ambiente está pronto e a conta de gestão de modelo foi criada para fornecer os recursos necessários para gerir e implementar modelos de Machine Learning (consulte [gestão de modelos do Azure Machine Learning](model-management-overview.md) para um Descrição geral).

## <a name="next-steps"></a>Passos Seguintes

* Para obter instruções sobre como implementar serviços web para serem executadas num computador local ou um cluster, continue para [implementar um modelo de aprendizagem automática como um serviço Web](model-management-service-deploy.md).
* Experimente um dos muito exemplos na galeria.
