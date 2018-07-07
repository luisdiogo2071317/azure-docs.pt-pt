---
title: Disponibilidade da infraestrutura do Azure
description: O artigo descreve os níveis de redundância para fornecer disponibilidade máxima dos dados dos clientes.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 18c6b87c9926b93eec26cca4028a38e472912c46
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902582"
---
# <a name="azure-infrastructure-availability"></a>Disponibilidade da infraestrutura do Azure
O Azure fornece disponibilidade robusta com base em ampla redundância obtida com tecnologia de virtualização. O Azure fornece vários níveis de redundância para fornecer disponibilidade máxima dos dados dos clientes.

## <a name="temporary-outages-and-natural-disaster"></a>Falhas temporárias e desastre natural
A equipe de operações de infraestrutura de nuvem da Microsoft e projeta, cria, opera e protege a infraestrutura de nuvem. Esta equipe garante que a infraestrutura do Azure é a entrega de elevada disponibilidade e confiabilidade, alta eficiência, escalabilidade inteligente e uma cloud segura, privada e confiável.

Fontes de alimentação ininterrupta e grandes bancos de baterias Certifique-se de que eletricidade permanece contínua se ocorrer uma interrupção de energia de curto prazo.

Os geradores de emergência fornecem o poder de cópia de segurança para falhas expandidas e a manutenção planeada. O Centro de dados é operado com reservas de combustível no local, se ocorrer um desastre natural.

Alta velocidade e redes de robusta fibra óptica ligarem os datacenters com outros hubs principais e os usuários da Internet. Nós de computação mais perto cargas de trabalho de anfitrião para os usuários finais para reduzir a latência, fornecem redundância geográfica em aumentam a resiliência em geral do serviço. Uma equipa de engenheiros trabalha ininterruptamente para se certificar de serviços de forma permanente disponíveis para clientes.

Microsoft garante elevada disponibilidade através de monitorização avançada e resposta a incidentes, suporte a serviços e cópia de segurança de capacidade de ativação pós-falha. Centros de operações do Microsoft distribuídos geograficamente operam 7/24/365. A rede do Azure é um dos maiores no mundo. Liga-se a rede de distribuição de conteúdo e óptica fibra data Centers e nós de extremidade para se certificar de alto desempenho e confiabilidade.

## <a name="disaster-recovery"></a>Recuperação após desastre
Azure mantém os dados dos clientes duráveis em duas localizações com o cliente com a possibilidade de escolha a localização do site de cópia de segurança. Em ambos os locais, o Azure mantém constantemente várias réplicas de bom estado de funcionamento (3) dos dados dos clientes.

## <a name="database-availability"></a>Disponibilidade de base de dados
Azure garante que uma base de dados está acessível através de um gateway de Internet com disponibilidade de base de dados constante da Internet. Monitorização avalia o estado de funcionamento e o estado das bases de dados Active Directory em intervalos de tempo de 5 minutos.

## <a name="storage-availability"></a>Disponibilidade de armazenamento
O Azure disponibiliza armazenamento através de um serviço de aplicações altamente escaláveis e duráveis de armazenamento, que fornece pontos finais de conectividade permitindo que seja acessível diretamente por uma aplicação de consumo. Através do serviço de armazenamento, os pedidos de armazenamento recebidos serão processados com eficiência com integridade transacional.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que a Microsoft faz para proteger a infraestrutura do Azure, veja:

- [Recursos do Azure, no local e a segurança física](azure-physical-security.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Recursos de segurança da base de dados do Microsoft Azure SQL](azure-infrastructure-sql.md)
- [Gerenciamento e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização de infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade de infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados do cliente no Azure](azure-protection-of-customer-data.md)
