---
title: Criar a sua primeira função no Linux no Azure
description: Saiba como criar a sua primeira função alojada no Linux no Azure com a CLI do Azure e as Azure Functions Core Tools.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: multiple
manager: jeconnoc
ms.openlocfilehash: 49845cb3b07076c566ea046fca49a72812857fbf
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619984"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Criar a sua primeira função alojada no Linux com as Ferramentas Base e a CLI do Azure (pré-visualização)

As funções do Azure permite-lhe executar o seu código num ambiente Linux sem ter de criar primeiro uma VM ou publicar uma aplicação web. Alojamento do Linux está atualmente em pré-visualização, requer [o runtime do Functions 2.0](functions-versions.md)e é executado apenas uma [plano do App Service](functions-scale.md#app-service-plan).

Este artigo de início rápido explica como utilizar a CLI do Azure para criar a sua primeira aplicação de funções em execução no Linux. O código de função é criado localmente e, em seguida, implementado no Azure através das [Azure Functions Core Tools](functions-run-local.md).

São suportados os seguintes passos num computador Mac, Windows ou Linux. Este artigo mostra-lhe como criar funções em qualquer um dos JavaScript ou C#.

## <a name="prerequisites"></a>Pré-requisitos

Antes de executar este exemplo, tem de ter o seguinte:

+ Instale a [versão 2.x das Ferramentas Base do Azure](functions-run-local.md#v2).

+ Instale a [CLI do Azure]( /cli/azure/install-azure-cli). Este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Pode também utilizar o [Azure Cloud Shell](https://shell.azure.com/bash).

+ Uma subscrição ativa do Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Criar o projeto de aplicação de funções local

Execute o seguinte comando na linha de comandos para criar um projeto de aplicação de funções na pasta `MyFunctionProj` do diretório local atual. Também é criado um repositório do GitHub em `MyFunctionProj`.

```bash
func init MyFunctionProj
```

Quando lhe for pedido, utilize as teclas de seta para selecionar um runtime de trabalho a partir das seguintes opções de linguagem:

+ `dotnet`: cria um projeto de biblioteca de classes .NET (.csproj).
+ `node`: cria um projeto do JavaScript.

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Criar um plano do Serviço de Aplicações do Linux

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Criar uma aplicação de funções do Linux no Azure

Precisa de uma aplicação de funções para alojar a execução das suas funções no Linux. A aplicação de funções proporciona um ambiente sem servidor para a execução do código da sua função. Permite-lhe agrupar funções como unidades lógicas para uma gestão, implementação e partilha de recursos mais fácil. Utilize o comando [az functionapp create](/cli/azure/functionapp#az_functionapp_create) para criar uma aplicação de funções no Linux.

No comando seguinte, utilize o nome da sua aplicação de funções exclusivo onde vir o marcador de posição `<app_name>` e o nome da conta de armazenamento para `<storage_name>`. O `<app_name>` também é o domínio DNS predefinido para a aplicação de funções. Este nome tem de ser exclusivo em todas as aplicações no Azure. Também deve configurar o `<language>` tempo de execução para a sua aplicação de função, da `dotnet` (C#) ou `node` (JavaScript).

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --plan myAppServicePlan \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

> [!NOTE]
> Se tiver um grupo de recursos existente denominado `myResourceGroup` com todas as aplicações que não são do Serviço de Aplicações, tem de utilizar um grupo de recursos diferente. Não pode alojar aplicações Windows e Linux no mesmo grupo de recursos.  

Depois de a aplicação de funções estar criada, verá a seguinte mensagem:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Agora, pode publicar o seu projeto para a nova aplicação de funções no Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Passos Seguintes

Este artigo mostrou como alojar a sua aplicação de funções num contentor do Serviço de Aplicações do Azure predefinido. Também pode alojar as suas funções no Linux no seu próprio contentor personalizado.

> [!div class="nextstepaction"] 
> [Criar uma função no Linux com uma imagem personalizada](functions-create-function-linux-custom-image.md)
