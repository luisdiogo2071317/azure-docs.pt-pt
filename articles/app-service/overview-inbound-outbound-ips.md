---
title: Endereços de IP de entrada/saída - serviço de aplicações do Azure | Documentos da Microsoft
description: Descreve como entrada e saída IP endereços são utilizados no serviço de aplicações e como encontrar informações sobre as mesmas para a sua aplicação.
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
ms.custom: seodec18
ms.openlocfilehash: 56ca87f318aa5f1843a3b28480be834df1669c71
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811014"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Endereços IP de entrada e saídos no serviço de aplicações do Azure

[Serviço de aplicações do Azure](overview.md) é um serviço de multi-inquilino, exceto para [ambientes do serviço de aplicações](environment/intro.md). Aplicações que não estejam num ambiente de serviço de aplicações (não no [escalão isolado](https://azure.microsoft.com/pricing/details/app-service/)) infraestrutura de rede de partilha com outras aplicações. Como resultado, os endereços IP de entrada e saídos de uma aplicação podem ser diferentes e podem até mesmo alterar em determinadas situações. 

[Ambientes de serviço de aplicações](environment/intro.md) utilizam infraestruturas de rede dedicada, para que as aplicações em execução num ambiente de serviço de aplicações obtém estáticas, endereços IP dedicado para ligações de entrada e saídas.

## <a name="when-inbound-ip-changes"></a>Quando altera o IP de entrada

Independentemente do número de instâncias de escalamento horizontal, cada aplicação tem um único endereço IP de entrada. O endereço IP de entrada podem ser alterados quando executar uma das seguintes ações:

- Eliminar uma aplicação e recrie-a num grupo de recursos diferentes.
- Eliminar a última aplicação num grupo de recursos _e_ combinação da região e recriá-lo.
- Eliminar um enlace SSL existente, tal como durante a renovação de certificado (consulte [renovar certificados](app-service-web-tutorial-custom-ssl.md#renew-certificates)).

## <a name="get-static-inbound-ip"></a>Obter entrado de IP estático

Por vezes, pode desejar um endereço IP estático e dedicado para a sua aplicação. Para obter um endereço IP de entrada estático, tem de configurar uma [enlace SSL baseado em IP](app-service-web-tutorial-custom-ssl.md#bind-your-ssl-certificate). Se não precisar verdadeiramente de uma funcionalidade SSL para proteger a sua aplicação, pode ainda carregar um certificado autoassinado para este enlace. Num enlace SSL baseado em IP, o certificado está vinculado para o endereço IP em si, por isso, provisões de serviço de aplicações um IP estático de endereços para que isso aconteça. 

## <a name="when-outbound-ips-change"></a>Quando alterar a IPs de saída

Independentemente do número de instâncias de escalamento horizontal, cada aplicação tem um número de conjunto de endereços IP de saída num determinado momento. Qualquer ligação de saída a partir da aplicação de serviço de aplicações, como para uma base de dados de back-end, utiliza um dos endereços IP de saída, como o endereço IP de origem. Não é possível saber com antecedência, qual o endereço IP, irá utilizar para fazer a ligação de saída, para que o seu serviço de back-end tem de abrir a firewall para todos os endereços IP de saída da sua aplicação uma instância de determinada aplicação.

O conjunto de IP de saída endereços pela sua aplicação muda quando dimensionar a sua aplicação entre as camadas inferiores (**básica**, **padrão**, e **Premium**) e o  **O Premium V2** escalão.

Pode encontrar o conjunto de todos os possíveis endereços IP de saída pode utilizar a sua aplicação, independentemente de preços escalões, procurando o `possibleOutboundIPAddresses` propriedade. Ver [encontrar IPs de saída](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Encontre os IPs de saída

Para localizar os endereços IP de saída atualmente a ser utilizados pela sua aplicação no portal do Azure, clique em **propriedades** na navegação do lado esquerdo da sua aplicação. 

Pode encontrar as mesmas informações ao executar o seguinte comando na [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).OutboundIpAddresses
```

Para localizar todas as possíveis endereços IP de saída para a sua aplicação, independentemente de preços escalões, execute o seguinte comando [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).PossibleOutboundIpAddresses
```

## <a name="next-steps"></a>Passos Seguintes

Saiba como restringir o tráfego de entrada através de endereços IP de origem.

> [!div class="nextstepaction"]
> [Restrições de IP estáticas](app-service-ip-restrictions.md)
