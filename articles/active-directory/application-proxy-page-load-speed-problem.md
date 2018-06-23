---
title: Uma aplicação de Proxy da aplicação demora demasiado tempo a carregar | Microsoft Docs
description: Resolver problemas de desempenho de carregamento de página com o Proxy de aplicações do Azure AD
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
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 26acc620184b51719a2ee55b75bd01966d225b8e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36330932"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Uma aplicação de Proxy da aplicação demora demasiado tempo a carregar

Este artigo ajuda-o a compreender por que motivo uma aplicação de Proxy de aplicações do Azure AD pode demorar muito tempo a carregar. Também explica o que pode fazer para resolver este problema.

## <a name="overview"></a>Descrição geral
Embora as aplicações estão a funcionar, pode registam uma latência de comprimento. Poderão existir tweaks de topologia de rede que pode efetuar para melhorar a velocidade. Para uma edição de avaliação das topologias diferentes, consulte o [documento de considerações de rede](manage-apps/application-proxy-network-topology.md).

Para além da topologia de rede, existem atualmente não existem recomendações adicionais para otimização de desempenho. Como o Proxy de aplicações, serviço expande poderá ficar para um centro de dados que esteja fisicamente mais próximo dos. A próximo proximidade pode ajudar a latência. Para obter uma lista dos centros de dados do Azure, consulte o [página de teste de latência](http://www.azurespeed.com/Azure/Latency). 

Os centros de dados com o serviço de Proxy de aplicações podem ser encontrados com o [ferramenta de teste de portas de conector](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Comentários sobre localizações de centro de dados de Proxy de aplicações 
É possível que existam centros de dados do Azure que não ainda incluem o Proxy de aplicações, mas seriam levar a uma melhoria de latência excelente para si. Enviar os dados do Centro de localização para aadapfeedback@microsoft.com. A Microsoft utiliza os seus comentários para planos de expansão.

Microsoft está a trabalhar nas capacidades adicionais para melhorar a latência. Assim que estes melhoramentos estão disponíveis, a documentação será atualizada.

## <a name="next-steps"></a>Passos Seguintes
[Trabalhar com servidores de proxy no local existentes](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
