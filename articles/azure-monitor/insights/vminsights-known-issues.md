---
title: O Azure Monitor para problemas conhecidos VMs (pré-visualização) | Documentos da Microsoft
description: Este artigo aborda problemas conhecidos com o Azure Monitor para as VMs, uma solução no Azure que combina o estado de funcionamento, a deteção de dependências de aplicativo e a monitorização do desempenho do sistema de operacional de VM do Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2019
ms.author: magoedte
ms.openlocfilehash: 677fec21b7491398da5e4958441e5405e0c10e0e
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745678"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Problemas conhecidos com o Azure Monitor para VMs (pré-visualização)

Este artigo aborda problemas conhecidos com o Azure Monitor para as VMs, uma solução no Azure que combina o estado de funcionamento, a deteção de componentes da aplicação e a monitorização do desempenho do sistema de operacional de VM do Azure. 

## <a name="health"></a>Estado de Funcionamento 
Os seguintes são problemas conhecidos da versão atual do recurso de estado de funcionamento:

- O painel de propriedades VM apresenta o sistema de operativo do Windows Server 2019 como Windows Server 2016. Isso será corrigido numa versão futura.
- Se uma VM do Azure é removida ou eliminada, é apresentada na vista de lista de VM durante algum tempo. Além disso, o estado de uma VM removida ou eliminada de clicar abre o **diagnóstico de estado de funcionamento** ver e, em seguida, inicia um ciclo de carregamento. Selecionar o nome da VM eliminada é aberto um painel com uma mensagem a indicar que a VM tiver sido eliminada.
- Alterações de configuração, como atualizar um limite, demorar até 30 minutos, mesmo que o portal ou a API de Monitor da carga de trabalho pode atualizá-las imediatamente. 
- O diagnóstico de estado de funcionamento experiência mais rapidamente do que as outras vistas de atualizações. As informações podem estar atrasadas quando alternar entre elas. 
- Encerrar as VMs de atualizações de alguns dos critérios de estado de funcionamento para *críticas* e outras pessoas *bom estado de funcionamento*. O estado VM líquido é apresentado como *críticos*.
- Para VMs do Linux, o título da página listando os critérios de estado de funcionamento para uma única vista VM com o nome de domínio completo da VM em vez do nome VM definido pelo utilizador. 
- Depois de desativar a monitorização para uma VM através de um dos métodos suportados e tentar implementar novamente, deve implantá-lo na mesma área de trabalho. Se optar por uma área de trabalho diferente e tente ver o estado de funcionamento para essa VM, poderá mostrar comportamento inconsistente.
- Total critério de estado de funcionamento de utilização da CPU para o Windows mostra um limiar de *não é igual a* **4**, a utilização de CPU do significado é superior a 95% e comprimento de fila de sistema for superior a 15. Esse critério de estado de funcionamento não é configurável incluídas nesta pré-visualização.  
- Depois de remover os componentes da solução da sua área de trabalho, pode continuar a ver o estado de funcionamento das VMs do Azure; especificamente mapa de dados de desempenho e ao navegar para a vista no portal. Dados, eventualmente, deixará de aparecer a partir da vista de desempenho e mapa após algum tempo; No entanto, a vista de estado de funcionamento irá continuar Mostrar o estado de funcionamento para as suas VMs. O **Experimente agora o** opção estará disponível para carregar para o re de vistas de desempenho e mapa.

## <a name="next-steps"></a>Passos Seguintes
Para compreender os requisitos e métodos para ativar a monitorização das suas máquinas virtuais, consulte [implementar o Azure Monitor para VMs](vminsights-onboard.md).
