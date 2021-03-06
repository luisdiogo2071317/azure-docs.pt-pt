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
ms.openlocfilehash: 95d89da66935ce933fee082a5f53ee2e36ea953f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344691"
---
No Cloud Shell, configure as credenciais de implementação com o [ `az webapp deployment user set` ](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) comando. Para a implementação de FTP e Git local numa aplicação Web, é preciso este utilizador de implementação. O nome de utilizador e a palavra-passe estão ao nível da conta. _São diferentes das credenciais da sua subscrição do Azure._

No comando a seguir, substitua *\<nome de utilizador>* e *\<palavra-passe>* (incluindo os parênteses) por um novo nome de utilizador e palavra-passe. O nome do utilizador tem de ser exclusivo no Azure. A palavra-passe tem de ter, pelo menos, oito carateres, com dois dos seguintes três elementos: letras, números, símbolos. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Deve obter um resultado JSON, com a palavra-passe apresentada como `null`. Se obtiver o erro `'Conflict'. Details: 409`, altere o nome de utilizador. Se obtiver o ` 'Bad Request'. Details: 400` erro, utilize uma palavra-passe mais forte.

Tem de configurar apenas uma vez; este utilizador de implementação pode usá-lo para todas as suas implementações do Azure.

> [!NOTE]
> Registe o nome de utilizador e palavra-passe. Irá necessitar deles para implementar a aplicação Web mais tarde.
>
>
