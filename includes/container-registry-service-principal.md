---
title: incluir ficheiro
description: incluir ficheiro
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 04/23/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 6ed114ea6162c9d4888b6f86998cfb422a3944e8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
## <a name="create-a-service-principal"></a>Criar um principal de serviço

Para criar um principal de serviço com acesso ao seu registo de contentor, pode utilizar o seguinte script. Atualização o `ACR_NAME` variável com o nome do seu registo de contentor e, opcionalmente, o `--role` valor o [az ad sp criar-para-rbac] [ az-ad-sp-create-for-rbac] comando para conceder permissões diferentes. Tem de ter o [CLI do Azure](/cli/azure/install-azure-cli) instalada para utilizar este script.

Depois de executar o script, tome nota do principal de serviço **ID** e **palavra-passe**. Depois de ter as respetivas credenciais, pode configurar as suas aplicações e serviços para autenticar para o registo do contentor como o principal de serviço.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Utilize um principal de serviço existente

Para conceder acesso de registo para um principal de serviço existente, tem de atribuir uma nova função para o principal de serviço. Tal como acontece com a criação de um novo serviço principal, pode conceder solicitação, push e pull e acesso de proprietário.

O script seguinte utiliza o [de criação da atribuição de função az] [ az-role-assignment-create] comando para conceder *solicitação* permissões para um principal de serviço que especificar no `SERVICE_PRINCIPAL_ID` variável. Ajustar o `--role` valor se desejar conceder um nível de acesso diferente.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
