---
title: incluir ficheiro
description: incluir ficheiro
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 11/26/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 8b236bc1f0089b89aca90e7c69e9b445b01a374b
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440358"
---
| Recurso | Limite predefinido |
| --- | --- |
| Dados processados |TB/firewall/mês, 1000 <sup>1</sup> |
|Regras|10 mil - tudo regra combinado de tipos|
|Global peering|Não suportado. Deve ter, pelo menos, uma implementação de firewall por região.|
|Máximas portas numa regra de rede único|15<br>Um intervalo de portas (por exemplo: 10 de 2) é contabilizado como dois.
|Tamanho mínimo de AzureFirewallSubnet |/26


<sup>1</sup> contacte o suporte do Azure no caso de precisa de aumentar estes limites.
