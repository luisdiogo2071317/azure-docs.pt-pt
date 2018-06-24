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
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36336031"
---
A partir de 1 de Julho de 2018, está a ser removido suporte para TLS 1.0 e 1.1 do Gateway de VPN do Azure. Gateway de VPN irá suportar apenas TLS 1.2. Para manter a conectividade e suporte TLS para os seus clientes de ponto a site do Windows 7 e Windows 8 que utilizam o TLS, recomendamos que instale as seguintes atualizações:

• [Atualização para a implementação Microsoft EAP que permite a utilização de TLS](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

• [Atualização para ativar o TLS 1.1 e TLS 1.2 como protocolos segura de predefinição na WinHTTP](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

Os algoritmos seguintes legados também vão ser preteridos para TLS no dia 1 de Julho de 2018:

* RC4 (Rivest cifras 4)
* DES (algoritmo de encriptação de dados)
* 3DES (algoritmo de encriptação de dados tripla)
* MD5 (mensagem de texto implícita 5)
