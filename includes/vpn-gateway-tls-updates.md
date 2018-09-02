---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f2d7aba05fc01c5a4dcdb123f25242c4e4a72578
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "43426433"
---
>[!NOTE]
>A partir de 1 de julho de 2018, o suporte será removido para o TLS 1.0 e 1.1 do Gateway de VPN do Azure. O Gateway de VPN irá suportar apenas o TLS 1.2. Para manter o suporte, consulte a [atualizações para ativar o suporte para TLS1.2](#tls1).

Além disso, os seguintes algoritmos de legado também vão ser preteridos para TLS no dia 1 de Julho de 2018:

* RC4 (Rivest Cipher 4)
* DES (Algoritmo de Encriptação de Dados)
* 3DES (Algoritmo Triplo de Encriptação de Dados)
* MD5 (Resumo de Mensagens 5)

### <a name="tls1"></a>Como posso ativar o suporte para TLS 1.2 no Windows 7 e Windows 8.1?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
