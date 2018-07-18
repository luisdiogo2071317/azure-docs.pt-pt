---
title: Gerir capturas de pacotes com o observador de rede do Azure - portal do Azure | Documentos da Microsoft
description: Esta página explica como gerir a funcionalidade de captura de pacotes do observador de rede através do portal do Azure
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 18e5f8eda51f8834f6346eef3d7ad31bc556290a
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090202"
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Gerir capturas de pacotes com o observador de rede do Azure com o portal

> [!div class="op_single_selector"]
> - [Portal do Azure](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI do Azure](network-watcher-packet-capture-manage-cli.md)
> - [API de REST do Azure](network-watcher-packet-capture-manage-rest.md)

Captura de pacotes do observador de rede permite-lhe criar sessões de captura para controlar o tráfego de e para uma máquina virtual. Filtros são fornecidos para a sessão de captura garantir que capturar apenas o tráfego que pretende. Captura de pacotes ajuda a diagnosticar anomalias de rede, de forma reativa e de forma pró-ativa. Outras utilizações incluem a recolha de estatísticas de rede, obter informações sobre a invasões de rede, para depurar as comunicações cliente-servidor e muito mais. Por poder para acionar remotamente capturas de pacotes, esta capacidade alivie o trabalho de executar uma captura de pacotes manualmente e a máquina desejado, que economiza um valioso tempo.

Este artigo orienta-o por tarefas de gestão diferentes que estão atualmente disponíveis para captura de pacotes.

- [**Iniciar uma captura de pacotes**](#start-a-packet-capture)
- [**Parar uma captura de pacotes**](#stop-a-packet-capture)
- [**Eliminar uma captura de pacotes**](#delete-a-packet-capture)
- [**Transferir uma captura de pacotes**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Antes de começar

Este artigo pressupõe que tem os seguintes recursos:

- Uma instância do observador de rede na região que pretende criar uma captura de pacotes
- Uma máquina virtual com a extensão de captura de pacotes ativada.

> [!IMPORTANT]
> Captura de pacotes requer uma extensão de máquina virtual `AzureNetworkWatcherExtension`. Para instalar a extensão numa VM do Windows, visite [extensão de máquina virtual de agente do observador de rede do Azure para Windows](../virtual-machines/windows/extensions-nwa.md) e para visite de VM do Linux [extensão da máquina virtual de agente do observador de rede do Azure para Linux](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-the-portal"></a>Extensão de agente de captura de pacotes através do portal

Para instalar o agente VM de captura de pacotes através do portal, navegue até à sua máquina virtual, clique em **extensões** > **Add** , procure por **rede observador de agente para Windows**

![Vista de agentes][agent]

## <a name="packet-capture-overview"></a>Descrição geral de captura de pacotes

Navegue para o [portal do Azure](https://portal.azure.com) e clique em **redes** > **observador de rede** > **captura de pacotes**

A página de visão geral mostra a que captura de uma lista de todos os pacotes existentes não importa o estado.

> [!NOTE]
> Captura de pacotes requer seguindo conectivity.
> * Conectividade de saída para a conta de armazenamento através da porta 443.
> * Conectividade de entrada e saída para 169.254.169.254
> * Conectividade de entrada e saída para 168.63.129.16

![ecrã de descrição geral de captura de pacotes][1]

## <a name="start-a-packet-capture"></a>Iniciar uma captura de pacotes

Clique em **adicionar** para criar uma captura de pacotes.

As propriedades que podem ser definidas numa captura de pacotes são:

**Definições principais**

- **Subscrição** -este valor é a subscrição que é utilizada, uma instância do observador de rede é necessária em cada subscrição.
- **Grupo de recursos** -o grupo de recursos da máquina virtual que estão a ser visada.
- **Máquina virtual de destino** -a máquina virtual que está a executar a captura de pacotes
- **O nome de captura de pacotes** -este valor é o nome da captura de pacotes.

**Capturar configuração**

- **Caminho do ficheiro local** -um caminho Local na máquina virtual onde é guardada a captura de pacotes (válido apenas quando **[ficheiro]** está selecionado). Tem de especificar um caminho válido. Se estiver a utilizar uma máquina virtual do Linux, o caminho tem de começar com / var / captura.
- **Conta de armazenamento** -determina se a captura de pacotes é guardada numa conta de armazenamento.
- **Ficheiro** -determina se uma captura de pacotes é guardada localmente na máquina virtual.
- **Contas de armazenamento** – a conta de armazenamento para guardar a captura de pacotes no selecionada. Localização predefinida é o id da conta de https://{storage name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription} /resourcegroups/ {nome da máquina do name}/providers/microsoft.compute/virtualmachines/{virtual de grupo de recursos} / {aa} / {MM} / {DD} / packetcapture_ {HH}_{MM}_. cap do {SS} _ {XXX}. (Apenas ativado se **armazenamento** está selecionado)
- **Caminho do ficheiro local** -o caminho local numa máquina virtual para salvar a captura de pacotes. (Apenas ativado se **ficheiro** está selecionado). Tem de ser fornecido um caminho válido. Para uma máquina virtual do Linux, o caminho tem de começar com */var/captura*.
- **Máximo de bytes por pacote** – o número de bytes de cada pacote que são capturados, todos os bytes são capturados se deixado em branco.
- **Máximo de bytes por sessão** - Total o número de bytes que são capturados, assim que o valor for atingido o pára de captura de pacotes.
- **Tempo limite (segundos)** -define um limite de tempo para a captura de pacotes parar. A predefinição é 18000 segundos.

> [!NOTE]
> Contas de armazenamento Premium não são atualmente suportadas para armazenar o pacote de captura.

**Filtragem (opcional)**

- **Protocolo** -o protocolo para filtrar para a captura de pacotes. Os valores disponíveis são TCP, UDP e qualquer um.
- **Endereço IP local** -este valor filtra a captura de pacotes para os pacotes em que o endereço IP local corresponde a este valor de filtro.
- **Porta local** -este valor filtra a captura de pacotes para pacotes onde a porta local corresponde a este valor de filtro.
- **Endereço IP remoto** -este valor filtra a captura de pacotes para pacotes onde o IP remoto corresponde a este valor de filtro.
- **Porta remota** -este valor filtra a captura de pacotes para pacotes onde a porta remota corresponde a este valor de filtro.

> [!NOTE]
> Valores de endereços IP e porta podem ser um valor único, intervalo de valores ou um conjunto. (ou seja, a 80-1024 para a porta) Pode definir filtros tantas quanto quiser.

Assim que os valores são preenchidos, clique em **OK** para criar a captura de pacotes.

![criar uma captura de pacotes][2]

Depois que o limite de tempo definido na captura de pacotes tiver expirado, a captura de pacotes será interrompida e pode ser revista. Pode também manualmente parar as sessões de capturas de pacotes.

## <a name="delete-a-packet-capture"></a>Eliminar uma captura de pacotes

Na vista de captura de pacotes, clique nas **menu de contexto** (...) ou com o botão direito e clique em **eliminar** para parar a captura de pacotes

![eliminar uma captura de pacotes][3]

> [!NOTE]
> Eliminar uma captura de pacotes não elimina o ficheiro na conta de armazenamento.

É-lhe perguntado para confirmar que pretende eliminar a captura de pacotes, clique em **Sim**

![Confirmação][4]

## <a name="stop-a-packet-capture"></a>Parar uma captura de pacotes

Na vista de captura de pacotes, clique nas **menu de contexto** (...) ou com o botão direito e clique em **parar** para parar a captura de pacotes

## <a name="download-a-packet-capture"></a>Transferir uma captura de pacotes

Depois de concluída a sua sessão de captura de pacotes, o ficheiro de captura é carregado para o armazenamento de BLOBs ou para um ficheiro local na VM. A localização de armazenamento da captura de pacotes é definida durante a criação da sessão. Uma ferramenta conveniente para aceder a estes ficheiros de captura guardados para uma conta de armazenamento é Explorador de armazenamento do Microsoft Azure, que pode ser baixado aqui:  http://storageexplorer.com/

Se for especificada uma conta de armazenamento, os arquivos de captura de pacotes são guardados para uma conta de armazenamento na seguinte localização:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Passos Seguintes

Saiba como automatizar as capturas de pacotes com alertas de Máquina Virtual, visualizando [criar uma captura de pacotes acionadas alerta](network-watcher-alert-triggered-packet-capture.md)

Localizar se determinado tráfego é permitido dentro ou fora da sua VM, visite a página [verificação do fluxo de IP de verificação](diagnose-vm-network-traffic-filtering-problem.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













