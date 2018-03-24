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
ms.openlocfilehash: c7d98350dc8f66ebd4097f22b44dcbbe2653b25d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Monitorizar ligações de rede com o observador de rede do Azure no portal do Azure

Saiba como utilizar o monitor de ligação para monitorizar a conectividade de rede entre uma máquina virtual do Azure e um endereço IP. Monitor de ligação fornece monitorização a um nível de ligação. Uma ligação é definida como uma combinação de origem e porta e endereço IP de destino. Monitor de ligação permite cenários, como a monitorização de conectividade de uma máquina virtual numa rede virtual a uma máquina virtual com o SQL server na rede virtual idêntica ou diferente, através da porta é 1433. Monitor de ligação fornece latência de ligação como uma métrica de Monitor do Azure, registada 60 em 60 segundos. Também fornece-lhe uma topologia de salto-por-hop e identifica problemas configuration afetar a ligação.


## <a name="prerequisites"></a>Pré-requisitos

Tem de cumprir os seguintes pré-requisitos antes de concluir os passos neste artigo:

* Uma instância do observador de rede na região que pretende monitorizar uma ligação para. Se ainda não tiver uma, pode criar um, efetuando os passos em [criar uma instância de observador de rede de Azure](network-watcher-create.md).
* Uma máquina virtual para monitorizar a partir da.
* Tem a `AzureNetworkWatcherExtension` instalado na máquina virtual que pretende monitorizar uma ligação a partir. Para instalar a extensão na máquina virtual do Windows, consulte [extensão da máquina virtual de agente de observador de rede do Azure para Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e instalar a extensão numa máquina virtual Linux, consulte [agente de observador de rede do Azure extensão da máquina virtual para Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no [portal do Azure](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Criar um monitor de ligação

Os passos seguintes permitem a monitorização de ligação a uma máquina virtual de destino através das portas 80 e 1433:

1. No lado esquerdo do portal, selecione **mais serviços**.
2. Comece a escrever *observador de rede*. Quando **observador de rede** aparece nos resultados da pesquisa, selecionados-lo.
3. Em **monitorização**, selecione **monitor de ligação (pré-visualização)**. Funcionalidades de pré-lançamento não têm o mesmo nível de fiabilidade ou a disponibilidade de região como funcionalidades de versão em geral.
4. Selecione **+ adicionar**.
5. Introduza ou selecione as informações da ligação que pretende monitorizar e, em seguida, selecione **adicionar**. No exemplo mostrado na imagem seguinte, a ligação monitorizada está entre o *MultiTierApp0* e *Database0* máquinas virtuais:

    ![Adicionar ligação Monitor](./media/connection-monitor/add-connection-monitor.png)

    Monitorização começa. Monitor de ligação sondas 60 em 60 segundos.

## <a name="view-connection-monitoring"></a>Vista de monitorização de ligação

1. Conclua os passos 1 a 3 no [criar um monitor de ligação](#create-a-connection-monitor) para ver a monitorização de ligação.
2. A imagem seguinte mostra os detalhes para a ligação de AppToDB(80). O **estado** está acessível. O **Graph vista** mostra o **tempo médio de ida e volta** e **% sondas falha**. O gráfico fornece informações de salto-por-hop e mostra o que não existem problemas têm maior impacto na reachability de destino.

    ![Monitor de ligação de vista](./media/connection-monitor/view-connection-monitor.png)

3. Visualizar o *AppToDB(1433)* monitor, mostrado na imagem seguinte, pode ver que para a mesma origem e de máquinas virtuais de destino, o estado não está acessível através da porta é 1433. O **vista de grelha** neste cenário fornece as informações de salto-por-hop e o problema afetar reachability. Neste caso, uma regra NSG está a bloquear todo o tráfego na porta 1433, o segundo hop.

    ![Monitor de ligação de vista](./media/connection-monitor/view-connection-monitor-2.png)

## <a name="next-steps"></a>Passos Seguintes

- Saiba como automatizar capturas de pacotes com alertas de máquina virtual por [criar uma captura de pacotes de alerta acionado](network-watcher-alert-triggered-packet-capture.md).
- Determinar se determinados o tráfego é permitido dentro ou fora da sua máquina virtual utilizando [Certifique-se de fluxo IP](network-watcher-check-ip-flow-verify-portal.md).