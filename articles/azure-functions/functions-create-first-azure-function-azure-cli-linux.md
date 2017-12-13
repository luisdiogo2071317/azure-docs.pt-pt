---
title: "Criar a sua primeira função no Linux a partir da CLI do Azure (pré-visualização) | Microsoft Docs"
description: "Saiba como criar a sua primeira Função do Azure em execução numa imagem do Linux predefinida com a CLI do Azure."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: d04e2000f2043e8bb11e15f6b9d7fd06ef5b9da3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Criar a sua primeira função em execução no Linux com a CLI do Azure (pré-visualização)

As Funções do Azure permitem-lhe alojar as suas funções no Linux num contentor predefinido do Serviço de Aplicações do Azure. Atualmente, esta funcionalidade está em pré-visualização. Também pode [trazer o seu próprio contentor personalizado](functions-create-function-linux-custom-image.md). 

Este tópico do guia de introdução explica como utilizar as Funções do Azure com a CLI do Azure para criar a sua primeira aplicação de funções no Linux alojada no contentor do Serviço de Aplicações predefinido. O código da função propriamente dito é implementado na imagem a partir de um repositório de exemplos do GitHub.    

São suportados os seguintes passos num computador Mac, Windows ou Linux. 

## <a name="prerequisites"></a>Pré-requisitos 

Para concluir este guia de início rápido, necessita de:

+ Uma subscrição ativa do Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.21 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Criar um plano do Serviço de Aplicações do Linux

O alojamento do Linux para Funções é atualmente suportado apenas num plano do Serviço de Aplicações. O alojamento do plano de consumo ainda não é suportado. Para saber mais sobre o alojamento, veja [Azure Functions hosting plans comparison (Comparação dos planos de alojamento das Funções do Azure)](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Criar uma aplicação de funções no Linux

Precisa de uma aplicação de funções para alojar a execução das suas funções no Linux. A aplicação de funções proporciona um ambiente para a execução do código da sua função. Permite-lhe agrupar funções como unidades lógicas para uma gestão, implementação e partilha de recursos mais fácil. Utilize o comando [az functionapp create](/cli/azure/functionapp#create) para criar uma aplicação de funções com um plano do Serviço de Aplicações do Linux. 

No comando seguinte, substitua o nome da sua aplicação de funções exclusivo onde vir o marcador de posição `<app_name>` e o nome da conta de armazenamento para `<storage_name>`. O `<app_name>` vai ser utilizado como o domínio DNS predefinido para a aplicação Function App, daí que o nome tenha de ser exclusivo em todas as aplicações no Azure. O parâmetro _deployment-source-url_ é um repositório de exemplo no GitHub que contém uma função acionada por HTTP "Hello World".

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
Depois de a aplicação de funções ter sido criada e implementada, a CLI do Azure mostra informações semelhantes às do exemplo seguinte:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Uma vez que `myAppServicePlan` é um plano do Linux, a imagem do docker incorporada é utilizada para criar o contentor que executa a aplicação de funções no Linux. 

>[!NOTE]  
>O repositório de exemplo inclui atualmente dois ficheiros de scripts, [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) e [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). O ficheiro .deployment indica ao processo de implementação para utilizar o deploy.sh como o [script de implementação personalizado](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script). Na versão de pré-visualização atual, os scripts são precisos para implementar a aplicação de funções numa imagem do Linux.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
