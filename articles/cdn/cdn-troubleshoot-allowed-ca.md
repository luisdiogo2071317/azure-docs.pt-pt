---
title: Permitido autoridades de certificação para ativar o HTTPS personalizado na CDN do Azure | Documentos da Microsoft
description: Se estiver a utilizar o seu próprio certificado para ativar o HTTPS num domínio personalizado, tem de utilizar uma autoridade de certificados permitidos (AC) para criá-lo.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: e9d9f5b053537b3d6881763bcb39d7894ba78869
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969514"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Permitido autoridades de certificação para ativar o HTTPS personalizado na CDN do Azure

Para um domínio personalizado da rede de entrega conteúdo (CDN) numa **CDN Standard do Microsoft Azure** ponto final, quando [ativar a funcionalidade HTTPS utilizando o seu próprio certificado](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), tem de utilizar um permitidos autoridade de certificação (AC) para criar o seu certificado SSL. Caso contrário, se utilizar uma AC não permitido ou um certificado autoassinado, sua solicitação será rejeitada.

> [!NOTE]
> A opção de utilizar o seu próprio certificado para ativar o HTTPS personalizados está disponível apenas com **CDN Standard do Microsoft Azure** perfis. 
>

As CAs seguintes são permitidas quando cria seu próprio certificado:

- AddTrust externo AC de raiz
- AC de raiz de AlphaSSL
- Infra-estrutura AME 01 de AC
- Infra-estrutura AME AC 02
- Ameroot
- AC de raiz do Pacífico
- Autoridade de certificação de raiz de AP 2013
- Autoridade de certificação de raiz de AP de 2014
- APCA DM3P
- AC de raiz do autopilot
- Raiz da CyberTrust Baltimore
- Autoridade de certificação primária pública classe 3
- Autoridade de certificação COMODO RSA
- Servidor seguro do COMODO RSA domínio validação AC
- AC de classe 3 de raiz de confiança de D de 2009 2
- Serviços em nuvem da DigiCert no AC-1
- AC de raiz Global DigiCert
- DigiCert alta garantia AC-3
- AC de raiz de Assurance EV DigiCert alta
- DigiCert SHA2 estendido AC do servidor de validação
- Servidor de alta garantia de DigiCert SHA2 AC
- Servidor seguro da DigiCert SHA2 AC
- X3 de AC de raiz de horário de Verão
- AC de classe 3 de raiz de confiança de D de 2009 2
- Encriptação em qualquer lugar DV TLS AC
- Autoridade de certificação de raiz da Entrust
- G2 da autoridade de certificação de raiz - Entrust
- Autoridade de certificação Entrust.NET (2048)
- AC de GeoTrust Global
- Autoridade de certificação primária GeoTrust
- Autoridade de certificação primária GeoTrust - G2
- Geotrust RSA 2018 de AC
- GlobalSign
- GlobalSign estendido G2 de AC - SHA256 - validação
- Validação de organização GlobalSign AC - G2
- AC de raiz de GlobalSign
- Aceda a autoridade de certificação de raiz Daddy - G2
- Aceda a autoridade de certificação segura de Daddy - G2
- Autoridade de raiz de Authenticode(tm) da Microsoft
- Serviços do Microsoft Exchange 2015 de AC
- Microsoft de raiz empresarial interna
- A TI da Microsoft ITO SSL AC 1
- SHA1 SSL de TI da Microsoft
- A TI da Microsoft SSL SHA2
- A TI da Microsoft TLS AC 1
- A TI da Microsoft TLS AC 2
- A TI da Microsoft TLS AC 4
- A TI da Microsoft TLS AC 5
- Autoridade de raiz da Microsoft
- Autoridade de certificação de raiz da Microsoft
- Autoridade de certificação de raiz Microsoft 2010
- Autoridade de certificação de raiz Microsoft 2011
- Servidor seguro da Microsoft 2011 de AC
- Raiz de parceiro de serviços da Microsoft
- Raiz do serviço de carimbo de data / hora da Microsoft
- Compatibilidade de Hardware do Windows da Microsoft
- MSIT Z2 DE AC
- AC empresarial do MSIT 1
- AC empresarial do MSIT 3
- RapidSSL RSA 2018 de AC
- Agência de raiz
- AC de SSL do Symantec classe 3 EV - G3
- Classe da Symantec 3 servidor seguro AC - G4
- Symantec Enterprise Mobile Root para a Microsoft
- AC de raiz de Thawte primário
- AC de raiz do principal de Thawte - G2
- AC de raiz do principal de Thawte - G3
- Thawte RSA 2018 de AC
- Carimbo de Thawte AC
- TrustAsia TLS RSA AC
- Objeto de USERFirst UTN
- AC de SSL de validação estendida 3 classe VeriSign
- Classe de VeriSign 3 validação estendida SSL SGC AC
- Autoridade de certificação primária pública classe 3 da VeriSign - G5
- Servidor internacional da VeriSign AC - classe 3
- Raiz do serviço de carimbo de data / hora da VeriSign
- Autoridade de certificação de raiz de VeriSign Universal
- WMSvc-SHA2-DALEDGE1008

