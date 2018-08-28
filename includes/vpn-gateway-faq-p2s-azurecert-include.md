---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 97d33bfcc8251b10ba121b7fb013800904450563
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "30197134"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Posso utilizar AC de raiz de PKI interna para a conetividade Ponto a Site?

Sim. Anteriormente, só podiam ser utilizados os certificados de raiz autoassinados. Ainda pode carregar 20 certificados de raiz.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Que ferramentas posso utilizar para criar certificados?

Pode utilizar a sua solução de PKI de Empresa (o PKI interno), o Azure PowerShell, MakeCert e OpenSSL.

### <a name="certsettings"></a>Existem instruções para parâmetros e definições de certificado?

* **Solução PKI de Empresa/Interno:** Veja os passos para [Gerar certificados](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Azure PowerShell:** Veja o artigo [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) para saber quais os passos a seguir.

* **MakeCert:** Veja o artigo [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) para saber quais os passos a seguir.

* **OpenSSL:** 

    * Ao exportar certificados, certifique-se de que converte o certificado de raiz para Base64.

    * Para o certificado de cliente:

      * Ao criar a chave privada, especifique o comprimento como 4096.
      * Ao criar o certificado para o parâmetro *-extensions*, especifique *usr_cert*.