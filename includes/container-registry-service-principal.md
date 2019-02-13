---
title: incluir ficheiro
description: incluir ficheiro
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 12/14/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 908d42bbecccf4a7c68a6cfde0c8cd960299df89
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56213160"
---
## <a name="create-a-service-principal"></a>Criar um principal de serviço

Para criar um principal de serviço com acesso ao seu registo de contentor, execute o seguinte script [Azure Cloud Shell](../articles/cloud-shell/overview.md) ou uma instalação local dos [CLI do Azure](/cli/azure/install-azure-cli). O script é formatado para o shell de Bash.

Antes de executar o script, atualize o `ACR_NAME` variável com o nome do seu registo de contentor. O `SERVICE_PRINCIPAL_NAME` valor tem de ser exclusivo no seu inquilino do Azure Active Directory. Se receber um "`'http://acr-service-principal' already exists.`" erro, especifique um nome diferente para o principal de serviço.

Opcionalmente, pode modificar o `--role` valor no comando [ad az sp create-for-rbac][/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac] se pretender conceder permissões diferentes. Para obter uma lista completa de funções, consulte [ACR funções e permissões](https://github.com/Azure/acr/blob/master/docs/roles-and-permissions.md).

Depois de executar o script, tome nota do principal de serviço **ID** e **palavra-passe**. Depois de ter as respetivas credenciais, pode configurar seus aplicativos e serviços para autenticar para o seu registo de contentor, como o principal de serviço.

<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh --> [!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Utilizar um principal de serviço existente

Para conceder acesso de registo para um principal de serviço, tem de atribuir uma nova função ao principal de serviço. Como com a criação de um novo serviço principal, pode conceder a solicitação, push e pull e acesso de proprietário, entre outros.

O script seguinte utiliza o comando [az atribuição de função create][/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create] para conceder *pull* permissões para um principal de serviço que especificar no `SERVICE_PRINCIPAL_ID`variável. Ajustar o `--role` valor se desejar conceder um nível diferente de acesso.


<!-- https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-assign-role/service-principal-assign-role.sh --> [!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
