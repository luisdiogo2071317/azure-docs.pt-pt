---
title: Parâmetros e cabeçalhos comuns
description: Os parâmetros e cabeçalhos comuns a todas as operações que pode fazer relacionadas com os recursos do Cofre de chaves.
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: bryanla
ms.openlocfilehash: de9243a0a95c14a048be124976b07853a48a9a08
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075231"
---
# <a name="common-parameters-and-headers"></a>Parâmetros e cabeçalhos comuns

As informações seguintes são comuns a todas as operações que pode fazer relacionadas com os recursos do Cofre de chaves:

- Substitua `{api-version}` com a api-version no URI.
- Substitua `{subscription-id}` com o identificador de subscrição no URI de
- Substitua `{resource-group-name}` com o grupo de recursos. Para obter mais informações, consulte Utilizar grupos de recursos para gerir os recursos do Azure.
- Substitua `{vault-name}` com o nome do Cofre de chaves no URI.
- Defina o cabeçalho de tipo de conteúdo como application/json.
- Defina o cabeçalho de autorização para um JSON Web Token que obtém do Azure Active Directory (AAD). Para obter mais informações, consulte [autenticar o Azure Resource Manager](authentication-requests-and-responses.md) pedidos.

## <a name="common-error-response"></a>Resposta de erro comuns
O serviço irá utilizar códigos de estado HTTP para indicar êxito ou falha. Além disso, as falhas de contenham uma resposta no seguinte formato:

   {  
     "error": {  
     "code": "BadRequest",  
     "message": "O sku de Cofre de chaves é inválido."  
     }  
   }  

|Nome do elemento | Tipo | Descrição |
|---|---|---|
| Código | cadeia | O tipo de erro que ocorreu.|
| message | cadeia | Uma descrição sobre o que causou o erro. |



## <a name="see-also"></a>Consultar Também
 [Referência da API REST do Cofre de chaves do Azure](/rest/api/keyvault/)
