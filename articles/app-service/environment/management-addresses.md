---
title: Endereços de gestão de ambiente de serviço de aplicações - Azure
description: Apresenta uma lista de endereços de gestão utilizados para um ambiente de serviço de aplicações de comando
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 7fb39886b19a2229188821eb39d4fb8a5928bb43
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276694"
---
# <a name="app-service-environment-management-addresses"></a>Endereços de gestão do ambiente de serviço de aplicações

O ambiente de serviço de aplicações (ASE) é uma implementação do serviço de aplicações do Azure numa sub-rede na rede Virtual do Azure (VNet).  O ASE tem de ser acessível a partir do plano de gestão utilizado pelo serviço de aplicações do Azure.  Este tráfego de gestão do ASE atravessa a rede controlada pelo utilizador. Se este tráfego é bloqueado ou misrouted, o ASE irá tornar-se suspensa. Para obter detalhes sobre as dependências de rede do ASE, leia [considerações e o ambiente de serviço de aplicações de rede][networking]. Para obter informações gerais sobre o ASE, pode começar com [introdução ao ambiente de serviço de aplicações][intro].

Este documento apresenta uma lista os endereços de origem do serviço de aplicações para o tráfego de gestão para o ASE e tem duas finalidades importantes.  

1. Pode utilizar estes endereços para criar grupos de segurança de rede para bloquear o tráfego de entrada.  
2. Pode criar rotas com estes endereços para suportar implementações de túnel forçado. Para obter detalhes sobre como configurar o ASE para operar num ambiente em que o tráfego de saída é enviado no local, leia [configurar o ASE com túnel forçado][forcedtunnel]

Todos os ASEs têm um VIP público, o tráfego de gestão entra em. O tráfego de gestão entrada nestes endereços vêm de portas 454 e 455 no VIP público do seu ASE.  

## <a name="list-of-management-addresses"></a>Lista de endereços de gestão ##

| Região | Endereços |
|--------|-----------|
| Todas as regiões públicas | 70.37.57.58, 157.55.208.185, 52.174.22.21, 13.94.149.179, 13.94.143.126, 13.94.141.115, 52.178.195.197, 52.178.190.65, 52.178.184.149, 52.178.177.147, 13.75.127.117, 40.83.125.161, 40.83.121.56, 40.83.120.64, 52.187.56.50, 52.187.63.37, 52.187.59.251, 52.187.63.19, 52.165.158.140, 52.165.152.214, 52.165.154.193, 52.165.153.122, 104.44.129.255, 104.44.134.255, 104.44.129.243, 104.44.129.141, 23.102.188.65, 191.236.154.88, 13.64.115.203, 65.52.193.203, 70.37.89.222, 52.224.105.172, 23.102.135.246 |
| Microsoft Azure Government (Fairfax ou MAG) | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="get-your-management-addresses-from-api"></a>Obtenha os endereços de gestão de API ##

Pode listar os endereços de gestão que correspondem ao seu ASE com a seguinte chamada à API.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

A API devolve um documento JSON que inclui todos os endereços de entrada para o seu ASE. A lista de endereços inclui os endereços de gestão, o VIP utilizado pelo seu ASE e o intervalo de endereços de sub-rede do ASE em si.  

Para chamar a API com o [armclient](https://github.com/projectkudu/ARMClient) utilize os seguintes comandos, mas substitua no seu ID de subscrição, o grupo de recursos e o nome do ASE.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
