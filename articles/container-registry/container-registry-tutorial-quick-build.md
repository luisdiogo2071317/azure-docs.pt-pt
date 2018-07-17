---
title: Tutorial – Compilar imagens de contentor na cloud com o Azure Container Registry Build
description: Neste tutorial, vai aprender a compilar uma imagem de contentor do Docker no Azure com o Azure Container Registry Build (ACR Build) e, em seguida, implementá-la no Azure Container Instances.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: d86c47c890fd15515c590e06b395ca82f9747ffe
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38461474"
---
# <a name="tutorial-build-container-images-in-the-cloud-with-azure-container-registry-build"></a>Tutorial: Compilar imagens de contentor na cloud com o Azure Container Registry Build

O **ACR Build** é um conjunto de funcionalidades no Azure Container Registry que fornece compilações de imagens de contentor do Docker simplificadas e eficientes no Azure. Neste artigo, vai aprender a utilizar a funcionalidade *Compilação Rápida* do ACR Build. A Compilação Rápida expande o “ciclo interno” de desenvolvimento para a cloud, ao fornecer-lhe a validação da compilação com êxito e o envio automático com êxito de imagens compiladas para o seu registo de contentor. As imagens são compiladas de forma nativa na cloud, junto do seu registo, o que permite uma implementação mais rápida.

Todos os seus conhecimentos do Dockerfile são diretamente transferíveis para o ACR Build. Não tem de alterar o Dockerfiles para compilar com o ACR Build na cloud, só o comando que executa.

Neste tutorial, a primeira parte de uma série:

> [!div class="checklist"]
> * Obter o código de origem da aplicação de exemplo
> * Compilar uma imagem do contentor no Azure
> * Implementar um contentor no Azure Container Instances

Nos tutoriais subsequentes, vai aprender a utilizar tarefas de compilação do ACR Build para obter compilações automatizadas da imagem de contentor após a atualização da imagem de base e da consolidação do código.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se quiser utilizar a CLI do Azure localmente, terá de ter a versão **2.0.32** ou posterior da CLI do Azure instalada. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar a CLI, veja [Instalar a CLI do Azure][azure-cli].

## <a name="prerequisites"></a>Pré-requisitos

### <a name="github-account"></a>Conta do GitHub

Crie uma conta em https://github.com, se ainda não tiver uma. Esta série de tutoriais utiliza um repositório do GitHub para demonstrar as compilações de imagem automatizadas no ACR Build.

### <a name="fork-sample-repository"></a>Bifurcar um repositório de exemplo

Em seguida, utilize a IU do GitHub para bifurcar o repositório de exemplo na conta do GitHub. Neste tutorial, vai criar uma imagem de contentor a partir da origem no repositório e, no próximo tutorial, vai enviar uma consolidação para a bifurcação do repositório para iniciar uma compilação automatizada.

Bifurcar este repositório: https://github.com/Azure-Samples/acr-build-helloworld-node

![Captura de ecrã do botão Bifurcar (realçado) no GitHub][quick-build-01-fork]

### <a name="clone-your-fork"></a>Clonar a bifurcação

Depois de concluir a bifurcação do repositório, clone a bifurcação e introduza o diretório que contém o clone local.

Clone o repositório com `git`, substitua **\<your-github-username\>** pelo nome de utilizador do GitHub:

```azurecli-interactive
git clone https://github.com/<your-github-username>/acr-build-helloworld-node
```

Introduza o diretório que contém o código de origem:

```azurecli-interactive
cd acr-build-helloworld-node
```

### <a name="bash-shell"></a>Shell do Bash

Os comandos nesta série de tutoriais estão formatados para a shell do Bash. Se preferir utilizar o PowerShell, a Linha de Comandos ou outra shell, poderá ter de ajustar o formato da continuação de linha e da variável de ambiente em conformidade.

## <a name="build-in-azure-with-acr-build"></a>Compilar no Azure com o ACR Build

Agora que já solicitou o código de origem para o computador, siga estes passos para criar um registo de contentor e compilar a imagem de contentor com o ACR Build.

Para a execução dos comandos de exemplo ser mais fácil, os tutoriais nesta série utilizam variáveis de ambiente da shell. Execute o comando seguinte para definir a variável `ACR_NAME`. Substitua **\<registry-name\>** por um nome exclusivo para o novo registo de contentor. O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. Os outros recursos que criar no tutorial baseiam-se neste nome, pelo que necessita de modificar apenas esta primeira variável.

```azurecli-interactive
ACR_NAME=<registry-name>
```

Com a variável de ambiente de registo de contentor preenchida, deve agora poder copiar e colar o resto dos comandos no tutorial sem editar nenhum dos valores. Execute os comandos seguintes para criar um grupo de recursos e um registo de contentor:

```azurecli-interactive
RES_GROUP=$ACR_NAME # Resource Group name

az group create --resource-group $RES_GROUP --location eastus
az acr create --resource-group $RES_GROUP --name $ACR_NAME --sku Standard --location eastus
```

Agora que tem um registo, utilize o ACR Build para compilar uma imagem de contentor a partir do código de exemplo. Execute o comando [az acr build][az-acr-build] para efetuar uma *Compilação Rápida*:

```azurecli-interactive
az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
```

O resultado do comando [az acr build][az-acr-build] é semelhante ao seguinte. Pode ver o carregamento do código de origem (o “contexto”) no Azure e os detalhes da operação `docker build` que o ACR Built executa na cloud. Uma vez que o ACR Build utiliza `docker build` para compilar as imagens, não são necessárias alterações ao Dockerfiles para começar a utilizar o ACR Build imediatamente.

```console
$ az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
Sending build context (4.909 KiB) to ACR.
Queued a build with build ID: aa1
Waiting for a build agent...
Sending build context to Docker daemon  22.53kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
Status: Image is up to date for node:9-alpine
 ---> 7af437a39ec2
Step 2/5 : COPY . /src
 ---> 0c4814714938
Step 3/5 : RUN cd /src && npm install
 ---> Running in 4f77ce7b330d
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN helloworld@1.0.0 No repository field.

up to date in 0.096s
Removing intermediate container 4f77ce7b330d
 ---> e0eb24339e07
Step 4/5 : EXPOSE 80
 ---> Running in 872bd29edbc7
Removing intermediate container 872bd29edbc7
 ---> 22ba8d8ffb4e
Step 5/5 : CMD ["node", "/src/server.js"]
 ---> Running in 1a40c05c4122
Removing intermediate container 1a40c05c4122
 ---> 0a9a4b74fb53
Successfully built 0a9a4b74fb53
Successfully tagged mycontainerregistry.azurecr.io/helloacrbuild:v1
time="2018-05-10T19:10:20Z" level=info msg="Running command docker push mycontainerregistry.azurecr.io/helloacrbuild:v1"
The push refers to repository [mycontainerregistry.azurecr.io/helloacrbuild]
d2b301f7ef94: Preparing
d0e0f2bb8747: Preparing
26b0c207c4a9: Preparing
917e7cdebc8b: Preparing
9dfa40a0da3b: Preparing
26b0c207c4a9: Pushed
d2b301f7ef94: Pushed
d0e0f2bb8747: Pushed
9dfa40a0da3b: Pushed
917e7cdebc8b: Pushed
v1: digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd size: 1366
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistry.azurecr.io/helloacrbuild:v1"
"["mycontainerregistry.azurecr.io/helloacrbuild@sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd"]"
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistry.azurecr.io
    repository: helloacrbuild
    tag: v1
    digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3

Build complete
Build ID: aa1 was successful after 52.522222729s
```

Perto do final do resultado, o ACR Build mostra as dependências que detetou para a imagem. Essas dependências permitem que o ACR Build automatize as compilações da imagem ao atualizar a imagem de base, por exemplo, quando uma imagem de base é atualizada com as correções do SO ou da estrutura. Mais adiante nesta série de tutoriais, vai obter informações sobre o suporte do ACR Build para as atualizações da imagem de base.

## <a name="deploy-to-azure-container-instances"></a>Implementar no Azure Container Instances

O ACR Build envia automaticamente imagens compiladas com êxito para o registo por predefinição, permitindo-lhe implementá-las imediatamente a partir do registo.

Nesta secção, vai criar um Azure Key Vault e um principal de serviço e, em seguida, VAI implementar o contentor no Azure Container Instances (ACI) ao utilizar as credenciais de um principal de serviço.

### <a name="configure-registry-authentication"></a>Configurar a autenticação do registo

Todos os cenários de produção devem utilizar [principais de serviço][service-principal-auth] para aceder aos registos de contentor do Azure. Os principais de serviço permitem-lhe ter controlo de acesso baseado em funções sobre as imagens de contentor. Por exemplo, pode configurar um principal de serviço com acesso a um registo apenas por pedido.

#### <a name="create-key-vault"></a>Criar o cofre de chaves

Se ainda não tiver um cofre no [Azure Key Vault](/azure/key-vault/), crie um com a CLI do Azure através dos comandos seguintes.

```azurecli-interactive
AKV_NAME=$ACR_NAME-vault

az keyvault create --resource-group $RES_GROUP --name $AKV_NAME
```

#### <a name="create-service-principal-and-store-credentials"></a>Criar o principal de serviço e armazenar as credenciais

Agora tem de criar um principal de serviço e armazenar as credenciais no cofre de chaves.

Utilize o comando [az ad sp create-for-rbac][az-ad-sp-create-for-rbac] para criar o principal de serviço e [az keyvault secret set][az-keyvault-secret-set] para armazenar a **palavra-passe** do principal de serviço no cofre:

```azurecli-interactive
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role reader \
                --query password \
                --output tsv)
```

O argumento `--role` no comando anterior configura o principal de serviço com a função de *leitor*, a qual concede acesso ao registo apenas por pedido. Para conceder acesso por pedido e por envio, altere o argumento `--role` para *contribuinte*.

Em seguida, armazene o *appId* do principal de serviço no cofre, que é o **nome de utilizador** que passa para o Azure Container Registry para autenticação:

```azurecli-interactive
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

Acabou de criar um Azure Key Vault e de armazenar dois segredos nele:

* `$ACR_NAME-pull-usr`: o ID do principal de serviço, para utilização como o **nome de utilizador** do registo de contentor.
* `$ACR_NAME-pull-pwd`: a palavra-passe do principal de serviço, para utilização como a **palavra-passe** do registo de contentor.

Agora, pode referenciar estes segredos por nome quando você ou as suas aplicações e serviços solicitarem imagens do registo.

### <a name="deploy-container-with-azure-cli"></a>Implementar o contentor com a CLI do Azure

Agora que as credenciais do principal de serviço estão armazenadas como segredos no Azure Key Vault, as aplicações e os serviços podem utilizá-las para aceder ao registo privado.

Execute o comando seguinte [az container create][az-container-create] para implementar uma instância de contentor. O comando utiliza as credenciais do principal de serviço armazenadas no Azure Key Vault para autenticar no registo de contentor.

```azurecli-interactive
az container create \
    --resource-group $RES_GROUP \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label acr-build-$ACR_NAME \
    --query "{FQDN:ipAddress.fqdn}" \
    --output table
```

O valor `--dns-name-label` tem de ser exclusivo no Azure, para que o comando anterior acrescente o nome do registo de contentor à etiqueta de nome DNS do contentor. O resultado do comando apresenta o nome de domínio completamente qualificado (FQDN) do contentor, por exemplo:

```console
$ az container create \
>     --resource-group $RES_GROUP \
>     --name acr-build \
>     --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
>     --registry-login-server $ACR_NAME.azurecr.io \
>     --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
>     --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
>     --dns-name-label acr-build-$ACR_NAME \
>     --query "{FQDN:ipAddress.fqdn}" \
>     --output table
FQDN
-------------------------------------------
acr-build-1175.eastus.azurecontainer.io
```

Tome nota do FQDN do contentor, vai utilizá-lo na próxima secção.

### <a name="verify-deployment"></a>Verificar a implementação

Para ver o processo de arranque do contentor, utilize o comando [az container attach][az-container-attach]:

```azurecli-interactive
az container attach --resource-group $RES_GROUP --name acr-build
```

O resultado `az container attach` apresenta primeiro o estado do contentor, à medida que solicita a imagem e começa e, em seguida, vincula STDOUT e STDERR da consola local nos do contentor.

```console
$ az container attach --resource-group $RES_GROUP --name acr-build
Container 'acr-build' is in state 'Waiting'...
Container 'acr-build' is in state 'Running'...
(count: 1) (last timestamp: 2018-04-03 19:45:37+00:00) pulling image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Successfully pulled image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Created container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Started container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413

Start streaming logs:
Server running at http://localhost:80
```

Quando apresentar `Server running at http://localhost:80`, navegue para o FQDN do contentor no browser para ver a aplicação em execução:

![Captura de ecrã da aplicação de exemplo composta no browser][quick-build-02-browser]

Para desanexar a consola do contentor, prima `Control+C`.

## <a name="clean-up-resources"></a>Limpar recursos

Pare a instância de contentor com o comando [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group $RES_GROUP --name acr-build
```

Para remover *todos* os recursos que criou neste tutorial, incluindo o registo de contentor, o cofre de chaves e o principal de serviço, emita os comandos seguintes. No entanto, estes recursos serão utilizados no [próximo tutorial](container-registry-tutorial-build-task.md) da série, por isso, poderá querer mantê-los se avançar diretamente para o próximo tutorial.

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Passos seguintes

Agora que testou o ciclo interno com uma compilação rápida, configure uma **tarefa de compilação** para acionar compilações de imagens de contentor quando consolidar o código de origem para um repositório de Git:

> [!div class="nextstepaction"]
> [Acionar compilações automáticas com tarefas de compilação](container-registry-tutorial-build-task.md)

<!-- LINKS - External -->
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-keyvault-create]: /cli/azure/keyvault/secret#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[service-principal-auth]: container-registry-auth-service-principal.md

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
