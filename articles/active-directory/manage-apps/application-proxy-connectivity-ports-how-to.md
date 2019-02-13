---
title: Como abrir as portas de firewall necessárias para uma aplicação de Proxy de aplicações | Documentos da Microsoft
description: Descubra o que portas abrir para o Proxy de aplicação do Azure AD funcione corretamente
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: efc8c0f0581878d41eb62cbfbd88aec730ecf789
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175263"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Como abrir as portas de firewall necessárias para uma aplicação de Proxy de aplicações

Para ver uma lista completa das portas necessárias e a função de cada porta, consulte a secção pré-requisitos do [documentação de Proxy de aplicações](application-proxy-add-on-premises-application.md). tenha em atenção que o Proxy de aplicações utiliza apenas a portas de saída.

Também pode verificar se tem todas as portas necessárias abrir abrindo o [ferramenta de teste de portas de conector](https://aadap-portcheck.connectorporttest.msappproxy.net/) da sua rede no local. Mais marcas de verificação verde significa maior resiliência. 

## <a name="app-proxy-regions"></a>Regiões de Proxy de aplicações

Estamos a trabalhar numa forma de informá-lo de que estas regiões tem de ser verde para. Por enquanto, certifique-se de que todos eles são. E.U.A. central também é necessário, independentemente de qual é a região.

Para certificar-se de que a ferramenta dá-lhe os resultados certos, certifique-se de que para:

-   Abra a ferramenta num browser a partir do servidor onde instalou o conector.

-   Certifique-se de que quaisquer proxies ou firewalls aplicáveis ao seu conector também são aplicadas a esta página. Isso pode ser feito no Internet Explorer, acedendo a **configurações**  - &gt; **opções da Internet**  - &gt; **ligações**  - &gt; **Definições de Lan**. Nesta página, verá o campo "Utilização de Proxy de servidor para sua LAN". Selecione esta caixa e colocar o endereço de proxy no campo "Address".

## <a name="next-steps"></a>Passos Seguintes
[Compreender os conectores de Proxy de aplicações do Azure AD](application-proxy-connectors.md)
