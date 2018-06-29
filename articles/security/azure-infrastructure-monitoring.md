---
title: Monitorização da infraestrutura do Azure
description: Este artigo aborda a monitorização de rede de produção do Azure.
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
ms.openlocfilehash: 17e7183ff56725462dc43cba21db418a86d86b51
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102547"
---
# <a name="monitoring-of-azure-infrastructure"></a>Monitorização da infraestrutura do Azure   

## <a name="configuration-and-change-management"></a>Gestão de configuração e alteração
Microsoft Azure analisa e atualiza definições de configuração e configurações de linha de base de hardware, software e dispositivos de rede anual. As alterações são desenvolvidas, testadas e aprovadas antes de introduzir o ambiente de produção de um ambiente de desenvolvimento e/ou de teste.

As configurações de linha de base necessárias para os serviços baseados no Azure é revisto pela equipa de segurança do Azure e a conformidade e as equipas de serviço. Reveja uma equipa de serviço faz parte de testes antes da implementação do serviço de produção.

## <a name="vulnerability-management"></a>Gestão de vulnerabilidade
Gestão de atualizações de segurança ajuda a proteger os sistemas de vulnerabilidades conhecidas. Sistemas de implementação integrada do Azure utiliza para gerir a distribuição e a instalação das atualizações de segurança para Microsoft software. Azure também é possível desenhar nos recursos do Microsoft Security Response Center (MSRC). MSRC identifica, monitoriza, responde a e resolve incidentes de segurança e da nuvem vulnerabilidades cerca de clock, todos os dias do ano.

## <a name="vulnerability-scanning"></a>Análise de vulnerabilidade
Análise de vulnerabilidade é executada em sistemas operativos de servidor, bases de dados e dispositivos de rede. Análises de vulnerabilidade são efetuadas numa base trimestral no mínimo. Microsoft Azure contratos com assessors independentes para efetuar testes penetração do limite do Microsoft Azure. Equipa Red exercícios regularmente também são executados e resultados utilizada para efetuar melhoramentos de segurança.

## <a name="protective-monitoring"></a>Monitorização protective
Microsoft Azure Security definiu os requisitos para a monitorização do Active Directory. Serviço equipas configurar ferramentas de monitorização de Active Directory, de acordo com estes requisitos. Ferramentas de monitorização de Active Directory incluem o agente de monitorização (MA) e o System Center Operations Manager. Estas ferramentas estão configuradas para fornecer alertas de tempo ao pessoal de segurança do Microsoft Azure em situações que requerem ação imediata.

## <a name="incident-management"></a>Gestão de incidentes
Microsoft implementa um processo de gestão de incidentes de segurança para o facilitar uma resposta coordenada para incidentes, um deve ocorrer.

Se o Microsoft toma em consideração de acesso não autorizado aos dados de cliente armazenados no respetivo equipamento ou das suas instalações, ou acesso não autorizado a esse equipamento ou instalações, resultando em perda, divulgação ou alteração dos dados de cliente, a Microsoft demora o seguinte ações:

- Detetar notifica o cliente do incidente de segurança
- Detetar investiga o incidente de segurança e fornece o cliente com informações detalhadas sobre o incidente de segurança
- Executa os passos razoáveis e linha de comandos para atenuar os efeitos e minimizar um danos resultantes de incidente de segurança.

Foi estabelecida uma arquitetura de gestão de incidentes com funções definidos e responsabilidades atribuídas. A equipa de gestão de incidentes (WASIM) do Windows Azure segurança é responsável por gerir incidentes de segurança, incluindo Escalamento e assegurar o envolvimento de equipas especialista em quando for necessário. Gestores de operações do Azure são responsáveis por supervisionar investigação e resolução de incidentes de segurança e privacidade.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)
