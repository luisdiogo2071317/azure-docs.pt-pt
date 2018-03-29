---
title: Gestão do Azure - monitorização | Microsoft Docs
description: O Azure tem vários serviços e ferramentas que funcionam em conjunto para fornecer uma gestão completa para não apenas as suas aplicações em execução no Azure, mas também na outras nuvens e no local.  Este artigo fornece uma descrição de alto nível de diferentes áreas de gestão e ligações para conteúdo as ferramentas do Azure para gerir as aplicações em nuvem e recursos.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: bwren
ms.openlocfilehash: b20283e1189e4f1a3555e2dd8d25972c9a677cd6
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="azure-management---monitoring"></a>Gestão do Azure - monitorização

Monitorização no Azure é um aspeto de gestão do Azure.  Este artigo descreve brevemente diferentes áreas de gestão necessários para implementar e manter as suas aplicações e recursos no Azure com ligações para documentação para ajudar a começar.

## <a name="management-in-azure"></a>Gestão no Azure

Gestão refere-se para as tarefas e os processos necessários para manter as suas aplicações empresariais e recursos que suportam-los.  O Azure tem vários serviços e ferramentas que funcionam em conjunto para fornecer uma gestão completa para não apenas as suas aplicações em execução no Azure, mas também na outras nuvens e no local.  Compreender as diferentes ferramentas disponíveis e como podem ser utilizadas em conjunto para uma variedade de cenários de gestão é o primeiro passo na conceção de um ambiente de gestão completa.

O diagrama seguinte ilustra as várias áreas da gestão, que são necessárias para manter a qualquer aplicação ou o recurso.  Estas áreas diferentes podem considerar em termos de um ciclo de vida onde cada é necessário sucessivamente contínuas ao longo do tempo de vida de um recurso.  Esta ação inicia com a implementação inicial, através do respetivo funcionamento contínuo, e, finalmente, quando for retirado.

![Capacidades de gestão](media/management-overview/management-capabilities.png)


Nenhum serviço Azure único completamente preenche os requisitos de uma área de gestão específico, mas em vez disso, cada são realizadas por vários serviços trabalhar em conjunto.  Alguns serviços fornecem a funcionalidade de destino, como o Application Insights que fornece a monitorização de aplicações web.  Fornecem outras funções comuns, tais como a análise de registos que armazena dados de gestão para outros serviços, permitindo-lhe analisar dados de diferentes tipos de recolhidos por diferentes serviços.  

Resumidamente, as secções seguintes descrevem as diferentes áreas de gestão e fornecem ligações para conteúdo de detalhado sobre os principais serviços do Azure se destina a resolvê-los.

## <a name="monitor"></a>Monitorizar
A monitorização é o ato de recolha e análise de dados para determinar o desempenho, o estado de funcionamento e a disponibilidade da sua aplicação de negócio e os recursos depende. Uma estratégia de monitorização efetiva irão ajudá-lo a compreender a operação de detalhado de diferentes componentes da aplicação e para aumentar o tempo de atividade por proativamente notificam dos problemas críticos, de modo a que o possa resolvê-los antes que comprometam.  Pode ler uma descrição geral de monitorização no Azure que identifica os diferentes serviços utilizados para uma estratégia de monitorização no [recursos e aplicações de monitorização do Azure](monitoring-overview.md).


## <a name="configure"></a>Configurar
Configurar refere-se para a implementação inicial e a configuração de aplicações e recursos e os respetivos manutenções correntes com correções de erros e atualizações.  Automatização destas tarefas através do script e de política permite-lhe eliminar redundância, minimizando o tempo e esforço e aumentar a precisão e a eficiência.  [A automatização do Azure](..\automation\automation-intro.md) fornece o volume dos serviços para automatizar tarefas de configuração.  Para além dos runbooks para automatizar processos, fornece configuração e gestão de atualizações, ajudá-lo a gerir a configuração através da política e identificar e a implementação de atualizações.

## <a name="govern"></a>Governar
Governação fornece mecanismos e processos para manter o controlo sobre as suas aplicações e recursos no Azure.  Envolve as iniciativas de planeamento e a definição de prioridades estratégicos.  Governação no Azure principalmente é implementada com dois serviços.  [Política do Azure](../azure-policy/azure-policy-introduction.md) permite-lhe criar, atribuir e, gerir as definições de política que impõem diferentes regras e ações sobre os recursos, para que esses recursos permanecem em conformidade com as normas empresariais e contratos de nível de serviço. [Gestão de custo do Azure por Cloudyn](../cost-management/overview.md) permite-lhe controlar a utilização de nuvem e despesas para os recursos do Azure e outros fornecedores de nuvem, incluindo AWS e Google.

## <a name="secure"></a>Proteger
Gerir a segurança das suas aplicações, recursos e dados envolve uma combinação de avaliar ameaças, recolher e analisar dados de segurança e garantir que as suas aplicações e recursos são concebidos e configurados de forma segura.  Análise de monitorização e ameaças de segurança são fornecidos pelo [Centro de segurança do Azure](../security-center/security-center-intro.md) que inclui unificada de gestão de segurança e proteção avançada contra ameaças em toda a cargas de trabalho de nuvem híbrida.  Também deverá ver [introdução à segurança do Azure](../security/azure-security.md) para obter informações de segurança no Azure e orientações sobre a configuração de forma segura a recursos do Azure.


## <a name="protect"></a>Proteger
Proteção refere-se para se certificar de que as suas aplicações e dados estão sempre disponíveis, mesmo em caso de falhas para além do seu controlo.  Proteção no Azure é fornecida por dois serviços.  [Cópia de segurança do Azure](../backup/backup-introduction-to-azure-backup.md) fornece cópia de segurança e recuperação dos seus dados, ou na nuvem ou no local.    [O Azure Site Recovery](../site-recovery/site-recovery-overview.md) garante a disponibilidade elevada da sua aplicação, fornecendo continuidade do negócio e recuperação imediata no caso de desastre.

## <a name="migrate"></a>Migrar 
Migração refere-se em transição cargas de trabalho atualmente em execução no local na nuvem do Azure.  [Migrar do Azure](../migrate/migrate-overview.md) é um serviço que ajuda a avaliar a adequabilidade a migração, incluindo o dimensionamento com base no desempenho e custo estimativas, do local de máquinas virtuais no Azure.  O Azure Site Recovery pode ajudar a efetuar a migração de máquinas virtuais real [no local](../site-recovery/migrate-tutorial-on-premises-azure.md) ou [da Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md).  [Migração de base de dados do Azure](../dms/dms-overview.md) irá ajudá-lo a migrar de várias origens de base de dados para plataformas de dados do Azure.