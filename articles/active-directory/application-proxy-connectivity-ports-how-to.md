---
title: Como abrir as portas de firewall necessárias para uma aplicação de Proxy de aplicações | Microsoft Docs
description: Determinar que portas abertas para o Proxy de aplicações do Azure AD funcione corretamente
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: bc1ed4a0b49588bd9828bd64508caa50e92c304a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335677"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Como abrir as portas de firewall necessárias para uma aplicação de Proxy de aplicações

Para ver uma lista completa das portas necessárias e a função de cada porta, consulte a secção pré-requisitos do [documentação do Proxy de aplicações](manage-apps/application-proxy-enable.md). tenha em atenção que o Proxy de aplicações utiliza apenas as portas de saída.

Também pode verificar se tem todas as portas necessárias abrir abrindo o [ferramenta de teste de portas de conector](https://aadap-portcheck.connectorporttest.msappproxy.net/) da sua rede no local. Marcas de verificação verde mais significa maior resiliência. 

## <a name="app-proxy-regions"></a>Regiões do Proxy de aplicação

Estamos a trabalhar de forma para informá-lo a que estas regiões tem de ser verde para si. Por agora, certifique-se de que todos eles são. E.u. a central também for necessária, independentemente da que região estiver no.

Para certificar-se de que a ferramenta dá-lhe os resultados da direita, não se esqueça de:

-   Abra a ferramenta num browser a partir do servidor onde instalou o conector.

-   Certifique-se de que quaisquer proxies ou firewalls aplicáveis ao seu conector também são aplicadas a esta página. Isto pode ser feito no Internet Explorer, acedendo a **definições**  - &gt; **opções da Internet**  - &gt; **ligações**  - &gt; **Definições de Lan**. Nesta página, verá o campo "Utilizar um Proxy de servidor para o LAN". Selecione esta caixa e colocar o endereço proxy no campo "Address".

## <a name="next-steps"></a>Passos Seguintes
[Compreender os conectores de Proxy de aplicações do Azure AD](manage-apps/application-proxy-connectors.md)
