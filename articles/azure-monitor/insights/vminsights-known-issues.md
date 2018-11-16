---
title: Problemas de Monitor do Azure para conhecido de VMs (pré-visualização) | Documentos da Microsoft
description: Monitor do Azure para VMs é uma solução no Azure que combina o estado de funcionamento e desempenho de monitorização do sistema de operacional de VM do Azure, bem como detetar automaticamente os componentes da aplicação e dependências com outros recursos e mapeia a comunicação entre -los. Este artigo aborda problemas conhecidos.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/07/2018
ms.author: magoedte
ms.openlocfilehash: 99f84e9784c448091c0257218855c3bf32c2f8f4
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51715837"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Problemas conhecidos com o Azure Monitor para VMs (pré-visualização)

Os seguintes são problemas conhecidos com a funcionalidade de estado de funcionamento do Monitor do Azure para VMs:

- A funcionalidade de estado de funcionamento está ativada para todas as VMs ligadas à área de trabalho do Log Analytics, mesmo quando é iniciada e concluída a partir de uma VM única.
- Se após a desativação da monitorização para uma VM com os métodos com suporte, quando a integração é tentada novamente, ele deve ser feito para a mesma área de trabalho.  Se for utilizada uma nova área de trabalho, ao visualizar o estado de funcionamento para essa VM, pode mostrar a comportamentos anómalos.
- Se uma VM do Azure não existe mais porque foi removido ou eliminado, irá aparecer na vista de lista de VM para três a sete dias. Além disso, ao clicar no estado de uma VM removida ou eliminada iniciaria o **diagnóstico de estado de funcionamento** exibição para ela, que, em seguida, entra num loop de carregamento. Selecionar o nome de uma VM eliminada inicia um painel com uma mensagem a indicar que a VM tiver sido eliminada.
- Com esta versão, não não possível modificar o período de tempo e a frequência dos critérios de estado de funcionamento. 
- Critérios de estado de funcionamento não podem ser desativados. 
- Após a integração, pode demorar tempo antes de dados são apresentados no Azure Monitor -> máquinas virtuais -> Estado de funcionamento ou a partir do painel de recursos do VM -> Insights
- Diagnóstico de estado de funcionamento experiência atualizações mais rapidamente do que qualquer exibição, portanto, ao alternar entre modos de exibição pode vir a ter atrasos de informações  
- Resumo de alertas incluído com o Azure Monitor para a VM é apenas para alertas acionados para os problemas de estado de funcionamento detetados com VMs do Azure monitorizado.
- O **lista de alertas** vista cuja condição do monitor de alertas de página no único mostra a VM e o Azure Monitor estiver definida para "acionada" nos últimos 30 dias.  Não são configuráveis. No entanto, depois de clicar em Resumo, uma vez a **lista de alerta** página de exibição é iniciada, pode alterar o valor do filtro de ambos os o monitor condição e o tempo de intervalo.
- No **lista de alertas** página de exibição, sugerimos que não modifica a **tipo de recurso**, **recursos**, e **serviço Monitor** filtros à medida que foram configuradas específicas para a solução (esta vista de lista mostra que alguns campos adicionais em comparação com o Azure monitor -> Vista de lista de alertas).    
- Em VMs do Linux **diagnóstico de estado de funcionamento** view tem o nome de domínio completo da VM em vez do nome VM definido pelo utilizador.
- Encerrar as VMs serão atualizados alguns dos respetivos critérios de estado de funcionamento para um estado crítico e outras pessoas para um bom estado de funcionamento com o estado de rede da VM num estado crítico.
- Não é possível alterar a gravidade do alerta de estado de funcionamento, eles só podem ser ativados ou desativados.  Além disso, algumas actualizações gravidades com base no estado de critérios de estado de funcionamento.
- Modificar qualquer definição de uma instância de critério de estado de funcionamento, levará a modificação da mesma definição de todas as instâncias de critérios de estado de funcionamento do mesmo tipo na VM. Por exemplo, se o limiar do disco livre de instância de critério de estado de funcionamento do espaço correspondente ao disco lógico é modificado c:, então este limiar será aplicada a todos os outros discos lógicos detetados e monitorizados para a mesma VM.   
- Limiares para os seguintes critérios de estado de funcionamento visando uma VM do Windows não são fáceis de serem modificados, uma vez que os respetivos Estados de funcionamento já estão definidos para **em execução** ou **disponível**. Quando consultados a partir do [API do Monitor de carga de trabalho](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), o mostra de estado de funcionamento a *comparisonOperator* valor de **LessThan** ou **GreaterThan**com um *limiar* valor de **4** para a entidade ou o serviço se:
   - Integridade do serviço cliente DNS – serviço não está em execução 
   - Integridade do serviço cliente DHCP – serviço não está em execução 
   - Estado de funcionamento do serviço RPC – serviço não está em execução 
   - Estado de funcionamento do serviço do firewall de Windows – serviço não está em execução
   - Estado de funcionamento do serviço do registo de eventos de Windows – serviço não está em execução 
   - Estado de funcionamento do servidor service – serviço não está em execução 
   - Funcionamento de serviço de gestão remota do Windows – serviço não está em execução 
   - Erro de sistema de ficheiros ou corrupção – disco lógico não está disponível

- Limiares para os seguintes critérios de estado de funcionamento do Linux não são fáceis de serem modificados, desde o respetivo estado de funcionamento já estão definidos para **true**.  O mostra de estado de funcionamento a *comparisonOperator* com um valor **LessThan** e *limiar* valor **1** quando consultados a partir da carga de trabalho API de monitorização para a entidade dependendo de seu contexto:
   - Estado do disco lógico – disco lógico não está online / disponíveis
   - Estado do disco – disco não está online / disponíveis
   - Estado do adaptador de rede - placa de rede está desativado  

- **Total de utilização da CPU** critério de estado de funcionamento no Windows mostra um limiar de **não é igual a 4** do portal e quando consultados a partir da API de monitorização da carga de trabalho quando a utilização da CPU é superior a 95% e é o comprimento da fila de sistema maior do que 15. Não é possível modificar este critério de estado de funcionamento nesta versão.  
- Alterações de configuração, como atualizar um limite, demora até 30 minutos para entrar em vigor mesmo que o portal ou a API de Monitor da carga de trabalho pode atualizar imediatamente.  
- Processador individual e os critérios de estado de funcionamento ao nível do processador lógico não estão disponíveis no Windows, só **utilização da CPU Total** está disponível para VMs do Windows.  
- Regras de alertas definidas para cada critério de estado de funcionamento não são expostas no portal do Azure. Apenas são configuráveis a partir da [API do Monitor de carga de trabalho](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager) para ativar ou desativar uma regra de alerta de estado de funcionamento.  
- Atribuir um [grupo de ação do Azure Monitor](../../monitoring-and-diagnostics/monitoring-action-groups.md) para Estado de funcionamento alertas não é possível a partir do portal do Azure. Só pode utilizar a API de definição de notificação para configurar um grupo de ação para ser acionada sempre que é acionado um alerta de estado de funcionamento. Atualmente, os grupos de ação podem ser atribuídos numa VM, que todos os *alertas de estado de funcionamento* disparado contra o acionador VM, os mesmos grupos de ação. Não há nenhum conceito de um grupo de ação separada para cada regra de alerta de estado de funcionamento, como o tradicionais de alertas do Azure. Além disso, apenas os grupos de ação configurados para enviar notificações, enviando um e-mail ou SMS são suportados quando são acionados alertas de estado de funcionamento. 

## <a name="next-steps"></a>Passos Seguintes
Revisão [carregar Monitor do Azure para VMs](vminsights-onboard.md) para compreender os requisitos e métodos para ativar a monitorização das suas máquinas virtuais.