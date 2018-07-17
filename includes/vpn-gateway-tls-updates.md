---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9a17f34333503436d3da340670abdde154e45ef6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38727535"
---
A partir de 1 de julho de 2018, o suporte será removido para o TLS 1.0 e 1.1 do Gateway de VPN do Azure. O Gateway de VPN irá suportar apenas o TLS 1.2. Para manter o suporte de TLS e a conectividade dos seus clientes de ponto a site do Windows 7 e Windows 8 que utilizam o TLS, recomendamos que instale as seguintes atualizações:

•   [Atualização do Microsoft EAP que permite a utilização de TLS](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

•   [Atualização do TLS 1.1 e TLS 1.2 como protocolos seguros predefinidos em WinHTTP](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

Os seguintes algoritmos legados também vão ser preteridos para TLS a 1 de julho de 2018:

* RC4 (Rivest Cipher 4)
* DES (Algoritmo de Encriptação de Dados)
* 3DES (Algoritmo Triplo de Encriptação de Dados)
* MD5 (Resumo de Mensagens 5)
