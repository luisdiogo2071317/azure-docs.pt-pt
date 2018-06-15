---
title: Instalar um certificado de cliente P2S | Azure
description: Instale um certificado de cliente Mac ou o Windows para autenticação de certificados P2S.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager, azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: cherylmc
ms.openlocfilehash: bf2788fff64ab8b3a5ccf75b8a80f2bd5aba5151
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
ms.locfileid: "30317998"
---
# <a name="install-a-client-certificate-for-point-to-site-azure-certificate-authentication-connections"></a>Instalar um certificado de cliente para ligações de autenticação de certificado do Azure de ponto a Site

Todos os clientes que se ligam a uma rede virtual com a autenticação de certificados do Azure de ponto a Site requerem um certificado de cliente. Este artigo ajuda-o a instalar um certificado de cliente que é utilizado para autenticação ao ligar a uma VNet com P2S.

## <a name="generate"></a>Gerar e exportar um certificado de cliente

Pode gerar um certificado de cliente a partir de um certificado de raiz que tenha sido criado através de uma solução de AC empresarial ou um certificado de raiz autoassinado. Consulte o [PowerShell](vpn-gateway-certificates-point-to-site.md) ou [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) instruções para obter os passos. Depois de gerar os certificados de cliente, exportá-las como ficheiros. pfx. Lembre-se de que incluem a cadeia de certificados inteira ao exportar.

## <a name="installwin"></a>Instalar certificado - Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Instalar certificado - Mac

Os clientes Mac VPN são suportados para apenas o modelo de implementação do Resource Manager. Não são suportadas para o modelo de implementação clássica.

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="next-steps"></a>Passos Seguintes

Continue com os passos de configuração de ponto a Site.

* [Portal do Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Portal do Azure (clássico)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
