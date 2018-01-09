---
title: "Processamento de notificações de manutenção para VMs com Linux no Azure | Microsoft Docs"
description: "Ver as notificações de manutenção para máquinas virtuais do Linux em execução no Azure e iniciar manutenção self-service."
services: virtual-machines-linux
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: zivr
ms.openlocfilehash: d551a62a59e0a7f63f5fd4862680a271de659a19
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Notificações de manutenção de processamento planeada para máquinas virtuais do Linux

Azure executa periodicamente atualizações para melhorar a fiabilidade, desempenho e segurança da infraestrutura de anfitrião para máquinas virtuais. As atualizações são as alterações, como o ambiente de alojamento de aplicação de patches ou atualizar e desativar o hardware. A maioria destas atualizações são efetuadas sem qualquer impacto para as máquinas virtuais alojadas. No entanto, há casos em que as atualizações de ter um impacto:

- Se a manutenção não requer um reinício, o Azure utiliza migração no local para colocar em pausa a VM enquanto o anfitrião está atualizado.

- Se a manutenção requer um reinício, receberá um aviso quando a manutenção planeada. Nestes casos, é-lhe dada uma janela de tempo em que pode iniciar a manutenção de si próprio, quando funciona para si.


Manutenção planeada, que requer uma reinicialização está agendada no waves. Cada wave tem um âmbito diferentes (regiões).

- Uma onda começa com uma notificação para os clientes. Por predefinição, a notificação é enviada para o proprietário da subscrição e os coproprietários. Pode adicionar mais opções de mensagens, como e-mail, SMS e webhooks e os destinatários para as notificações de utilizar o Azure [alertas de registo de atividade](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).  
- No momento da notificação, um *self-service janela* é disponibilizado. Durante este período, pode encontrar a que as máquinas virtuais estão incluídos neste wave e proativamente iniciar manutenção, de acordo com as suas próprias necessidades de agendamento.
- Depois da janela de self-service, uma *janela de manutenção agendada* começa. Numa determinada fase durante este período, o Azure agendas e aplica-se a manutenção necessária à máquina virtual. 

O objetivo no ter dois windows é para lhe dar tempo suficiente para iniciar a manutenção e reiniciar a máquina virtual ao saber quando Azure irá iniciar automaticamente manutenção.


Pode utilizar o portal do Azure, o PowerShell, a REST API e a CLI para consultar as janelas de manutenção para as suas VMs e iniciar manutenção self-service.

 > [!NOTE]
 > Se tentar iniciar a manutenção e o pedido falha, o Azure marca a VM como **ignorada**. Já não poderá utilizar a opção de manutenção de iniciada pelo cliente. A VM tem de ser reiniciado pelo Azure durante a fase de manutenção agendada.


 
## <a name="should-you-start-maintenance-using-during-the-self-service-window"></a>Deve começar a utilizar o manutenção durante a janela de self-service?  

As seguintes diretrizes devem ajudá-lo a decidir se deve utilizar esta capacidade e iniciar manutenção ao seu próprio tempo.

> [!NOTE] 
> Manutenção de self-service poderão não estar disponível para todas as suas VMs. Para determinar se a implementar proativa está disponível para a VM, procure o **iniciar agora** do Estado de manutenção. Manutenção de self-service não está atualmente disponível para serviços em nuvem (função da Web/trabalho), o Service Fabric e os conjuntos de dimensionamento de Máquina Virtual.


Manutenção de self-service não é recomendada para implementações que utilizam **conjuntos de disponibilidade** , uma vez que estes são setups elevadas, onde o domínio de atualização apenas uma é afetado em qualquer momento. 
    - Permitir que o acionador do Azure a manutenção, mas tenha em atenção que a ordem dos domínios de atualização que está a ser afetado não necessariamente acontecer sequencialmente e que existe uma pausa de 30 minutos entre domínios de atualização.
    - Se uma perda temporária de algumas das suas capacidade (número de domínios de 1/atualização) for uma preocupação,-pode facilmente ser compensada para atribuindo a instâncias de adição durante o período de manutenção. 

**Não** utilizar self-service manutenção nos seguintes cenários: 
    - Se desligar as VMs com frequência, ou manualmente, utilizando DevTest labs, utilizando o encerramento automático ou os seguintes uma agenda, foi possível reverter o estado de manutenção e assim causar período de indisponibilidade adicional.
    - Em VMs de curta duração que sabe que serão eliminados antes do fim de wave a manutenção. 
    - Para cargas de trabalho com um Estado de grande armazenados no disco local (efémeras) que seja mantida após a atualização, se pretendido. 
    - Nos casos em que redimensionar, muitas vezes, a VM, dado que foi possível reverter o estado de manutenção. 
    - Se lhe ADOTARAM agendados eventos que permitem proativa ativação pós-falha ou encerramento correto da carga de trabalho, 15 minutos antes do início do encerramento de manutenção

**Utilize** manutenção self-service, se estiver a planear executar a VM interrompida durante a fase de manutenção agendada e nenhuma das counter-indicações mencionadas acima são aplicáveis. 

É melhor utilizar o self-service de manutenção nos seguintes casos:
    - Terá de comunicar uma janela de manutenção exato para a gestão ou o cliente do fim. 
    - Tem de concluir a manutenção por uma data especificada. 
    - Tem de controlar a sequência de manutenção, por exemplo, aplicação multicamadas para garantir a recuperação segura.
    - Precisa de mais de 30 minutos a VM do tempo de recuperação entre dois domínios de atualização (UDs). Para controlar o tempo entre domínios de atualização, deve acionar manutenção no seu domínio de atualização de um VMs (UD) a uma hora.



## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>Localizar VMs agendadas para utilizar a CLI de manutenção

Manutenção planeada informações podem ser vistas utilizando [vm do azure get--vista de instância](/cli/azure/vm?view=azure-cli-latest#az_vm_get_instance_view).
 
Informações de manutenção são devolvidas apenas se não houver manutenção planeada. Se não existir que nenhuma manutenção agendada que impactos a VM, o comando não devolve quaisquer informações de manutenção. 

```azure-cli
az vm get-instance-view -g rgName -n vmName
```

Os seguintes valores são devolvidos em MaintenanceRedeployStatus: 

| Valor | Descrição   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Indica se pode iniciar manutenção na VM neste momento ||
| PreMaintenanceWindowStartTime         | O início da janela de manutenção self-service quando podem iniciar manutenção na VM ||
| PreMaintenanceWindowEndTime           | O fim da janela de manutenção self-service quando podem iniciar manutenção na VM ||
| MaintenanceWindowStartTime            | O início da janela manutenção agendada no qual o Azure inicia manutenção em VM ||
| MaintenanceWindowEndTime              | O fim da janela manutenção agendada no qual o Azure inicia manutenção em VM ||
| LastOperationResultCode               | O resultado da última tentativa para iniciar a manutenção da VM ||




## <a name="start-maintenance-on-your-vm-using-cli"></a>Iniciar manutenção na VM a utilizar a CLI

A seguinte chamada iniciará manutenção numa VM, se `IsCustomerInitiatedMaintenanceAllowed` está definido como true.

```azure-cli
az vm perform-maintenance rgName vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="classic-deployments"></a>Implementações clássicas

Se ainda tiver legadas VMs que foram implementados utilizando o modelo de implementação clássica, pode utilizar CLI 1.0 a consulta para as VMs e iniciar a manutenção.

Certifique-se de que está no modo correto para trabalhar com VMS clássicas, escrevendo:

```
azure config mode asm
```

Para obter o estado de manutenção de uma VM chamada *myVM*, tipo:

```
azure vm show myVM 
``` 

Iniciar manutenção as VMS clássicas denominado *myVM* no *myService* serviço e *myDeployment* implementação, escreva:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```


## <a name="faq"></a>FAQ


**P: por que motivo precisa de reiniciar o meu máquinas de virtuais agora?**

**R:** enquanto a maioria das atualizações e melhoramentos para a plataforma do Azure não afetar a disponibilidade da máquina virtual, há casos em que podemos não é possível evitar reiniciando das máquinas virtuais alojadas no Azure. Podemos ter acumulados várias alterações que requerem-nos reiniciar os nossos servidores que resultará no reinício de máquinas virtuais.

**P: se posso seguir as recomendações para elevada disponibilidade através de um conjunto de disponibilidade, pude seguro?**

**R:** máquinas virtuais implementadas na disponibilidade de um conjunto ou conjuntos de dimensionamento de máquina virtual têm a noção de domínios de atualização (UD). Quando efetuar a manutenção, o Azure honra a restrição UD e não irá reiniciar as máquinas virtuais de diferentes UD (no mesmo conjunto de disponibilidade).  Azure também tem de aguardar pelo menos 30 minutos antes de mover para o seguinte grupo de máquinas virtuais. 

Para obter mais informações sobre como de elevada disponibilidade, consulte [regiões e disponibilidade para máquinas virtuais no Azure](regions-and-availability.MD).

**P: como posso ser notificado sobre a manutenção planeada?**

**R:** uma onda de manutenção planeada é iniciada através da definição de uma agenda a um ou mais regiões do Azure. Logo, é enviada uma notificação por e-mail para os proprietários de subscrição (um e-mail por subscrição). Canais adicionais e os destinatários para esta notificação foi possível configurar a utilizar alertas de registo de atividade. No caso de implementar uma máquina virtual numa região em que já foi agendada manutenção planeada, não irá receber a notificação mas em vez disso tem de verificar o estado de manutenção da VM.

**P: não vejo qualquer indicação de manutenção planeada no portal, Powershell ou CLI, o que é incorreto?**

**R:** informações relacionadas com a manutenção planeada estão disponíveis durante uma onda de manutenção planeada apenas para as VMs que vai ser afetado por-lo. Por outras palavras, se vir não os dados, poderia ser que o wave de manutenção tem já foi concluída (ou não iniciado) ou que a máquina virtual já estiver alojada num servidor atualizado.

**P: existe uma forma de saber exatamente quando a minha máquina virtual será afetada?**

**R:** ao definir a agenda, iremos definir um intervalo de tempo de vários dias. No entanto, a sequenciação exata de servidores (e as VMs) esse período é desconhecida. Podem utilizar os clientes que gostaria de saber o momento exato para as respetivas VMs [agendada eventos](scheduled-events.md) e consultar a partir da máquina virtual e receber uma notificação de 15 minutos antes de um reinício VM.

**P: quanto-leva-o para reiniciar a minha máquina virtual?**

**R:** dependendo do tamanho da sua VM, reiniciar o computador pode demorar vários minutos durante a janela de manutenção de self-service. Durante o Azure iniciada reinícios na janela de manutenção agendada, o reinício demorará, normalmente, cerca de 25 minutos. Tenha em atenção que no caso de utilizar serviços em nuvem (função da Web/trabalho), conjuntos de dimensionamento de Máquina Virtual ou conjuntos de disponibilidade, irá ser dada 30 minutos entre cada grupo de VMs (UD) durante a janela de manutenção agendada.

**P: qual é a experiência no caso dos serviços Cloud (função da Web/trabalho), o Service Fabric e os conjuntos de dimensionamento de Máquina Virtual?**

**R:** enquanto estas plataformas são afetadas pela manutenção planeada, os clientes que utilizam estas plataformas é consideradas seguras dada que só VMs numa única atualização domínio (UD) vai ser o impacto em qualquer momento. Manutenção de self-service não está atualmente disponível para serviços em nuvem (função da Web/trabalho), o Service Fabric e os conjuntos de dimensionamento de Máquina Virtual.

**P: Posso recebeu uma mensagem de e-mail sobre a desativação de hardware, este é o mesmo que a manutenção planeada?**

**R:** enquanto a desativação de hardware é um evento de manutenção planeada, não ainda temos simplificar este caso de utilização para a nova experiência.  

**P: não vejo qualquer informação de manutenção na minha VMs. O que aconteceu de errado?**

**R:** existem vários motivos por que motivo não está a ver as informações de manutenção nas suas VMs:
1.  Estão a utilizar uma subscrição marcada como Microsoft interna.
2.  As suas VMs não estão agendadas para manutenção. É possível que terminou a wave de manutenção, cancelada ou modificada para que as suas VMs já não são afetadas por este.
3.  Não tem o **manutenção** coluna adicionada para a vista de lista VM. Enquanto podemos adicionar esta coluna para a vista predefinida, os clientes que configurado para ver as colunas não predefinido tem de adicionar manualmente o **manutenção** coluna para a sua vista de lista VM.

**P: minha VM está agendada para manutenção para a segunda vez. Porquê?**

**R:** existem vários casos de utilização, onde poderá ver a VM agendada para manutenção depois de já ter concluído a implementar de manutenção:
1.  Podemos ter cancelado wave a manutenção e reiniciado-lo com um payload de diferentes. Isto pode dever-se ao que foi detetada payload com falhas e precisamos simplesmente implementar um payload adicional.
2.  A VM foi *serviço healed* para outro nó devido a uma falha de hardware
3.  Selecionou a parar (desalocar) e reinicie a VM
4.  Tiver **encerramento automático** ativada para a VM


**P: manutenção do meu conjunto de disponibilidade demora muito tempo e passam a ver estado "ignorada" em algumas das minha disponibilidade definido instâncias. Porquê?** 

**R:** se clicou atualizar várias instâncias num conjunto de disponibilidade no sucessivamente curto, Azure ficarão em fila estes pedidos e começa a atualizar apenas as VMs no domínio de uma atualização (UD) de cada vez. No entanto, uma vez que é possível que exista uma pausa entre domínios de atualização, a atualização poderá aparecer demorem mais tempo. Se a fila de atualização demora mais de 60 minutos, algumas instâncias irão mostrar o **ignorada** Estado, mesmo que tenham sido atualizados com êxito. Para evitar este estado incorreto, a atualização define a sua disponibilidade clicando apenas na instância dentro da disponibilidade de um definido e aguarde a atualização essa VM seja concluída antes de clicar na VM seguinte num domínio de atualização diferentes.


## <a name="next-steps"></a>Próximos Passos

Saiba como pode registar eventos de manutenção de dentro da VM utilizando [agendada eventos](scheduled-events.md).
