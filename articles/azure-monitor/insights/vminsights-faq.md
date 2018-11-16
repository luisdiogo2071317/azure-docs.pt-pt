---
title: Monitor do Azure para VMs (pré-visualização) perguntas mais frequentes | Documentos da Microsoft
description: Monitor do Azure para VMs (pré-visualização) é uma solução no Azure que combina o estado de funcionamento e desempenho de monitorização do sistema de operacional de VM do Azure, bem como detetar automaticamente os componentes da aplicação e dependências com outros recursos e mapeia a comunicação entre eles. Este artigo responde a perguntas comuns.
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
ms.openlocfilehash: 124488d13d1d303743900ab1de41768633e4f6e0
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51715729"
---
# <a name="azure-monitor-for-vms-preview-frequently-asked-questions"></a>Monitor do Azure para VMs (pré-visualização) perguntas mais frequentes
O FAQ do Microsoft é uma lista de perguntas freqüentes sobre o Azure Monitor para as VMs. Se tiver perguntas adicionais sobre a solução, vá para o [fórum de discussão](https://feedback.azure.com/forums/34192--general-feedback) e poste suas perguntas. Quando uma pergunta é colocada frequentemente, adicionamo-la a este artigo para que ele pode ser encontrado rapidamente e facilmente.

## <a name="can-i-onboard-to-an-existing-workspace"></a>Posso carregar para uma área de trabalho existente?
Se as máquinas virtuais já estiverem ligadas a uma área de trabalho do Log Analytics, pode continuar a usar essa área de trabalho quando a integração para o Azure Monitor para as VMs, fornecidas for em uma das regiões suportadas listadas [aqui](vminsights-onboard.md#prerequisites).

Quando a integração, podemos configurar contadores de desempenho para a área de trabalho que fará com que todas as VMs a comunicar dados para a área de trabalho para iniciar a recolha destas informações para exibição e análise no Azure Monitor para as VMs.  Como resultado, verá os dados de desempenho de todas as VMs ligadas à área de trabalho selecionada.  As funcionalidades de estado de funcionamento e o mapa apenas estão ativadas para as VMs que especificou para carregar.

Para obter mais informações sobre o desempenho do que os contadores estão ativados, consulte a nossa [inclusão](vminsights-onboard.md) artigo.

## <a name="can-i-onboard-to-a-new-workspace"></a>Posso carregar para uma nova área de trabalho? 
Se as suas VMs não estiverem atualmente ligadas numa área de trabalho do Log Analytics existente, tem de criar uma nova área de trabalho para armazenar os dados.  Criar uma nova área de trabalho padrão é feito automaticamente se configurar o numa única VM do Azure para o Azure Monitor para VMs através do portal do Azure.

Se optar por utilizar o método baseado em script, essas etapas são abordadas os [inclusão](vminsights-onboard.md) artigo. 

## <a name="what-do-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>O que devo fazer se a minha VM já está a comunicar com uma área de trabalho existente?
Se já está a recolher dados das suas máquinas virtuais, poderá ter já configurado-lo a dados de relatórios numa área de trabalho do Log Analytics existente.  Desde que essa área de trabalho é de uma de nossas regiões suportadas, pode ativar o Azure Monitor para VMs a essa área de trabalho já existente.  Se já estiver a utilizar a área de trabalho não estiver em uma de nossas regiões suportadas, não será capaz de carregar para o Azure Monitor para VMs neste momento.  Estamos a trabalhar ativamente para oferecer suporte a mais regiões.

>[!NOTE]
>Podemos configurar contadores de desempenho para a área de trabalho que afeta todas as VMs que reportam à área de trabalho, se é ou não tiver optado por carregá-los para o Azure Monitor para as VMs. Para obter mais detalhes sobre como os contadores de desempenho são configurados para a área de trabalho, consulte nosso [documentação](../../log-analytics/log-analytics-data-sources-performance-counters.md). Para obter informações sobre os contadores configurado para o Azure Monitor para VMs, consulte nosso [documentação de inclusão](vminsights-onboard.md#performance-counters-enabled).  

## <a name="why-did-my-vm-fail-to-onboard"></a>Por que a minha VM não conseguir carregar?
Quando uma VM do Azure no portal do Azure de integração, ocorrem os seguintes passos:

* Uma área de trabalho do Log Analytics do padrão é criada, se que a opção tiver sido selecionada.
* Os contadores de desempenho são configurados para a área de trabalho selecionada. Caso este passo falhe, observe que algumas das tabelas e gráficos de desempenho não estão a mostrar dados para a VM efetuou. Pode corrigir este problema ao executar o script do PowerShell documentado [aqui](vminsights-onboard.md#enable-with-powershell).
* O agente Log Analytics está instalado em VMs do Azure com uma extensão de VM, se for determinado é necessário.  
* O Azure Monitor para agente de dependência de mapa de VMs é instalado em VMs do Azure com uma extensão, se for determinado é necessário.  
* Componentes de Monitor do Azure que suporta a funcionalidade de estado de funcionamento estão configurados, se necessário, e a VM está configurada para dados de estado de funcionamento do relatório.

Durante o processo de inclusão, verificamos para obter o estado em cada um dos acima descritos para retornar um status de notificação para si no portal.  Configuração de área de trabalho e a instalação do agente normalmente demora 5 a 10 minutos.  Visualização de dados de monitorização e estado de funcionamento no portal do demoram um 5 a 10 minutos adicionais.  

Se iniciou a integração e consulte as mensagens que indica que a VM tem de ser carregada, permitem até 30 minutos para a VM concluir o processo. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>Não vejo algumas ou quaisquer dados nos gráficos de desempenho para a minha VM
Se não vir dados de desempenho na tabela de disco ou em alguns dos gráficos de desempenho, em seguida, os contadores de desempenho não podem ser configurados na área de trabalho. Para resolver, execute o seguinte [script do PowerShell](vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-azure-monitor-for-vms-map-feature-different-from-service-map"></a>O que diferencia Azure Monitor para a funcionalidade de mapa de VMs do mapa de serviço?
O Azure Monitor para a funcionalidade de mapa de VMs é baseado em mapa de serviço, mas tem as seguintes diferenças:

* A vista do mapa pode ser acessada do painel da VM e do Azure Monitor para VMs do Azure Monitor.
* As ligações no mapa estão agora clicáveis e apresentam uma vista dos dados de métrica de ligação no painel lateral para a ligação selecionada.
* Há uma nova API, que é utilizada para criar os mapas para suportar melhor mapas mais complexos.
* VMs monitorizadas agora estão incluídas no nó do grupo de cliente e o gráfico em anel mostra a proporção de máquinas virtuais não monitorizadas do vs monitorizados no grupo.  Também pode ser utilizado para filtrar a lista de máquinas quando o grupo é expandido.
* Monitorizado máquinas de virtuais agora são incluídas em nós de grupo de porta de servidor e o gráfico em anel mostra a proporção de máquinas não monitorizadas do vs monitorizados no grupo.  Também pode ser utilizado para filtrar a lista de máquinas quando o grupo é expandido.
* O estilo de mapa foi atualizado para ser mais consistente com o mapa da aplicação do Application insights.
* Os painéis do lado foram atualizados, mas ainda não tem o conjunto completo de integração que eram suportadas no mapa de serviço - Gestão de atualizações, controlo de alterações, segurança e serviço de atendimento. 
* A opção para escolher os grupos e as máquinas para mapear foi atualizada e agora suporta subscrições, grupos de recursos, os conjuntos de dimensionamento de máquina virtual do Azure e serviços em nuvem.
* Não é possível criar novos grupos de máquina de mapa de serviço no Azure Monitor para a funcionalidade de mapa de VMs.  

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Por que motivo é que meu gráficos de desempenho mostra linhas pontilhadas?

Isto pode ocorrer por diversos motivos.  Em casos onde há uma lacuna na recolha de dados Vamos descrever as linhas como pontilhada.  Se modificar a frequência de amostragem de dados para os contadores de desempenho ativado (a predefinição é recolher dados de 60 em 60 segundos), pode ver linhas pontilhadas no gráfico, se optar por um intervalo de tempo de estreito para o gráfico e a frequência de amostragem é inferior a o tamanho de registo utilizado no gráfico (por exemplo, a frequência de amostragem é de 10 minutos e cada bucket no gráfico é de 5 minutos).  Escolher um intervalo de tempo maior para ver deve fazer com que as linhas do gráfico para aparecem como linhas sólidas em vez de pontos neste caso.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>São os grupos suportados com o Azure Monitor para VMs?
Sim, depois de instalar o agente de dependência informações que recolhemos as VMs para apresentar grupos com base na subscrição, grupo de recursos, máquina virtual conjuntos de dimensionamento e serviços cloud.  Se tem estado a utilizar o mapa de serviço e criar grupos de máquinas, estas serão apresentadas também.  Grupos de computadores também serão apresentada no filtro de grupos, se tiver criado para a área de trabalho que está a visualizar. 

## <a name="how-do-i-see-the-details-for-what-is-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Como posso ver os detalhes para o que está orientando o é o percentil 95 gráficos de linhas no agregado desempenho?
Por predefinição, a lista é ordenada para mostrar a as VMs que tenham o valor mais alto para o percentil 95 para a métrica selecionada, exceto para o gráfico de memória disponível, que mostra as máquinas com o menor valor de percentil de 5.  Ao clicar no gráfico abrirá o **lista de N principais** vista com a métrica apropriada selecionada.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-different-vnets-and-subnets"></a>Como a funcionalidade de mapa com IPs duplicado em diferentes vnets e sub-redes?
Se está duplicando intervalos de IP com VMs ou máquina virtual do Azure com conjuntos de dimensionamento em vnets e sub-redes, pode causar Azure Monitor para o mapa de VMs apresentar informações incorretas. Este é um problema conhecido e estamos a analisar opções para melhorar esta experiência.

## <a name="does-map-feature-support-ipv6"></a>É mapeado suporte a recursos IPv6?
Funcionalidade do mapa atualmente suporta apenas IPv4 e estamos a analisar o suporte para IPv6. Também suportamos IPv4 encapsulado dentro de IPv6.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-the-map-is-difficult-to-view"></a>Quando carrego um mapa de um grupo de recursos ou outro grupo grandes mapa é difícil ver
Embora tenhamos feito melhorias ao mapa para lidar com configurações de grandes e complexas, percebemos que um mapa pode ter muitos de nós, ligações e a funcionar como um cluster de nó.  Estamos empenhados em continuar melhorar o suporte para aumentar a escalabilidade.   

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-than-the-network-chart-on-the-azure-vm-overview-page"></a>Por que o gráfico de rede na guia desempenho ter um aspeto diferente no gráfico de rede na página de descrição geral de VM do Azure?

A página de descrição geral para uma VM do Azure apresenta gráficos com base em medição do anfitrião da atividade na VM do convidado.  Para o gráfico de rede na descrição de geral de VM do Azure, mostra apenas o tráfego de rede que será cobrado.  Isso não inclui o tráfego de inter-vnet.  Os dados e gráficos mostrados para o Azure Monitor para VMs é baseado nos dados da VM do convidado e a rede de gráficos apresenta todo o tráfego de TCP/IP que é a entrada e saída para essa VM, incluindo inter-vnet.

## <a name="are-their-limitations-if-i-am-on-the-log-analytics-free-pricing-plan"></a>São as suas limitações se estou no plano de preços gratuito do Log Analytics?
Se tiver configurado o Azure Monitor com uma área de trabalho do Log Analytics a utilizar o *gratuito* preços escalão, o Azure Monitor para a funcionalidade de mapa de VMs só irá suportar cinco máquinas ligadas ligado à área de trabalho. Se tiver cinco VMs ligadas a uma área de trabalho gratuita, desligar uma das VMs e, em seguida, ligar mais tarde uma nova VM, a nova VM não é monitorizada e refletida na página de mapa.  

Sob essa condição, serão solicitadas com o **experimentar agora** opção quando abrir a VM e selecione **Insights (pré-visualização)** no painel esquerdo, até mesmo depois que tiver sido já instalado na VM.  No entanto, não obterá opções tal como faria normalmente se esta VM não foram carregadas para o Azure Monitor para as VMs. 

## <a name="next-steps"></a>Passos Seguintes
Revisão [carregar Monitor do Azure para VMs](vminsights-onboard.md) para compreender os requisitos e métodos para ativar a monitorização das suas máquinas virtuais.
