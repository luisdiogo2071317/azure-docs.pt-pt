---
title: Saiba como integrar soluções de gestão de atualizações, controlo de alterações e inventário na automatização do Azure
description: Saiba como para carregar uma Azure Virtual máquina com soluções de gestão de atualizações, controlo de alterações e inventário que fazem parte da automatização do Azure
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: b3ff1991d76b3ab5b4b3c44f4fbe28fbb986d4bd
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Integrar soluções de gestão de atualizações, controlo de alterações e inventário

A automatização do Azure fornece soluções para gerir atualizações de segurança do sistema operativo, controlar as alterações e de inventário que está instalado nos seus computadores. Existem várias formas de carregar máquinas, pode carregar a solução [de uma máquina virtual](automation-onboard-solutions-from-vm.md), da sua conta de automatização ou por [runbook](automation-onboard-solutions.md). Este artigo abrange integração estas soluções da sua conta de automatização.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure https://portal.azure.com

## <a name="enable-solutions"></a>Ativar soluções

Navegue até à sua conta de automatização e selecione **inventário** ou **alterações** em **gestão de configuração**.

Escolha a área de trabalho de análise de registo e a conta de automatização e clique em **ativar** para ativar a solução. A solução demora até 15 minutos a ativar.

![Integrar soluções de inventário](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

A solução de Controlo de Alterações e Inventário fornece a capacidade de [controlar as alterações](automation-vm-change-tracking.md) e [inventário](automation-vm-inventory.md) nas suas máquinas virtuais. Neste passo, vai ativar a solução numa máquina virtual.

Quando a notificação de inclusão da solução de controlo de alterações e inventário estiver concluída, clique em **Gestão de Atualizações**, em **GESTÃO DE CONFIGURAÇÃO**.

A solução de Gestão de Atualizações permite gerir atualizações e correções para as VMs do Azure. Pode avaliar o estado das atualizações disponíveis, agendar a instalação de atualizações necessárias e rever os resultados de implementação para verificar se as atualizações foram aplicadas com êxito à VM. Esta ação ativada a solução para a VM.

Selecione **gestão de atualizações** em **gestão de ATUALIZAÇÕES**. A área de trabalho do Log Analytics selecionada é a mesma área de trabalho utilizado no passo anterior. Clique em **Ativar** para integrar a solução de Gestão de atualizações. A solução demora até 15 minutos a ativar.

![Solução de atualização carregar](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Configuração de âmbito

Cada solução utiliza uma configuração de âmbito na área de trabalho para os computadores que obter a solução de destino. A configuração do âmbito é um grupo de um ou mais pesquisas guardadas, o que é utilizado para limitar o âmbito da solução para computadores específicos. Para aceder às configurações de âmbito, na sua conta de automatização em **recursos relacionados**, selecione **área de trabalho**. Em seguida, na área de trabalho em **ORIGENS de dados da área de trabalho**, selecione **configurações de âmbito**.

As duas configurações de âmbito criada por predefinição **MicrosoftDefaultScopeConfig ChangeTracking** e **MicrosoftDefaultScopeConfig atualizações**.

## <a name="saved-searches"></a>Pesquisas guardadas

Quando é adicionado um computador para a gestão de atualizações ou as soluções de controlo de alterações e de inventário, eles são adicionados a uma das duas pesquisas guardadas na sua área de trabalho. Estes procuras guardadas são consultas que contêm os computadores que são direcionados para estas soluções.

Navegue até à sua conta de automatização e selecione **pesquisas guardadas** em **geral**. As pesquisas guardadas dois utilizadas por estas soluções podem ser vistas na tabela seguinte:

|Nome     |Categoria  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Atualizações        | Updates__MicrosoftDefaultComputerGroup         |

Selecione a pesquisa guardada para ver a consulta utilizada para preencher o grupo. A imagem seguinte mostra a consulta e os respetivos resultados:

![Pesquisas guardadas](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-an-azure-machine"></a>Carregar um computador do Azure

Da sua automatização conta Selecione **inventário** ou **alterações** em **gestão de configuração**, ou **gestão de atualizações** em **gestão de ATUALIZAÇÕES**.

Clique em **+ adicionar a VM do Azure**, selecione uma VM a partir da lista. No **gestão de atualizações** página, clique em **ativar**. Esta ação adiciona a VM atual para o grupo de computadores guardado pesquisa para a solução.

## <a name="onboard-a-non-azure-machine"></a>Carregar uma máquina não do Azure

Da sua automatização conta Selecione **inventário** ou **alterações** em **gestão de configuração**, ou **gestão de atualizações** em **gestão de ATUALIZAÇÕES**.

Clique em **máquina não do Azure de adicionar**. Esta ação abre uma nova janela do browser com as instruções sobre como instalar e configurar o Microsoft Monitoring Agent no computador para a máquina pode começar a Reporting Services para a solução de cópia de segurança. Se estiver a integração numa máquina que atualmente gerido pelo System Center Operations Manager, um novo agente não é necessário, as informações de área de trabalho são introduzidas para o agente existente.

## <a name="onboard-machines-in-the-workspace"></a>Carregar máquinas na área de trabalho

Da sua automatização conta Selecione **inventário** ou **alterações** em **gestão de configuração**, ou **gestão de atualizações** em **gestão de ATUALIZAÇÕES**.

Selecione **gerir máquinas**. Isto abre-se a **gerir máquinas** página. Esta página permite-lhe ativar a solução num conjunto de máquinas, todas as máquinas disponíveis, selecione ou ativar a solução para todas as máquinas atuais e ativá-lo em todas as máquinas futuras.

![Pesquisas guardadas](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="selected-machines"></a>Máquinas selecionadas

Para ativar a solução para uma ou mais máquinas, selecione **ativar em máquinas selecionadas** e clique em **adicionar** junto a cada máquina que pretende adicionar à solução. Esta tarefa adiciona os nomes de computador selecionado para o grupo de computadores guardado consulta de pesquisa para a solução.

### <a name="all-available-machines"></a>Todas as máquinas disponíveis

Para ativar a solução para todas as máquinas disponíveis, selecione **ativar em todas as máquinas disponíveis**. Isto desativa o controlo ao adicionar máquinas individualmente. Esta tarefa adiciona todos os nomes das máquinas para a área de trabalho para o grupo de computadores guardado consulta de pesquisa de relatórios.

### <a name="all-available-and-future-machines"></a>Todas as máquinas disponíveis e do futuras

Para ativar a solução para todas as máquinas disponíveis e todas as máquinas de futuras, selecione **ativar em todas as máquinas disponíveis e do futuras**. Esta opção elimina as pesquisas guardadas e configurações de âmbito da área de trabalho. Esta ação abre a solução para todas as máquinas de não do Azure que estiverem a relatar a área de trabalho e do Azure.

## <a name="next-steps"></a>Passos Seguintes

Continue para os tutoriais sobre as soluções para saber como utilizá-los.

* [Tutorial - Gerir atualizações para a VM](automation-tutorial-update-management.md)

* [Tutorial - identificar o software numa VM](automation-tutorial-installed-software.md)

* [Tutorial - alterações numa VM de resolução de problemas](automation-tutorial-troubleshoot-changes.md)