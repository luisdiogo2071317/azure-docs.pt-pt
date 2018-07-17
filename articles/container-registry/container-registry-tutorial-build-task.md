---
title: Tutorial – Automatizar compilações da imagem de contentor com o Azure Container Registry Build
description: Neste tutorial, vai aprender a configurar uma tarefa de compilação para acionar automaticamente compilações da imagem de contentor na cloud ao consolidar o código de origem para um repositório de Git.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 71ea0f489df6969f0916ac14d187e10a90a520cd
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38722716"
---
# <a name="tutorial-automate-container-image-builds-with-azure-container-registry-build"></a>Tutorial: Automatizar as compilações da imagem de contentor com o Azure Container Registry Build

Para além da [Compilação Rápida](container-registry-tutorial-quick-build.md), o ACR Build suporta as compilações automatizadas da imagem de contentor do Docker com a *tarefa de compilação*. Neste tutorial, vai utilizar a CLI do Azure para criar uma tarefa de compilação para acionar automaticamente compilações da imagem na cloud ao consolidar o código de origem para um repositório de Git.

Neste tutorial, a segunda parte da série:

> [!div class="checklist"]
> * Criar uma tarefa de compilação
> * Testar a tarefa de compilação
> * Ver o estado da compilação
> * Acionar a tarefa de compilação com uma consolidação do código

Este tutorial parte do princípio de que já concluiu os passos no [tutorial anterior](container-registry-tutorial-quick-build.md). Se ainda não o fez, conclua os passos na secção [Pré-requisitos](container-registry-tutorial-quick-build.md#prerequisites) do tutorial anterior antes de continuar.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se quiser utilizar a CLI do Azure localmente, terá de ter a versão **2.0.32** ou posterior da CLI do Azure instalada. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar a CLI, veja [Instalar a CLI do Azure][azure-cli].

## <a name="prerequisites"></a>Pré-requisitos

### <a name="get-sample-code"></a>Obter o código de exemplo

Este tutorial parte do princípio de que já concluiu os passos no [tutorial anterior](container-registry-tutorial-quick-build.md) e já bifurcou e clonou o repositório de exemplo. Se ainda não o fez, conclua os passos na secção [Pré-requisitos](container-registry-tutorial-quick-build.md#prerequisites) do tutorial anterior antes de continuar.

### <a name="container-registry"></a>Registo de contentor

Para concluir este tutorial tem de ter um registo de contentor do Azure na sua subscrição do Azure. Se precisar de um registo, veja o [tutorial anterior](container-registry-tutorial-quick-build.md) ou [Início Rápido: Criar um registo de contentor com a CLI do Azure](container-registry-get-started-azure-cli.md).

## <a name="build-task"></a>Tarefa de compilação

Uma tarefa de compilação define as propriedades de uma compilação automatizada, incluindo a localização do código de origem da imagem de contentor e o evento que aciona a compilação. Quando ocorre um evento definido na tarefa de compilação, como, por exemplo, uma consolidação para um repositório de Git, o ACR Build inicia uma compilação da imagem de contentor na cloud. Por predefinição, envia uma imagem compilada com êxito para o registo de contentor do Azure especificado na tarefa.

O ACR Build suporta atualmente os acionadores de tarefas de compilação seguintes:

* Consolidação para um repositório de Git
* Atualização da imagem de base

## <a name="create-a-build-task"></a>Criar uma tarefa de compilação

Nesta secção, vai criar primeiro um token de acesso pessoal (PAT) do GitHub para utilizar com o ACR Build. Em seguida, vai criar uma tarefa de compilação que aciona uma compilação quando o código é consolidado para a bifurcação do repositório.

### <a name="create-a-github-personal-access-token"></a>Criar um token de acesso pessoal do GitHub

Para acionar uma compilação numa consolidação para um repositório de Git, o ACR Build necessita de um token de acesso pessoal (PAT) para aceder ao repositório. Siga estes passos para gerar um PAT no GitHub:

1. Navegue para a página de criação do PAT no GitHub em https://github.com/settings/tokens/new
1. Escreva uma breve **descrição** para o token, por exemplo, “Demonstração das Tarefas do ACR Build”
1. Em **repositório**, ative **repo:status** e **public_repo**

   ![Captura de ecrã da página de geração do Token de Acesso Pessoal no GitHub][build-task-01-new-token]

1. Selecione o botão **Gerar token** (poderá ser-lhe pedido para confirmar a palavra-passe)
1. Copie e guarde o token gerado numa  **localização segura** (utilizará este token ao definir uma tarefa de compilação na secção seguinte)

   ![Captura de ecrã do Token de Acesso Pessoal gerado no GitHub][build-task-02-generated-token]

### <a name="create-the-build-task"></a>Criar a tarefa de compilação

Agora que concluiu os passos necessários para ativar o ACR Build para ler o estado de consolidação e criar webhooks num repositório, pode criar uma tarefa de compilação para aciona uma compilação da imagem de contentor nas consolidações para o repositório.

Em primeiro lugar, preencha estas variáveis de ambiente da shell com os valores adequados para o seu ambiente. Não é estritamente necessário, mas torna-se um pouco mais fácil para neste tutorial executar os comandos da CLI do Azure com várias linhas. Se não preencher as variáveis, terá de substituir manualmente cada valor sempre que aparecerem nos comandos de exemplo.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the previous section
```

Agora, crie a tarefa de compilação ao executar o comando seguinte [az acr build-task create][ az-acr-build-task-create]:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --branch master \
    --git-access-token $GIT_PAT
```

Esta tarefa de compilação especifica que sempre que um código é consolidado no ramo *mestre* do repositório especificado por `--context`, o ACR Build compilará a imagem de contentor do código nesse ramo. O argumento `--image` especifica um valor com parâmetros de `{{.Build.ID}}` para a parte da versão da etiqueta da imagem, o que garante que a imagem compilada está correlacionada com uma compilação específica e é etiquetada de forma exclusiva.

O resultado de um comando [az acr build-task create][az-acr-build-task-create] bem sucedido é semelhante ao seguinte:

```console
$ az acr build-task create \
>     --registry $ACR_NAME \
>     --name buildhelloworld \
>     --image helloworld:{{.Build.ID}} \
>     --context https://github.com/$GIT_USER/acr-build-helloworld-node \
>     --branch master \
>     --git-access-token $GIT_PAT
{
  "additionalProperties": {},
  "alias": "buildhelloworld",
  "creationDate": "2018-05-10T19:34:48.086776+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/mycontainerregistry/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry/buildTasks/buildhelloworld",
  "location": "eastus",
  "name": "buildhelloworld",
  "platform": {
    "additionalProperties": {},
    "cpu": 1,
    "osType": "Linux"
  },
  "properties": {
    "additionalProperties": {
      "imageName": null
    },
    "baseImageDependencies": null,
    "baseImageTrigger": "Runtime",
    "branch": "master",
    "buildArguments": [],
    "contextPath": null,
    "dockerFilePath": "Dockerfile",
    "imageNames": [
      "helloworld:{{.Build.ID}}"
    ],
    "isPushEnabled": true,
    "noCache": false,
    "provisioningState": "Succeeded",
    "type": "Docker"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "mycontainerregistry",
  "sourceRepository": {
    "additionalProperties": {},
    "isCommitTriggerEnabled": true,
    "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
    "sourceControlAuthProperties": null,
    "sourceControlType": "GitHub"
  },
  "status": "Enabled",
  "tags": null,
  "timeout": 3600,
  "type": "Microsoft.ContainerRegistry/registries/buildTasks"
}
```

## <a name="test-the-build-task"></a>Testar a tarefa de compilação

Tem agora uma tarefa de compilação que define a sua compilação. Para testar a definição da compilação, acione manualmente uma compilação ao executar o comando[az acr build-task run][az-acr-build-task-run]:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Por predefinição, o comando `az acr build-task run` transmite a saída de registo para a consola quando executar o comando. Aqui, o resultado mostra que a compilação **aa2** foi colocada em fila e compilada.

```console
$ az acr build-task run --registry $ACR_NAME --name buildhelloworld
Queued a build with build ID: aa2
Waiting for a build agent...
time="2018-05-10T19:37:17Z" level=info msg="Running command git clone https://x-access-token:*************@github.com/gituser/acr-build-helloworld-node /root/acr-builder/src"
Cloning into '/root/acr-builder/src'...
time="2018-05-10T19:37:17Z" level=info msg="Running command git checkout master"
Already on 'master'
Your branch is up to date with 'origin/master'.
920f16cfafa36d0bc3f397c3dd48185a03499404
time="2018-05-10T19:37:17Z" level=info msg="Running command git rev-parse --verify HEAD"
time="2018-05-10T19:37:17Z" level=info msg="Running command docker build --pull -f Dockerfile -t mycontainerregistry.azurecr.io/helloworld:aa2 ."
Sending build context to Docker daemon  209.9kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
Status: Image is up to date for node:9-alpine
 ---> 7af437a39ec2
Step 2/5 : COPY . /src
 ---> 48a7735fa94e

[...]

26b0c207c4a9: Pushed
917e7cdebc8b: Pushed
aa2: digest: sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89 size: 1367
time="2018-05-10T19:37:57Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistrtyy.azurecr.io/helloworld:aa2"
"["mycontainerregistrtyy.azurecr.io/helloworld@sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89"]"
time="2018-05-10T19:37:57Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistrtyy.azurecr.io
    repository: helloworld
    tag: aa2
    digest: sha256:6975f01e2e202c084581e676acbe6047788fbe616836328b0b31ce8c58e9fc89
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
  git:
    git-head-revision: 920f16cfafa36d0bc3f397c3dd48185a03499404

Build complete
Build ID: aa2 was successful after 46.491407373s
```

## <a name="view-build-status"></a>Ver o estado da compilação

Pode ocasionalmente ser útil ver o estado de uma compilação em curso que não tenha sido acionada manualmente. Por exemplo, durante a resolução de problemas de compilações acionadas por consolidações de código de origem. Nesta secção, vai acionar uma compilação manual, mas suprimir o comportamento predefinido de transmissão do registo de compilação para a consola. Em seguida, vai utilizar o comando `az acr build-task logs` para monitorizar a compilação em curso.

Em primeiro lugar, acione uma compilação manualmente como fez anteriormente, mas especifique o argumento `--no-logs` para suprimir o registo para a consola:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld --no-logs
```

Em seguida, utilize o comando `az build-task logs` para ver o registo da compilação atualmente em execução:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

O registo da compilação atualmente em execução é transmitido em fluxo para a consola e deverá ser semelhante à saída seguinte (apresentado aqui truncado):

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build
Build ID: aa3

[...]

Build complete
Build ID: aa3 was successful after 1m14.26397548s
```

## <a name="trigger-a-build-with-a-commit"></a>Acionar uma compilação com uma consolidação

Agora que testou a tarefa de compilação ao executá-la manualmente, acione-a automaticamente com uma alteração do código de origem.

Em primeiro lugar, confirme que está no diretório que contém o clone local do [repositório][sample-repo]:

```azurecli-interactive
cd acr-build-helloworld-node
```

Em seguida, execute os comandos seguintes para criar, consolidar e emitir um novo ficheiro para a bifurcação do repositório no GitHub:

```azurecli-interactive
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Build"
git push origin master
```

Poderá ser-lhe pedido para fornecer as credenciais do GitHub, quando executar o comando `git push`. Forneça o seu nome de utilizador do GitHub e introduza o token de acesso pessoal (PAT) que criou anteriormente para a palavra-passe.

```console
$ git push origin master
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Assim que tiver enviado a consolidação para o repositório, o webhook criado pelo ACR Build é acionado e coemça uma compilação no Azure Container Registry. Apresenta os registos da compilação atualmente em execução para poder verificar e monitorizar o progresso da compilação:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

O resultado é semelhante ao seguinte, ao mostrar a compilação atualmente em execução (ou a última executada):

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build
Build ID: aa4

[...]

Build complete
Build ID: aa4 was successful after 39.164385024s
```

## <a name="list-builds"></a>Listar as compilações

Para ver uma lista das compilações que o ACR Build concluiu para o seu registo, execute o comando [az acr build-task list-builds][az-acr-build-task-list-builds]:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

O resultado do comando deve ter um aspeto semelhante ao que se segue. São apresentadas as compilações executadas pelo ACR Build e na coluna ACIONAR é apresentado “Consolidação de Git” para a compilação mais recente:

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
----------  ---------------  ----------  ---------  ----------  --------------------  ----------
aa4         buildhelloworld  Linux       Succeeded  Git Commit  2018-05-10T19:49:40Z  00:00:45
aa3         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:41:50Z  00:01:20
aa2         buildhelloworld  Linux       Succeeded  Manual      2018-05-10T19:37:11Z  00:00:50
aa1                          Linux       Succeeded  Manual      2018-05-10T19:10:14Z  00:00:55
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a utilizar uma tarefa de compilação para acionar automaticamente compilações da imagem de contentor no Azure ao consolidar o código de origem para um repositório de Git. Avance para o próximo tutorial para saber como criar tarefas de compilação que acionam compilações quando é atualizada a imagem de base de uma imagem de contentor.

> [!div class="nextstepaction"]
> [Automatizar compilações ao atualizar a imagem de base](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-build-task-list-builds]: /cli/azure/acr#az-acr-build-task-list-build

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
