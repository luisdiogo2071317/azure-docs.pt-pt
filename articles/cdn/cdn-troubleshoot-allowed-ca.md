---
title: Permitido autoridades de certificação para ativar HTTPS personalizada no Azure CDN | Microsoft Docs
description: Se estiver a utilizar o seu próprio certificado para permitir HTTPS um domínio personalizado, tem de utilizar uma autoridade de certificados permitidos (AC) para criá-la.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: 3c41ca7e375324ff784bf7bee347bb56400ddfbd
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238300"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Permitido autoridades de certificação para ativar HTTPS personalizado na CDN do Azure

Para um domínio personalizado de Azure rede de entrega conteúdos (CDN) num **CDN do Azure Standard da Microsoft** ponto final, quando é [ativar a funcionalidade HTTPS utilizando o seu próprio certificado](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), tem de utilizar um permitido autoridade de certificação (AC) para criar o certificado SSL. Caso contrário, se utilizar uma AC não permitido ou um certificado autoassinado, será rejeitado o pedido.

> [!NOTE]
> Está disponível apenas com a opção de utilizar o seu próprio certificado para permitir HTTPS personalizado **CDN do Azure Standard da Microsoft** perfis. 
>

As ACs seguintes são permitidas quando criar o seu próprio certificado:

- AddTrust externo AC raiz
- AC de raiz da Ásia-Pacífico
- Autoridade de certificação de raiz da Ásia-Pacífico 2013
- Autoridade de certificação de raiz da Ásia-Pacífico 2014
- APCA DM3P
- AC de raiz autopilot
- Root da CyberTrust Baltimore
- Classe 3 primário autoridade de certificação pública
- Autoridade de certificação COMODO RSA
- Servidor seguro do COMODO RSA domínio validação AC
- Raiz de confiança D classe 3 AC 2 2009
- 1 de AC dos serviços de nuvem DigiCert
- AC de raiz Global DigiCert
- Alta DigiCert garantia AC-3
- AC de raiz de garantia EV alta DigiCert
- Servidor de certeza alta DigiCert SHA2 AC
- Servidor segura de DigiCert SHA2 AC
- GlobalSign
- GlobalSign expandido validação AC - SHA256 - G2
- Validação de organização GlobalSign AC - G2
- AC de raiz GlobalSign
- Autoridade de raiz Authenticode(tm) da Microsoft
- Serviços do Microsoft Exchange AC 2015
- Raiz empresarial interna Microsoft
- SSL de ITO de TI da Microsoft AC 1
- SHA1 SSL de TI da Microsoft
- SHA2 SSL de TI da Microsoft
- CA de TLS de TI da Microsoft 1
- CA de TLS de TI da Microsoft 2
- CA de TLS de TI da Microsoft 4
- CA de TLS de TI da Microsoft 5
- Autoridade de raiz da Microsoft
- Autoridade de certificação de raiz da Microsoft
- Autoridade de certificação de raiz Microsoft 2010
- Autoridade de certificação de raiz Microsoft 2011
- Servidor seguro de Microsoft 2011 de AC
- Serviços da Microsoft do parceiro raiz
- Raiz do serviço de carimbo de data / hora da Microsoft
- Compatibilidade de Hardware do Microsoft Windows
- MSIT Z2 DE AC
- AC empresarial do MSIT 1
- AC empresarial do MSIT 3
- Agência de raiz
- AC de SSL de 3 EV Symantec classe - G3
- Classe da Symantec 3 servidor seguro AC - G4
- Symantec Enterprise Mobile Root para Microsoft
- Thawte Timestamping AC
- Objeto de USERFirst UTN
- Classe de VeriSign 3 validação expandida SSL AC
- Classe de VeriSign 3 validação expandida SSL SGC AC
- Classe de VeriSign 3 primário autoridade de certificação pública - G5
- Servidor de internacional VeriSign AC - classe 3
- Raiz do serviço de carimbo de data / hora VeriSign
- Autoridade de certificação de raiz VeriSign Universal
- WMSvc-SHA2-DALEDGE1008

