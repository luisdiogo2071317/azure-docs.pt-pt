---
title: Monitor do Azure para VMs (pré-visualização) perguntas mais frequentes | Documentos da Microsoft
description: Monitor do Azure para VMs (pré-visualização) é uma solução no Azure que combina o estado de funcionamento e a monitorização do desempenho do sistema de operacional de VM do Azure. Automaticamente Deteta componentes da aplicação e dependências com outros recursos e mapeia a comunicação entre eles. Este artigo responde às perguntas mais frequentes.
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
ms.date: 11/08/2018
ms.author: magoedte
ms.openlocfilehash: f5865cf72f413db49e70a08305de54aff955607b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53075238"
---
# <a name="azure-monitor-for-vms-preview-faq"></a>Monitor do Azure para VMs (pré-visualização) FAQ
Este artigo responde a perguntas freqüentes sobre o Azure Monitor para as VMs. Se tiver perguntas adicionais sobre a solução, vá para o [fórum de discussão do Azure](https://feedback.azure.com/forums/34192--general-feedback) e poste suas perguntas. Quando perguntas são solicitadas com frequência, podemos adicioná-los para este artigo para que eles podem ser encontrados rápida e facilmente.

## <a name="can-i-deploy-vms-to-an-existing-workspace"></a>Pode implementar VMs numa área de trabalho existente?
Se as máquinas virtuais já estiverem ligadas a uma área de trabalho do Log Analytics, pode continuar a utilizar essa área de trabalho, ao implementá-las para o Azure Monitor para as VMs. A área de trabalho tem de existir uma das regiões suportadas listadas na secção "Pré-requisitos" [implementar o Azure Monitor para VMs (pré-visualização)](vminsights-onboard.md#prerequisites).

Durante a implementação, podemos configurar contadores de desempenho para a área de trabalho. Esta ação faz com que as VMs que apresentam dados de relatório para a área de trabalho para começar a recolher as informações para e de análise no Azure Monitor para as VMs. Como resultado, verá os dados de desempenho de todas as VMs que estejam ligadas à área de trabalho selecionada. As funcionalidades de estado de funcionamento e o mapa estão ativadas apenas para as VMs que especificou para a implementação.

Para obter mais informações sobre o desempenho do que os contadores estão ativados, consulte [implementar o Azure Monitor para VMs (pré-visualização)](vminsights-onboard.md).

## <a name="can-i-deploy-vms-to-a-new-workspace"></a>Pode implementar VMs para uma nova área de trabalho? 
Se as VMs atualmente não estão ligadas a uma área de trabalho do Log Analytics existente, tem de criar uma nova área de trabalho para armazenar os dados. Pode criar um automaticamente ao configurar uma única VM para o Azure Monitor para as VMs no portal do Azure.

Se optar por utilizar o método baseado em script, consulte [implementar o Azure Monitor para VMs (pré-visualização)](vminsights-onboard.md). 

## <a name="what-can-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>O que devo fazer se a minha VM já está a comunicar com uma área de trabalho existente?
Se já está a recolher dados das suas máquinas virtuais, poderá ter já configurado-los a dados de relatórios numa área de trabalho do Log Analytics existente. Desde que essa área de trabalho é de uma de nossas regiões suportados, pode ativar o Azure Monitor para as VMs para essa área de trabalho já existente. Estamos a trabalhar ativamente para oferecer suporte a mais regiões.

>[!NOTE]
>Podemos configurar contadores de desempenho para a área de trabalho, o que afeta todas as VMs que reportam à área de trabalho, se é ou não tiver optado por implementá-las para o Azure Monitor para as VMs. Para obter mais informações sobre como os contadores de desempenho são configurados para a área de trabalho, consulte a secção de "Contadores de desempenho de configurar" de [Windows e Linux origens de dados de desempenho do Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md). Para obter informações sobre os contadores configurado para o Azure Monitor para VMs, veja [implementar o Azure Monitor para VMs (pré-visualização)](vminsights-onboard.md). 

## <a name="why-did-my-vm-deployment-fail"></a>Por que falhar a minha implementação de VM?
Quando implementa uma VM do Azure no portal do Azure, ocorrem os seguintes eventos:

* Uma área de trabalho do Log Analytics do padrão é criada, se que a opção tiver sido selecionada.
* Os contadores de desempenho estão configurados para a área de trabalho selecionada. Caso este passo falhe, algumas das tabelas e gráficos de desempenho não apresentam dados para a VM que implementou. Pode corrigir este problema ao executar o script do PowerShell que está documentado na seção "Ativar com o PowerShell" [implementar o Azure Monitor para VMs (pré-visualização)](vminsights-onboard.md#enable-with-powershell).
* O agente Log Analytics é instalado em VMs do Azure com uma extensão de VM, caso seja necessário. 
* O Azure Monitor para agente de dependência de mapa de VMs é instalado em VMs do Azure com uma extensão, caso seja necessário. 
* Componentes de Monitor do Azure que suportam a funcionalidade de estado de funcionamento estão configurados, se necessário, e a VM está configurada para dados de estado de funcionamento do relatório.

Durante a implementação, verifique o estado para cada um dos passos anteriores e retornar um status de notificação para si no portal. Configuração de área de trabalho e a instalação do agente normalmente demora 5 a 10 minutos. Visualização de dados de monitorização e estado de funcionamento no portal do Azure demoram um 5 a 10 minutos adicionais. 

Se iniciou a implementação e consulte as mensagens que indicam que a VM tem de ser implementado, que até 30 minutos para a VM concluir o processo. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>Não vejo algumas ou quaisquer dados nos gráficos de desempenho para a minha VM
Se os dados de desempenho não for apresentados na tabela de disco ou os gráficos de desempenho, os contadores de desempenho podem não estar configurados na área de trabalho. Para resolver este problema, execute o script do PowerShell que está documentado na seção "Ativar com o PowerShell" [implementar o Azure Monitor para VMs (pré-visualização)](vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-the-azure-monitor-for-vms-map-feature-different-from-service-map"></a>Como o Azure Monitor para a funcionalidade de mapa de VMs é diferente do mapa de serviço?
O Azure Monitor para a funcionalidade de mapa de VMs baseia-se no mapa de serviço, mas tem as seguintes diferenças:

* Pode aceder a vista do mapa a partir do painel da VM e do Azure Monitor para VMs do Azure Monitor.
* As ligações no mapa estão agora clicáveis e exibir dados de métrica de ligação no painel lateral.
* Uma nova API é utilizada para criar os mapas para suportar melhor mapas mais complexos.
* VMs monitorizadas estão agora no nó do grupo de cliente e de anel gráfico apresenta a taxa de máquinas de virtuais monitorizadas para não monitorizado. Também pode filtrar a lista de máquinas quando o grupo é expandido.
* Máquinas de virtuais monitorizadas já se encontram em nós de grupo de porta de servidor e o gráfico em anel apresenta a proporção de computadores monitorados para não monitorizado. Também pode filtrar a lista de máquinas quando o grupo é expandido.
* O estilo de mapa foi atualizado para ser mais consistente com o mapa da aplicação do Azure Application Insights.
* Os painéis do lado foram atualizados, mas ainda não tiver o conjunto completo de integrações que eram suportadas no mapa de serviço: gestão de atualizações, controlo de alterações, segurança e serviço de atendimento. 
* A opção para escolher os grupos e as máquinas para mapear foi atualizada. Ele agora suporta subscrições, grupos de recursos, os conjuntos de dimensionamento de máquina virtual do Azure e serviços em nuvem.
* Não é possível criar novos grupos de máquina de mapa de serviço no Azure Monitor para a funcionalidade de mapa de VMs. 

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Por que motivo é que meu gráficos de desempenho mostra linhas pontilhadas?

Gráficos de desempenho mostra linhas pontilhadas em vez de linhas sólidas por vários motivos:
* Pode haver uma lacuna na recolha de dados. 

* A definição de amostragem de dados predefinida é de 60 em 60 segundos. Poderá ver linhas pontilhadas se optar por um intervalo de tempo de estreito para o gráfico e a frequência de amostragem é menor que o tamanho de registo utilizado no gráfico. Digamos que escolheu uma frequência de amostragem de 10 minutos e cada bucket no gráfico é de 5 minutos. Neste caso, a escolha de uma vasta gama de tempo para ver deve fazer com que as linhas do gráfico para aparecem como linhas sólidas em vez de pontos.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>São os grupos suportados com o Azure Monitor para VMs?
Sim, depois de instalar o agente de dependência informações que recolhemos as VMs para apresentar grupos com base na subscrição, grupo de recursos, máquina virtual conjuntos de dimensionamento e serviços em nuvem. Se já utiliza o mapa de serviço e criar grupos de máquinas, estes grupos são apresentados também. Grupos de computadores também serão apresentada no filtro de grupos, se tiver criado para a área de trabalho que está a ver. 

## <a name="how-can-i-display-the-details-about-whats-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Como posso exibir os detalhes sobre o que está orientando o é o percentil 95 gráficos de linhas no agregado desempenho?
Por predefinição, a lista é ordenada para mostrar a as VMs que tenham o valor mais alto para o percentil 95 para a métrica selecionada. Uma exceção é o **memória disponível** gráfico, que mostra as máquinas com o menor valor de percentil de quinto. Selecione o gráfico para abrir o **lista de N principais** vista com a métrica apropriada selecionada.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-various-virtual-networks-and-subnets"></a>Como a funcionalidade de mapa com IPs duplicados entre várias redes virtuais e sub-redes?
Se estiver a duplicar a intervalos de IP utilizando qualquer uma das VMs ou conjuntos de dimensionamento de máquina virtual do Azure em sub-redes e redes virtuais, pode causar o Azure Monitor para a funcionalidade de mapa de VMs apresentar informações incorretas. Estamos cientes desse problema e estamos a analisar opções para melhorar a experiência.

## <a name="does-the-map-feature-support-ipv6"></a>A funcionalidade de mapa suporta IPv6?
A funcionalidade de mapa suporta atualmente apenas IPv4, e estamos a analisar o suporte para IPv6. Também suportamos IPv4 encapsulado dentro de IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-why-is-the-map-difficult-to-view"></a>Quando carrego um mapa de um grupo de recursos ou outro grupo grande, por que é o mapa difícil ver?
Embora Melhorámos a funcionalidade de mapa para lidar com configurações de grandes e complexas, percebemos que um mapa pode ter muitos nós, ligações e nós a trabalhar como um cluster. Estamos empenhados em continuar melhorar o suporte para aumentar a escalabilidade.  

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-from-the-network-chart-on-the-azure-vm-overview-page"></a>Por que o gráfico de rede na guia desempenho ter um aspeto diferente no gráfico de rede na página de descrição geral de VM do Azure?

A página de descrição geral para uma VM do Azure apresenta gráficos com base em medição do anfitrião da atividade na VM do convidado. A rede de gráficos na página de descrição geral de VM do Azure apresenta apenas o tráfego de rede que será cobrado. Este ecrã não inclui o tráfego entre redes virtuais. Os dados e gráficos apresentados para o Azure Monitor para VMs baseia-se nos dados da VM do convidado e a rede de gráficos apresenta todos os de TCP/IP de tráfego que tem entrada e saída para essa VM, incluindo o tráfego entre redes virtuais.

## <a name="what-are-the-limitations-of-the-log-analytics-free-pricing-plan"></a>Quais são as limitações do Log Analytics gratuitamente plano de preços?
Se tiver configurado Monitor do Azure com uma área de trabalho do Log Analytics, utilizando o *gratuito* escalão de preço, o Azure Monitor para a funcionalidade de mapa de VMs suporta a ligação de apenas cinco máquinas para a área de trabalho. 

Por exemplo, digamos que tenha cinco VMs ligadas a uma área de trabalho gratuita. Se desligar uma VM e, em seguida, ligar mais tarde um novo, a nova VM não é monitorizada e refletida na página de mapa. Neste cenário, ao abrir a nova VM será avisado para utilizar o **experimentar agora** opção e selecione **Insights (pré-visualização)** no painel à esquerda, mesmo depois de tiver sido instalado na VM. No entanto, não são pedido com opções como normalmente seria se a VM não foi implementada para o Azure Monitor para as VMs. 

## <a name="next-steps"></a>Passos Seguintes
Para compreender os requisitos e métodos para ativar a monitorização das suas máquinas virtuais, consulte [implementar o Azure Monitor para VMs (pré-visualização)](vminsights-onboard.md).
