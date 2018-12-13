---
title: Restringir o cliente IPs - serviço de aplicações do Azure | Documentos da Microsoft
description: Como utilizar as restrições de IP com o serviço de aplicações do Azure
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 7/30/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 01c948bbfd1883e24dc161d46ba53aef42af0f1d
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250705"
---
# <a name="azure-app-service-static-ip-restrictions"></a>Restrições de IP estático do serviço de aplicações do Azure #

Restrições de IP permitem-lhe definir uma prioridade ordenadas permitir/recusar a lista de endereços IP que têm permissão para aceder à sua aplicação. Lista de permissões pode incluir endereços IPv4 e IPv6. Quando há uma ou mais entradas, existe, em seguida, um implícita Negar tudo o que existe no final da lista. 

A capacidade de restrições de IP funciona com todos os que serviço de aplicações alojadas cargas de trabalho, que incluem; aplicações Web, aplicações api, as aplicações do linux, aplicações de contentor do linux e as funções. 

Quando é efetuado um pedido à sua aplicação, o endereço de IP é avaliado relativamente a lista de restrições de IP. Se o endereço não é permitido o acesso com base nas regras na lista, o serviço responde com uma [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) código de estado.

A capacidade de restrições de IP é implementada nas funções de front-end de serviço de aplicações, que são a montante, os anfitriões de trabalho em que o seu código é executado. Por conseguinte, as restrições de IP são, efetivamente, ACLs de rede.  

![Fluxo de restrições de IP](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Por um período, a capacidade de restrições de IP no portal do era uma camada sobre a capacidade de ipSecurity no IIS. A capacidade de restrições de IP atual é diferente. Ainda é possível configurar ipSecurity dentro de sua Web. config do aplicativo, mas as regras de restrições de IP Front-end com base serão aplicadas antes que qualquer tráfego atinja o IIS.

## <a name="adding-and-editing-ip-restriction-rules-in-the-portal"></a>Adicionar e editar regras de restrição de IP no portal ##

Para adicionar uma regra de restrição de IP para a sua aplicação, utilize o menu para abrir **rede**>**restrições de IP** e clique em **configurar restrições de IP**

![Opções de rede do serviço de aplicações](media/app-service-ip-restrictions/ip-restrictions.png)  

A partir da interface do Usuário de restrições de IP, pode rever a lista de regras de restrição de IP definidas para a sua aplicação.

![restrições de IP da lista](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Se as regras foram configuradas conforme esta imagem, a aplicação só seria aceitar o tráfego de 131.107.159.0/24 e poderia ser negada a partir de qualquer outro endereço IP.

Pode clicar em **[+] adicionar** para adicionar uma nova regra de restrição de IP. Depois de adicionar uma regra, ele entrarão em vigor imediatamente. As regras são impostas por ordem de prioridade a partir do número mais baixo e a aumentar. Há uma implícita Negar tudo o que está em vigor depois de adicionar até mesmo uma única regra. 

![Adicionar uma regra de restrição de IP](media/app-service-ip-restrictions/ip-restrictions-add.png)

A notação de endereço IP tem de ser especificada na notação CIDR para endereços IPv4 e IPv6. Para especificar um endereço exato, pode usar algo como 1.2.3.4/32 onde os quatro primeiros octetos representam o seu endereço IP e /32 é a máscara. A notação de CIDR de IPv4 para todos os endereços é 0.0.0.0/0. Para saber mais sobre a notação CIDR, leia [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  

Pode clicar em qualquer linha para editar uma regra de restrição de IP existente. As edições entram em vigor de imediato, incluindo alterações na ordem de prioridade.

![Editar uma regra de restrição de IP](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Para eliminar uma regra, clique no **...**  na sua regra e clique em **remover**. 

![Eliminar regra de restrição de IP](media/app-service-ip-restrictions/ip-restrictions-delete.png)

## <a name="programmatic-manipulation-of-ip-restriction-rules"></a>Manipulação programática de regras de restrição de IP ##

Atualmente não há CLI ou PowerShell para a nova capacidade de restrições de IP, mas os valores podem ser definidos manualmente, com uma operação PUT na configuração da aplicação no Gestor de recursos. Por exemplo, pode utilizar resources.azure.com e edite o bloco de ipSecurityRestrictions para adicionar o JSON necessário. 

A localização para estas informações no Gestor de recursos é:

Management.Azure.com/subscriptions/**ID de subscrição**/resourceGroups/**grupos de recursos**/providers/Microsoft.Web/sites/**nome da aplicação web**  /config/web? a API-version = 2018-02-01

A sintaxe JSON de exemplo anterior é:

    "ipSecurityRestrictions": [
      {
        "ipAddress": "131.107.159.0/24",
        "action": "Allow",
        "tag": "Default",
        "priority": 100,
        "name": "allowed access"
      }
    ],
