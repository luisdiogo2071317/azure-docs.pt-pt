---
title: Criar uma função no Linux com uma imagem personalizada (pré-visualização) | Microsoft Docs
description: Saiba como criar Funções do Azure em execução numa imagem personalizada do Linux.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: c5de0b1384958bc8553aa3722ad6a5829b69ab12
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38488704"
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Criar uma função no Linux com uma imagem personalizada (pré-visualização)

As Funções do Azure permitem-lhe alojar as suas funções no Linux no seu próprio contentor personalizado. Também pode [alojar num contentor predefinido do Serviço de Aplicações do Azure](functions-create-first-azure-function-azure-cli-linux.md). Esta funcionalidade está atualmente em pré-visualização e precisa [do runtime do Functions 2.0](functions-versions.md), que também está em pré-visualização.

Neste tutorial, irá aprender a implementar uma aplicação de funções como uma imagem personalizada do Docker. Este padrão é útil se precisar de personalizar a imagem de contentor do Serviço de Aplicações incorporada. Pode querer utilizar uma imagem personalizada quando as suas funções precisarem de uma versão de idioma ou dependência específicas, ou de uma configuração não fornecida na imagem incorporada.

Este tutorial explica como utilizar as Funções do Azure para criar e enviar uma imagem personalizada para o Docker Hub. Em seguida, pode utilizar esta imagem como origem de implementação para uma aplicação de funções executada no Linux. Pode utilizar o Docker para criar e enviar a imagem. Pode utilizar a CLI do Azure para criar uma aplicação de funções e implementar a imagem a partir do Docker Hub. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma imagem personalizada com o Docker.
> * Publicar uma imagem personalizada num registo de contentor. 
> * Criar uma conta de Armazenamento do Azure. 
> * Criar um plano do Serviço de Aplicações do Linux. 
> * Implementar uma aplicação de funções a partir do Docker Hub.
> * Adicionar as definições de aplicação à aplicação de funções. 

São suportados os seguintes passos num computador Mac, Windows ou Linux.  

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de:

* [Git](https://git-scm.com/downloads)
* Uma [subscrição do Azure](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) ativa
* [Docker](https://docs.docker.com/install/)
* Uma [conta do Docker Hub](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Transferir o exemplo

Numa janela de terminal, execute o comando seguinte para clonar o repositório de aplicação de exemplo no seu computador local e altere o diretório que contém o código de exemplo.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>Compilar a imagem a partir do Dockerfile

Neste repositório Git, veja _Dockerfile_. Este ficheiro descreve o ambiente que é necessário para executar a aplicação de funções no Linux. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Quando estiver a alojar uma imagem num registo de contentor privado, deve adicionar as definições de ligação à aplicação de funções com as variáveis **ENV** no Dockerfile. Uma vez que este tutorial não pode garantir a utilização de um registo privado, as definições de ligação são [adicionadas após a implementação através da CLI do Azure](#configure-the-function-app) como melhor prática de segurança.   

### <a name="run-the-build-command"></a>Executar o comando Compilar
Para compilar a imagem do Docker, execute o comando `docker build` e indique um nome, `mydockerimage`, e uma etiqueta, `v1.0.0`. Substitua `<docker-id>` pelo ID da sua conta do Docker Hub.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

O comando produz um resultado semelhante ao seguinte:

```bash
Sending build context to Docker daemon  169.5kB
Step 1/3 : FROM microsoft/azure-functions-runtime:v2.0.0-jessie
v2.0.0-jessie: Pulling from microsoft/azure-functions-runtime
b178b12f7913: Pull complete
2d9ce077a781: Pull complete
4775d4ba55c8: Pull complete
Digest: sha256:073f45fc167b3b5c6642ef4b3c99064430d6b17507095...
Status: Downloaded newer image for microsoft/azure-functions-runtime:v2.0.0-jessie
 ---> 217799efa500
Step 2/3 : ENV AzureWebJobsScriptRoot /home/site/wwwroot
 ---> Running in 528fa2077d17
 ---> 7cc6323b8ae0
Removing intermediate container 528fa2077d17
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5bdac9878423
Successfully built 5bdac9878423
Successfully tagged ggailey777/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Testar a imagem localmente
Verifique se a imagem incorporada funciona ao executar a imagem do Docker num contentor local. Emita o comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) e transmita o nome e a etiqueta da imagem ao mesmo. Certifique-se de que utiliza o argumento `-p` para especificar a porta.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Com a imagem personalizada em execução num contentor local do Docker, certifique-se de que a aplicação de funções e o contentor estão a funcionar corretamente ao navegar para <http://localhost:8080>.

![Teste a aplicação de funções localmente.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Depois de ter verificado a aplicação de funções no contentor, pare a execução. Agora, pode enviar a imagem personalizada para a sua conta do Docker Hub.

## <a name="push-the-custom-image-to-docker-hub"></a>Enviar a imagem personalizada para o Docker Hub

Um registo de contentor é uma aplicação que aloja imagens e disponibiliza serviços de imagens e contentores. Para partilhar a sua imagem, tem de enviá-la para um registo. O Docker Hub é um registo de contentores para imagens do Docker que lhe permite alojar os seus próprios repositórios, sejam públicos ou privados. 

Antes de poder enviar uma imagem, tem de iniciar sessão no Docker Hub com o comando [docker login](https://docs.docker.com/engine/reference/commandline/login/). Substitua `<docker-id>` pelo nome da sua conta e introduza a palavra-passe na consola, quando lhe for pedido. Para outras opções de palavra-passe do Docker Hub, veja a [documentação do comando docker login](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id> 
```

A mensagem "início de sessão bem-sucedido" confirma que tem sessão iniciada. Depois de iniciar sessão, envie a imagem para o Docker Hub com o comando [docker push](https://docs.docker.com/engine/reference/commandline/push/).

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Veja o resultado do comando para confirmar se o envio foi bem-sucedido.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Agora, pode utilizar esta imagem como origem de implementação para uma nova aplicação de funções no Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.21 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Criar um plano do Serviço de Aplicações do Linux

O alojamento do Linux para Funções não é atualmente suportado em planos de consumo. Tem de executar um plano do Serviço de Aplicações do Linux. Para saber mais sobre o alojamento, veja [Azure Functions hosting plans comparison (Comparação dos planos de alojamento das Funções do Azure)](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Criar e implementar a imagem personalizada

Uma aplicação de funções aloja a execução das suas funções. Utilize o comando [az functionapp create](/cli/azure/functionapp#az_functionapp_create) para criar uma aplicação de funções a partir de uma imagem do Docker Hub. 

No comando seguinte, substitua o nome da sua aplicação de funções exclusivo onde vir o marcador de posição `<app_name>` e o nome da conta de armazenamento para `<storage_name>`. O `<app_name>` vai ser utilizado como o domínio DNS predefinido para a aplicação Function App, daí que o nome tenha de ser exclusivo em todas as aplicações no Azure. Como anteriormente, `<docker-id>` é o nome da sua conta do Docker.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0 
```
Depois de a aplicação Function App ter sido criada, a CLI do Azure mostra informações semelhantes às do exemplo seguinte:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
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

O parâmetro _deployment-container-image-name_ indica a imagem alojada no Docker Hub a utilizar para criar a aplicação de funções. 


## <a name="configure-the-function-app"></a>Configurar a aplicação de funções

A função precisa da cadeia de ligação para ligar à conta de armazenamento predefinida. Quando estiver publicar a sua imagem personalizada numa conta de contentor privada, deve definir estas definições de aplicação como variáveis de ambiente no Dockerfile com a [instrução ENV](https://docs.docker.com/engine/reference/builder/#env) ou equivalente. 

Neste caso, `<storage_account>` é o nome da conta de armazenamento que criou. Obtenha a cadeia de ligação com o comando [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Adicione estas definições de aplicação na aplicação de funções com o comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_set).

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Agora, pode testar as suas funções em execução no Linux no Azure.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma imagem personalizada com o Docker.
> * Publicar uma imagem personalizada num registo de contentor. 
> * Criar uma conta de Armazenamento do Azure. 
> * Criar um plano do Serviço de Aplicações do Linux. 
> * Implementar uma aplicação de funções a partir do Docker Hub.
> * Adicionar as definições de aplicação à aplicação de funções.

Saiba como ativar a funcionalidade de integração contínua integrada na plataforma principal do Serviço de Aplicações. Pode configurar a sua aplicação de funções para que o contentor seja novamente implementado ao atualizar a imagem no Hub do Docker.

> [!div class="nextstepaction"] 
> [Implementação contínua com a Aplicação Web para Contentores](../app-service/containers/app-service-linux-ci-cd.md)
