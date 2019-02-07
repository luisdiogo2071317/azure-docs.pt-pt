---
title: Implementar a API do Azure para FHIR com a CLI do Azure
description: Implemente a API do Azure para FHIR com a CLI do Azure.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 880f9d67f87d32fbc03bc04877267b5b26022381
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823579"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-cli"></a>Início rápido: Implementar a API do Azure para FHIR com a CLI do Azure

Neste início rápido, irá aprender a implementar a API do Azure para FHIR no Azure com a CLI do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-azure-resource-manager-template"></a>Criar o modelo Azure Resource Manager

Crie um modelo Azure Resource Manager com o seguinte conteúdo:

[!code-json[](samples/azuredeploy.json)]

Guarde-o com o nome `azuredeploy.json`

## <a name="create-azure-resource-manager-parameter-file"></a>Criar ficheiro de parâmetros do Azure Resource Manager

Crie um ficheiro de parâmetros de modelo do Azure Resource Manager com o seguinte conteúdo:

[!code-json[](samples/azuredeploy.parameters.json)]

Guarde-o com o nome `azuredeploy.parameters.json`

Os valores de ID de objeto `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` e `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` correspondem ao objeto de IDs de utilizadores específicos do Azure Active Directory ou principais no diretório associado à subscrição de serviço. Se quiser saber o ID de objeto de um utilizador específico, pode encontrá-lo com um comando como:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

Continue a ler o guia de procedimentos [encontrar IDs de objeto de identidade](find-identity-object-ids.md) para obter mais detalhes.

# <a name="create-azure-resource-group"></a>Criar grupo de recursos do Azure

Escolha um nome para o grupo de recursos que irá conter a API do Azure para FHIR e criá-la:

```azurecli-interactive
az group create --name "myResourceGroup" --location westus2
```

## <a name="deploy-the-azure-api-for-fhir-account"></a>Implementar a API do Azure para a conta FHIR

Utilizar o modelo (`azuredeploy.json`) e o ficheiro de parâmetros de modelo (`azuredeploy.parameters.json`) para implementar a API do Azure para FHIR:

```azurecli-interactive
az group deployment create -g "myResourceGroup" --template-file azuredeploy.json --parameters @{azuredeploy.parameters.json}
```

## <a name="fetch-fhir-api-capability-statement"></a>Obter a declaração de capacidade de API de FHIR

Obtenha uma instrução de capacidade da API do FHIR com:

```azurecli-interactive
curl --url "https://nameOfFhirAccount.azurehealthcareapis.com/fhir/metadata"
```

## <a name="clean-up-resources"></a>Limpar recursos

Se não pretender continuar a utilizar esta aplicação, elimine o grupo de recursos com os seguintes passos:

```azurecli-interactive
az group delete --name "myResourceGroup"
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou a API do Azure para FHIR na sua subscrição. Para saber como aceder à API de FHIR com o Postman, avance para o tutorial do Postman.

>[!div class="nextstepaction"]
>[Aceda à API de FHIR com o Postman](access-fhir-postman-tutorial.md)