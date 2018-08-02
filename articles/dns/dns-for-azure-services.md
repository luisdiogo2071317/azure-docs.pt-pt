---
title: Utilizar o DNS do Azure com outros serviços do Azure | Documentos da Microsoft
description: Noções básicas sobre como utilizar o DNS do Azure para resolver o nome para outros serviços do Azure
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: victorh
ms.openlocfilehash: 39fe23d7289dc78736dd5a85d4100af82b1d7b4a
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398220"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Como funciona o DNS do Azure com outros serviços do Azure

O DNS do Azure é um DNS gestão e o nome resolução serviço hospedado. Isto permite-lhe criar nomes DNS públicos para as aplicações e serviços que implementou no Azure. A criação de um nome para um serviço do Azure no seu domínio personalizado é tão simples quanto adicionar um registo do tipo correto para o seu serviço.

* Para endereços IP alocados dinamicamente, tem de criar um registo CNAME de DNS que mapeia para o nome DNS que o Azure criou para o seu serviço. Normas DNS o impede de utilizar um registo CNAME para o vértice da zona.
* Para endereços IP atribuídos estaticamente, pode criar um registo de DNS A utilizar qualquer nome, incluindo uma *domínio sem "www"* nome no vértice da zona.

A tabela seguinte descreve os tipos de registos suportados que podem ser utilizados para vários serviços do Azure. Como pode ver desta tabela, o DNS do Azure só suporta registos DNS para recursos de rede com acesso à Internet. Não é possível utilizar o DNS do Azure para resolução de nomes de endereços internas, privadas.

| Serviço do Azure | Interface de Rede | Descrição |
| --- | --- | --- |
| Gateway de Aplicação |[IP público de front-end](dns-custom-domain.md#public-ip-address) |Pode criar um registo de DNS A ou CNAME. |
| Load balancer |[IP público de front-end](dns-custom-domain.md#public-ip-address)  |Pode criar um registo de DNS A ou CNAME. Balanceador de carga pode ter um endereço IP público IPv6 que é dinamicamente atribuído. Por conseguinte, tem de criar um registo CNAME para um endereço IPv6. |
| Gestor de Tráfego |Nome público |Só pode criar um CNAME que mapeia para o nome de trafficmanager.net atribuído ao perfil do Traffic Manager. Para obter mais informações, consulte [funciona como o Gestor de tráfego](../traffic-manager/traffic-manager-how-it-works.md). |
| Serviço em Nuvem |[IP público](dns-custom-domain.md#public-ip-address) |Para endereços IP atribuídos estaticamente, pode criar um registo DNS. Para endereços IP alocados dinamicamente, tem de criar um registo CNAME que mapeia para o *cloudapp.net* nome.|
| Serviço de Aplicações | [IP externo](dns-custom-domain.md#app-service-web-apps) |Para endereços IP externos, pode criar um registo DNS. Caso contrário, tem de criar um registo CNAME que mapeia para o nome de azurewebsites.net. Para obter mais informações, consulte [mapear um nome de domínio personalizado para uma aplicação do Azure](../app-service/app-service-web-tutorial-custom-domain.md) |
| VMs do Resource Manager |[IP público](dns-custom-domain.md#public-ip-address) |VMs do Resource Manager pode ter endereços IP públicos. Uma VM com um endereço IP público também poderão estar atrás de um balanceador de carga. Pode criar um registo DNS A ou CNAME para o endereço público. Este nome personalizado pode ser utilizado para ignorar o VIP no balanceador de carga. |
| VMs clássicas |[IP público](dns-custom-domain.md#public-ip-address) |As VMs clássicas criado com o PowerShell ou CLI pode ser configurado com um endereço de virtual (reservado) dinâmico ou estático. Pode criar um CNAME DNS ou um registo, respetivamente. |
