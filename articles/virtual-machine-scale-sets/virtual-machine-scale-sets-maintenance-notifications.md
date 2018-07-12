---
title: Manipular notificações de manutenção para conjuntos de dimensionamento de máquinas virtuais no Azure | Documentos da Microsoft
description: Ver notificações de manutenção para conjuntos de dimensionamento de máquinas virtuais no Azure e comece a manutenção self-service.
services: virtual-machine-scale-sets
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: shants
ms.openlocfilehash: 4ce984686c2bb320d5d32771d31b81cdec1153af
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38314407"
---
# <a name="handling-planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Notificações de manutenção planeada de tratamento para conjuntos de dimensionamento de máquinas virtuais

O Azure realiza periodicamente atualizações para melhorar a fiabilidade, o desempenho e a segurança da infraestrutura de anfitrião para máquinas virtuais. As atualizações são as alterações, como o ambiente de alojamento de aplicação de patches ou atualizar e desativar o hardware. A maioria destas atualizações são efetuadas sem nenhum impacto para as máquinas virtuais alojadas. No entanto, existem casos em que as atualizações têm um impacto:

- Se a manutenção não requer um reinício, o Azure utiliza migração no local para colocar em pausa a VM, enquanto o anfitrião está atualizado. Essas operações de manutenção não rebootful são o domínio de falha aplicada por domínio de falha e o progresso é interrompido se qualquer sinais de estado de funcionamento de aviso são recebidas.

- Se a manutenção requer uma reinicialização, receberá um aviso de quando a manutenção está prevista. Nestes casos, é-lhe dada uma janela de tempo em que pode iniciar a manutenção por conta própria, quando ele funciona melhor para si.


Manutenção planeada, que requer uma reinicialização, é agendada em etapas. Cada fase tem escopo diferente (regiões).

- Uma onda começa com uma notificação para os clientes. Por predefinição, a notificação é enviada para o proprietário da subscrição e os coproprietários. Pode adicionar mais opções de mensagens, como e-mail, SMS e Webhooks e os destinatários de notificações através do Azure [alertas de registo de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).  
- No momento da notificação, um *janela de self-service* é disponibilizado. Durante este período, pode encontrar a quais das suas máquinas virtuais estão incluídos nessa onda e iniciar proativamente a manutenção, de acordo com suas necessidades de agendamento.
- Depois da janela de self-service, um *janela de manutenção agendada* começa. Em algum momento durante este período, o Azure agendas e aplica-se a manutenção necessária à máquina virtual. 

O objetivo de ter duas janelas é para lhe dar tempo suficiente para iniciar a manutenção e reiniciar a máquina de virtual sabendo quando o Azure irá automaticamente iniciar manutenção.


Pode utilizar o portal do Azure, PowerShell, a REST API, e a CLI para consultar as janelas de manutenção para o dimensionamento de máquinas virtuais VMs do conjunto e iniciar a manutenção self-service.

  
## <a name="should-you-start-maintenance-during-the-self-service-window"></a>Deve iniciar manutenção durante a janela de self-service?  

As seguintes diretrizes devem ajudar a decidir se pretende utilizar esta capacidade e iniciar a manutenção no seu próprio tempo.

> [!NOTE] 
> Manutenção self-service pode não estar disponível para todas as suas VMs. Para determinar se a reimplementação proativa está disponível para a sua VM, procure o **iniciar agora** no estado de manutenção. Manutenção self-service não está atualmente disponível para serviços Cloud (função da Web/trabalho) e o Service Fabric.


Manutenção self-service não é recomendada para implementações que utilizam **conjuntos de disponibilidade** , uma vez que estes são configurações de elevada disponibilidade, onde apenas um domínio de atualização é afetado num determinado momento. 

- Permitir que o Azure acionar a manutenção. Para manutenção que exige reinicialização, tenha em atenção que a manutenção será feita o domínio de atualização por domínio de atualização, que os domínios de atualização não necessariamente recebem a manutenção em seqüência, e que não há uma pausa de 30 minutos entre domínios de atualização.
- Se uma perda temporária de algumas das sua capacidade (contagem de domínios de atualização/1) é uma preocupação, ele pode facilmente ser compensado de ao alocar instâncias adicionais durante o período de manutenção.
- Para uma manutenção que não necessita de reiniciar, as atualizações são aplicadas ao nível do domínio de falhas. 
    
**Não** utilizar manutenção self-service nos seguintes cenários: 

- Se encerrar as VMs com frequência, seja manualmente, usando os laboratórios Dev/Test, usando o encerramento automático ou seguir uma programação, pode reverter o estado de manutenção e, portanto, fazer com que o tempo de inatividade adicional.
- Em VMs de curta duração que saiba será eliminado antes do final da onda de manutenção. 
- Para cargas de trabalho com um grande estado armazenado no disco local (efémeros) que for o pretendido sejam mantidas após a atualização. 
- Nos casos em que redimensiona a sua VM muitas vezes, como ele foi possível reverter o estado de manutenção. 
- Se já ADOTARAM o eventos agendados, que permitem a ativação pós-falha proativa ou encerramento correto de sua carga de trabalho, 15 minutos antes do início de desligamento de manutenção.

**Utilize** manutenção self-service, se estiver a planear executar a VM sem interrupções durante a fase de manutenção agendada e nenhuma as indicações contra-argumentação mencionadas acima é aplicável. 

É melhor usar a manutenção self-service nos seguintes casos:

- Precisa para comunicar uma janela de manutenção exato para a sua gestão ou o cliente final. 
- Precisa para concluir a manutenção por uma determinada data. 
- Precisa controlar a sequência de manutenção, por exemplo, aplicação de várias camadas para garantir a recuperação de segura.
- Precisa de mais de 30 minutos de tempo de recuperação VM entre dois domínios de atualização (UDs). Para controlar o tempo entre domínios de atualização, tem de acionar manutenção em suas VMs de um domínio de atualização (UD) ao mesmo tempo.

 
## <a name="view-virtual-machine-scale-sets-impacted-by-maintenance-in-the-portal"></a>Ver conjuntos de dimensionamento de máquinas virtuais afetados pela manutenção no portal do

Quando estiver agendada uma onda de manutenção planeada, pode ver a lista de conjuntos de dimensionamento de máquinas virtuais que são afetadas pela onda de manutenção futura com o portal do Azure. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** no painel de navegação esquerdo e escolha **conjuntos de dimensionamento de máquinas virtuais**.
3. Sobre o **conjuntos de dimensionamento de máquinas virtuais** página, selecione a **editar colunas** opção na parte superior para abrir a lista de colunas disponíveis.
4. Na **secção de colunas disponíveis**, selecione o **manutenção self-service** item e utilizar os botões de seta para movê-lo para o **nas colunas selecionadas** lista. Pode alternar o menu suspenso na **colunas disponíveis** secção da **todos os** para **propriedades** para tornar o **manutenção self-service** item mais fácil a encontrar. Assim que tiver o **manutenção self-service** item no **nas colunas selecionadas** secção, selecione o **aplicar** na parte inferior da página. 

Depois de seguir os passos acima, o **manutenção self-service** coluna aparecerá na lista de conjuntos de dimensionamento de máquina virtual. Cada conjunto de dimensionamento de máquina virtual pode ter um dos seguintes valores para a coluna de manutenção self-service:

| Valor | Descrição |
|-------|-------------|
| Sim | Pelo menos uma máquina virtual no conjunto de dimensionamento de máquina virtual está numa janela de self-service. Pode iniciar a manutenção em qualquer altura durante esta janela de self-service. | 
| Não | Não existem não existem máquinas virtuais que estão numa janela de self-service no dimensionamento de máquinas de virtuais afetadas definidas. | 
| - | Seus conjuntos de dimensionamento de máquinas virtuais não fazem parte de uma onda de manutenção planeada.| 

## <a name="notification-and-alerts-in-the-portal"></a>Notificações e alertas no portal

Azure comunica um agendamento para a manutenção planeada ao enviar um e-mail para o grupo proprietário e os coproprietários de subscrição. Pode adicionar os destinatários adicionais e canais para esta comunicação através da criação de alertas de registo de atividades do Azure. Para obter mais informações, consulte [monitorizar a atividade de subscrição com o registo de atividades do Azure] (... / monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu à esquerda, selecione **Monitor**. 
3. Na **monitorizar - alertas (clássico)** painel, clique em **+ Adicionar alerta de registo de atividade**.
4. Complete as informações na **Adicionar alerta de registo de atividade** página e certifique-se de definir o seguinte **critérios**:
   - **Categoria de evento**: Estado de funcionamento do serviço
   - **Serviços**: conjuntos de dimensionamento de máquinas virtuais e máquinas virtuais
   - **Tipo de**: manutenção planeada 
    
Para saber mais sobre como configurar alertas de registo de atividade, consulte [criar alertas de registo de atividades](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Iniciar manutenção no seu conjunto de dimensionamento de máquina virtual do portal

Ao olhar para a descrição geral dos conjuntos de dimensionamento de máquina virtual, poderá ver a manutenção mais detalhes relacionados. Se, pelo menos, uma máquina virtual no conjunto de dimensionamento de máquina virtual está incluída na onda de manutenção planeada, será adicionada uma nova faixa de opções de notificação perto da parte superior da página. Pode clicar na faixa de opções de notificação para ir para o **manutenção** página para ver a instância de máquina virtual é afetada pela manutenção planeada. 

A partir daí, será possível iniciar manutenção marcando a caixa correspondente à máquina virtual afetada e, em seguida, clicar em **iniciar manutenção** opção.

Assim que iniciar a manutenção, máquinas virtuais afetadas no seu conjunto de dimensionamento de máquina virtual irá entrar em manutenção e fique temporariamente indisponível. Se perdeu a janela de self-service, ainda será capaz de ver a janela quando o conjunto de dimensionamento de máquina virtual será mantido pelo Azure.
 
## <a name="check-maintenance-status-using-powershell"></a>Verificar o estado de manutenção com o PowerShell

Pode utilizar o Azure Powershell para ver quando as VMs nos seus conjuntos de dimensionamento de máquina virtual estão agendadas para manutenção. Informações de manutenção planeada estão disponíveis a partir da [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) cmdlet quando utiliza o `-InstanceView` parâmetro.
 
Informações de manutenção são devolvidas apenas se há manutenção planeada. Se nenhuma manutenção está agendada que afeta a instância VM, o cmdlet não devolve quaisquer informações de manutenção. 

```powershell
Get-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

As seguintes propriedades são devolvidas em MaintenanceRedeployStatus: 
| Valor | Descrição   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Indica se pode iniciar a manutenção na VM neste momento ||
| PreMaintenanceWindowStartTime         | O início da janela de manutenção self-service quando pode iniciar a manutenção na sua VM ||
| PreMaintenanceWindowEndTime           | Fim da janela de manutenção self-service quando pode iniciar a manutenção na sua VM ||
| MaintenanceWindowStartTime            | Início da manutenção agendada no qual o Azure inicia manutenção na sua VM ||
| MaintenanceWindowEndTime              | Fim da janela agendadas de manutenção em que o Azure inicia manutenção na sua VM ||
| LastOperationResultCode               | O resultado da última tentativa de iniciar a manutenção na VM ||



### <a name="start-maintenance-on-your-vm-instance-using-powershell"></a>Iniciar a manutenção em sua instância de VM com o PowerShell

Pode iniciar a manutenção numa VM se **IsCustomerInitiatedMaintenanceAllowed** está definido como true através da utilização [Set-AzureRmVmss](/powershell/module/azurerm.compute/set-azurermvmss) cmdlet com `-PerformMaintenance` parâmetro.

```powershell
Set-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-using-cli"></a>Verificar o estado de manutenção com a CLI

Informações de manutenção planeada podem ser vistas utilizando [az vmss-instâncias de lista](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances).
 
Informações de manutenção são devolvidas apenas se há manutenção planeada. Se não houver nenhuma manutenção agendada que afeta a instância VM, o comando não devolve quaisquer informações de manutenção. 

```azure-cli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

As seguintes propriedades são devolvidas em MaintenanceRedeployStatus para cada instância VM: 
| Valor | Descrição   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Indica se pode iniciar a manutenção na VM neste momento ||
| PreMaintenanceWindowStartTime         | O início da janela de manutenção self-service quando pode iniciar a manutenção na sua VM ||
| PreMaintenanceWindowEndTime           | Fim da janela de manutenção self-service quando pode iniciar a manutenção na sua VM ||
| MaintenanceWindowStartTime            | Início da manutenção agendada no qual o Azure inicia manutenção na sua VM ||
| MaintenanceWindowEndTime              | Fim da janela agendadas de manutenção em que o Azure inicia manutenção na sua VM ||
| LastOperationResultCode               | O resultado da última tentativa de iniciar a manutenção na VM ||


### <a name="start-maintenance-on-your-vm-instance-using-cli"></a>Iniciar a manutenção em sua instância de VM com a CLI

A chamada seguinte irá iniciar a manutenção numa instância VM, se `IsCustomerInitiatedMaintenanceAllowed` está definido como true.

```azure-cli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>FAQ


**P: por que precisa reiniciar agora minhas máquinas virtuais?**

**R:** enquanto a maioria das atualizações e melhoramentos para a plataforma do Azure não afetar a disponibilidade da máquina virtual, há casos em que nós não é possível evitar o reinício das máquinas virtuais alojadas no Azure. Temos acumulado várias alterações, requerer que o reinício dos nossos servidores que resultarão no reinício das máquinas virtuais.

**P: se eu seguir as recomendações para elevada disponibilidade através de um conjunto de disponibilidade, sou seguro?**

**R:** definir de máquinas virtuais implementadas de disponibilidade ou conjuntos de dimensionamento de máquinas virtuais têm a noção de domínios de atualização (UD). Quando efetuar a manutenção, o Azure honra a restrição UD e não irá reiniciar as máquinas virtuais de UD diferentes (dentro do mesmo conjunto de disponibilidade).  Azure também tem de aguardar durante, pelo menos, 30 minutos antes de passar para o grupo seguinte de máquinas virtuais. 

Para obter mais informações sobre a elevada disponibilidade, consulte [regiões e disponibilidade para máquinas virtuais no Azure](../virtual-machines/windows/regions-and-availability.md).

**P: como posso ser notificado sobre manutenção planeada?**

**R:** uma onda de manutenção planeada é iniciada ao definir uma agenda a um ou mais regiões do Azure. Logo depois, uma notificação por e-mail é enviada para os proprietários de subscrições (um e-mail por subscrição). Canais adicionais e os destinatários para obter esta notificação poderiam ser configurados com alertas de registo de atividade. No caso de implementar uma máquina virtual para uma região em que a manutenção planeada já foi agendada, não irá receber a notificação, mas em vez disso, tem de verificar o estado de manutenção da VM.

**P: não vejo qualquer indicação de manutenção planeada no portal do Powershell ou CLI. O que há de errado?**

**R:** informações relacionadas com a manutenção planeada estão disponíveis durante uma onda de manutenção planeada apenas para as VMs que vão ser afetadas pela mesma. Em outras palavras, se ver dados não, é possível que a onda de manutenção já concluiu (ou não iniciado) ou que sua máquina virtual já esteja alojada num servidor atualizado.

**P: existe uma forma de saber exatamente quando a minha máquina virtual será afetada?**

**R:** ao definir a agenda, definimos uma janela de tempo de vários dias. No entanto, a sequenciação exata de servidores (e as VMs) durante esse período é desconhecida. Podem utilizar os clientes que gostariam de saber o tempo exato para as suas VMs [eventos agendados](../virtual-machines/windows/scheduled-events.md) e consultar a partir da máquina virtual e receber uma notificação de 15 minutos antes de um reinício VM.

**P: o tempo que demora a reiniciar a minha máquina virtual?**

**R:** dependendo do tamanho da sua VM, reinício poderá demorar vários minutos durante a janela de manutenção self-service. Durante o Azure iniciou reinicializações na janela de manutenção agendada, o reinício demorará, normalmente, cerca de 25 minutos. Tenha em atenção que no caso de usar os serviços Cloud (função da Web/trabalho), conjuntos de dimensionamento de máquinas virtuais ou conjuntos de disponibilidade, terá a chance de 30 minutos entre cada grupo de VMs (UD) durante a janela de manutenção agendada. 

**P: não vejo qualquer informação de manutenção nas minhas VMs. O que aconteceu de errado?**

**R:** por vários motivos por que não esteja a ver quaisquer informações de manutenção nas suas VMs:
   - Está a utilizar uma subscrição marcada como Microsoft interna.
   - As VMs não são agendadas para manutenção. É possível que terminou a onda de manutenção, cancelada ou modificada para que as suas VMs já não são afetadas pela mesma.
   - Não tem o **manutenção** coluna adicionada à sua exibição de lista VM. Apesar de termos adicionado esta coluna para a exibição padrão, os clientes que configuraram para ver colunas não predefinidas tem de adicionar manualmente os **manutenção** coluna à sua vista de lista VM.

**P: a minha VM está agendada para manutenção pela segunda vez. Porquê?**

**R:** existem vários casos de utilização em que verá sua VM agendada para manutenção depois de já ter concluído a reimplementação da manutenção:
   - Temos cancelada a onda de manutenção e reiniciado-lo com um payload de diferente. É possível que foi detetada payload com falha e precisamos simplesmente de implementar um payload adicional.
   - A VM esteve *corrigido por outro serviço* para outro nó devido a uma falha de hardware.
   - Selecionou a parar (desaloque) e reinicie a VM.
   - Tiver **encerramento automático** ativado para a VM.

## <a name="next-steps"></a>Passos Seguintes

Saiba como pode se registrar para eventos de manutenção a partir de dentro da VM com [eventos agendados](../virtual-machines/windows/scheduled-events.md).
