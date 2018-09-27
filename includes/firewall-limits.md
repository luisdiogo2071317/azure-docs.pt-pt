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
ms.openlocfilehash: 0a4a66543b323b1dbe56158851dafcb1dfe695f0
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47186255"
---
| Recurso | Limite predefinido |
| --- | --- |
| Dados processados |TB/firewall/mês, 1000 <sup>1</sup> |
|Regras|regras de aplicações de k de 10, as regras de rede de 10 mil|
|VNet peering|Para implementações de hub- and -spoke, Máx. de VNETs spoke de 50.|
|Global peering|Não suportado. Deve ter, pelo menos, uma implementação de firewall por região.|
|Máximas portas numa regra de rede único|15<br>Um intervalo de portas (por exemplo: 10 de 2) é contabilizado como dois.


<sup>1</sup> contacte o suporte do Azure no caso de precisa de aumentar estes limites.
