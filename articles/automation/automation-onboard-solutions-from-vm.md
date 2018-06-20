---
title: Integrar soluções de gestão de atualizações, controlo de alterações e inventário de uma VM do Azure
description: Saiba como para carregar uma máquina virtual do Azure com soluções de gestão de atualizações, controlo de alterações e inventário que fazem parte da automatização do Azure.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: f270b2ccea51e83bc6475051b8667bf73d7fd717
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221517"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Integrar soluções de gestão de atualizações, controlo de alterações e inventário de uma máquina virtual do Azure

A automatização do Azure fornece soluções para o ajudar a gerir as atualizações de segurança do sistema operativo, controlar as alterações e de inventário que está instalado nos seus computadores. Existem várias formas de carregar máquinas. Pode carregar a solução de uma máquina virtual, [da sua conta de automatização](automation-onboard-solutions-from-automation-account.md), [da navegação várias máquinas](automation-onboard-solutions-from-browse.md), ou utilizando um [runbook](automation-onboard-solutions.md). Este artigo abrange integração estas soluções de uma máquina virtual do Azure.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="enable-the-solutions"></a>Ativar as soluções

Aceda a uma máquina virtual existente. Em **operações**, selecione **gestão de atualizações**, **inventário**, ou **de registo de alterações**.

Para ativar a solução para a VM, certifique-se de que **ativar para esta VM** está selecionada. Para carregar várias máquinas para a solução, selecione **ativar para VMs nesta subscrição**e, em seguida, selecione **clique para selecionar máquinas para ativar**. Para saber como carregar várias máquinas em simultâneo, consulte [soluções de integrar a gestão de atualizações, controlo de alterações e inventário](automation-onboard-solutions-from-automation-account.md).

Selecione a área de trabalho do Log Analytics do Azure e a conta de automatização e, em seguida, selecione **ativar** para ativar a solução. A solução demora até 15 minutos a ativar.

![Integrar a solução de gestão de atualizações](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Vá para as outras soluções e, em seguida, selecione **ativar**. A análise de registos e automatização conta na lista pendente está desativada, porque estas soluções utilizam o mesmo espaço de trabalho e a conta de automatização como solução de anteriormente ativada.

> [!NOTE]
> **Registo de alterações** e **inventário** utilizar a mesma solução. Quando uma destas soluções estiver ativada, o outro também está ativado.

## <a name="scope-configuration"></a>Configuração de âmbito

Cada solução utiliza uma configuração de âmbito na área de trabalho para os computadores que obter a solução de destino. A configuração do âmbito é um grupo de um ou mais pesquisas guardadas, que são utilizadas para limitar o âmbito da solução para computadores específicos. Para aceder as configurações de âmbito, na sua conta de automatização em **recursos relacionados**, selecione **área de trabalho**. Na área de trabalho, sob **ORIGENS de dados da área de trabalho**, selecione **configurações de âmbito**.

Se a área de trabalho selecionada ainda não tiver as soluções de gestão de atualizações ou controlo de alterações, são criadas as seguintes configurações de âmbito:

* **MicrosoftDefaultScopeConfig ChangeTracking**

* **Atualizações de MicrosoftDefaultScopeConfig**

Se a área de trabalho selecionada já está a solução, a solução não está implementada novamente e a configuração do âmbito não é adicionada.

Selecione as reticências (**...** ) em qualquer uma das configurações e, em seguida, selecione **editar**. No **Editar configuração de âmbito** painel, selecione **selecionar grupos de computador**. O **grupos de computadores** painel mostra as pesquisas guardadas que são utilizadas para criar a configuração do âmbito.

## <a name="saved-searches"></a>Pesquisas guardadas

Quando é adicionado um computador para a atualização soluções de gestão, controlo de alterações ou inventário, o computador é adicionado a uma das duas pesquisas guardadas na sua área de trabalho. As procuras guardadas são consultas que contêm os computadores que são direcionados para estas soluções.

Vá para a área de trabalho. Em **geral**, selecione **pesquisas guardadas**. As pesquisas guardadas dois que são utilizadas por estas soluções são apresentadas na tabela seguinte:

|Nome     |Categoria  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Atualizações        | Updates__MicrosoftDefaultComputerGroup         |

Selecione um das pesquisas guardadas para ver a consulta que é utilizada para preencher o grupo. A imagem seguinte mostra a consulta e os respetivos resultados:

![Pesquisas guardadas](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Passos Seguintes

Continue para os tutoriais para as soluções saber como utilizá-los:

* [Tutorial - Gerir atualizações para a VM](automation-tutorial-update-management.md)
* [Tutorial - identificar o software numa VM](automation-tutorial-installed-software.md)
* [Tutorial - alterações numa VM de resolução de problemas](automation-tutorial-troubleshoot-changes.md)
