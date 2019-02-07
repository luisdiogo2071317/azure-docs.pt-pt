---
title: Encontrar IDs de objeto de identidade para autenticação – API do Azure para FHIR
description: Este artigo explica como localizar o objeto de identidade necessários para configurar a autenticação para API do Azure para FHIR de IDs
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: b120b21a89a7105a25ba610402da99f9dce4b7e1
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824181"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Encontrar IDs de objeto de identidade para a configuração de autenticação

Neste artigo, irá aprender como encontrar os IDs de objeto necessários para configurar a lista de IDs de objeto de identidade permitido para a API do Azure para FHIR de identidade.

A API do Azure totalmente gerido para FHIR&reg; serviço está configurado para permitir o acesso para uma lista predefinida de identidade IDs de objeto. Quando uma aplicação ou o utilizador está a tentar aceder à API de FHIR, um token de portador deve ser apresentado. Este token de portador terão determinadas afirmações (campos). Para conceder acesso à API do FHIR, o token tem de conter o emissor certo (`iss`), público-alvo (`aud`) e um ID de objeto (`oid`) de uma lista de IDs de objeto permitidos. Um ID de objeto de identidade é o ID de objeto de um utilizador ou um serviço principal no Azure Active Directory.

## <a name="configure-list-of-allowed-object-ids"></a>Configurar a lista de IDs de objeto permitidos

Quando cria uma nova API do Azure para a instância de FHIR, pode configurar uma lista de IDs de objeto permitidos:

![Configurar os IDs de objeto permitidos](media/quickstart-paas-portal/configure-allowed-oids.png)

Estes IDs de objeto podem ser IDs para utilizadores específicos ou principais de serviço no Azure Active Directory.

## <a name="find-user-object-id-using-powershell"></a>Encontrar o ID de objeto de utilizador com o PowerShell

Se tiver um usuário com o nome de utilizador `myuser@consoso.com`, é possível localizar os utilizadores `ObjectId` com o seguinte comando do PowerShell:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

em alternativa, pode utilizar a CLI do Azure:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

## <a name="find-service-principal-object-id-using-powershell"></a>Localizar o ID de objeto do principal de serviço com o PowerShell

Suponha que se registrou uma [do serviço de aplicações de cliente](register-service-azure-ad-client-app.md) e gostaria de permitir que este cliente de serviço aceder à API do Azure para FHIR, pode encontrar o ID de objeto para o cliente do principal de serviço com o seguinte comando do PowerShell:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

onde `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` é o ID de aplicação de cliente do serviço. Em alternativa, pode utilizar o `DisplayName` do cliente do serviço:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Se estiver a utilizar a CLI do Azure, pode utilizar:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX | jq -r .objectId
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como encontrar necessárias para configurar as identidades que têm permissão para aceder a uma API do Azure para a instância de FHIR de IDs de objeto de identidade. Em seguida, implante uma API totalmente gerido do Azure para FHIR:
 
>[!div class="nextstepaction"]
>[Implementar o servidor Open FHIR de origem](fhir-paas-portal-quickstart.md)