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
ms.openlocfilehash: b2a96aad793d956b3ea3de06fba74bcf68e50786
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
Pode utilizar um certificado de raiz que tenha sido gerado com uma solução empresarial (recomendado) ou pode gerar um certificado autoassinado. Depois de criar o certificado de raiz, pode exportar os dados do certificado público (não a chave privada) como ficheiro .cer X.509 com codificação Base-64 e carregá-los para o Azure.

* **Certificado empresarial:** se estiver a utilizar uma solução empresarial, pode servir-se da sua cadeia de certificados existente. Obtenha o ficheiro .cer para o certificado de raiz que pretende utilizar.
* **Certificado de raiz autoassinado:** se não estiver a utilizar uma solução de certificação empresarial, tem de criar um certificado de raiz autoassinado. É importante seguir os passos de um dos artigos de certificados P2S abaixo. Caso contrário, os certificados que criar não serão compatíveis com ligações de P2S e os clientes recebem um erro de ligação quando tentarem estabelecer ligação. Pode utilizar o Azure PowerShell, MakeCert ou OpenSSL. Os passos nos artigos fornecidos geram um certificado compatível:

  * [Instruções do Windows 10 PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): estas instruções requerem o Windows 10 e o PowerShell para gerar certificados. Os certificados de cliente gerados a partir do certificado de raiz podem ser instalados em qualquer cliente P2S suportado.
  * [Instruções do MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): utilize o MakeCert se não tiver acesso a um computador com o Windows 10 para gerar certificados. O MakeCert foi preterido, mas ainda o pode utilizar para gerar certificados. Os certificados de cliente gerados a partir do certificado de raiz podem ser instalados em qualquer cliente P2S suportado.
