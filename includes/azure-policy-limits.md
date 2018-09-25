---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: c3365450c90c4fda37884e8998fad70f5d164244
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006531"
---
Existe um número máximo para cada tipo de objeto do Azure Policy. Uma entrada de _Âmbito_ significa a subscrição ou o [grupo de gestão](../articles/governance/management-groups/overview.md).

| Onde | O quê | Contagem máxima |
|---|---|---|
| Âmbito | Definições de Política | 250 |
| Âmbito | Definições de Iniciativa | 100 |
| Inquilino | Definições de Iniciativa | 1000 |
| Âmbito | Atribuições de Política/Iniciativa | 100 |
| Definição de Política | Parâmetros | 20 |
| Definição de Iniciativa | Políticas | 100 |
| Definição de Iniciativa | Parâmetros | 100 |
| Atribuições de Política/Iniciativa | Exclusões (notScopes) | 100 |
| Regra de Política | Condicionais Aninhadas | 512 |
