---
title: Permitido autoridades de certificação para ativar o HTTPS personalizado no serviço de porta de entrada do Azure | Documentos da Microsoft
description: Se estiver a utilizar o seu próprio certificado para ativar o HTTPS num domínio personalizado, tem de utilizar uma autoridade de certificados permitidos (AC) para criá-lo.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: sharadag
ms.openlocfilehash: de709133099674a0aa0386113b6459f8bc05e378
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047868"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door-service"></a>Permitido autoridades de certificação para ativar o HTTPS personalizado no serviço de porta de entrada do Azure

Para um domínio personalizado de serviço de porta de entrada do Azure, quando [ativar a funcionalidade HTTPS utilizando o seu próprio certificado](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), tem de utilizar uma autoridade de certificados permitidos (AC) para criar o seu certificado SSL. Caso contrário, se utilizar uma AC não permitido ou um certificado autoassinado, sua solicitação será rejeitada.

As CAs seguintes são permitidas quando cria seu próprio certificado:

- AddTrust externo AC de raiz
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
- Servidor de alta garantia de DigiCert SHA2 AC
- Servidor seguro da DigiCert SHA2 AC
- AC de GeoTrust Global
- Autoridade de certificação primária GeoTrust
- Autoridade de certificação primária GeoTrust - G2
- GlobalSign
- GlobalSign estendido G2 de AC - SHA256 - validação
- Validação de organização GlobalSign AC - G2
- AC de raiz de GlobalSign
- Aceda a autoridade de certificação de raiz Daddy - G2
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
- Agência de raiz
- AC de SSL do Symantec classe 3 EV - G3
- Classe da Symantec 3 servidor seguro AC - G4
- Symantec Enterprise Mobile Root para a Microsoft
- AC de raiz de Thawte primário
- AC de raiz do principal de Thawte - G2
- AC de raiz do principal de Thawte - G3
- Carimbo de Thawte AC
- Objeto de USERFirst UTN
- AC de SSL de validação estendida 3 classe VeriSign
- Classe de VeriSign 3 validação estendida SSL SGC AC
- Autoridade de certificação primária pública classe 3 da VeriSign - G5
- Servidor internacional da VeriSign AC - classe 3
- Raiz do serviço de carimbo de data / hora da VeriSign
- Autoridade de certificação de raiz de VeriSign Universal
- WMSvc-SHA2-DALEDGE1008
