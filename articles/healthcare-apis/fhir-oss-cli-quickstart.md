---
title: Implementar o servidor de Open FHIR de origem para o Azure com a CLI do Azure
description: Este início rápido explica como implementar o servidor Open FHIR de Microsoft de origem para o Azure.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 6bb2806aa1ba830f1ef3215a61db4d40f531bdd8
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823530"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-cli"></a>Início rápido: Implementar o servidor de FHIR de fonte aberto com a CLI do Azure

Neste início rápido, irá aprender como implantar um FHIR de origem aberto&reg; server no Azure com a CLI do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-resource-group"></a>Criar grupo de recursos

Escolha um nome para o grupo de recursos que irá conter os recursos aprovisionados e criá-la:

```azurecli-interactive
servicename="myfhirservice"
az group create --name $servicename --location westus2
```

## <a name="deploy-template"></a>Implementar o modelo

O servidor de FHIR da Microsoft para o Azure [repositório do GitHub](https://github.com/Microsoft/fhir-server) contém um modelo que irá implementar todos os recursos necessários. Implemente-a com:

```azurecli-interactive
az group deployment create -g $servicename --template-uri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json --parameters serviceName=$servicename
```

## <a name="verify-fhir-server-is-running"></a>Certifique-se de que o servidor FHIR está em execução

Obtenha uma instrução de capacidade do servidor FHIR com:

```azurecli-interactive
metadataurl="https://${servicename}.azurewebsites.net/metadata"
curl --url $metadataurl
```

Irá demorar um minuto ou para o servidor responda pela primeira vez.

## <a name="clean-up-resources"></a>Limpar recursos

Se não pretender continuar a utilizar esta aplicação, elimine o grupo de recursos com os seguintes passos:

```azurecli-interactive
az group delete --name $servicename
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou o Microsoft Open FHIR servidor de origem para o Azure na sua subscrição. Para saber como aceder à API de FHIR com o Postman, avance para o tutorial do Postman.
 
>[!div class="nextstepaction"]
>[Aceda à API de FHIR com o Postman](access-fhir-postman-tutorial.md)