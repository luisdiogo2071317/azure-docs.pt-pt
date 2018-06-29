---
title: Gestão e operações de produção do Azure
description: Este artigo fornece uma descrição geral da gestão e a operação de rede de produção do Azure.
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
ms.openlocfilehash: dc389f5f5c155555deb860f041b15b0ea49ee416
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102536"
---
# <a name="azure-production-operations-and-management"></a>Gestão e operações de produção do Azure    
Gestão e a operação de rede de produção do Azure é um esforço coordenado entre as equipas de operações do Azure e SQL Database do Azure. Várias aplicações e de sistema desempenho ferramentas de monitorização são utilizadas no ambiente. Dispositivos de rede, servidores, serviços e processos de aplicação são monitorizados com as ferramentas apropriadas.

Vários níveis da monitorização, registo e de relatórios são implementados para garantir uma execução segura de serviços em execução no ambiente do Microsoft Azure, incluindo as seguintes ações:

- Principalmente, o Microsoft Azure monitorização do agente (MA) recolhe informações de registo de monitorização e diagnóstico de vários locais, incluindo o FC e raiz SO e escreve-nos ficheiros de registo. Eventualmente pushes um subconjunto digested das informações para uma conta do Storage do Azure previamente configurada. Além disso, a monitorização e diagnóstico do serviço (MDS) é um serviço freestanding que lê dados de registo de monitorização e diagnóstico vários e resume as informações. MDS escreve as informações de um registo integrado. Azure utiliza o personalizada do Azure segurança monitorização (ASM), que é uma extensão para o sistema de monitorização do Azure. Tem componentes que observar, analisam e elaborar relatórios sobre eventos de segurança pertinentes dos vários pontos de plataforma.
- Plataforma de WinFabric de base de dados SQL do Microsoft Azure fornece serviços de supervisão operacional da base de dados do Microsoft Azure SQL, desenvolvimento, implementação e gestão. Oferece serviços de implementação distribuída, com vários passos, a monitorização de estado de funcionamento, reparações automáticas e compatibilidade da versão de serviço. Fornece os seguintes serviços:

   - Serviço de modelação capacidades com o ambiente de desenvolvimento de alta-fidelidade (Centro de dados clusters são dispendiosas e escassos).
   - A implementação de um clique e fluxos de trabalho de atualização para o arranque de configuração do serviço e manutenção.
   - O estado de funcionamento com fluxos de trabalho de reparação automática de relatórios para ativar a autorrecuperação.
   - Em tempo real de monitorização, alertas e depuração instalações em todos os nós de um sistema distribuído.
   - Centralizada de recolha de dados operacionais e métricas de raiz distribuída fazer com que as informações de análise e serviço.
   - Operacional, as ferramentas de implementação, monitorização e gestão de alterações.
   - Scripts de plataforma e watchdog do Microsoft Azure SQL da base de dados WinFabric continuamente a executar e monitorizar em tempo real.

Se ocorrerem a qualquer anomalias, o processo de resposta a incidentes, seguido pela equipa do Azure incidente triagem está ativado. O técnico de suporte do Azure adequada é notificado para responder ao incidente. Problema de controlo e a resolução estão documentados e geridos num sistema de emissão de permissões centralizado. Métricas de tempo de atividade do sistema estão disponíveis na divulgação não contrato (contrato de confidencialidade) e mediante pedido.

## <a name="corporate-network-and-multi-factor-access-to-production"></a>Rede empresarial e de acesso Multifator para produção
A base de utilizadores de rede empresarial inclui técnicos de suporte do Microsoft Azure. A rede empresarial suporta funções empresariais internas e inclui o acesso a aplicações internas que são utilizados para o suporte ao cliente do Azure. A rede empresarial é logicamente e fisicamente separada da rede produção do Azure. Técnico do Microsoft Azure à rede empresarial através de estações de trabalho do Microsoft Azure e computadores portáteis. Todos os utilizadores têm de ter uma conta do Active Directory (AD), incluindo um nome de utilizador e palavra-passe para aceder a recursos da rede empresarial. Acesso à rede empresarial utiliza as contas de anúncios, que são emitidas para a equipa da Microsoft, contratantes, fornecedores e geridas pelo MSIT. Os identificadores de utilizador exclusivo distinguir técnico com base no respetivo estado de emprego na Microsoft.

Acesso a aplicações do Azure internos é controlado através da autenticação com os serviços de Federação do Active Directory (ADFS). AD FS são um serviço alojado pelo MSIT que fornece autenticação de utilizadores de rede empresarial através de um afirmações de utilizador e de token seguro. AD FS permite que aplicações internas do Microsoft Azure autenticar os utilizadores contra o domínio do AD de empresarial Microsoft. Para aceder à rede de produção do ambiente de rede empresarial, o utilizador tem de ser autenticado utilizando a autenticação multifator.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)
