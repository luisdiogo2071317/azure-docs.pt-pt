---
title: Tutorial do Azure Container Registry - Preparar um registo de contentor do Azure georreplicado
description: Criar um registo de contentor do Azure, configurar a georreplicação, preparar uma imagem do Docker e implementá-la para o registo. Parte um de uma série com três partes.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/26/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 3463acc3db3dae9633635aaf7410d876aacf9b38
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-prepare-a-geo-replicated-azure-container-registry"></a>Tutorial: Preparar um registo de contentor georreplicado do Azure

Um registo de contentor do Azure é um registo privado do Docker implementado no Azure que pode manter próximo em rede das suas implementações. Neste conjunto de três tutoriais, vai aprender a utilizar a georreplicação para implementar uma aplicação web em ASP.NET Core a executar num contentor do Linux para duas instâncias das [Aplicações Web para Contentores](../app-service/containers/index.yml). Vai ver como o Azure implementa automaticamente a imagem para cada instância de imagem Web a partir do repositório georreplicado mais próximo.

Neste tutorial, a primeira parte de uma série de três:

> [!div class="checklist"]
> * Criar um registo de contentor georreplicado do Azure
> * Clonar o código-fonte da aplicação do GitHub
> * Criar uma imagem de contentor do Docker a partir da origem da aplicação
> * Envie a imagem do contentor para o seu registo

Nos tutoriais seguintes, vai implementar o contentor do seu registo privado para uma aplicação web a executar em duas regiões do Azure. Em seguida, atualiza o código na aplicação e ambas as instâncias da Aplicação Web com um único `docker push` no seu registo.

## <a name="before-you-begin"></a>Antes de começar

Este tutorial requer a execução da versão 2.0.20 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

Este tutorial pressupõe conhecimentos básicos dos principais conceitos do Docker, como contentores, imagens de contentores e comandos simples do Docker. Se for necessário, veja [Get started with Docker]( https://docs.docker.com/get-started/) (Introdução ao Docker) para obter um manual sobre as noções básicas dos contentores.

Para concluir este tutorial, precisa de um ambiente de desenvolvimento do Docker. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ou [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

O Azure Cloud Shell não inclui os componentes do Docker necessários para concluir todos os passos deste tutorial. Portanto, recomendamos uma instalação local da CLI do Azure e do ambiente de desenvolvimento do Docker.

> [!IMPORTANT]
> A funcionalidade de georreplicação do Azure Container Registry está atualmente em **pré-visualização**. As pré-visualizações são tornadas disponíveis para si na condição de concordar com os [termos suplementares de utilização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).
>

## <a name="create-a-container-registry"></a>Criar um registo de contentores

Inicie sessão no [portal do Azure](http://portal.azure.com).

Selecione **Criar um recurso** > **Contentores** > **Azure Container Registry**.

![Criar um registo de contentor no portal do Azure][tut-portal-01]

Configure o seu novo registo com as seguintes definições:

* **Nome do registo**: crie um nome de registo que seja globalmente exclusivo no Azure e contenha apenas 5-50 carateres alfanuméricos
* **Grupo de Recursos**: **Criar novo** > `myResourceGroup`
* **Localização**: `West US`
* **Utilizador administrador**: `Enable` (necessário para a Aplicação Web para Contentores, para solicitar imagens)
* **SKU**: `Premium` (necessário para a georreplicação)

Selecione **Criar** para implementar a instância ACR.

![Criar um registo de contentor no portal do Azure][tut-portal-02]

Em todo o resto deste tutorial, utilizamos `<acrName>` como um marcador de posição para o **Nome do registo** de contentor que escolheu.

> [!TIP]
> Por os registos de contentor do Azure serem normalmente recursos de longo prazo utilizados em vários anfitriões de contentores, recomendamos que crie o seu registo no seu próprio grupo de recursos. À medida que configura registos e webhooks georreplicados, esses recursos adicionais são colocados no mesmo grupo de recursos.
>

## <a name="configure-geo-replication"></a>Configurar georreplicação

Agora que tem um registo Premium, pode configurar a georreplicação. A sua aplicação web, que vai configurar no próximo tutorial para que execute em duas regiões, pode então puxar as suas imagens de contentor a partir do registo mais próximo.

Navegue para o seu novo registo de contentor no portal do Azure e selecione **Replicações** em **SERVIÇOS**:

![Replicações na IU de registo de contentor do portal do Azure][tut-portal-03]

É mostrado um mapa que mostra hexágonos verdes que representam as regiões do Azure disponíveis para georreplicação:

 ![Mapa de região no portal do Azure][tut-map-01]

Replique o seu registo para a região E.U.A. Leste ao selecionar o respetivo hexágono verde, selecione **Criar** em **Criar replicação**:

 ![Criar a IU de replicação no portal do Azure][tut-portal-04]

Quando a replicação estiver completa, o portal reflete *Pronto* para ambas as regiões. Utilize o botão **Atualizar** para atualizar o estado da replicação; pode demorar cerca de um minuto para que as réplicas sejam criadas e sincronizadas.

![IU de estado de replicação no portal do Azure][tut-portal-05]

## <a name="container-registry-login"></a>Iniciar sessão no registo de contentor

Agora que configurou a georreplicação, crie uma imagem de contentor e envie-a para o seu registo. Deve primeiro iniciar a sessão na sua instância do ACR antes de enviar imagens para a mesma. Com as [SKUs Básica, Standard e Premium](container-registry-skus.md), pode autenticar ao utilizar a sua identidade do Azure.

Utilize o comando [az acr login](https://docs.microsoft.com/cli/azure/acr#az_acr_login) para autenticar e colocar as credenciais em cache para o seu registo. Substitua `<acrName>` com o nome do registo que criou nos passos anteriores.

```azurecli
az acr login --name <acrName>
```

O comando devolver `Login Succeeded` quando concluído.

## <a name="get-application-code"></a>Obter o código da aplicação

O exemplo neste tutorial inclui uma pequena aplicação web criada com [ASP.NET Core](http://dot.net). A aplicação serve uma página HTML que mostra a região da qual a imagem foi implementada pelo Azure Container Registry.

![Tutorial de aplicação mostrada no browser][tut-app-01]

Utilize o git para transferir o exemplo para um diretório local e `cd` para o diretório:

```bash
git clone https://github.com/Azure-Samples/acr-helloworld.git
cd acr-helloworld
```

## <a name="update-dockerfile"></a>Atualizar o Dockerfile

O Dockerfile incluído nos exemplos mostra como é que o contentor é criado. Inicia a partir de uma imagem oficial [aspnetcore](https://store.docker.com/community/images/microsoft/aspnetcore), copia os ficheiros de aplicação para o contentor, instala dependências, compila a saída com a imagem oficial [aspnetcore-build](https://store.docker.com/community/images/microsoft/aspnetcore-build) e, por fim, cria uma imagem aspnetcore otimizada.

O Dockerfile está localizado em `./AcrHelloworld/Dockerfile` na origem clonada.

```dockerfile
FROM microsoft/aspnetcore:2.0 AS base
# Update <acrName> with the name of your registry
# Example: uniqueregistryname.azurecr.io
ENV DOCKER_REGISTRY <acrName>.azurecr.io
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY AcrHelloworld/AcrHelloworld.csproj AcrHelloworld/
RUN dotnet restore
COPY . .
WORKDIR /src/AcrHelloworld
RUN dotnet build -c Release -o /app

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS production
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "AcrHelloworld.dll"]
```

A aplicação na imagem *acr-helloworld* tenta determinar a região da qual o seu contentor foi implementado ao fazer consultas DNS para obter informações sobre o servidor de início de sessão do registo. Tem de especificar o URL do servidor de sessão do seu registo na variável de ambiente `DOCKER_REGISTRY` no Dockerfile.

Primeiro, obtenha o URL do servidor de sessão do registo com o comando `az acr show`. Substitua `<acrName>` com o nome do registo que criou nos passos anteriores.

```azurecli
az acr show --name <acrName> --query "{acrLoginServer:loginServer}" --output table
```

Saída:

```bash
AcrLoginServer
-----------------------------
uniqueregistryname.azurecr.io
```

Em seguida, atualize a linha `DOCKER_REGISTRY` com o seu URL do servidor de sessão do registo. Neste exemplo, vamos atualizar a linha para refletir o nosso nome de registo de exemplo, *uniqueregistryname*:

```dockerfile
ENV DOCKER_REGISTRY uniqueregistryname.azurecr.io
```

## <a name="build-container-image"></a>Criar imagem de contentor

Agora que atualizou o Dockerfile com o URL do seu registo, pode utilizar `docker build` para criar a imagem do contentor. Execute o seguinte comando para criar a imagem e marcá-la com o URL do seu registo privado, substituindo novamente `<acrName>` com o nome do seu registo:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

Várias linhas de saída são apresentadas conforme a imagem do Docker é criada (mostrada aqui truncada):

```bash
Sending build context to Docker daemon  523.8kB
Step 1/18 : FROM microsoft/aspnetcore:2.0 AS base
2.0: Pulling from microsoft/aspnetcore
3e17c6eae66c: Pulling fs layer
...
Step 18/18 : ENTRYPOINT dotnet AcrHelloworld.dll
 ---> Running in 6906d98c47a1
 ---> c9ca1763cfb1
Removing intermediate container 6906d98c47a1
Successfully built c9ca1763cfb1
Successfully tagged uniqueregistryname.azurecr.io/acr-helloworld:v1
```

Utilize o comando `docker images` para ver a imagem incorporada:

```bash
docker images
```

Saída:

```bash
REPOSITORY                                      TAG    IMAGE ID        CREATED               SIZE
uniqueregistryname.azurecr.io/acr-helloworld    v1     01ac48d5c8cf    About a minute ago    284MB
...
```

## <a name="push-image-to-azure-container-registry"></a>Enviar imagens para o Azure Container Registry

Por fim, utilize o comando `docker push` para enviar a imagem *acr-helloworld* para o seu registo. Substitua `<acrName>` com o nome do seu registo.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Por ter configurado o seu registo para georreplicação, a sua imagem é automaticamente replicada para ambas as regiões *E.U.A. Oeste* e *E.U.A. Leste* com apenas este comando `docker push`.

Saída:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
cd54739c444b: Pushed
d6803756744a: Pushed
b7b1f3a15779: Pushed
a89567dff12d: Pushed
59c7b561ff56: Pushed
9a2f9413d9e4: Pushed
a75caa09eb1f: Pushed
v1: digest: sha256:0799014f91384bda5b87591170b1242bcd719f07a03d1f9a1ddbae72b3543970 size: 1792
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um registo de contentor georreplicado privado, criou uma imagem de contentor e então enviou essa imagem para o seu registo. Ao seguir os passos neste tutorial, conseguiu:

> [!div class="checklist"]
> * Criar um registo de contentor georreplicado do Azure
> * Clonar o código-fonte da aplicação a partir do GitHub
> * Criar uma imagem de contentor do Docker a partir da origem da aplicação
> * Enviar a imagem do contentor para o seu registo

Avance para o próximo tutorial para aprender a implementar o seu contentor para várias Aplicações Web para Contentores, ao utilizar a georreplicação para servir as imagens localmente.

> [!div class="nextstepaction"]
> [Implementar aplicação web do Azure Container Registry](container-registry-tutorial-deploy-app.md)

<!-- IMAGES -->
[tut-portal-01]: ./media/container-registry-tutorial-prepare-registry/tut-portal-01.png
[tut-portal-02]: ./media/container-registry-tutorial-prepare-registry/tut-portal-02.png
[tut-portal-03]: ./media/container-registry-tutorial-prepare-registry/tut-portal-03.png
[tut-portal-04]: ./media/container-registry-tutorial-prepare-registry/tut-portal-04.png
[tut-portal-05]: ./media/container-registry-tutorial-prepare-registry/tut-portal-05.png
[tut-app-01]: ./media/container-registry-tutorial-prepare-registry/tut-app-01.png
[tut-map-01]: ./media/container-registry-tutorial-prepare-registry/tut-map-01.png
