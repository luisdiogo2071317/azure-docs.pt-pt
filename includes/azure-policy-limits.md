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
ms.openlocfilehash: 0a54dfdb810ea578c1e7c8fcc7ca0343e72164ae
ms.sourcegitcommit: 3dcb1a3993e51963954194ba2a5e42260d0be258
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50964643"
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
| Atribuições de Política/Iniciativa | Exclusões (notScopes) | 250 |
| Regra de Política | Condicionais Aninhadas | 512 |
