---
title: Tutorial – Automatizar as compilações da imagem de contentor ao atualizar a imagem de base com o Azure Container Registry Build
description: Neste tutorial, vai aprender a configurar uma tarefa de compilação para acionar automaticamente as compilações da imagem de contentor na cloud ao atualizar uma imagem de base.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 976f61d99b88d241b39bfec9d95e16de272d9c14
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-automate-image-builds-on-base-image-update-with-azure-container-registry-build"></a>Tutorial: Automatizar as compilações da imagem ao atualizar a imagem de base com o Azure Container Registry Build

O ACR Build suporta a execução de compilações automatizadas quando a imagem de base de um contentor é atualizada. Por exemplo, quando corrige o SO ou a estrutura de aplicações numa das suas imagens de base. Neste tutorial, vai aprender a criar uma tarefa de compilação no ACR Build para acionar uma compilação na cloud quando uma imagem de base do contentor tiver sido enviada para o seu registo.

Neste tutorial, a última parte da série:

> [!div class="checklist"]
> * Compilar a imagem de base
> * Criar uma tarefa de compilação das imagens da aplicação
> * Atualizar a imagem de base para acionar uma compilação das imagens da aplicação
> * Apresentar a compilação acionada
> * Verificar a imagem da aplicação atualizada

> [!IMPORTANT]
> O ACR Build encontra-se em pré-visualização e é apenas suportado pelos registos de contentor do Azure nas regiões **EUA Leste** (eastus) e **Europa Ocidental** (westeurope). As pré-visualizações ser-lhe-ão disponibilizadas na condição de concordar com os [termos suplementares de utilização][terms-of-use]. Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se quiser utilizar a CLI do Azure localmente, terá de ter a versão **2.0.32** ou posterior da CLI do Azure instalada. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar a CLI, veja [Instalar a CLI 2.0 do Azure][azure-cli].

## <a name="prerequisites"></a>Pré-requisitos

### <a name="complete-previous-tutorials"></a>Tutoriais anteriores concluídos

Este tutorial pressupõe que já tenha concluído os passos nos dois primeiros tutoriais da série, nos quais pôde:

* Criar um registo de contentor do Azure
* Bifurcar um repositório de exemplo
* Clonar um repositório de exemplo
* Criar token de acesso pessoal do GitHub

Se ainda não o fez, conclua os dois primeiros tutoriais antes de continuar:

[Compilar imagens de contentor na cloud com o Azure Container Registry Build](container-registry-tutorial-quick-build.md)

[Automatizar as compilações da imagem de contentor com o Azure Container Registry Build](container-registry-tutorial-build-task.md)

### <a name="configure-environment"></a>Configurar o ambiente

Preencha estas variáveis de ambiente da shell com os valores adequados para o seu ambiente. Não é estritamente necessário, mas torna-se um pouco mais fácil para neste tutorial executar os comandos da CLI do Azure com várias linhas. Se não preencher as variáveis, terá de substituir manualmente cada valor sempre que aparecerem nos comandos de exemplo.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Imagens de base

O Dockerfiles, que define a maioria das imagens de contentor, especifica a imagem principal na qual se baseia a imagem, que é referida, muitas vezes, como a sua *imagem de base*. As imagens de base, normalmente, contêm o sistema operativo, por exemplo [Alpine Linux][base-alpine] ou [Windows Nano Server][base-windows], no qual são aplicadas as restantes camadas do contentor. As imagens de base também podem incluir estruturas de aplicações, tais como [Node.js][base-node] ou [.NET Core][base-dotnet].

### <a name="base-image-updates"></a>Atualizações da imagem de base

Frequentemente, uma imagem de base é atualizada pelo responsável pela manutenção da imagem para incluir as novas funcionalidades ou os melhoramentos no SO ou na estrutura. Os patches de segurança são outra causa comum para a atualização da imagem de base.

Quando uma imagem de base é atualizada, será necessário recompilar todas as imagens de contentor no seu registo baseadas nas mesmas para incluir as novas funcionalidades e correções. O ACR Build permite compilar automaticamente as imagens quando é atualizada a imagem de base de um contentor.

### <a name="base-image-update-scenario"></a>Cenário de atualização da imagem de base

Este tutorial orienta-o através de um cenário de atualização da imagem de base. O [exemplo de código][code-sample] inclui dois Dockerfiles: uma imagem da aplicação e uma imagem que é especificada como a sua base. Nas próximas secções, vai criar uma tarefa do ACR Build para acionar automaticamente uma compilação da imagem da aplicação quando uma nova versão da imagem de base é enviada para o seu registo de contentor.

[Dockerfile-app][dockerfile-app]: uma pequena aplicação Web Node.js que compõe uma página Web estática, que apresenta a versão do Node.js na qual se baseia. A cadeia de versão é simulada, na qual apresenta o conteúdo de uma variável de ambiente, `NODE_VERSION`, definido na imagem de base.

[Dockerfile-base][dockerfile-base]: a imagem que `Dockerfile-app` especifica como a sua base. A própria baseia-se numa imagem do [Nó][base-node] e inclui a variável de ambiente `NODE_VERSION`.

Nas próximas secções, vai criar uma tarefa de compilação, atualizar o valor `NODE_VERSION` no Dockerfile da imagem de base e, em seguida, utilizar o ACR Build para compilar a imagem de base. Quando o ACR Build envia a nova imagem de base para o registo, aciona automaticamente uma compilação da imagem da aplicação. Opcionalmente, pode executar a imagem de contentor da aplicação localmente para ver as diferentes cadeias de versão nas imagens da compilação.

## <a name="build-base-image"></a>Compilar a imagem de base

Comece por compilar a imagem de base com uma *Compilação Rápida* do ACR Build. Tal como explicado no [primeiro tutorial](container-registry-tutorial-quick-build.md) da série, esta operação não só compila a imagem como a envia para o seu registo de contentor, caso a compilação seja concluída com êxito.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-build-task"></a>Criar a tarefa de compilação

Em seguida, crie uma tarefa de compilação com [az acr build-task create][az-acr-build-task-create]:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --build-arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --file Dockerfile-app \
    --branch master \
    --git-access-token $GIT_PAT
```

Esta tarefa de compilação é semelhante à tarefa criada no [tutorial anterior](container-registry-tutorial-build-task.md). Dá instruções ao ACR Build para acionar uma compilação da imagem quando as consolidações são enviadas por push para o repositório especificado por `--context`.

A diferença está no seu comportamento, pois também aciona uma compilação da imagem quando a *imagem de base* é atualizada. O Dockerfile especificado pelo argumento `--file`, [Dockerfile-app][dockerfile-app], suporta a especificação de uma imagem a partir do mesmo registo que a sua base:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Quando executa uma tarefa de compilação, o ACR Build deteta as dependências de uma imagem. Se a imagem de base especificada na instrução `FROM` residir no mesmo registo, adicionará um hook para garantir que esta imagem é recompilada sempre que a imagem de base é atualizada.

> [!NOTE]
> Durante a pré-visualização, o ACR Build permite acionar a compilação de uma imagem derivada apenas quando ambas, a imagem de base e a imagem que a referencia, residirem no mesmo registo de contentor do Azure.

## <a name="build-application-container"></a>Compilar o contentor de aplicação

Para ativar o ACR Build para determinar e controlar dependências de uma imagem de contentor (que incluem a imagem de base), **terá** primeiro de acionar a tarefa de compilação **, pelo menos, uma vez**.

Utilize [az acr build-task run][az-acr-build-task-run] para acionar manualmente a tarefa de compilação e criar a imagem da aplicação:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Depois de concluída a compilação, tome nota do **ID de Compilação** (por exemplo, “eastus6”) se quiser concluir o passo opcional seguinte.

### <a name="optional-run-application-container-locally"></a>Opcional: executar o contentor da aplicação localmente

Se estiver a trabalhar localmente (não estiver no Cloud Shell) e tiver o Docker instalado, execute o contentor para ver a aplicação composta num browser, antes de recompilar a imagem de base. Se estiver a utilizar o Cloud Shell, ignore esta secção (o Cloud Shell não suporta `az acr login` nem `docker run`).

Em primeiro lugar, inicie sessão no registo de contentor com [az acr login][az-acr-login]:

```azurecli
az acr login --name $ACR_NAME
```

Agora, execute o contentor localmente com `docker run`. Substitua **\<build-id\>** pelo ID de Compilação encontrado no resultado do passo anterior (por exemplo, “eastus5”).

```azurecli
docker run -d -p 8080:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navegue para http://localhost:8080 no browser, deverá ver o número de versão do Node.js composto na página Web, semelhante ao que se segue. Num passo posterior, pode efetuar o bump da versão ao adicionar um “a” na cadeia de versão.

![Captura de ecrã da aplicação de exemplo composta no browser][base-update-01]

## <a name="list-builds"></a>Listar as compilações

Em seguida, liste as compilações que o ACR Build concluiu para o registo:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

Se tiver concluído o tutorial anterior (e não tiver eliminado o registo), deverá ver um resultado semelhante ao seguinte. Tome nota do número das compilações e do ID DE COMPILAÇÃO mais recente para poder comparar o resultado depois de atualizar a imagem de base na próxima secção.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER       STARTED               DURATION
----------  ---------------  ----------  ---------  ------------  --------------------  ----------
eastus6     buildhelloworld  Linux       Succeeded  Manual        2018-04-22T00:03:46Z  00:00:40
eastus5                                  Succeeded  Manual        2018-04-22T00:01:45Z  00:00:25
eastus4     buildhelloworld  Linux       Succeeded  Git Commit    2018-04-21T23:52:33Z  00:00:30
eastus3     buildhelloworld  Linux       Succeeded  Manual        2018-04-21T23:50:10Z  00:00:35
eastus2     buildhelloworld  Linux       Succeeded  Manual        2018-04-21T23:46:15Z  00:00:55
eastus1                                  Succeeded  Manual        2018-04-21T23:24:05Z  00:00:35
```

## <a name="update-base-image"></a>Atualizar a imagem de base

Nesta secção, vai simular uma correção da estrutura na imagem de base. Edite **Dockerfile-base** e adicione um “a” depois do número de versão definido em `NODE_VERSION`:

```Dockerfile
ENV NODE_VERSION 9.11.1a
```

Execute uma Compilação Rápida no ACR Build para compilar a imagem de base modificada. Tome nota do **ID de Compilação** no resultado.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Depois de concluída a compilação e a nova imagem de base ter sido enviada para o registo, o ACR Build aciona uma compilação da imagem da aplicação. Pode demorar algum tempo para que a tarefa de compilação do ACR Build criada anteriormente acione a compilação da imagem da aplicação, porque tem de detetar a imagem de base recentemente concluída e enviada.

## <a name="list-builds"></a>Listar as compilações

Agora que atualizou a imagem de base, liste as suas compilações novamente para compará-las com a lista anterior. Se, à primeira, o resultado não for diferente, execute periodicamente o comando para ver a nova compilação aparecer na lista.

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

O resultado é semelhante ao seguinte. O ACIONADOR da última compilação executada deve ser “Atualizar Imagem”, que indica que a tarefa de compilação foi iniciada pela Compilação Rápida da imagem de base.

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER       STARTED               DURATION
----------  ---------------  ----------  ---------  ----------    --------------------  ----------
eastus8     buildhelloworld  Linux       Succeeded  Image Update  2018-04-22T00:09:24Z  00:00:50
eastus7                                  Succeeded  Manual        2018-04-22T00:08:49Z  00:00:40
eastus6     buildhelloworld  Linux       Succeeded  Image Update  2018-04-20T00:15:30Z  00:00:43
eastus5     buildhelloworld  Linux       Succeeded  Manual        2018-04-20T00:10:05Z  00:00:45
eastus4     buildhelloworld  Linux       Succeeded  Git Commit    2018-04-19T23:40:38Z  00:00:40
eastus3     buildhelloworld  Linux       Succeeded  Manual        2018-04-19T23:36:37Z  00:00:40
eastus2     buildhelloworld  Linux       Succeeded  Manual        2018-04-19T23:35:27Z  00:00:40
eastus1                                  Succeeded  Manual        2018-04-19T22:51:13Z  00:00:30
```

Se quiser executar o passo seguinte opcional de execução do contentor recentemente compilado para ver o número da versão atualizada, tome nota do valor do **ID DA COMPILAÇÃO** da compilação acionada pela Atualização da Imagem (no resultado anterior é “eastus6”).

### <a name="optional-run-newly-built-image"></a>Opcional: executar a imagem recém-compilada

Se estiver a trabalhar localmente (não estiver no Cloud Shell) e tiver o Docker instalado, execute a nova imagem da aplicação depois de concluída a sua compilação. Substitua `<build-id>` pelo ID DE COMPLIAÇÃO que obteve no passo anterior. Se estiver a utilizar o Cloud Shell, ignore esta secção (o Cloud Shell não suporta `docker run`).

```bash
docker run -d -p 8081:80 $ACR_NAME.azurecr.io/helloworld:<build-id>
```

Navegue para http://localhost:8081 no browser, deverá ver o número de versão do Node.js atualizado (com o “a”) na página Web:

![Captura de ecrã da aplicação de exemplo composta no browser][base-update-02]

O que é importante ter em atenção é que atualizou a imagem de **base** com um novo número de versão, mas a imagem da **aplicação** da última compilação apresenta a nova versão. O ACR Build captou a alteração para a imagem de base e recompilou automaticamente a imagem da aplicação.

## <a name="clean-up-resources"></a>Limpar recursos

Para remover todos os recursos que criou nesta série de tutoriais, incluindo o registo de contentor, a instância de contentor, o cofre de chaves e o principal de serviço, emita os comandos seguintes:

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a utilizar uma tarefa de compilação para acionar automaticamente compilações da imagem de contentor quando a imagem de base é atualizada. Agora, pode avançar para saber mais sobre a autenticação do registo do contentor.

> [!div class="nextstepaction"]
> [Autenticação no Azure Container Registry](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png