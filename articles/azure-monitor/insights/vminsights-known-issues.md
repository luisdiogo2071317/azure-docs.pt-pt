---
title: O Azure Monitor para problemas conhecidos VMs (pré-visualização) | Documentos da Microsoft
description: Este artigo aborda problemas conhecidos com o Azure Monitor para as VMs, uma solução no Azure que combina o estado de funcionamento e a monitorização do desempenho do sistema de operacional de VM do Azure. Monitor do Azure para VMs também automaticamente Deteta componentes da aplicação e dependências com outros recursos e mapeia a comunicação entre eles.
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
ms.date: 12/26/2018
ms.author: magoedte
ms.openlocfilehash: c329e1fa80c6647bb78b11917ecd012461e62ea4
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790508"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Problemas conhecidos com o Azure Monitor para VMs (pré-visualização)

Este artigo aborda problemas conhecidos com o Azure Monitor para as VMs, uma solução no Azure que combina o estado de funcionamento e a monitorização do desempenho do sistema de operacional de VM do Azure. 

Os seguintes são problemas conhecidos com a funcionalidade de estado de funcionamento:

- A funcionalidade de estado de funcionamento está ativada para todas as VMs que estão ligadas à área de trabalho do Log Analytics. É portanto, mesmo quando a ação começa e termina numa única VM.
- Para VMs do Linux, o título da página listando os critérios de estado de funcionamento para uma única vista VM com o nome de domínio completo da VM em vez do nome VM definido pelo utilizador.  
- Depois de desativar a monitorização para uma VM ao utilizar os métodos com suporte e tentar implementar novamente, deve implantá-lo na mesma área de trabalho. Se utilizar uma nova área de trabalho e tente para ver o estado de funcionamento para essa VM, poderá mostrar comportamento anómalo.
- Se uma VM do Azure é removida ou eliminada, é apresentada na vista de lista de VM durante algum tempo. Além disso, o estado de uma VM removida ou eliminada de clicar abre o **diagnóstico de estado de funcionamento** ver e, em seguida, inicia um ciclo de carregamento. Selecionar o nome da VM eliminada é aberto um painel com uma mensagem a indicar que a VM tiver sido eliminada.
- Com esta versão, não não possível modificar o período de tempo e a frequência dos critérios de estado de funcionamento. 
- Critérios de estado de funcionamento não podem ser desativados. 
- Após a implementação, pode demorar tempo antes de dados são apresentados no **do Azure Monitor** > **máquinas virtuais** > **estado de funcionamento** painel ou o  **Recurso de VM** > **Insights** painel.
- O diagnóstico de estado de funcionamento experiência mais rapidamente do que qualquer outro tipo de vista de atualizações. As informações podem estar atrasadas quando alternar entre modos de exibição. 
- O resumo de alertas incluído com o Azure Monitor para a VM apresenta apenas os alertas que o resultado a partir de problemas de estado de funcionamento detetados com monitorizada VMs do Azure.
- Encerrar as VMs de atualizações de alguns dos critérios de estado de funcionamento para *críticas* e outras pessoas *bom estado de funcionamento*. O estado VM líquido é apresentado como *críticos*.
- Não é possível alterar a gravidade do alerta de estado de funcionamento, eles só podem ser ativados ou desativados. Além disso, algumas actualizações gravidades com base no estado de critérios de estado de funcionamento.   
- Se modificar qualquer definição de uma instância de critério de estado de funcionamento, todas as instâncias de critérios de estado de funcionamento do mesmo tipo na VM são modificadas. Por exemplo, se o limiar da instância de critério de estado de funcionamento do espaço livre de disco que corresponde ao disco lógico c: for modificado, este limite aplica-se a todos os outros discos lógicos que são detetados e monitorizados para a mesma VM.  
- Os limites de critérios de estado de funcionamento que se destinam uma VM do Windows não são fáceis de serem modificados, porque os Estados de funcionamento estão definidos como *em execução* ou *disponível*. Quando consulta o estado de funcionamento do [API do Monitor de carga de trabalho](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), ele exibe a *comparisonOperator* valor de **LessThan** ou **GreaterThan** com um *limiar* o valor de **4** para a entidade ou o serviço se:
   - Estado de funcionamento do serviço do cliente DNS – serviço não está em execução. 
   - Estado de funcionamento do serviço do cliente DHCP – serviço não está em execução. 
   - Estado de funcionamento do serviço RPC – serviço não está em execução. 
   - Estado de funcionamento do serviço do firewall de Windows – serviço não está em execução.
   - Estado de funcionamento do serviço do registo de eventos de Windows – serviço não está em execução. 
   - Estado de funcionamento do servidor service – serviço não está em execução. 
   - Funcionamento de serviço de gestão remota do Windows – serviço não está em execução. 
   - Erro de sistema de ficheiros ou corrupção – disco lógico não está disponível.

- Limiares para os seguintes critérios de estado de funcionamento do Linux não são fáceis de serem modificados, porque o respetivo estado de funcionamento já está definido como *true*. Apresenta o estado de funcionamento a *comparisonOperator* com um valor **LessThan** e *limiar* valor de **1** quando consultados a partir do Carga de trabalho monitorização o API para a entidade, dependendo de seu contexto:
   - Estado do disco lógico – o disco lógico não está online / disponíveis
   - Estado do disco – o disco não está online / disponíveis
   - Estado do adaptador de rede - placa de rede está desativado  

- Alterações de configuração, como atualizar um limite, demorar até 30 minutos, mesmo que o portal ou a API de Monitor da carga de trabalho pode atualizá-las imediatamente. 
- Processador individual e os critérios de estado de funcionamento ao nível do processador lógico não estão disponíveis no Windows. Apenas a utilização Total de CPU está disponível para VMs do Windows. 
- Regras de alerta que estão definidas para cada critério de estado de funcionamento não são apresentadas no portal do Azure. Pode ativar ou desativar um alerta de estado de funcionamento apenas na regra a [API do Monitor de carga de trabalho](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components). 
- Não é possível atribuir um [grupo de ação do Azure Monitor](../../azure-monitor/platform/action-groups.md) para alertas de estado de funcionamento no portal do Azure. Pode utilizar apenas a API de definição de notificação para configurar um grupo de ação para ser acionada sempre que é acionado um alerta de estado de funcionamento. Atualmente, pode atribuir grupos de ação em relação a uma VM para que todos os *alertas de estado de funcionamento* disparado contra o acionador VM, os mesmos grupos de ação. Ao contrário dos alertas do Azure tradicionais, não há conceito de um grupo de ação separada para cada regra de alerta de estado de funcionamento. Além disso, apenas os grupos de ação que estão configurados para fornecer e-mail ou notificações por SMS são suportados quando são acionados alertas de estado de funcionamento. 

## <a name="next-steps"></a>Passos Seguintes
Para compreender os requisitos e métodos para ativar a monitorização das suas máquinas virtuais, consulte [implementar o Azure Monitor para VMs](vminsights-onboard.md).
