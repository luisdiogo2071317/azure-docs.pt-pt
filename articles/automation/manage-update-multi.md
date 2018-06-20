---
title: Gerir atualizações para várias máquinas virtuais do Azure
description: Este artigo descreve como gerir atualizações para máquinas virtuais do Azure.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 66d50c94f2aad15e0d4a1b7500e8a4aeb45b1742
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214258"
---
# <a name="manage-updates-for-multiple-machines"></a>Gerir atualizações de várias máquinas

Pode utilizar a solução de gestão de atualizações para gerir as atualizações e correções de erros de máquinas virtuais Windows e Linux. Na sua conta da [Automatização do Azure](automation-offering-get-started.md), pode:

- Integrar as máquinas virtuais
- Avaliar o estado das atualizações disponíveis
- Agendar a instalação de atualizações necessárias
- Reveja os resultados de implementação para verificar que as atualizações foram aplicadas com êxito para todas as máquinas virtuais para que a gestão de atualizações está ativada

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a gestão de atualizações, tem de:

- Uma conta Run As de Automatização. Para saber como criar um, consulte [introdução da automatização do Azure](automation-offering-get-started.md).
- Uma máquina virtual ou um computador que tenha instalado um dos sistemas operativos suportados.

## <a name="supported-operating-systems"></a>Sistemas operativos suportados

Gestão de atualizações é suportada nos seguintes sistemas operativos:

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Suporta apenas atualizar avaliações.         |
|Windows Server 2008 R2 SP1 e posterior     |É necessário o Windows PowerShell 4.0 ou posterior. ([Transferir WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))</br> 5.1 do Windows PowerShell é recomendada para uma maior fiabilidade. ([Transferir WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) e 7 (x64)      | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|Red Hat Enterprise 6 (x86/x64) e 7 (x64)     | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|SUSE Linux Enterprise Server 11 (x86/x64) e 12 (x64)     | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|Ubuntu 12.04 LTS, 14.04 LTS e 16.04 LTS (x86/x64)      |Os agentes do Linux têm de ter acesso a um repositório de atualização.         |

> [!NOTE]
> Para evitar que as atualizações sejam aplicadas fora de uma janela de manutenção no Ubuntu, reconfigure o pacote Unattended-Upgrade para desativar as atualizações automáticas. Para obter informações, veja [Automatic Updates topic in the Ubuntu Server Guide](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) (Tópico de Atualizações Automáticas no Guia do Ubuntu Server).

Os agentes do Linux têm de ter acesso a um repositório de atualização.

Esta solução não suporta um agente do Operations Management Suite (OMS) para o Linux que está configurado para o relatório para várias áreas de trabalho do Log Analytics do Azure.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Ative a gestão de atualização para máquinas virtuais do Azure

No portal do Azure, abra a sua conta de automatização e, em seguida, selecione **gestão de atualizações**.

Selecione **adicionar a VM do Azure**.

![Adicionar o separador de VM do Azure](./media/manage-update-multi/update-onboard-vm.png)

Selecione uma máquina virtual para carregar. 

Em **ativar a gestão de atualização**, selecione **ativar** integrar a máquina virtual.

![Caixa de diálogo Ativar Gestão de Atualizações](./media/manage-update-multi/update-enable.png)

Quando a integração for concluída, a gestão de atualizações está ativada para a máquina virtual.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Ative a gestão de atualização para máquinas virtuais do Azure não e computadores

Para saber como ativar a gestão de atualizações para as máquinas virtuais do Azure de Windows e computadores, consulte [computadores Windows ligar para o serviço de análise de registos do Azure](../log-analytics/log-analytics-windows-agent.md).

Para saber como ativar a gestão de atualizações para as máquinas virtuais Linux não do Azure e computadores, consulte [ligar os computadores com Linux a análise de registos](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Computadores de vista ligados a sua conta de automatização

Depois de ativar a gestão de atualizações para as suas máquinas, pode ver as informações da máquina, selecionando **computadores**. Pode ver informações *nome da máquina*, *estado de conformidade*, *ambiente*, *tipo de SO*, *críticos e atualizações de segurança instaladas*, *outras atualizações instaladas*, e *preparação de agente de atualização* para os seus computadores.

  ![Separador Ver computadores](./media/manage-update-multi/update-computers-tab.png)

Computadores recentemente tem sido ativadas para a gestão de atualizações podem não ter sido avaliado em matéria ainda. O estado de estado de conformidade para os computadores é **não avaliado**. Eis uma lista de valores possíveis para o estado de compatibilidade:

- **Em conformidade**: computadores que estão em falta não críticos ou atualizações de segurança.

- **Não compatível**: computadores que estão em falta, pelo menos, um críticas ou de atualização de segurança.

- **Não é avaliada**: os dados da avaliação de atualização não foi recebidos do computador durante o período de tempo esperado. Para computadores Linux, o período de tempo esperado é nas últimas 3 horas. Para computadores Windows, o período de tempo esperado é nas últimas 12 horas.

Para ver o estado do agente, selecione a hiperligação no **preparação de agente de ATUALIZAÇÃO** coluna. A seleção desta opção abre o **Worker híbrido** painel e mostra o estado do Worker híbrido. A imagem seguinte mostra um exemplo de um agente que ainda não foi ligado a gestão de atualizações por um longo período de tempo:

![Separador Ver computadores](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Ver avaliações de atualizações

Depois de ativar a gestão de atualizações, o **gestão de atualizações** abre o painel. Pode ver uma lista de atualizações em falta no separador **Atualizações em falta**.

## <a name="collect-data"></a>Recolher dados

Os agentes que estão instalados nos computadores e máquinas virtuais recolhem dados sobre atualizações. Os agentes de enviam os dados para a gestão de atualizações do Azure.

### <a name="supported-agents"></a>Agentes suportados

A tabela seguinte descreve as origens ligadas que são suportadas por esta solução:

| Origem ligada | Suportadas | Descrição |
| --- | --- | --- |
| Agentes do Windows |Sim |Gestão de atualizações recolhe informações sobre atualizações do sistema de agentes do Windows e, em seguida, inicia a instalação de atualizações necessárias. |
| Agentes do Linux |Sim |Gestão de atualizações recolhe informações sobre atualizações do sistema de agentes Linux e, em seguida, inicia a instalação de atualizações necessárias no distribuições suportadas. |
| Grupo de gestão do Operations Manager |Sim |Gestão de atualizações recolhe informações sobre atualizações do sistema de agentes no grupo de gestão ligado. |
| Conta de armazenamento do Azure |Não |Armazenamento do Azure não inclui informações sobre as atualizações do sistema. |

### <a name="collection-frequency"></a>Frequência da recolha

Uma análise é executada duas vezes por dia para cada computador gerido do Windows. A cada 15 minutos, a API do Windows denomina-se a consulta para a hora da última atualização determinar se o estado mudou. Se alterar o estado, inicia uma análise de compatibilidade. Uma análise é executada a cada 3 horas para cada computador Linux gerido.

Pode demorar entre 30 minutos e 6 horas para o dashboard apresentar dados atualizados a partir de computadores geridos.

## <a name="schedule-an-update-deployment"></a>Agendar uma implementação de atualizações

Para instalar atualizações, agende uma implementação que está alinhada com a janela de agendamento e o serviço de versão. Pode escolher quais os tipos de atualização a incluir na implementação. Por exemplo, pode incluir atualizações de segurança ou críticas e excluir update rollups.

Para agendar uma nova implementação de atualização para um ou mais máquinas virtuais, em **gestão de atualizações**, selecione **implementação de atualização de agendamento**.

No **novo atualizar implementação** painel, especifique as seguintes informações:

- **Nome**: introduza um nome exclusivo para identificar a implementação da atualização.
- **Sistema operativo**: selecione **Windows** ou **Linux**.
- **Máquinas para atualizar**: selecione as máquinas virtuais que pretende atualizar. A preparação da máquina é apresentada no **preparação de agente de ATUALIZAÇÃO** coluna. Pode ver o estado de funcionamento da máquina antes de agendar a implementação da atualização.

  ![Novo painel de implementação de atualização](./media/manage-update-multi/update-select-computers.png)

- **Classificação da atualização**: selecione os tipos de software para incluir na implementação de atualização. Para obter uma descrição dos tipos de classificação, consulte [classificações de atualização](automation-update-management.md#update-classifications). Os tipos de classificação são:
  - Atualizações críticas
  - Atualizações de segurança
  - Update rollups
  - Pacotes de funcionalidades
  - Service packs
  - Atualizações de definições
  - Ferramentas
  - Atualizações

- **Atualizações para excluir**: a seleção desta opção abre o **excluir** página. Introduza nomes de pacote para excluir ou artigos da KB.

- **Definições da agenda**: pode aceitar a data e hora predefinidas, que é 30 minutos após a hora atual. Também pode especificar uma hora diferente.

   Também pode especificar se a implementação ocorre uma vez ou de acordo com um agendamento periódico. Para configurar uma agenda periódica, em **periodicidade**, selecione **periódica**.

   ![Caixa de diálogo Definições de Agendamento](./media/manage-update-multi/update-set-schedule.png)
- **Janela de manutenção (minutos)**: Especifique o período de tempo que pretende que a implementação da atualização para ocorrer. Esta definição ajuda a garantir que as alterações são realizadas nos seus períodos de administração definidos.

Quando tiver terminado de configurar a agenda, selecione o **criar** botão para regressar ao dashboard de estado. O **agendada** tabela mostra a agenda de implementação que criou.

> [!WARNING]
> A máquina virtual para as atualizações que requerem o reinício, reinicie automaticamente.

## <a name="view-results-of-an-update-deployment"></a>Ver resultados de uma implementação de atualização

Após a implementação planeada é iniciada, pode ver o estado dessa implementação no **implementações de atualizações** separador em **gestão de atualizações**.

Se a implementação estiver em execução, o estado será **Em curso**. Após a conclusão da implementação com êxito, o estado é alterado para **com êxito**.

Se uma ou mais atualizações falharem na implementação, o estado é **Falha parcial**.

![Estado da implementação de atualizações](./media/manage-update-multi/update-view-results.png)

Para ver o dashboard relativo a uma implementação de atualizações, selecione a implementação concluída.

O **atualizar resultados** painel mostra o número total de atualizações e os resultados da implementação da máquina virtual. A tabela à direita fornece uma repartição detalhada de cada atualização e os resultados da instalação. Os resultados da instalação podem ser um dos seguintes valores:

- **Não foi efetuada uma tentativa**: A atualização não foi instalada porque tempo insuficiente não estava disponível com base na janela de manutenção definidas.
- **Foi concluída com êxito**: A atualização foi bem-sucedida.
- **Não foi possível**: A atualização falhou.

Para ver todas as entradas de registo que a implementação criou, selecione **Todos os registos**.

Para ver a sequência de tarefa do runbook que gere a implementação da atualização na máquina virtual de destino, selecione o mosaico de saída.

Para ver informações detalhadas sobre os erros da implementação, selecione **Erros**.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre a gestão de atualização, incluindo os registos, de saída e de erros, veja [solução de gestão de atualizações no Azure](../operations-management-suite/oms-solution-update-management.md).
