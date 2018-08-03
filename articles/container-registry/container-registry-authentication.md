---
title: Autenticar com um Azure container registry
description: Opções de autenticação para um Azure container registry, incluindo o Azure Active Directory do serviço de início de sessão de entidades de segurança, direta e do Registro.
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 01/23/2018
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b97a0fffa6f21e7a8b06dc406711f249310fd73
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431594"
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Autenticar com um registo privado de contentor do Docker

Existem várias formas de autenticar com um Azure container registry, cada um dos quais é aplicável a um ou mais cenários de utilização do registo.

Pode iniciar sessão para um registo diretamente via [início de sessão individual](#individual-login-with-azure-ad), e seus aplicativos e orquestradores de contentor podem efetuar a autenticação automática, ou "sem interface", com um Azure Active Directory (Azure AD) [ principal de serviço](#service-principal).

O registo de contentor do Azure não suporta operações de Docker não autenticadas ou acesso anónimo. Para imagens públicas, pode usar [Docker Hub](https://docs.docker.com/docker-hub/).

## <a name="individual-login-with-azure-ad"></a>Início de sessão individual com o Azure AD

Ao trabalhar com o registo diretamente, como extrair imagens e enviar imagens a partir da sua estação de trabalho de desenvolvimento, autenticar com o [início de sessão az acr](/cli/azure/acr?view=azure-cli-latest#az-acr-login) comando no [CLI do Azure](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

Quando iniciar sessão com `az acr login`, a CLI utiliza o token criado quando tiver executado `az login` para autenticar facilmente a sua sessão com o registo. Uma vez que já iniciou sessão desse modo, as suas credenciais estão em cache e subsequentes `docker` comandos não necessitam de um nome de utilizador ou palavra-passe. Se o seu token expira, pode atualizá-la utilizando o `az acr login` comando novamente para o autenticar. Usando `az acr login` com identidades do Azure fornece [acesso baseado em funções](../role-based-access-control/role-assignments-portal.md).

## <a name="service-principal"></a>Principal de serviço

Pode atribuir um [principal de serviço](../active-directory/develop/active-directory-application-objects.md) para o seu registo, e a sua aplicação ou serviço pode utilizá-lo para a autenticação sem periféricos. Principais de serviço permitem [acesso baseado em funções](../role-based-access-control/role-assignments-portal.md) para um registo, e pode atribuir múltiplos principais de serviço para um registo. Vários principais de serviço permitem-lhe definir acesso diferentes para diferentes aplicações.

As funções disponíveis são:

  * **Leitor**: pull
  * **Contribuinte**: enviar e receber
  * **Proprietário**: extrair, push e atribuir funções a outros utilizadores

Principais de serviço permitem a conectividade sem interface para um registo em cenários de push e pull semelhante ao seguinte:

  * *Leitor*: implementações de contentores a partir de um registo a sistemas de orquestração, incluindo o Kubernetes, DC/OS e Docker Swarm. Pode também tirar de registos de contentores para serviços relacionados do Azure, tal como [AKS](../aks/index.yml), [serviço de aplicações](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/), e outras pessoas.

  * *Contribuinte*: soluções de integração e implementação contínua como Visual Studio Team Services (VSTS) ou o Jenkins que criar imagens de contentor e envie-os para um registo.

> [!TIP]
> Pode voltar a gerar a palavra-passe de um principal de serviço ao executar o [az ad sp reset-credentials](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-reset-credentials) comando.
>

Também pode iniciar sessão diretamente com um principal de serviço. Forneça o ID da aplicação e a palavra-passe do serviço principal para o `docker login` comando:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Depois de iniciar sessão, Docker coloca em cache as credenciais, pelo que não precisa se lembrar a ID da aplicação.

Dependendo da versão do Docker que tiver instalado, poderá ver um aviso de segurança recomenda a utilização do `--password-stdin` parâmetro. Enquanto a sua utilização está fora do âmbito deste artigo, recomendamos que siga esta melhor prática. Para obter mais informações, consulte a [início de sessão do docker](https://docs.docker.com/engine/reference/commandline/login/) referência do comando.

Para obter mais informações sobre como utilizar um principal de serviço para a autenticação sem interface para o ACR, consulte [autenticação do Azure Container Registry com principais de serviço](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Conta de admin

Cada registo de contentor inclui uma conta de utilizador de administrador, o que está desativada por predefinição. Pode permitir que o utilizador de administrador e gerir as respetivas credenciais na [portal do Azure](container-registry-get-started-portal.md#create-a-container-registry), ou utilizando a CLI do Azure.

> [!IMPORTANT]
> A conta de administrador foi concebida para um único utilizador aceder ao registo, principalmente para fins de teste. Não é recomendável compartilhar as credenciais da conta de administrador com vários utilizadores. Todos os utilizadores a autenticação com a conta de administrador são apresentados como um único utilizador com acesso de push e pull para o registo. Alterar ou desativar esta conta Desabilita o acesso de registo para todos os utilizadores que utilizam as suas credenciais. Identidade individual é recomendada para utilizadores e os principais de serviço para cenários sem periféricos.
>

A conta de administrador é fornecido com duas palavras-passe, que podem ser regeneradas. Duas palavras-passe permitem-lhe manter a ligação ao registo, utilizando uma palavra-passe enquanto volta a gerar a outra. Se a conta de administrador estiver ativada, pode passar o nome de utilizador e a palavra-passe para o `docker login` comando para a autenticação básica para o registo. Por exemplo:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

Novamente, o Docker recomenda que use o `--password-stdin` parâmetro em vez de fornecê-lo na linha de comandos para maior segurança. Também pode especificar apenas seu nome de utilizador, sem `-p`e a palavra-passe quando lhe for pedido.

Para permitir que o utilizador de administrador num registo existente, pode utilizar o `--admin-enabled` parâmetro do [atualização do az acr](/cli/azure/acr?view=azure-cli-latest#az-acr-update) comando na CLI do Azure:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Pode ativar o utilizador de administrador no portal do Azure ao navegar até seu registro, selecionando **chaves de acesso** em **definições**, em seguida, **ativar** em **administrador utilizador**.

![Ative o utilizador de administrador da interface do Usuário no portal do Azure][auth-portal-01]

## <a name="next-steps"></a>Passos Seguintes

* [Enviar a sua primeira imagem com a CLI do Azure](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
