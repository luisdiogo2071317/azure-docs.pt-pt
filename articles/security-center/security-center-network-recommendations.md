---
title: Protegendo sua rede no Centro de segurança do Azure | Documentos da Microsoft
description: Este documento trata recomendações no Centro de segurança do Azure que o ajudam a proteger sua rede do Azure e mantenha-se em conformidade com as políticas de segurança.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 12c00d6dfac6c9c2a377a8c142118ff6fd0af751
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302277"
---
# <a name="protecting-your-network-in-azure-security-center"></a>Protegendo sua rede no Centro de segurança do Azure
Centro de segurança do Azure analisa o estado de segurança dos seus recursos do Azure. Quando o Centro de segurança identifica potenciais vulnerabilidades de segurança, cria recomendações que descreve o processo de configuração de controlos necessários.  Recomendações se aplicam a tipos de recursos do Azure: máquinas virtuais (VMs), rede, SQL e aplicações.

Este artigo aborda recomendações que se aplicam à sua rede.  Centro de recomendações de rede em torno de firewalls de próxima geração, grupos de segurança de rede, configurando regras de tráfego de entrada e muito mais.  Utilize a tabela abaixo como referência para ajudar a compreender as recomendações de rede disponível e o que cada um deles faz se as aplicar.

## <a name="available-network-recommendations"></a>Recomendações de rede disponível
| Recomendação | Descrição |
| --- | --- |
| [Adicionar uma Firewall da Próxima Geração](security-center-add-next-generation-firewall.md) |Recomenda-se de que adiciona a Next Generation Firewall (NGFW) a partir de um parceiro da Microsoft para aumentar sua proteções de segurança. |
| [Encaminhar o tráfego apenas através da NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Recomenda-se de que configura regras de grupo (NSG) de segurança de rede que imponham o tráfego de entrada para a sua VM através da sua NGFW. |
| [Ativar grupos de segurança de rede em sub-redes ou máquinas virtuais](security-center-enable-network-security-groups.md) |Recomenda-se de que ativa NSGs em sub-redes ou VMs. |
| [Restringir o acesso através da Internet destinada ao ponto final](security-center-restrict-access-through-internet-facing-endpoints.md) |Recomenda-se de que configura regras de tráfego de entrada para NSGs. |

## <a name="see-also"></a>Consulte também
Para saber mais sobre as recomendações que se aplicam a outros tipos de recursos do Azure, consulte o seguinte:

* [Proteger as máquinas virtuais no Centro de Segurança do Azure](security-center-virtual-machine-recommendations.md)
* [Proteger as aplicações no Centro de Segurança do Azure](security-center-application-recommendations.md)
* [Proteger o seu serviço do SQL do Azure no Centro de segurança do Azure](security-center-sql-service-recommendations.md)

Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições e grupos de recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
