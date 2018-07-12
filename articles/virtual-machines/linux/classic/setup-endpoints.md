---
title: Configurar pontos finais numa VM do Linux clássica | Documentos da Microsoft
description: Saiba como configurar a pontos finais para uma VM do Linux no portal do Azure para permitir a comunicação com uma máquina virtual do Linux no Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 03cb90dcdcc7a7407898ddd8cbc30f1af0833676
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38295692"
---
# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Como configurar pontos de extremidade numa máquina virtual clássica do Linux no Azure
Todas as máquinas virtuais do Linux que criar no Azure com o modelo de implementação clássica podem automaticamente comunicar através de um canal de rede privada com outras máquinas virtuais no mesmo serviço cloud ou rede virtual. No entanto, os computadores na Internet ou de outras redes virtuais requerem pontos finais para direcionar o tráfego de rede de entrada para uma máquina virtual. Este artigo também está disponível para [as máquinas virtuais do Windows](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Na **Resource Manager** modelo de implementação, pontos finais são configurados utilizando **grupos de segurança de rede (NSGs)**. Para obter mais informações, consulte [abrir portas e pontos finais](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Quando cria uma máquina virtual Linux no portal do Azure, um ponto de extremidade para Secure Shell (SSH) é normalmente criado automaticamente. Pode configurar pontos finais adicionais ao criar a máquina virtual ou, em seguida, conforme necessário.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Passos Seguintes
* Também pode criar um ponto final da VM utilizando o [Interface de linha de comandos do Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Executar o **criar o ponto final da vm do azure** comando.
* Se tiver criado uma máquina virtual no modelo de implementação do Resource Manager, pode utilizar a CLI do Azure no modo Resource Manager para [criar grupos de segurança de rede](../../../virtual-network/tutorial-filter-network-traffic-cli.md) para controlar o tráfego para a VM.
