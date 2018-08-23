---
title: Diretrizes de limitação do Azure Key Vault
description: Limitação do Key Vault limita o número de chamadas simultâneas para impedir o uso excessivo de recursos.
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: ''
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.date: 05/10/2018
ms.author: bryanla
ms.openlocfilehash: 28756cf28305927246d82f1f006f02b2e9b96469
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42057630"
---
# <a name="azure-key-vault-throttling-guidance"></a>Diretrizes de limitação do Azure Key Vault

Limitação é um processo iniciar que limita o número de chamadas simultâneas para o serviço do Azure para impedir o uso excessivo de recursos. Cofre de chaves do Azure (AKV) foi concebida para lidar com um grande volume de pedidos. Se ocorrer um número massivo de pedidos, limitar os pedidos de seu cliente ajuda a manter a otimizar o desempenho e fiabilidade do serviço AKV.

Limites de limitação variar com base no cenário. Por exemplo, se estiver executando um grande volume de escritas, a possibilidade de limitação é maior do que se estiver a efetuar apenas leituras.

## <a name="how-does-key-vault-handle-its-limits"></a>Como é que o Cofre de chaves com seus limites?

Limites de serviço do Cofre de chaves estão lá impedir a utilização indevida dos recursos e garantir a qualidade de serviço para todos os clientes de Key Vault. Quando um limiar de serviço for excedido, o Key Vault limita os pedidos adicionais desse cliente durante um período de tempo. Quando isto acontecer, o Key Vault retorna o código de estado HTTP 429 (demasiados pedidos), e os pedidos falharem. Além disso, pedidos falhados que devolvem uma contagem 429 para os limites de limitação controlados pelo Cofre de chaves. 

Se tiver um caso comercial válido para limites de limitação mais elevados, entre em contato conosco.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Como limitar a sua aplicação em resposta a limites de serviço

Seguem-se **melhores práticas** para a sua aplicação de limitação de:
- Reduza o número de operações por pedido.
- Reduza a frequência de pedidos.
- Evite as repetições imediatas. 
    - Todos os pedidos de acumulam em relação a seus limites de utilização.

Quando implementar o tratamento de erros da sua aplicação, utilize o código de erro HTTP 429 para detetar a necessidade de limitação do lado do cliente. Se o pedido falhar novamente com um código de erro HTTP 429, tiver ainda com um limite de serviço do Azure. Continue a utilizar o recomendado lado do cliente a limitação de método, repetir o pedido até ter êxito.

### <a name="recommended-client-side-throttling-method"></a>Método de limitação do lado do cliente recomendado

No código de erro HTTP 429, começar o seu cliente usando uma abordagem de término exponencial de limitação:

1. Aguarde 1 segundo, o pedido de repetição
2. Se ainda otimizado espera 2 segundos, repita o pedido
3. Se ainda otimizado espera 4 segundos, repita o pedido
4. Se ainda otimizado espera 8 segundos, repita o pedido
5. Se ainda otimizado espera 16 segundos, repita o pedido

Neste ponto, deverá não receber códigos de resposta de HTTP 429.

## <a name="see-also"></a>Consulte também

Para uma orientação mais aprofundada de limitação na Cloud da Microsoft, consulte [padrão de limitação](https://docs.microsoft.com/azure/architecture/patterns/throttling).

