---
title: Política de gestão de API do Azure de exemplo - autorizar o acesso a utilizar o token Google OAuth | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como autorizar o acesso aos pontos finais da sua utilização Google como um fornecedor de tokens de OAuth.
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
ms.openlocfilehash: 22aed976ef69288aa0e49215a739174786843527
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284594"
---
# <a name="authorize-access-using-google-oauth-token"></a>Autorizar o acesso a utilizar o token OAuth do Google

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como autorizar o acesso aos pontos finais da sua utilização Google como um fornecedor de tokens de OAuth. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Simple Google OAuth validate-jwt.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

