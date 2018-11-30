---
title: Grupo de recursos de eliminação do Azure Resource Manager
description: Descreve como o Azure Resource Manager ordena a eliminação de recursos quando um tipo de eliminar um grupo de recursos.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2018
ms.author: tomfitz
ms.openlocfilehash: 8b0711cab07584aa84ab437a2a4efb5aab92f3d1
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319359"
---
# <a name="azure-resource-manager-resource-group-deletion"></a>Eliminação do grupo de recursos do Azure Resource Manager

Este artigo descreve como o Azure Resource Manager ordena a eliminação de recursos quando elimina um grupo de recursos.

## <a name="determine-order-of-deletion"></a>Determinar a ordem de eliminação

Quando elimina um grupo de recursos, o Resource Manager determina a ordem para eliminar recursos. Ele usa o seguinte ordem:

1. Todos os recursos subordinados (aninhados) são eliminados.

2. Recursos geridos por outros recursos são eliminados, em seguida. Um recurso pode ter o `managedBy` propriedade definida para indicar que um recurso diferente o gere. Quando essa propriedade é definida, o recurso que gere o outro recurso é eliminado antes os outros recursos.

3. Os recursos restantes são eliminados após as duas categorias anteriores.

## <a name="resource-deletion"></a>Eliminação do recurso

Depois da ordem é determinada, o Resource Manager emite uma operação de eliminação para cada recurso. Ele aguarda todas as dependências concluir antes de continuar.

Para operações síncronas, são os códigos de resposta com êxito esperado:

* 200
* 204
* 404

Para operações assíncronas, a resposta bem-sucedida esperada é 202. Gestor de recursos controla o cabeçalho de localização ou o cabeçalho de operação do azure-async para determinar o estado da operação assíncrona delete.
  
### <a name="errors"></a>Erros

Quando uma operação de eliminação retorna um erro, o Resource Manager tentará novamente com a chamada de eliminação. As repetições ocorrem para 5xx, 429 e códigos de estado 408. Por predefinição, o período de tempo entre tentativas é de 15 minutos.

## <a name="after-deletion"></a>Após a eliminação

Gestor de recursos emite uma chamada GET para cada recurso que ele estava a tentar eliminar. A resposta a esta chamada GET deve ser 404. Quando o Resource Manager obtém um 404, considera a eliminação para serem concluídos com êxito. Gestor de recursos remove o recurso a partir da respetiva cache.

No entanto, se a chamada GET do recurso retorna um 200 ou 201, o Resource Manager recria o recurso.

### <a name="errors"></a>Erros

Se a operação de obtenção de devolver um erro, o Resource Manager repete GET para o seguinte código de erro:

* Menos de 100
* 408
* 429
* Superior a 500

Para outros códigos de erro, o Resource Manager não consegue a eliminação do recurso.

## <a name="next-steps"></a>Passos Seguintes

* Para compreender os conceitos do Resource Manager, veja [descrição geral do Azure Resource Manager](resource-group-overview.md).
* Para ver as operações de um fornecedor de recursos, consulte [API REST do Azure](/rest/api/).
