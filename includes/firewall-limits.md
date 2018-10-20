---
title: incluir ficheiro
description: incluir ficheiro
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 7/30/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: f1fd32fc02bee47d229093680166e6a13a5ba721
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "49458849"
---
| Recurso | Limite predefinido |
| --- | --- |
| Dados processados |TB/firewall/mês, 1000 <sup>1</sup> |
|Regras|10 mil - tudo regra combinado de tipos|
|VNet peering|Para implementações de hub- and -spoke, Máx. de VNETs spoke de 50.|
|Global peering|Não suportado. Deve ter, pelo menos, uma implementação de firewall por região.|
|Máximas portas numa regra de rede único|15<br>Um intervalo de portas (por exemplo: 10 de 2) é contabilizado como dois.


<sup>1</sup> contacte o suporte do Azure no caso de precisa de aumentar estes limites.
