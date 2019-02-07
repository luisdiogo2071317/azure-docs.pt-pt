---
title: O Azure Container Registry - funções e permissões
description: Utilize o controlo de acesso baseado em função do Azure (RBAC) e gestão de identidades e acessos (IAM) para fornecer permissões detalhadas para recursos num Azure container registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 12/17/2018
ms.author: danlep
ms.openlocfilehash: e2ec1b7ad6d1489836937d30b89d0f0f681a9bfa
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55819591"
---
# <a name="azure-container-registry-roles-and-permissions"></a>Permissões e funções de registo de contentor do Azure

O serviço do Azure Container Registry suporta um conjunto de funções do Azure que fornecer diferentes níveis de permissões para um Azure container registry. Utilizar o Azure [controlo de acesso baseado em funções](../role-based-access-control/index.yml) (RBAC) para atribuir permissões específicas para os utilizadores ou principais que precisam para interagir com um registo de serviço.

| / Permissões da função       | Gestor de recursos de acesso| Criar/eliminar registo | [Enviar imagem](#push-image) | [Imagem de extração](#pull-image) | Alterar as políticas |   [Imagens de início de sessão](#sign-images)  |
| ---------| --------- | --------- | --------- | --------- | --------- | --------- |
| Proprietário | X | X | X | X | X |  |  
| Contribuinte | X | X | X | X | X |  |  
| Leitor | X |  |  | X |  |  | 
| AcrPush |  |  | X | X |  |  |  
| AcrPull |  |  |  | X |  |  |  
| AcrImageSigner |  |  |  |  |  | X |

## <a name="differentiate-users-and-services"></a>Diferenciar a utilizadores e serviços

Quaisquer permissões de tempo são aplicadas, é recomendado fornecer o conjunto mais limitado de permissões para uma pessoa ou serviço realizar uma tarefa. Os seguintes conjuntos de permissão representam um conjunto de recursos que podem ser utilizados por seres humanos e de serviços sem periféricos.

### <a name="cicd-solutions"></a>Soluções de CI/CD

Ao automatizar `docker build` comandos de soluções de CI/CD, precisa `docker push` capacidades. Nestes cenários de serviço sem periféricos, sugerimos que atribuir a **AcrPush** função. Esta função, ao contrário do mais abrangente **contribuinte** função, impede que a conta de efetuar outras operações de registo ou aceder ao Azure Resource Manager.

### <a name="container-host-nodes"></a>Nós de anfitrião de contentor

Da mesma forma, nós os contentores em execução têm do **AcrPull** função, mas não deve exigir **leitor** capacidades.

### <a name="visual-studio-code-docker-extension"></a>Extensão do Visual Studio Docker de código

Para ferramentas como o Visual Studio Code [extensão Docker](https://code.visualstudio.com/docs/azure/docker), é necessário acesso de fornecedor de recursos adicionais para listar os registos de contentores do Azure disponíveis. Neste caso, fornecer aos utilizadores acesso para o **leitor** ou **contribuinte** função. Estas funções permitem `docker pull`, `docker push`, `az acr list`, `az acr build`e outros recursos. 

## <a name="access-resource-manager"></a>Gestor de recursos de acesso

Acesso de Gestor de recursos do Azure é necessário para o portal do Azure e [CLI do Azure](/cli/azure/). Por exemplo, para obter uma lista de registos ao utilizar o `az acr list` de comando, tem de ter esta permissão definida. 

## <a name="createdelete-registry"></a>Criar/eliminar registo

A capacidade de criar e eliminar registos de contentores do Azure.

## <a name="push-image"></a>Enviar imagem

A capacidade de `docker push` uma imagem, ou enviar outro artefatos suportados, para um registo. Requer [autenticação](container-registry-authentication.md) com o registro usando a identidade autorizada. 

## <a name="pull-image"></a>Imagem de extração

A capacidade de `docker pull` um não-colocados em quarentena de imagem ou extrair outro artefatos suportados, a partir de um registo. Requer [autenticação](container-registry-authentication.md) com o registro usando a identidade autorizada.

## <a name="change-policies"></a>Alterar as políticas

A capacidade para configurar políticas num registo. Políticas incluem a remoção de imagem, permitindo a quarentena e assinatura de imagem.

## <a name="sign-images"></a>Imagens de início de sessão

A capacidade de iniciar sessão imagens, normalmente, é atribuídas a um processo automatizado, que usaria um principal de serviço. Esta permissão é normalmente combinada com [imagem push](#push-image) para permitir o envio de uma imagem de fidedigna para um registo. Para obter detalhes, consulte [conteúdo de confiança no Azure Container Registry](container-registry-content-trust.md).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre a atribuição de funções RBAC para uma identidade do Azure utilizando o [portal do Azure](../role-based-access-control/role-assignments-portal.md), o [CLI do Azure](../role-based-access-control/role-assignments-cli.md), ou outras ferramentas do Azure.

* Saiba mais sobre [opções de autenticação](container-registry-authentication.md) do Azure Container Registry.
