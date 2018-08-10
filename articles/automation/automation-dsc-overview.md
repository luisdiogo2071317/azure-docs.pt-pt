---
title: Descrição geral da configuração de estado da automatização do Azure
description: Uma descrição geral do Azure automatização Estado Configuration (DSC), seus termos e problemas conhecidos
keywords: PowerShell dsc, configuração de estado pretendido, azure dsc de powershell
services: automation
ms.service: automation
ms.component: dsc
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d2d46803f81b369f8f24a6f0e3c7f32955503e4a
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006066"
---
# <a name="azure-automation-state-configuration-overview"></a>Descrição geral da configuração de estado da automatização do Azure

A configuração de estado de automatização do Azure é um serviço do Azure que permite-lhe escrever, gerir e compilar PowerShell Desired State Configuration (DSC) [configurações](/powershell/dsc/configurations), importar [recursos de DSC](/powershell/dsc/resources), e Atribua as configurações de nós de destino, tudo na cloud.

## <a name="why-use-azure-automation-state-configuration"></a>Por que usar a configuração de estado de automatização do Azure

Configuração de estado da automatização do Azure fornece várias vantagens com o uso de DSC fora do Azure.

### <a name="built-in-pull-server"></a>Servidor de solicitação incorporada

Configuração de estado da automatização do Azure fornece um servidor de solicitação de DSC semelhante para o [DSC-serviço do Windows funcionalidade](/powershell/dsc/pullserver) para que nós de destino recebem automaticamente as configurações, está em conformidade com o estado pretendido e comunicar de volta a seus conformidade. O servidor de solicitação incorporada na automatização do Azure elimina a necessidade de configurar e manter o seu próprio servidor de solicitação. A automatização do Azure pode visar máquinas físicas ou virtuais Windows ou Linux, na cloud ou no local.

### <a name="management-of-all-your-dsc-artifacts"></a>Gestão de todos os seus artefactos de DSC

A configuração de estado de automatização do Azure traz a mesma camada de gerenciamento para [PowerShell Desired State Configuration](/powershell/dsc/overview) como a automatização do Azure fornece para scripts do PowerShell.

No portal do Azure ou no PowerShell, pode gerir todos os seus DSC configurações, recursos e nós de destino.

![Captura de ecrã do painel de automatização do Azure](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importar dados de relatórios para o Log Analytics

Nós que são geridos com a configuração de estado de automatização do Azure enviam dados de estado de geração de relatórios detalhados para o servidor de solicitação incorporada. Pode configurar a configuração de estado de automatização do Azure para enviar estes dados para a área de trabalho do Log Analytics. Para saber como enviar dados de estado de configuração de estado para a área de trabalho do Log Analytics, veja [reencaminhar Azure Automation configuração de estado reportar dados à Log nalytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Vídeo de introdução

Prefere ver do que ler? Dê uma olhada no vídeo seguinte de Maio de 2015, quando configuração de estado de automatização do Azure foi anunciada pela primeira vez.

> [!NOTE]
> Embora os conceitos e o ciclo de vida descritos neste vídeo estiverem corretos, a configuração de estado de automatização do Azure tem progredido muito desde que este vídeo foi gravado. Agora está disponível em geral, tem uma interface do Usuário muito mais extensa no portal do Azure e oferece suporte a muitos recursos adicionais.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Passos Seguintes

- Para começar a utilizar, consulte o artigo [introdução à configuração de estado de automatização do Azure](automation-dsc-getting-started.md)
- Para saber como a carregar nós, consulte [integrar computadores para gestão de configuração de estado de automatização do Azure](automation-dsc-onboarding.md)
- Para obter informações sobre como compilar configurações de DSC para que pode atribuí-las a nós de destino, consulte [compilar configurações na configuração de estado de automatização do Azure](automation-dsc-compile.md)
- Para referência de cmdlets do PowerShell, consulte [cmdlets de configuração de estado de automatização do Azure](/powershell/module/azurerm.automation/#automation)
- Para obter informações sobre preços, consulte [preços de configuração de estado de automatização do Azure](https://azure.microsoft.com/pricing/details/automation/)
- Para ver um exemplo do uso de configuração de estado de automatização do Azure num pipeline de implementação contínua, consulte [contínua através do Azure Automation estado de configuração da implementação e Chocolatey](automation-dsc-cd-chocolatey.md)