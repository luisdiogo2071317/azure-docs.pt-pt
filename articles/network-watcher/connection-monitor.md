---
title: Monitorizar ligações de rede com o observador de rede do Azure - portal do Azure | Microsoft Docs
description: Saiba como monitorizar a conectividade de rede com o observador de rede do Azure no portal do Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: 0d550d3bda119cfcb9ecc6f852006d5e325fdfa3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Monitorizar ligações de rede com o observador de rede do Azure no portal do Azure

Saiba como utilizar o monitor de ligação para monitorizar a conectividade de rede entre uma Máquina Virtual do Azure (VM) e um endereço IP. Monitor de ligação fornece monitorização entre o endereço IP de origem e de destino e a porta. Monitor de ligação permite cenários, como a monitorização de conectividade de uma VM numa rede virtual para uma VM com o SQL server na rede virtual idêntica ou diferente, através da porta é 1433. Monitor de ligação fornece latência de ligação como uma métrica de Monitor do Azure, registada 60 em 60 segundos. Também fornece-lhe uma topologia de salto-por-hop e identifica problemas configuration afetar a ligação.

## <a name="prerequisites"></a>Pré-requisitos

Tem de cumprir os seguintes pré-requisitos antes de concluir os passos neste artigo:

* Uma instância do observador de rede na região que pretende monitorizar uma ligação para. Se ainda não tiver uma, pode criar um, efetuando os passos em [criar uma instância de observador de rede de Azure](network-watcher-create.md).
* Monitorização a partir de uma VM. Para saber como criar uma VM, consulte Criar uma [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ou [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) VM.
* Tem a `AzureNetworkWatcherExtension` instalado na VM que pretende monitorizar uma ligação a partir. Para instalar a extensão na VM do Windows, consulte [extensão da máquina virtual de agente de observador de rede do Azure para Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e instalar a extensão, consulte uma VM com Linux [extensão da máquina virtual para o agente de observador de rede do Azure Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no [portal do Azure](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Criar um monitor de ligação

Os passos seguintes permitem a monitorização de ligação para uma VM de destino através das portas 80 e 1433:

1. No lado esquerdo do portal, selecione **todos os serviços**.
2. Comece a escrever *observador de rede* no **filtro** caixa. Quando **observador de rede** aparece nos resultados da pesquisa, selecionados-lo.
3. Em **monitorização**, selecione **monitor ligação**.
4. Selecione **+ adicionar**.
5. Introduza ou selecione as informações da ligação que pretende monitorizar e, em seguida, selecione **adicionar**. No exemplo mostrado na imagem seguinte, a ligação monitorizada é do *MultiTierApp0* VM para o *Database0* VM através da porta 80:

    ![Adicionar ligação Monitor](./media/connection-monitor/add-connection-monitor.png)

    Monitorização começa. Monitor de ligação sondas 60 em 60 segundos.
6. Conclua o passo 5 novamente, especificando o mesmo VMs de origem e de destino e os seguintes valores:
    
    |Definição  |Valor          |
    |---------|---------      |
    |Nome     | AppToDB(1433) |
    |Porta     | 1433          |

## <a name="view-connection-monitoring"></a>Vista de monitorização de ligação

1. Conclua os passos 1 a 3 no [criar um monitor de ligação](#create-a-connection-monitor) para ver a monitorização de ligação.
2. A imagem seguinte mostra os detalhes para a *AppToDB(80)* ligação. O **estado** está acessível. O **Graph vista** mostra o **tempo médio de ida e volta** e **% sondas falha**. O gráfico fornece informações de salto-por-hop e mostra o que não existem problemas têm maior impacto na reachability de destino.

    ![Vista do gráfico](./media/connection-monitor/view-graph.png)

3. Visualizar o *AppToDB(1433)* ligação, mostrada na imagem seguinte, pode ver que para a mesma origem e destino VMs, o estado não está acessível através da porta é 1433. O **vista de grelha** neste cenário fornece as informações de salto-por-hop e o problema afetar reachability. Neste caso, uma regra NSG está a bloquear todo o tráfego na porta 1433, o segundo hop.

    ![Vista de grelha](./media/connection-monitor/view-grid.png)

## <a name="next-steps"></a>Passos Seguintes

- Saiba como automatizar capturas de pacotes com alertas VM por [criar uma captura de pacotes de alerta acionado](network-watcher-alert-triggered-packet-capture.md).
- Determinar se determinados o tráfego é permitido dentro ou fora da VM utilizando [Certifique-se de fluxo IP](network-watcher-check-ip-flow-verify-portal.md).