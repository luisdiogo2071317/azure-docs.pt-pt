---
title: 'Requisitos do QoS - ExpressRoute: Azure | Documentos da Microsoft'
description: Esta página fornece os requisitos detalhados para configurar e gerir o QoS. Skype para serviços de negócios/voz são discutidos.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: bc946587cf54142278472652449db40096ca27b8
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2019
ms.locfileid: "54117219"
---
# <a name="expressroute-qos-requirements"></a>Requisitos do QoS do ExpressRoute
O Skype para Empresas tem várias cargas de trabalho que exigem um tratamento do QoS diferenciado. Se planear consumir serviços de voz através do ExpressRoute, deve cumprir os requisitos descritos abaixo.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> Os requisitos do QoS aplicam-se apenas ao peering da Microsoft. Os valores DSCP no seu tráfego recebido de rede no peering público do Azure e no peering privado do Azure serão repostos a 0. 
> 
> 

A tabela seguinte fornece uma lista de marcações DSCP utilizadas pelo Skype para Empresas. Para obter mais informações, veja [Gerir QoS para Skype para Empresas](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/managing-quality-of-service-QoS).

| **Classe de Tráfego** | **Tratamento (Marcação DSCP)** | **Cargas de trabalho do Skype para Empresas** |
| --- | --- | --- |
| **Voz** |EF (46) |Voz do Skype/Lync |
| **Interativo** |AF41 (34) |Video, VBSS |
| |AF21 (18) |Partilha de aplicações | 
| **Predefinição** |AF11 (10) |Transferência de ficheiros |
| |CS0 (0) |Tudo o resto |

* Deve classificar as cargas de trabalho e marcar os valores DSCP corretos. Siga as orientações fornecidas [aqui](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/configuring-port-ranges-for-your-skype-clients#configure-quality-of-service-policies-for-clients-running-on-windows-10) para saber como configurar as marcações DSCP na sua rede.
* Deve configurar e suportar várias filas do QoS na rede. Voz tem de ser uma classe autónoma e receber o tratamento EF especificado no [RFC 3246](https://www.ietf.org/rfc/rfc3246.txt). 
* Pode decidir o mecanismo de colocação em fila, a política de deteção de congestionamento e a alocação de largura de banda por classe de tráfego. No entanto, a marcação DSCP para cargas de trabalho do Skype para Empresas tem de ser preservada. Se estiver a utilizar marcações DSCP não listadas acima, por exemplo, AF31 (26), terá de reescrever este valor DSCP para 0 antes de enviar o pacote à Microsoft. A Microsoft só envia pacotes marcados com o valor DSCP mostrado na tabela acima. 

## <a name="next-steps"></a>Passos Seguintes
* Veja os requisitos para [Encaminhamento](expressroute-routing.md) e [NAT](expressroute-nat.md).
* Consulte as ligações seguintes para configurar a ligação do ExpressRoute.
  
  * [Crie um circuito do ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Configure o encaminhamento](expressroute-howto-routing-classic.md)
  * [Ligar uma VNet a um circuito do ExpressRoute](expressroute-howto-linkvnet-classic.md)

