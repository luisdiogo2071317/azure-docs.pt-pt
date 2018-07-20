---
title: Perguntas mais frequentes (FAQ) do sobre o Centro de segurança do Azure | Documentos da Microsoft
description: Encontre respostas para perguntas sobre o Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2018
ms.author: rkarlin
ms.openlocfilehash: 320c7c483e865c85948d32ee2b5b70a92181920f
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160074"
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Perguntas mais frequentes (FAQ) do Centro de Segurança do Azure
Encontre respostas para perguntas sobre o Centro de segurança do Azure, um serviço que o ajuda a prevenir, detetar e responder a ameaças com maior visibilidade e controlo da segurança dos seus recursos do Microsoft Azure.

> [!NOTE]
> A partir do início de junho de 2017, o Centro de Segurança irá utilizar o Microsoft Monitoring Agent para recolher e armazenar dados. Para obter mais informações, consulte [migração de plataforma do Centro de segurança do Azure](security-center-platform-migration.md). As informações neste artigo representam a funcionalidade do Centro de Segurança após a transição para o Agente de Monitorização da Microsoft.
>
>

## <a name="general-questions"></a>Perguntas gerais
### <a name="what-is-azure-security-center"></a>O que é o Centro de Segurança do Azure?
O Centro de Segurança do Azure ajuda-o a evitar, detetar e responder a ameaças com uma maior visibilidade e controlo sobre a segurança dos seus recursos Azure. Fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições, ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança.

### <a name="how-do-i-get-azure-security-center"></a>Como obtenho o Centro de segurança do Azure?
Centro de segurança do Azure é ativado com a sua subscrição do Microsoft Azure e acessado a partir da [portal do Azure](https://azure.microsoft.com/features/azure-portal/). ([Iniciar sessão portal](https://portal.azure.com), selecione **procurar**e desloque-se para **Centro de segurança**).  

## <a name="billing"></a>Faturação
### <a name="how-does-billing-work-for-azure-security-center"></a>Como funciona a faturação do Centro de segurança do Azure?
Centro de segurança é disponibilizado em dois escalões:

O **escalão gratuito** fornece visibilidade sobre o estado de segurança dos seus recursos do Azure, política de segurança básica, recomendações de segurança e integração com produtos de segurança e serviços de parceiros.

O **escalão Standard** adiciona ameaças avançadas capacidades de deteção, incluindo ameaças inteligência, análise comportamental, deteção de anomalias, incidentes de segurança e relatórios de atribuição de ameaças. O escalão Standard é gratuito durante os 60 dias iniciais. Se optar por continuar a utilizar o serviço além desses 60 dias, começamos automaticamente a cobrar o serviço.  Para atualizar, selecione [escalão de preço](https://docs.microsoft.com/azure/security-center/security-center-pricing) na política de segurança.

## <a name="permissions"></a>Permissões
Centro de segurança do Azure utiliza [controlo de acesso baseado em funções (RBAC)](../role-based-access-control/role-assignments-portal.md), que fornece [funções incorporadas](../role-based-access-control/built-in-roles.md) que podem ser atribuídas a utilizadores, grupos e serviços no Azure.

Centro de segurança avalia a configuração dos seus recursos para identificar problemas de segurança e vulnerabilidades. No Centro de segurança, apenas verá informações relacionadas com a um recurso quando for atribuída a função de proprietário, contribuinte ou leitor para a subscrição ou grupo de recursos que um recurso pertence.

Ver [permissões no Centro de segurança do Azure](security-center-permissions.md) para saber mais sobre as funções e ações permitidas no Centro de segurança.

## <a name="data-collection"></a>Recolha de dados
Centro de segurança recolhe dados a partir das suas máquinas virtuais do Azure (VMs) e computadores não Azure para monitorizar ameaças e vulnerabilidades de segurança. Os dados são recolhidos com o Microsoft Monitoring Agent, que lê várias configurações relacionadas com segurança e registos de eventos a partir da máquina e copia os dados para a sua área de trabalho para análise.

### <a name="how-do-i-disable-data-collection"></a>Como desativar a recolha de dados?
Aprovisionamento automático está desativada por predefinição. Pode desativar aprovisionamento automático de recursos em qualquer altura ao desativar esta definição na política de segurança. Aprovisionamento Automático é altamente recomendado para obter alertas de segurança e recomendações sobre as atualizações do sistema, vulnerabilidades do SO e o endpoint protection.

Para desativar a recolha de dados [inicie sessão no portal do Azure](https://portal.azure.com), selecione **procurar**, selecione **Centro de segurança**e selecione **selecione política**. Selecione a subscrição para a qual pretende desativar o aprovisionamento automático. Quando seleciona uma assinatura **política de segurança – recolha de dados** abre. Sob **aprovisionamento automático**, selecione **desativar**.

### <a name="how-do-i-enable-data-collection"></a>Como ativar a recolha de dados?
Pode ativar a recolha de dados para a sua subscrição do Azure na política de segurança. Para ativar a recolha de dados. [Inicie sessão no portal do Azure](https://portal.azure.com), selecione **procure**, selecione **Centro de segurança**e selecione **política de segurança**. Selecione a subscrição que pretende ativar o aprovisionamento automático. Quando seleciona uma assinatura **política de segurança – recolha de dados** abre. Sob **aprovisionamento automático**, selecione **no**.

### <a name="what-happens-when-data-collection-is-enabled"></a>O que acontece quando a recolha de dados está ativada?
Quando o aprovisionamento automático está ativado, o Centro de segurança Aprovisiona o Microsoft Monitoring Agent em todos os suportadas VMs do Azure e novas que são criadas. Aprovisionamento Automático é vivamente recomendado mas instalação manual de agente também está disponível. [Saiba como instalar a extensão do Microsoft Monitoring Agent](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

O agente permite que o evento de criação de processo 4688 e o *CommandLine* campo dentro 4688 de evento. Novos processos criados na VM são registados pelo registo de eventos e monitorizados pelos serviços de deteção do Centro de segurança. Para obter informações sobre os detalhes registados para cada novo processo, consulte [campos de descrição no 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). Além disso, o agente recolhe os 4688 eventos criados na VM e armazena-os na pesquisa.

Quando o Centro de segurança detetar atividade suspeita na VM, o cliente é notificado por e-mail, se [informações de contacto de segurança](security-center-provide-security-contact-details.md) foi fornecido. Um alerta é também visível no painel de alertas de segurança do Centro de segurança.

> [!NOTE]
> - Para ativar a recolha de dados para [controlos de aplicações adaptativos](security-center-adaptive-application.md), Centro de segurança configura uma política de AppLocker local no modo de auditoria para permitir que todos os aplicativos. Isso fará com que o AppLocker gerar eventos que, em seguida, são recolhidos e utilizados pelo centro de segurança. É importante observar que esta política não irá ser configurada em quaisquer máquinas em que já existe uma política de AppLocker configurada. 

### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>O agente de monitorização impacto sobre o desempenho dos meus servidores?
O agente consome uma quantia nominal de recursos do sistema e deve ter pouco impacto sobre o desempenho. Para obter mais informações sobre o impacto no desempenho e o agente e a extensão, consulte a [guia de operações e planeamento](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Onde são armazenados os meus dados?
Dados recolhidos por este agente são armazenados numa área de trabalho do Log Analytics existente associados à subscrição ou uma nova área de trabalho. Para obter mais informações, consulte [segurança de dados](security-center-data-security.md).

## <a name="using-azure-security-center"></a>O Centro de segurança do Azure
### <a name="what-is-a-security-policy"></a>O que é uma política de segurança?
Uma política de segurança define o conjunto de controlos que são recomendados para recursos dentro da subscrição especificada. No Centro de segurança do Azure, é possível definir políticas para as suas subscrições do Azure, de acordo com requisitos de segurança da sua empresa e o tipo de aplicações ou sensibilidade dos dados em cada subscrição.

As políticas de segurança ativadas no monitorização e recomendações de segurança de unidade de centro de segurança do Azure. Para saber mais sobre as políticas de segurança, veja [monitorização de estado de funcionamento de segurança no Centro de segurança do Azure](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Quem pode modificar uma política de segurança?
Para modificar uma política de segurança, tem de ser um administrador de segurança ou um proprietário ou contribuinte dessa subscrição.

Para saber como configurar uma política de segurança, veja [definir políticas de segurança no Centro de segurança do Azure](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>O que é uma recomendação de segurança?
Centro de segurança do Azure analisa o estado de segurança dos seus recursos do Azure. Quando são identificadas potenciais vulnerabilidades de segurança, recomendações são criadas. As recomendações orientam-no durante o processo de configuração o controle necessário. Os exemplos são:

* Aprovisionamento de antimalware para ajudar a identificar e remover software malicioso
* [Grupos de segurança de rede](../virtual-network/security-overview.md) e regras para controlar o tráfego para as máquinas virtuais
* Aprovisionamento de uma firewall de aplicações web para ajudar na defesa contra ataques que visam as suas aplicações web
* Implementação de atualizações do sistema em falta
* Abordagem de configurações de SO que não correspondam às linhas de base recomendadas

Apenas as recomendações que estão ativadas nas políticas de segurança são mostradas aqui.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Como posso ver o estado de segurança atual dos meus recursos do Azure?
O **descrição geral do Centro de segurança** painel mostra a postura de segurança geral do seu ambiente dividida por computação, rede, armazenamento e dados e aplicativos. Cada tipo de recurso tem uma apresentação do indicador se quaisquer potenciais vulnerabilidades de segurança que tenham sido identificadas. Clicar em cada mosaico apresenta uma lista de problemas de segurança identificadas pelo centro de segurança, juntamente com um inventário dos recursos na sua subscrição.

### <a name="what-triggers-a-security-alert"></a>O que aciona um alerta de segurança?
Centro de segurança do Azure automaticamente recolhe, analisa e funde dados de registo de recursos do Azure, rede e soluções de parceiros como antimalware e firewalls. Quando são detetadas ameaças, é criado um alerta de segurança. Os exemplos incluem a deteção de:

* Virtual Machines comprometidas a comunicar com endereços IP maliciosos conhecidos
* Software maligno avançado detetado com o relatório de erros do Windows
* Ataques de força bruta contra máquinas virtuais
* Alertas de segurança de soluções de segurança de parceiros integradas, como o anti-malware ou Firewalls de aplicações Web

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>O que é a diferença entre ameaças detetados e alertados pelo Microsoft Security Response Center em comparação com o Centro de segurança do Azure?
O Microsoft Security Response Center (MSRC) executa a segurança selecione monitorização da rede do Azure e infraestrutura e recebe a reclamações de abuso e inteligência de ameaças de terceiros. Quando o MSRC fica sabendo que os dados do cliente foram acedidos por uma entidade não autorizada ou ilícita ou que utilização do cliente do Azure não estão em conformidade com os termos para utilizar aceitável, um Gestor de incidentes de segurança notifica o cliente. Normalmente, a notificação ocorre ao enviar um e-mail para os contactos de segurança especificados no Centro de segurança do Azure ou o proprietário da subscrição do Azure, se não for especificado um contacto de segurança.

Centro de segurança é um serviço do Azure que monitoriza o ambiente do Azure do cliente e aplica-se a análise para detetar automaticamente uma vasta gama de atividades maliciosas potencialmente continuamente. Estas deteções são apresentadas como alertas de segurança no dashboard do Centro de segurança.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Quais recursos do Azure são monitorizados pelo centro de segurança do Azure?
Centro de segurança do Azure monitoriza os seguintes recursos do Azure:

* Máquinas virtuais (VMs) (incluindo [serviços Cloud](../cloud-services/cloud-services-choose-me.md))
* Redes Virtuais do Azure
* Serviço SQL do Azure
* Conta de armazenamento do Azure
* Aplicações Web do Azure (no [ambiente do serviço de aplicações](../app-service/environment/intro.md))
* Soluções de parceiro integradas com a sua subscrição do Azure, como uma firewall de aplicações web em VMs e no ambiente de serviço de aplicações

## <a name="virtual-machines"></a>Virtual Machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Que tipos de máquinas virtuais são suportados?
Monitorização e recomendações estão disponíveis para máquinas virtuais (VMs) criadas com ambos os [clássico e modelos de implementação do Resource Manager](../azure-classic-rm.md).

Ver [plataformas suportadas no Centro de segurança do Azure](security-center-os-coverage.md) para obter uma lista das plataformas suportadas.

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Por que o Centro de segurança do Azure não reconhece a solução de antimalware em execução na minha VM do Azure?
Centro de segurança do Azure tem visibilidade para antimalware instalado através de extensões do Azure. Por exemplo, o Centro de segurança não é capaz de detetar antimalware que foi pré-instalado numa imagem fornecida ou se tiver instalado antimalware nas suas máquinas virtuais através de seus próprios processos (por exemplo, sistemas de gestão de configuração).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Por que motivo recebo a mensagem "Dados de análise em falta" para a minha VM?
Esta mensagem é apresentada quando não existem dados de análise para uma VM não existe. Pode demorar algum tempo (menos de uma hora) para dados de análise preencher após a recolha de dados está ativada no Centro de segurança do Azure. Após o preenchimento inicial de dados de análise, poderá receber esta mensagem porque não existe nenhum dado de análise de todo ou não existe nenhum dado de análise recentes. Verificações não preencher para uma VM no estado parado. Esta mensagem também pode aparecer se os dados de análise não tiverem sido povoadas recentemente (em conformidade com a política de retenção para o agente do Windows, que tem um valor predefinido de 30 dias).

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>A frequência com que o Centro de segurança analisar para vulnerabilidades do sistema operativo, atualizações do sistema e problemas do endpoint protection?
A latência no Centro de segurança verifica a existência de vulnerabilidades, atualizações, e é de problemas:

- Configurações de segurança do sistema operacional – dados é atualizado no prazo de 48 horas
- Atualizações do sistema – dados é atualizada dentro de 24 horas
- Problemas de proteção de ponto de extremidade – dados serão atualizados dentro de 8 horas

Centro de segurança, normalmente, verifica a existência de novos dados a cada hora. Os valores de latência acima são um cenário mais desfavorável onde não há uma verificação recente ou falha de uma análise.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Por que motivo recebo a mensagem "O agente da VM está em falta?"
O agente da VM tem de ser instalado em VMs para ativar a recolha de dados. O Agente de VM é instalado por predefinição em VMs que são implementadas a partir do Azure Marketplace. Para obter informações sobre como instalar o agente da VM em outras VMs, consulte a mensagem de blogue [agente da VM e extensões](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
