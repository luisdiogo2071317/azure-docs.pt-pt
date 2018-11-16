---
title: O que é o Azure Monitor para VMs (pré-visualização)? | Microsoft Docs
description: Monitor do Azure para VMs é um recurso do Azure Monitor, que combina o estado de funcionamento e desempenho de monitorização do sistema de operacional de VM do Azure, bem como detetar automaticamente os componentes da aplicação e dependências com outros recursos e mapeia a comunicação entre eles. Este artigo fornece uma visão geral.
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
ms.openlocfilehash: d37ca7d46f1231a8e1b0b258c10089fe6ba81fba
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51715873"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>O que é o Azure Monitor para VMs (pré-visualização)?

Monitor do Azure para VMs monitoriza as suas máquinas virtuais do Azure (VM) e conjuntos de dimensionamento de máquina virtual do Azure em escala ao analisar o desempenho e estado de funcionamento do Windows e VMs do Linux, incluindo os respetivos diferentes processos e interconectadas dependências em outros os recursos e processos externos. A solução inclui suporte para monitorização do desempenho e dependências de aplicações para as VMs alojadas no local ou de outro fornecedor de cloud. Ele inclui três recursos principais para fornecer esta informação detalhada:

* Componentes lógicos de VMs do Azure a executar o sistema operacional Windows e Linux são medidos com base num conjunto de critérios de estado de funcionamento pré-configuradas e alertas quando a condição avaliada é cumprida.  
* Métricas de desempenho de núcleos de processador, memória, disco e placa de rede do sistema de operativo da VM de convidado são recolhidas e apresentadas nos gráficos de desempenho populares predefinidas.
* Mapa de dependência que mostra os componentes interconectados detetados com essa VM de vários grupos de recursos e subscrições.  

Estas funcionalidades estão organizadas nas três perspectivas:

* Estado de Funcionamento
* Desempenho
* Mapa

>[!NOTE]
>Atualmente, a funcionalidade de estado de funcionamento só é oferecida para máquinas virtuais do Azure e o dimensionamento de máquinas virtuais conjuntos. Desempenho e mapa suportam VMs do Azure e máquinas virtuais alojadas no seu ambiente ou de outro fornecedor de cloud.
>

Integração com o Log Analytics oferece poderosas agregação, filtragem e capacidade de executar a análise de tendências dos dados ao longo do tempo. A monitorização abrangente das cargas de trabalho não pode ser obtida como está com o Azure Monitor, o mapa de serviço ou o Log Analytics.  

Pode ver estes dados no contexto de uma VM única da máquina virtual diretamente ou com o Azure Monitor proporciona uma exibição agregada das suas VMs com base no ponto de vista do seguinte para cada funcionalidade:

* Estado de funcionamento - VMs relacionados com um grupo de recursos
* Mapa e desempenho - as VMs configuradas para reportar a uma área de trabalho do Log Analytics específica

![Perspetiva de informações da máquina virtual do portal](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

DevOps efetivamente pode fornecer um desempenho previsível e a disponibilidade de aplicações vitais ao identificar eventos críticos do sistema operativo e afunilamentos de desempenho, problemas de rede e compreender se um problema está relacionado com outras dependências.  

## <a name="data-usage"></a>Utilização de dados 

Logo que carregar o Azure Monitor para as VMs, os dados recolhidos por suas VMs é ingerido e armazenado no Azure Monitor. Monitor do Azure para VMs é faturado para os dados ingeridos e mantidas, vários critérios de estado de funcionamento métrica séries temporais monitorizadas, alerta as regras criadas, as notificações enviadas, preço por publicado no Monitor do Azure [página de preços](https://azure.microsoft.com/pricing/details/monitor/)

O tamanho do registo varia com base no comprimentos de cadeia de caracteres de contadores e pode aumentar com o número de discos lógicos e adaptadores de rede. Se já tiver uma área de trabalho e está a recolher estes contadores, não haverá quaisquer encargos duplicados aplicados. Se já estiver a utilizar o mapa de serviço, a única alteração que verá é os dados de ligação adicional a ser enviados para o Azure Monitor.

## <a name="next-steps"></a>Passos Seguintes
Revisão [carregar Monitor do Azure para VMs](vminsights-onboard.md) para compreender os requisitos e métodos para ativar a monitorização das suas máquinas virtuais.
