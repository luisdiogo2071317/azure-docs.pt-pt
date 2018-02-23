---
title: "Monitorizar ligações de rede com o observador de rede do Azure - portal do Azure | Microsoft Docs"
description: Saiba como monitorizar a conectividade de rede com o observador de rede do Azure no portal do Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: beaa458d727a05eccf496933deb3c998828868cd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Monitorizar ligações de rede com o observador de rede do Azure no portal do Azure

Saiba como utilizar o monitor de ligação para monitorizar a conectividade de rede entre uma máquina virtual do Azure e um endereço IP. O endereço IP pode ser atribuído a outro recurso do Azure ou um recurso de Internet ou no local.

## <a name="prerequisites"></a>Pré-requisitos

Tem de cumprir os seguintes pré-requisitos antes de concluir os passos neste artigo:

* Uma instância do observador de rede na região que pretende monitorizar uma ligação para. Se ainda não tiver uma, pode criar um, efetuando os passos em [criar uma instância de observador de rede de Azure](network-watcher-create.md).
* Uma máquina virtual para monitorizar a partir da.
* Tem a `AzureNetworkWatcherExtension` instalado na máquina virtual que pretende monitorizar uma ligação a partir. Para instalar a extensão na máquina virtual do Windows, consulte [extensão da máquina virtual de agente de observador de rede do Azure para Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e instalar a extensão numa máquina virtual Linux, consulte [agente de observador de rede do Azure extensão da máquina virtual para Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no [portal do Azure](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Criar um monitor de ligação

1. No lado esquerdo do portal, selecione **mais serviços**.
2. Comece a escrever *observador de rede*. Quando **observador de rede** aparece nos resultados da pesquisa, selecionados-lo.
3. Em **monitorização**, selecione **monitor de ligação (pré-visualização)**. Funcionalidades de pré-lançamento não têm o mesmo nível de fiabilidade ou a disponibilidade de região como funcionalidades de versão em geral.
4. Selecione **+ adicionar**.
5. Introduza ou selecione as informações adequadas para a ligação que pretende monitorizar e, em seguida, selecione **adicionar**. Neste exemplo, uma ligação entre o *myVmSource* e *myVmDestination* máquinas virtuais está a ser monitorizado através da porta 80.
    
    |  Definição                                 |  Valor               |
    |  -------------------------------------   |  ------------------- |
    |  Nome                                    |  myConnectionMonitor |
    |  Máquina virtual de origem                  |  myVmSource          |
    |  Porta de origem                             |                      |
    |  Destino, selecione uma máquina virtual   |  myVmDestination     |
    |  Porta de destino                        |  80                  |

6. Monitorização começa. Monitor de ligação sondas 60 em 60 segundos.
7. Monitor de ligação mostra as média ida e volta hora e a percentagem de sondas que não obedeçam a. Pode ver os dados de monitor de grelha ou um gráfico.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como automatizar capturas de pacotes com alertas de máquina virtual por [criar uma captura de pacotes de alerta acionado](network-watcher-alert-triggered-packet-capture.md).

- Determinar se determinados o tráfego é permitido dentro ou fora da sua máquina virtual utilizando [Certifique-se de fluxo IP](network-watcher-check-ip-flow-verify-portal.md).