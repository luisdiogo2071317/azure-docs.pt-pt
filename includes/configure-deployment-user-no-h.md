---
title: incluir ficheiro
description: incluir ficheiro
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 463e8b0122339831d5d6a65b6e41d4f697a82013
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/03/2018
ms.locfileid: "28986140"
---
No Cloud Shell, crie credenciais de implementação com o comando [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az_webapp_deployment_user_set). Para a implementação de FTP e Git local numa aplicação Web, é preciso este utilizador de implementação. O nome de utilizador e a palavra-passe estão ao nível da conta. _São diferentes das credenciais da sua subscrição do Azure._

No comando a seguir, substitua *\<nome de utilizador>* e *\<palavra-passe>* (incluindo os parênteses) por um novo nome de utilizador e palavra-passe. O nome do utilizador tem de ser exclusivo no Azure. A palavra-passe tem de ter, pelo menos, oito carateres, com dois dos seguintes três elementos: letras, números, símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Deve obter um resultado JSON, com a palavra-passe apresentada como `null`. Se obtiver o erro `'Conflict'. Details: 409`, altere o nome de utilizador. Se obtiver o ` 'Bad Request'. Details: 400` erro, utilize uma palavra-passe mais forte.

Este utilizador de implementação só é criado uma vez; pode utilizá-lo em todas as suas implementações do Azure.

> [!NOTE]
> Registe o nome de utilizador e palavra-passe. Irá necessitar deles para implementar a aplicação Web mais tarde.
>
>
