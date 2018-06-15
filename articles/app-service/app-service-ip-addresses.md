---
title: Endereços IP no App Service do Azure | Microsoft Docs
description: Descreve como entrada e saída IP endereços são utilizados no serviço de aplicações e como localizar as informações nos mesmos para a sua aplicação.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: cephalin
ms.openlocfilehash: 906a5d511615c57b6ff807ac240a838c63917e66
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31789946"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Endereços IP de entrada e saídos no App Service do Azure

[App Service do Azure](app-service-web-overview.md) é um serviço de multi-inquilino, exceto para [ambientes do App Service](environment/intro.md). Aplicações que não estejam num ambiente de serviço de aplicações (não o [camada isolada](https://azure.microsoft.com/pricing/details/app-service/)) infraestrutura de rede de partilha com outras aplicações. Como resultado, os endereços IP de entrada e saídos de uma aplicação podem ser diferentes e podem alterar, mesmo em determinadas situações. 

[Ambientes do App Service](environment/intro.md) utilizam infraestruturas de rede dedicado, para aplicações em execução num ambiente de serviço de aplicações obter estáticas, endereços IP dedicado para ligações de entrada e saídas.

## <a name="when-inbound-ip-changes"></a>Quando altera a entrada IP

Independentemente do número de instâncias de escalamento horizontal, cada aplicação tem um único endereço IP de entrada. O endereço IP de entrada podem ser alterados quando efetuar uma das seguintes ações:

- Eliminar uma aplicação e recriá-la num grupo de recursos diferentes.
- Eliminar a última aplicação num grupo de recursos _e_ combinação de região e recriá-la.
- Eliminar um enlace SSL existente, tal como durante a renovação de certificado (consulte [renovar certificados](app-service-web-tutorial-custom-ssl.md#renew-certificates)).

## <a name="get-static-inbound-ip"></a>Obter a entrada de IP estático

Por vezes, é aconselhável um endereço IP estático, dedicado para a sua aplicação. Para obter um endereço IP estático entrado, terá de configurar um [enlace SSL baseado em IP](app-service-web-tutorial-custom-ssl.md#bind-your-ssl-certificate). Se, na verdade, não precisa de funcionalidade SSL para proteger a sua aplicação, pode carregar um certificado autoassinado para este enlace mesmo. Num enlace SSL baseado em IP, o certificado está vinculado para o endereço IP, por isso, Aprovisiona do serviço de aplicações um IP estático de endereços para torná-lo a acontecer. 

## <a name="when-outbound-ips-change"></a>Quando alterar IPs de saída

Independentemente do número de instâncias de escalamento horizontal, cada aplicação tem um número de conjunto de endereços IP saídos em qualquer momento. Qualquer ligação de saída da aplicação do serviço de aplicações, tais como uma base de dados de back-end, utiliza um dos endereços IP saídos como o endereço IP de origem. Não é possível saber antecipadamente qual o endereço IP uma instância de aplicações irá utilizar para efetuar a ligação de saída, pelo que o serviço de back-end tem de abrir a firewall para todos os endereços IP saídos da sua aplicação.

O conjunto de IP saída endereços para as alterações de aplicação ao dimensionar a sua aplicação entre as camadas inferiores (**básico**, **padrão**, e **Premium**) e o  **Premium V2** camada.

Pode encontrar o conjunto de todos os possíveis saídos endereços IP pode utilizar a sua aplicação, independentemente de preços camadas, procurando o `possibleOutboundIPAddresses` propriedade. Consulte [localizar IPs saída](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Localizar os IPs de saída

Para obter os endereços IP saídos atualmente a ser utilizados pela sua aplicação no portal do Azure, clique em **propriedades** na navegação esquerda da sua aplicação. 

Pode encontrar as mesmas informações executando o seguinte comando no [nuvem Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

Para encontrar possíveis todos os endereços IP de saída para a sua aplicação, independentemente de preços camadas, execute o seguinte comando [nuvem Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

## <a name="next-steps"></a>Passos Seguintes

Saiba como restringir o tráfego de entrada por endereços IP de origem.

> [!div class="nextstepaction"]
> [Restrições de IP estáticas](app-service-ip-addresses.md)
