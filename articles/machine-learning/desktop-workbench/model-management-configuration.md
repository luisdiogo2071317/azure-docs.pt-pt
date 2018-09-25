---
title: Configuração de gestão de modelo do Azure Machine Learning e configuração | Documentos da Microsoft
description: Este documento descreve os passos e conceitos envolvidos na definir e configurar a gestão de modelos no Azure Machine Learning.
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/29/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 6660657141cc5aac532d121b61c7c8db6a24ccda
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968718"
---
# <a name="model-management-setup"></a>Configuração de gestão de modelo

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



## <a name="overview"></a>Descrição geral
Este documento apresenta-lhe começar a utilizar a gestão de modelos do Azure ML para implementar e gerir modelos como serviços da web de aprendizagem. 

Utilizar a gestão de modelos do Azure ML, pode com eficiência implementar e gerir modelos de Machine Learning, que são criados usando várias estruturas, incluindo SparkML, Keras, TensorFlow, o Microsoft Cognitive Toolkit ou Python. 

No final deste documento, será capaz de ter o seu ambiente de gestão de modelo configurado e pronto para implementar modelos de aprendizagem.

## <a name="what-you-need-to-get-started"></a>O que precisa para começar a utilizar
Para obter o máximo proveito deste guia, deve ter acesso de proprietário a uma subscrição do Azure que pode implementar os seus modelos para.
A CLI vem pré-instalada no Azure Machine Learning Workbench e, no [Azure DSVMs](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).

## <a name="using-the-cli"></a>Com a CLI
Para utilizar as interfaces de linha de comandos (CLI) do Workbench, clique em **arquivo** -] **Interface de linha de comandos aberta**. 

Num dados de Máquina Virtual de ciência, ligue e abra o prompt de comando. Tipo de `az ml -h` para ver as opções. Para obter mais detalhes sobre os comandos, utilize o-- sinalizador de ajuda.

Em todos os outros sistemas, terá de instalar a CLI.

### <a name="installing-or-updating-on-windows"></a>Instalar (ou atualizar) no Windows

Instalar o Python de https://www.python.org/. Certifique-se de que selecionou para instalar o pip.

Abra um prompt de comando usando executar como administrador e execute os seguintes comandos:

```cmd
pip install azure-cli
pip install azure-cli-ml
```
 
>[!NOTE]
>Se tiver uma versão anterior, desinstale-primeiro com o seguinte comando:
>

```cmd
pip uninstall azure-cli-ml
```

### <a name="installing-or-updating-on-linux"></a>Instalar (ou atualizar) no Linux
Execute o seguinte comando na linha de comando e siga as instruções:

```bash
sudo -i
pip install azure-cli
pip install azure-cli-ml
```

Após a instalação estiver concluída, execute o seguinte comando:

```bash
sudo /opt/microsoft/azureml/initial_setup.sh
```

>[!NOTE]
>Terminar sessão e voltar a iniciar sessão a sessão SSH para que as alterações entrem em vigor.
>Pode utilizar os comandos anteriores para atualizar uma versão anterior do CLIs na DSVM.
>

## <a name="deploying-your-model"></a>Implementar o seu modelo
Utilize a CLI para implementar modelos como serviços da web. Os serviços da web podem ser implementados localmente ou num cluster.

Começar com uma implementação local, validar que o modelo e o código funcionam, em seguida, implementar num cluster para efeitos de escala de produção.

Para começar, terá de configurar o ambiente de implantação. Configuração do ambiente é uma tarefa de tempo. Depois da configuração estiver concluída, é possível reutilizar o ambiente para implementações subsequentes. Consulte a secção seguinte para obter mais detalhes.

Quando concluir a configuração do ambiente:
- Lhe for pedido para iniciar sessão no Azure. Para iniciar sessão, utilize um browser para abrir a página https://aka.ms/devicelogin e introduza o código fornecido para autenticar.
- Durante o processo de autenticação, lhe for pedido para uma conta autenticar com o. Importante: Selecione uma conta que tenha uma subscrição do Azure válida e permissões suficientes para criar recursos na conta. - quando o início de sessão estiver concluído, as informações da sua subscrição são apresentadas e lhe for perguntado se desejar continuar com o conta selecionada.

### <a name="environment-setup"></a>Configuração do ambiente
Para iniciar o processo de configuração, terá de registar o fornecedor de ambiente, introduzindo o seguinte comando:

```azurecli
az provider register -n Microsoft.MachineLearningCompute
```

#### <a name="local-deployment"></a>Implementação de local
Para implementar e testar o seu serviço web no computador local, configure um ambiente local com o seguinte comando:

```azurecli
az ml env setup -l [location of Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
```
>[!NOTE] 
>Implementação do serviço local web requer o para instalar o Docker no computador local. 
>

O comando de configuração do ambiente local cria os seguintes recursos na sua subscrição:
- Um grupo de recursos (se não for indicado)
- Uma conta de armazenamento
- Um Azure Container Registry (ACR)
- Application Insights

Após a conclusão da configuração com êxito, defina o ambiente para ser utilizado com o seguinte comando:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

#### <a name="cluster-deployment"></a>Implementação de cluster
Utilize a implementação de Cluster para cenários de produção de grande escala. Ele configura um cluster de ACS com o Kubernetes como o orchestrator. O cluster de ACS pode ser dimensionado para processar um débito maior de suas chamadas de serviço da web.

Para implementar o seu serviço web num ambiente de produção, primeiro configure o ambiente com o seguinte comando:

```azurecli
az ml env setup -c --name [your environment name] --location [Azure region e.g. eastus2] [-g [resource group]]
```

O comando de configuração do ambiente de cluster cria os seguintes recursos na sua subscrição:
- Um grupo de recursos (se não for indicado)
- Uma conta de armazenamento
- Um Azure Container Registry (ACR)
- Uma implementação de Kubernetes num cluster do Azure Container Service (ACS)
- Application Insights

O grupo de recursos, a conta de armazenamento e o ACR são criadas rapidamente. A implementação do ACS pode demorar até 20 minutos. 

Após a conclusão da configuração, tem de definir o ambiente para ser utilizado para esta implementação. Utilize o seguinte comando:

```azurecli
az ml env set -n [environment name] -g [resource group]
```

>[!NOTE] 
> Depois do ambiente for criado, para implementações subsequentes, só tem de utilizar o comando set acima para reutilizá-la.
>

>[!NOTE] 
>Para criar um ponto final HTTPS, especifique um certificado SSL ao criar um cluster utilizando as-- opções de nome de certificado e --pem do certificado no programa de configuração do az ml env. Esta ação configura o cluster para atender a solicitações de https, protegida com o certificado fornecido. Após a configuração estiver concluída, crie um registo CNAME DNS que aponta para o FQDN do cluster.

### <a name="create-an-account"></a>Criar uma Conta
Uma conta é necessária para implementação de modelos. Terá de fazer isto vez por conta e pode reutilizar a mesma conta em várias implementações.

Para criar uma nova conta, utilize o seguinte comando:

```azurecli
az ml account modelmanagement create -l [Azure region, e.g. eastus2] -n [your account name] -g [resource group name] --sku-instances [number of instances, e.g. 1] --sku-name [Pricing tier for example S1]
```

Para utilizar uma conta existente, utilize o seguinte comando:
```azurecli
az ml account modelmanagement set -n [your account name] -g [resource group it was created in]
```

### <a name="deploy-your-model"></a>Implementar o seu modelo
Agora está pronto para implementar o seu modelo guardado como um serviço web. 

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```

### <a name="next-steps"></a>Próximos Passos
Experimente um dos muito exemplos na galeria.
