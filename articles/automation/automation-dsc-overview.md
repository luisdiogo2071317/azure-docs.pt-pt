---
title: Descrição geral de DSC de automatização do Azure
description: Uma descrição geral do Azure Automation Desired State Configuration (DSC), seus termos e problemas conhecidos
keywords: PowerShell dsc, configuração de estado pretendido, azure dsc de powershell
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f6d49ffa59ed53c0a1966a4132fd5fe1689a13ce
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247354"
---
# <a name="azure-automation-dsc-overview"></a>Descrição geral de DSC de automatização do Azure

DSC de automatização do Azure é um serviço do Azure que permite-lhe escrever, gerir e compilar PowerShell Desired State Configuration (DSC) [configurações](https://msdn.microsoft.com/powershell/dsc/configurations), importar [recursos de DSC](https://msdn.microsoft.com/powershell/dsc/resources)e atribuir as configurações para nós de destino, tudo na cloud.

## <a name="why-use-azure-automation-dsc"></a>Porquê utilizar o DSC de automatização do Azure

DSC de automatização do Azure proporciona várias vantagens com o uso de DSC fora do Azure.

### <a name="built-in-pull-server"></a>Servidor de solicitação incorporada

Automatização do Azure fornece um servidor de solicitação de DSC semelhante para o [DSC-serviço do Windows funcionalidade](/powershell/dsc/pullserver) para que nós de destino recebem automaticamente as configurações, está em conformidade com o estado pretendido e devolver relatórios sobre a compatibilidade dos mesmos.
O servidor de solicitação incorporada na automatização do Azure elimina a necessidade de configurar e manter o seu próprio servidor de solicitação.
A automatização do Azure pode visar máquinas físicas ou virtuais Windows ou Linux, na cloud ou no local.

### <a name="management-of-all-your-dsc-artifacts"></a>Gestão de todos os seus artefactos de DSC

Azure Automation DSC traz a mesma camada de gerenciamento para [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) como a automatização do Azure fornece para scripts do PowerShell.

No portal do Azure ou no PowerShell, pode gerir todos os seus DSC configurações, recursos e nós de destino.

![Captura de ecrã do painel de automatização do Azure](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importar dados de relatórios para o Log Analytics

Nós que são geridos com o Azure Automation DSC enviam dados de estado de geração de relatórios detalhados para o servidor de solicitação incorporada.
Pode configurar o DSC de automatização do Azure para enviar estes dados para a área de trabalho do Log Analytics.
Para saber como enviar dados de estado de DSC para sua área de trabalho do Log Analytics, veja [reencaminhar o DSC de automatização do Azure a comunicar dados para o Log Analytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Vídeo de introdução

Prefere ver do que ler? Dê uma olhada no vídeo seguinte de Maio de 2015, quando DSC de automatização do Azure foi anunciada pela primeira vez.

>[!NOTE]
>Embora os conceitos e o ciclo de vida descritos neste vídeo estiverem corretos, DSC de automatização do Azure tem progredido muito desde que este vídeo foi gravado.
>Agora está disponível em geral, tem uma interface do Usuário muito mais extensa no portal do Azure e oferece suporte a muitos recursos adicionais.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Passos Seguintes

* Para saber como a carregar nós para serem geridos com o DSC de automatização do Azure, consulte [integrar computadores para gestão pelo DSC de automatização do Azure](automation-dsc-onboarding.md)
* Para começar a utilizar o DSC de automatização do Azure, veja [introdução ao DSC de automatização do Azure](automation-dsc-getting-started.md)
* Para obter informações sobre como compilar configurações de DSC para que pode atribuí-las a nós de destino, consulte [compilar configurações no DSC de automatização do Azure](automation-dsc-compile.md)
* Para referência de cmdlet do PowerShell para DSC de automatização do Azure, consulte [cmdlets de DSC de automatização do Azure](/powershell/module/azurerm.automation/#automation)
* Para obter informações sobre preços, consulte [preço de DSC de automatização do Azure](https://azure.microsoft.com/pricing/details/automation/)
* Para ver um exemplo de como utilizar o DSC de automatização do Azure num pipeline de implementação contínua, consulte [implementação contínua para o IaaS VMs a utilizar o Azure Automation DSC e Chocolatey](automation-dsc-cd-chocolatey.md)
