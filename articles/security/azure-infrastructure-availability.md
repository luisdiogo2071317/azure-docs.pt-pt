---
title: Disponibilidade da infraestrutura do Azure
description: O artigo descreve os níveis de redundância para fornecer a máxima disponibilidade dos dados dos clientes.
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
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: db13e4835e483b4738074a71861737c4851d8dbc
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102512"
---
# <a name="availability-of-azure-infrastructure"></a>Disponibilidade da infraestrutura do Azure
O Azure oferece disponibilidade robusta com base no redundância extensa atingida com tecnologia de virtualização. O Azure oferece vários níveis de redundância para fornecer a máxima disponibilidade dos dados dos clientes.

## <a name="temporary-outages-and-natural-disaster"></a>Falhas temporárias e desastre natural
A equipa de infraestrutura de nuvem da Microsoft e operações designs, baseia-se, funciona e protege a infraestrutura de nuvem. Esta equipa garante que a infraestrutura do Azure está a fornecer elevada disponibilidade e fiabilidade, eficiência elevada, inteligente escalabilidade e uma nuvem segura, privada & fidedigna.

Fontes de alimentação ininterrupta e bancos vasta de baterias Certifique-se de que eletricidade permanece contínua se ocorrer uma interrupção de energia de curta duração.

Emergência generators fornecem a energia de cópia de segurança para falhas expandidas e a manutenção planeada. O Centro de dados é operado com trazidos fuel reservas se ocorrer um desastre natural.

Alta velocidade e redes ótica fibra robusta ligar centros de dados com outros utilizadores da Internet e hubs principais. Nós de computação próximo cargas de trabalho de anfitriões para os utilizadores finais para reduzir a latência, fornecem redundância geográfica e aumentam a resiliência geral do serviço. Uma equipa de engenheiros de cerca de clock funciona para garantir que os serviços estão disponíveis de forma permanente para clientes.

A Microsoft garante elevada disponibilidade através de monitorização avançada e resposta a incidentes, suporte de serviço e cópia de segurança capacidade de ativação pós-falha. Os centros de operações do Microsoft distribuídos geograficamente operam 24/7/365. A rede do Azure é uma do maior no mundo. A rede de distribuição de conteúdo e ótica fibra liga centros de dados e nós de contorno para se certificar de elevado desempenho e fiabilidade.

## <a name="disaster-recovery"></a>Recuperação após desastre
Azure mantém os dados de cliente duráveis em duas localizações com o cliente ter a capacidade de escolher a localização do site de cópia de segurança. Em ambos os locais, o Azure mantém constantemente várias réplicas de bom estado de funcionamento (3) de dados de cliente.

## <a name="database-availability"></a>Disponibilidade da base de dados
Azure garante que uma base de dados está acessível através de um gateway de Internet com disponibilidade de base de dados de constante de Internet. Monitorização avalia o estado de funcionamento e o estado de bases de dados do Active Directory em intervalos de tempo de 5 minutos.

## <a name="storage-availability"></a>Disponibilidade de armazenamento
Azure fornece armazenamento através de um serviço de armazenamento durável e altamente escalável, que fornece pontos finais de conectividade, permitindo que seja acessível diretamente por uma aplicação de consumo. Através do serviço de armazenamento, os pedidos recebidos de armazenamento serão processados eficiente com integridade transacional.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)
