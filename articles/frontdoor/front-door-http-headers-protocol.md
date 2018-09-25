---
title: Serviço de porta de entrada do Azure - suporte de protocolo de cabeçalhos HTTP | Documentos da Microsoft
description: Este artigo ajuda-o a compreender os protocolos de cabeçalho HTTP suportados por porta de entrada
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 0dcb769627714be9da55faf2a8e82c8750789498
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038855"
---
# <a name="azure-front-door-service---http-headers-protocol-support"></a>Serviço de porta de entrada do Azure - suporte de protocolo de cabeçalhos HTTP
Este documento descreve o protocolo que suporte o serviço de porta de entrada do Azure com várias partes do caminho da chamada conforme descrito pela imagem abaixo. As seções abaixo fornecem mais informações sobre os cabeçalhos HTTP que suporta a porta de entrada.

![Protocolo de cabeçalhos de HTTP do serviço de porta de entrada do Azure][1]

>[!WARNING]
>O serviço de porta de entrada do Azure não fornece garantias em qualquer cabeçalho HTTP que não está documentado aqui.

## <a name="1-client-to-front-door"></a>1. Cliente para a porta de entrada
Porta de entrada aceita a maioria dos cabeçalhos da solicitação recebida (sem modificá-los), no entanto, existem alguns cabeçalhos reservados que serão removidos da solicitação recebida se eles são enviados. Isto inclui cabeçalhos com os prefixos seguintes:
 - X-FD *
 - X-MS *

## <a name="2-front-door-to-backend"></a>2. Porta de entrada para o back-end

Porta da frente irão incluir os cabeçalhos da solicitação recebida, a menos que eles foram removidos devido a restrições mencionadas acima. Porta de entrada também adicionará os cabeçalhos seguintes:

| Cabeçalho  | Exemplo e uma descrição |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ = =* </br> Esta é uma cadeia de referência exclusivo que identifica um pedido servido por porta de entrada. É fundamental para resolução de problemas como é usada para pesquisar os registos de acesso.|
| X-MS-RequestChain |  *X-MS-RequestChain: saltos = 1* </br> Este é um cabeçalho que utiliza a porta de entrada para detetar os loops de pedido e os usuários não devem tomar uma dependência no mesmo. |
| X-MS-Via |  *X-MS-Via: Azure* </br> Este valor é adicionado por porta de entrada para indicar que o Azure/porta de entrada foi um destinatário intermediário para o pedido entre o cliente e o back-end. |

## <a name="3-front-door-to-client"></a>3. Porta de entrada para o cliente

Seguem-se os cabeçalhos que são enviados do porta de entrada para os clientes. Os cabeçalhos enviados para a porta da frente do back-end são transmitidos para o cliente também.

| Cabeçalho  | Exemplo |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ = =* </br> Esta é uma cadeia de referência exclusivo que identifica um pedido servido por porta de entrada. É fundamental para resolução de problemas como é usada para pesquisar os registos de acesso.|

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar uma porta de entrada](quickstart-create-front-door.md).
- Saiba mais [como funciona a porta da frente](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png