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
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Posso utilizar AC de raiz de PKI interna para a conetividade Ponto a Site?

Sim. Anteriormente, só podiam ser utilizados os certificados de raiz autoassinados. Ainda pode carregar 20 certificados de raiz.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Ferramentas de que pode utilizar para criar certificados?

Pode utilizar a sua solução de PKI empresarial (a PKI interna), do Azure PowerShell, MakeCert e OpenSSL.

### <a name="certsettings"></a>Existem instruções para os parâmetros e definições de certificado?

* **Solução PKI/Enterprise PKI interna:** consulte os passos para [gerar certificados](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **O Azure PowerShell:** consulte o [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) artigo para obter os passos.

* **MakeCert:** consulte o [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) artigo para obter os passos.

* **OpenSSL:** 

    * Ao exportar certificados, lembre-se de que converter o certificado de raiz para Base64.

    * Para o certificado de cliente:

      * Ao criar a chave privada, especifique o comprimento como 4096.
      * Ao criar o certificado para o *-extensões* parâmetro, especifique *usr_cert*.