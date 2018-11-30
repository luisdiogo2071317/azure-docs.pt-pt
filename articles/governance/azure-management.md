---
title: Descrição geral da gestão do Azure
description: Descrição geral das áreas de gestão para aplicações do Azure e de recursos com ligações para conteúdo em ferramentas de gestão do Azure.
author: DCtheGeek
manager: carmonm
ms.service: governance
ms.topic: article
ms.date: 09/18/2018
ms.author: dacoulte
ms.openlocfilehash: 180b0cb9f52858d9b0f079ea711fd5ccab738ecf
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582318"
---
# <a name="management-in-azure"></a>Gestão no Azure

Governação no Azure é um aspecto de gestão do Azure. Este artigo descreve resumidamente as diferentes áreas de gestão necessárias para implementar e manter as suas aplicações e recursos no Azure com ligações para documentação que irá ajudá-lo a começar.

Gestão refere-se aos processos e tarefas necessários para manter as suas aplicações empresariais e os recursos que suportam esses processos e tarefas. O Azure tem vários serviços e ferramentas que funcionam em conjunto para assegurar uma gestão completa não só das suas aplicações em execução no Azure, como também noutras clouds e no local. Compreender as diferentes ferramentas disponíveis e como pode utilizá-las em conjunto num vasto leque de cenários de gestão é o primeiro passo na conceção de um ambiente de gestão completo.

O diagrama seguinte ilustra as diferentes áreas de gestão necessárias para manter qualquer aplicação ou recurso. Pode pensar nestas diferentes áreas em termos de um ciclo de vida em que cada um é necessário em sucessão contínua ao longo do tempo de vida de um recurso. Começa com a implementação inicial, passando pela respetivo funcionamento contínuo, até que, por fim, é descontinuado.

![Capacidades de gestão](../monitoring/media/management-overview/management-capabilities.png)

Nenhum serviço do Azure individual preenche completamente os requisitos de uma área de gestão específica, na medida em que cada área é sustentada por vários serviços que se complementam. Alguns serviços fornecem funcionalidade direcionada, como é o caso do Application Insights, que fornece a monitorização de aplicações Web. Outros serviços fornecem funções comuns, como acontece com o Log Analytics, que armazena dados de gestão de outros serviços, o que lhe permite analisar dados de vários tipos recolhidos por diferentes serviços.

As secções seguintes descrevem resumidamente as diferentes áreas de gestão e fornecem ligações para conteúdo detalhado sobre os principais serviços do Azure que visam abordar essas áreas.

## <a name="monitor"></a>Monitorizar

A monitorização é o ato de recolha e análise de dados para determinar o desempenho, o estado de funcionamento e a disponibilidade da sua aplicação empresarial e dos recursos de que esta depende. Uma estratégia de monitorização eficaz irá ajudá-lo a compreender o funcionamento detalhado dos diferentes componentes da sua aplicação e a aumentar o tempo de atividade ao notificá-lo proativamente dos problemas críticos, para que possa resolvê-los antes que se tornem realmente graves. Pode ler uma descrição geral da Monitorização no Azure que identifica os diferentes serviços utilizados numa estratégia de monitorização em [Monitorizar aplicações e recursos do Azure](../monitoring/monitoring-overview.md).

## <a name="configure"></a>Configurar

Configurar refere-se à implementação inicial e à configuração dos recursos e aplicações e à respetiva manutenção contínua com correções de erros e atualizações. A automatização destas tarefas através de scripts e políticas permite-lhe eliminar a redundância, minimizar o tempo e esforço que despende e aumentar a precisão e eficiência. A [Automatização do Azure](..\automation\automation-intro.md) fornece-lhe o conjunto de serviços que lhe permite automatizar as tarefas de configuração. Para além dos runbooks para a automatização de processos, este serviço fornece a gestão de atualizações e configuração, o que o ajuda a gerir a configuração através de políticas e a identificar e implementar atualizações.

## <a name="govern"></a>Governação

A governação fornece mecanismos e processos para manter o controlo sobre os recursos e aplicações no Azure. A governação envolve o planeamento das suas iniciativas e a definição de prioridades estratégicas.
A governação no Azure é implementada principalmente com dois serviços. O [Azure Policy](../azure-policy/azure-policy-introduction.md) permite-lhe criar, atribuir e gerir definições de política que impõem diferentes regras e ações aos recursos, para que esses recursos mantenham a conformidade face às normas empresariais e aos contratos de nível de serviço. O [Azure Cost Management da Cloudyn](../cost-management/overview.md) permite-lhe controlar a utilização da cloud e as despesas dos seus recursos do Azure e outros fornecedores de cloud, incluindo AWS e Google.

## <a name="secure"></a>Proteger

Gerenciando a segurança das suas aplicações, recursos e dados envolve uma combinação de ameaças avaliando, recolher e analisar os dados de segurança e garantir que suas aplicações e recursos são criados e configurados de forma segura. Análise de ameaças e monitorização de segurança são fornecidas pelo centro de segurança do Azure, que inclui a gestão de segurança unificada e proteção avançada contra ameaças entre cargas de trabalho híbrida na cloud. Também deve ver a [Introdução à Segurança do Azure](../security/azure-security.md) para obter informações abrangentes sobre segurança no Azure e orientação sobre como configurar recursos do Azure com segurança.

## <a name="protect"></a>Proteger

Proteção passa por se certificar de que os seus dados e aplicações estão sempre disponíveis, mesmo em caso de falhas que estejam para além do seu controlo. A proteção no Azure é fornecida por dois serviços. [O Azure Backup](../backup/backup-introduction-to-azure-backup.md)fornece cópia de segurança e recuperação de seus dados, seja na cloud ou no local. O [Azure Site Recovery](../site-recovery/site-recovery-overview.md) garante a elevada disponibilidade da sua aplicação ao assegurar a continuidade do negócio e a recuperação imediata em caso de desastre.

## <a name="migrate"></a>Migrar

Migração refere-se à transição das cargas de trabalho atualmente em execução no local para a cloud do Azure.
O [Azure Migrate](../migrate/migrate-overview.md) é um serviço que o ajuda a avaliar a adequação da migração, incluindo o dimensionamento com base no desempenho e estimativas de custos, de máquinas virtuais no local para o Azure. O Azure Site Recovery pode ajudá-lo a fazer a migração real de máquinas virtuais a partir [do local](../site-recovery/migrate-tutorial-on-premises-azure.md) ou [do Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md). O [Azure Database Migration](../dms/dms-overview.md) irá ajudá-lo a migrar várias origens de dados para as Plataformas de Dados do Azure.
