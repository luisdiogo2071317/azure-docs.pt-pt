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
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: bryanla
ms.openlocfilehash: dae5e1ab6244d2898bc218ed5db3b6b2b90150cf
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44294058"
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
     "message": "o sku de Cofre de chaves é inválido."  
     }  
   }  

|Nome do elemento | Tipo | Descrição |
|---|---|---|
| Código | cadeia | O tipo de erro que ocorreu.|
| message | cadeia | Uma descrição sobre o que causou o erro. |



## <a name="see-also"></a>Consultar Também
 [Referência da API REST do Cofre de chaves do Azure](/rest/api/keyvault/)
