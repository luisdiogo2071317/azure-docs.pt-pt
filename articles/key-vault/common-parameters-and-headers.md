---
title: Os parâmetros e cabeçalhos comuns
description: Os parâmetros e cabeçalhos comuns a todas as operações que pode fazer relacionado com recursos do Cofre de chaves.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: ead1ac550c9b7c489edefd35d5672a9955e78255
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="common-parameters-and-headers"></a>Os parâmetros e cabeçalhos comuns

As informações seguintes são comuns a todas as operações que pode fazer relacionadas com recursos do Cofre de chaves:

- Substitua `{api-version}` com a versão de api no URI.
- Substitua `{subscription-id}` com o identificador de subscrição no URI do
- Substitua `{resource-group-name}` com o grupo de recursos. Para obter mais informações, consulte Utilizar grupos de recursos para gerir os recursos do Azure.
- Substitua `{vault-name}` com o nome do Cofre de chaves no URI.
- Defina o cabeçalho de tipo de conteúdo application/JSON.
- Defina o cabeçalho de autorização para um Web Token JSON que obtém a partir do Azure Active Directory (AAD). Para obter mais informações, consulte [autenticar o Azure Resource Manager](authentication-requests-and-responses.md) pedidos.

## <a name="common-error-response"></a>Resposta de erro comuns
O serviço irá utilizar códigos de estado HTTP para indicar o êxito ou falha. Além disso, as falhas de contenham uma resposta no seguinte formato:

   {  
     "erro": {  
     "código": "BadRequest"  
     "mensagem": "o sku do Cofre de chaves é inválido."  
     }  
   }  

|Nome do elemento | Tipo | Descrição |
|---|---|---|
| código | cadeia | O tipo de erro que ocorreu.|
| message | cadeia | Uma descrição que causou o erro. |



## <a name="see-also"></a>Consultar Também
 [Referência de API de REST do Cofre de chaves do Azure](/rest/api/keyvault/)
