---
title: Saiba como soluções de gestão de atualizações, controlo de alterações e inventário integrar uma Máquina Virtual do Azure
description: Saiba como para carregar uma Azure Virtual máquina com soluções de gestão de atualizações, controlo de alterações e inventário que fazem parte da automatização do Azure
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: f8c9cb33eb90232f5eb241add284f7ea7b64bc05
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Integrar soluções de gestão de atualizações, controlo de alterações e inventário de uma Máquina Virtual do Azure

A automatização do Azure fornece soluções para gerir atualizações de segurança do sistema operativo, controlar as alterações e de inventário que está instalado nos seus computadores. Existem várias formas de carregar máquinas, pode carregar a solução de uma máquina virtual, [da sua conta de automatização](automation-onboard-solutions-from-automation-account.md), ou por [runbook](automation-onboard-solutions.md). Este artigo abrange integração estas soluções de uma Máquina Virtual do Azure.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure https://portal.azure.com

## <a name="enable-the-solutions"></a>Ativar as soluções

Navegue para uma máquina virtual existente e selecione **gestão de atualizações**, **inventário**, ou **alterações** em **operações**.

Escolha a área de trabalho e a conta de automatização do Log Analytics e clique em **Ativar** para ativar a solução. A solução demora até 15 minutos a ativar.

![Carregar a solução de Atualização](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Navegue para as outras soluções e clique em **ativar**, a análise de registos e a conta de automatização caixas de lista pendente estão desativadas por utilizarem a mesma conta de área de trabalho e automatização como solução de anteriormente ativada.

![Carregar a solução de Atualização](media/automation-onboard-solutions-from-vm/onboard-solutions2.png)

> [!NOTE]
> **Registo de alterações** e **inventário** utilizar a mesma solução, quando um está ativado o outro está ativado, bem como.

## <a name="scope-configuration"></a>Configuração de âmbito

Cada solução utiliza uma configuração de âmbito na área de trabalho para os computadores que obter a solução de destino. A configuração do âmbito é um grupo de um ou mais pesquisas guardadas, o que é utilizado para limitar o âmbito da solução para computadores específicos. Para aceder às configurações de âmbito, na sua conta de automatização em **recursos relacionados**, selecione **área de trabalho** , em seguida, na área de trabalho em **ORIGENS de dados da área de trabalho**, Selecione **configurações de âmbito**.

As configurações de dois âmbito criadas por predefinição são **MicrosoftDefaultScopeConfig ChangeTracking** e **MicrosoftDefaultScopeConfig atualizações**.

Clique nas reticências (…) em qualquer um das configurações e selecione **editar**. No **Editar configuração de âmbito** página, selecione **selecionar grupos de computador** para abrir o **grupos de computadores** página. Esta página mostra as pesquisas guardadas que são utilizadas para criar a configuração do âmbito.

## <a name="saved-searches"></a>Pesquisas guardadas

Quando é adicionado um computador para a gestão de atualizações ou as soluções de controlo de alterações e de inventário, eles são adicionados a uma das duas pesquisas guardadas na sua área de trabalho. Estes procuras guardadas são consultas que contêm os computadores que são direcionados para estas soluções.

Navegue até à sua área de trabalho e selecione **pesquisas guardadas** em **geral**. As pesquisas guardadas dois utilizadas por estas soluções podem ser vistas na tabela seguinte:

|Nome     |Categoria  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Atualizações        | Updates__MicrosoftDefaultComputerGroup         |

Selecione a pesquisa guardada para ver a consulta utilizada para preencher o grupo. A imagem seguinte mostra a consulta e os respetivos resultados.

![Pesquisas guardadas](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Passos Seguintes

Continue para os tutoriais sobre as soluções para saber como utilizá-los.

* [Tutorial - Gerir atualizações para a VM](automation-tutorial-update-management.md)

* [Tutorial - identificar o software numa VM](automation-tutorial-installed-software.md)

* [Tutorial - alterações numa VM de resolução de problemas](automation-tutorial-troubleshoot-changes.md)
