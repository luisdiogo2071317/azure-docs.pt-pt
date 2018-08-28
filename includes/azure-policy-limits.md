---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/16/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 0e5d1214a8e1af8299cb40d1a3b31ff6eafc8c5c
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40185235"
---
Existe um número máximo para cada tipo de objeto do Azure Policy. Uma entrada de _Âmbito_ significa a subscrição ou o [grupo de gestão](../articles/azure-resource-manager/management-groups-overview.md).

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
