---
title: Criar uma aplicação de funções no Linux num plano do serviço de aplicações do Azure
description: Saiba como criar uma aplicação de função que é executada no Linux num plano do serviço de aplicações com a CLI do Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: d9800ff3fc82636c5cae12167738667ec84326ee
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856621"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan-preview"></a>Criar uma aplicação de funções no Linux num plano do serviço de aplicações do Azure (pré-visualização)

As Funções do Azure permitem-lhe alojar as suas funções no Linux num contentor predefinido do Serviço de Aplicações do Azure. Este artigo explica-lhe como utilizar a CLI do Azure para criar uma aplicação de funções de Linux alojados no Azure que é executado num [plano do App Service](functions-scale.md#app-service-plan). Também pode [trazer o seu próprio contentor personalizado](functions-create-function-linux-custom-image.md). Alojamento do Linux está atualmente em pré-visualização.

Num plano do serviço de aplicações, é responsável por dimensionando a sua aplicação de função. Para tirar partido das capacidades das funções do Azure sem servidor, também pode alojar as suas funções no Linux numa [plano de consumo](functions-scale.md#consumption-plan).

Pode seguir os passos abaixo num computador Mac, Windows ou Linux.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

+ Uma subscrição ativa do Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.21 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Criar um plano do Serviço de Aplicações do Linux

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Criar uma aplicação de funções no Linux

Precisa de uma aplicação de funções para alojar a execução das suas funções no Linux. A aplicação de funções proporciona um ambiente para a execução do código da sua função. Permite-lhe agrupar funções como unidades lógicas para uma gestão, implementação e partilha de recursos mais fácil. Utilize o comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create) para criar uma aplicação de funções com um plano do Serviço de Aplicações do Linux.

No comando seguinte, substitua o nome da sua aplicação de funções exclusivo onde vir o marcador de posição `<app_name>` e o nome da conta de armazenamento para `<storage_name>`. O `<app_name>` vai ser utilizado como o domínio DNS predefinido para a aplicação Function App, daí que o nome tenha de ser exclusivo em todas as aplicações no Azure. Também deve configurar o `<language>` tempo de execução para a sua aplicação de função, da `dotnet` (C#), `node` (JavaScript), ou `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --plan myAppServicePlan \
--name <app_name> --storage-account  <storage_name> --runtime <language>
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

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources-simple.md)]

## <a name="next-steps"></a>Próximos Passos

Este artigo mostra-lhe como criar uma aplicação de funções de Linux alojado no Azure. Agora, pode [implementar um projeto de função](https://docs.microsoft.com/cli/azure/functionapp/deployment/source?view=azure-cli-latest) para esta aplicação de função. Pode utilizar as ferramentas de núcleo de funções do Azure para [criar um projeto de funções](functions-run-local.md#create-a-local-functions-project) no seu computador local e implemente-a sua nova aplicação de funções do Linux.  

> [!div class="nextstepaction"] 
> [Criar código e testar as Funções do Azure localmente](functions-run-local.md)
