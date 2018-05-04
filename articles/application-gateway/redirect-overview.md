---
title: Descrição geral do redirecionamento para o Gateway de aplicação do Azure
description: Saiba mais sobre a capacidade de redirecionamento no Gateway de aplicação do Azure
services: application-gateway
documentationcenter: na
author: amsriva
manager: timlt
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/19/2018
ms.author: amsriva
ms.openlocfilehash: f503998668f35ec5bc17db74ee74c4a26465ab04
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="application-gateway-redirect-overview"></a>Descrição geral de redirecionamento do Gateway de aplicação

É um cenário comum para muitas aplicações web para suportar HTTP automática para o redirecionamento de HTTPS para garantir que todas as comunicações entre aplicações e os seus utilizadores ocorre através de um caminho encriptado. No passado, os clientes utilizou técnicas, como criar um conjunto de back-end dedicado cujo objetivo único é redirecionar pedidos a receber de HTTP para HTTPS.

Gateway de aplicação suporta agora a capacidade de redirecionar o tráfego no gateway. Isto simplifica a configuração da aplicação, otimiza a utilização de recursos e suporta novos cenários de redirecionamento, incluindo global e com base no caminho de redirecionamento. Suporte de redirecionamento do Gateway de aplicação não está limitado a HTTP -> redirecionamento HTTPS individualmente. Tem um mecanismo de redirecionamento genérico, que permite o redirecionamento de tráfego recebido na entidade um serviço de escuta para outro serviço de escuta no Gateway de aplicação. Redirecionamento de site externo também é suportado.

Suporte de redirecionamento do Gateway de aplicação oferece as seguintes capacidades:

-  **Redirecionamento global**

   Redirecionamentos a partir do serviço de um escuta no outro serviço de escuta no gateway. Isto permite que o HTTP para redirecionamento de HTTPS, num site.
- **Com base no caminho de redirecionamento**

   Este tipo de redirecionamento de permite HTTP para o redirecionamento de HTTPS apenas na área de um site específico, por exemplo uma área de carrinho de compras em falta por que/carrinho / *.
- **Redirecionar para o site externo**

![redirecionar](./media/redirect-overview/redirect.png)

Com esta alteração, que os clientes precisam criar um novo objeto de configuração de redirecionamento, que especifica o serviço de escuta de destino ou um site externo ao qual o redirecionamento for pretendido. O elemento de configuração também suporta opções para ativar a acrescentar a cadeia de caminho e consulta URI para o URL redirecionado. Também pode escolher se o redirecionamento é um temporário (código de estado HTTP 302) ou um redirecionamento permanente (código de estado HTTP 301). Depois de criado, esta configuração de redirecionamento está ligada ao serviço de escuta de origem através de uma nova regra. Quando utilizar uma regra básica, a configuração de redirecionamento está associada um serviço de escuta de origem e é um redirecionamento global. Quando é utilizada uma regra com base no caminho, a configuração de redirecionamento está definida no mapa de caminho do URL. Por isso, só se aplica à área de caminho específico de um site.

### <a name="next-steps"></a>Passos Seguintes

[Configurar redirecionamento de URL num gateway de aplicação](tutorial-url-redirect-powershell.md)
