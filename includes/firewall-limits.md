---
title: incluir ficheiro
description: incluir ficheiro
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 12/14/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 7d905550114bb76a0a091146b3972bab4a652022
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430025"
---
| Recurso | Limite predefinido |
| --- | --- |
| Dados processados |TB/firewall/mês, 1000 <sup>1</sup> |
|Regras|10 mil - tudo regra combinado de tipos|
|Global peering|Não suportado. Deve ter, pelo menos, uma implementação de firewall por região.|
|Máximas portas numa regra de rede único|15<br>Um intervalo de portas (por exemplo: 2 - 10) é contabilizado como dois.
|Tamanho mínimo de AzureFirewallSubnet |/26|
|Intervalo de portas nas regras de rede e da aplicação|0-64, 000. Trabalho está em curso para reduzir esta limitação.|
|


<sup>1</sup> contacte o suporte do Azure no caso de precisa de aumentar estes limites.
