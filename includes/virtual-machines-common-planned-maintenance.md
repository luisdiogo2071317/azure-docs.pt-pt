---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 07/05/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: f203e056df00fb1a9b1e7e43930955040dfce4aa
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39030005"
---
O Azure realiza periodicamente atualizações para melhorar a fiabilidade, o desempenho e a segurança da infraestrutura de anfitrião para máquinas virtuais. Estas atualizações variam entre a aplicação de patches a componentes de software no ambiente de alojamento (como o sistema operativo, o hipervisor e vários agentes implementados no anfitrião) à atualização de componentes de rede e desativação de hardware. A maioria destas atualizações são efetuadas sem nenhum impacto para as máquinas virtuais alojadas. No entanto, existem casos em que as atualizações têm um impacto:

- Se uma atualização sem reinicialização for possível, o Azure utiliza preservação da manutenção de memória para colocar em pausa a VM, enquanto o anfitrião é atualizado ou a VM é movida para um anfitrião já atualizado completamente.

- Se a manutenção requer uma reinicialização, receberá um aviso de quando a manutenção está prevista. Nestes casos, também receberá uma janela de tempo em que pode iniciar a manutenção por conta própria, ao mesmo tempo que funcione para si.

Esta página descreve a forma como o Microsoft Azure executa ambos os tipos de manutenção. Para obter mais informações sobre eventos não planeados (falhas), consulte Gerir a disponibilidade das máquinas virtuais para [Windows](../articles/virtual-machines/windows/manage-availability.md) ou [Linux](../articles/virtual-machines/linux/manage-availability.md).

Aplicações em execução numa máquina virtual podem obter informações sobre atualizações futuras com o serviço de metadados do Azure para [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) ou [Linux](../articles/virtual-machines/linux/instance-metadata-service.md).

Para obter informações "procedimentos" sobre o gerenciamento de manutenção planeada, consulte "Manipulação planeado notificações de manutenção" para [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) ou [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="memory-preserving-maintenance"></a>Memória preservação da manutenção

Quando as atualizações não requerem um reinício total, mecanismos de manutenção preservando de memória são utilizados para limitar o impacto para a máquina virtual. A máquina virtual é colocada em pausa durante até 30 segundos, preservando a memória RAM, enquanto o ambiente de hospedagem aplica-se as atualizações e correções necessárias ou muda a VM para um anfitrião já atualizado. A máquina virtual é retomada, em seguida, e o relógio da máquina virtual é sincronizado automaticamente. 

Essas operações de manutenção não rebootful são o domínio de falha aplicada por domínio de falha e o progresso é interrompido se qualquer sinais de estado de funcionamento de aviso são recebidas.

Alguns aplicativos podem ser afetados por estes tipos de atualizações. Aplicativos que realizam em tempo real processamento de eventos, como o suporte de dados de transmissão em fluxo ou transcodificação ou alto débito, cenários, de rede não podem ser criados para tolerar uma pausa de segundo 30. <!-- sooooo, what should they do? --> No caso da VM está a ser movida para outro anfitrião, algumas cargas de trabalho confidenciais podem observar uma degradação do desempenho de pequenas em alguns minutos que levou à pausa de Máquina Virtual. 


## <a name="maintenance-requiring-a-reboot"></a>Necessidade de um reinício de manutenção

Quando as VMs têm de ser reiniciado para manutenção planeada, será notificado com antecedência. Manutenção planeada tem duas fases: a janela de self-service e uma janela de manutenção agendada.

O **janela de self-service** permite-lhe iniciar a manutenção nas suas VMs. Durante este período, pode consultar cada VM para ver o respetivo estado e verificar o resultado de sua última solicitação de manutenção.

Quando começa a manutenção self-service, a VM é movida para um nó que já foi atualizado e, em seguida, liga-o novamente. Uma vez que a VM reiniciar, o disco temporário é perdido e endereços IP dinâmicos associados à interface de rede virtual são atualizados.

Se começa a manutenção self-service e existe um erro durante o processo, a operação está parada, a VM não é atualizada e terá a opção para repetir a manutenção self-service. 

Quando a janela de self-service tiver passado, o **janela de manutenção agendada** começa. Durante este período de tempo, pode ainda de consulta para a janela de manutenção, mas já não será possível iniciar a manutenção por conta própria.

Para informações sobre como gerenciar a necessidade de um reinício de manutenção, consulte "Manipulação planeado notificações de manutenção" para [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) ou [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Considerações de disponibilidade durante a manutenção agendada 

Se optar por esperar até a janela de manutenção agendada, existem alguns aspetos a considerar para manter a maior disponibilidade das suas VMs. 

#### <a name="paired-regions"></a>Regiões emparelhadas

Cada região do Azure é emparelhada com outra região na mesma geografia, juntos eles fazem um par regional. Durante a manutenção agendada, o Azure irá atualizar apenas as VMs numa única região de um par de região. Por exemplo, ao atualizar as máquinas virtuais nos E.U.A. Centro-Norte, o Azure não atualiza quaisquer máquinas virtuais nos E.U.A. Centro-Sul ao mesmo tempo. No entanto, outras regiões, como a Europa do Norte, podem estar sob manutenção em simultâneo como os E.U.A. Leste. Noções básicas sobre como funcionam os pares de região pode ajudá-lo melhor distribuir as VMs em várias regiões. Para obter mais informações, consulte [pares de região do Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Conjuntos de disponibilidade e conjuntos de dimensionamento

Ao implementar uma carga de trabalho em VMs do Azure, pode criar as VMs dentro de um conjunto de disponibilidade para proporcionar elevada disponibilidade para a sua aplicação. Isto garante que, durante um período de indisponibilidade ou eventos de manutenção rebootful,, pelo menos, uma máquina virtual está disponível.

Dentro de um conjunto de disponibilidade, VMs individuais são distribuídas por até 20 domínios de atualização (UDs). Durante a manutenção agendada, apenas um domínio de atualização individual é afetado num determinado momento. Lembre-se de que a ordem dos domínios de atualização que está a ser afetado não necessariamente ocorre sequencialmente. 

Os conjuntos de dimensionamento de máquinas virtuais são um recurso de computação do Azure que permite-lhe implementar e gerir um conjunto de VMs idênticas como um único recurso. O conjunto de dimensionamento será automaticamente implantado em vários domínios de atualização, como VMs num conjunto de disponibilidade. Assim como com os conjuntos de disponibilidade, com conjuntos de dimensionamento apenas um domínio com uma única atualização é afetado num determinado momento durante a manutenção agendada.

Para obter mais informações sobre como configurar as máquinas virtuais para elevada disponibilidade, consulte Gerir a disponibilidade das suas máquinas virtuais para [Windows](../articles/virtual-machines/windows/manage-availability.md) ou [Linux](../articles/virtual-machines/linux/manage-availability.md).
