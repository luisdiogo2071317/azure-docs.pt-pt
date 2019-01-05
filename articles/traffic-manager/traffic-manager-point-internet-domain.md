---
title: Apontar um domínio de Internet da empresa para um nome de domínio do Gestor de tráfego do Azure | Documentos da Microsoft
description: Este artigo ajuda-o a apontar o nome de domínio da sua empresa para um nome de domínio do Traffic Manager.
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 2b9b718a61d13790de38ea82f9b479e372e9de63
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052688"
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Apontar um domínio de Internet da empresa para um domínio do Gestor de Tráfego do Azure

Quando cria um perfil do Gestor de Tráfego, o Azure atribui automaticamente um nome DNS a esse perfil. Para utilizar um nome da sua zona DNS, crie um registo DNS CNAME que mapeia para o nome de domínio do seu perfil do Gestor de Tráfego. Pode encontrar o nome de domínio do Gestor de Tráfego na secção **Geral** na página Configuração do perfil do Gestor de Tráfego.

Por exemplo, para apontar o nome `www.contoso.com` para o nome de DNS do Gestor de Tráfego `contoso.trafficmanager.net`, cria o registo de recursos de DNS seguinte:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Todos os pedidos de tráfego para *www.contoso.com* são direcionados para *contoso.trafficmanager.net*.

> [!IMPORTANT]
> Não é possível apontar um domínio de segundo nível, como *contoso.com*, para o domínio do Traffic Manager. Os padrões de protocolo DNS não permitem registos CNAME para nomes de domínio de segundo nível.

## <a name="next-steps"></a>Passos Seguintes

* [Métodos de encaminhamento do Gestor de Tráfego](traffic-manager-routing-methods.md)
* [Gestor de Tráfego – Desativar, ativar ou eliminar um perfil](disable-enable-or-delete-a-profile.md)
* [Gestor de Tráfego – Desativar ou ativar um ponto final](disable-or-enable-an-endpoint.md)
