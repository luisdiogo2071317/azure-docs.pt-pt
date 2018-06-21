---
title: Azure API política de amostra de gestão - OAuth2 de utilização para autorização entre o gateway e um back-end | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como utilizar o OAuth2 para autorização entre o gateway e um back-end. Mostra como obter um token de acesso do AAD e reencaminhá-lo para o back-end.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: d064e918d514b9be1b9fa2dbf30c10edf5167908
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287825"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Utilizar o OAuth2 para autorização entre o gateway e um back-end

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como utilizar o OAuth2 para autorização entre o gateway e um back-end. Mostra como obter um token de acesso do AAD e reencaminhá-lo para o back-end. 

Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

O script seguinte utiliza as propriedades que aparecem em {{propriedade}}. Para saber mais sobre as propriedades e como utilizá-los nas políticas de API Management, consulte [isto](../api-management-howto-properties.md) tópico.
 
## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

