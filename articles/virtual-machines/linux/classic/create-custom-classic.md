---
title: "Criar uma VM com Linux clássica utilizando a CLI do Azure 1.0 | Microsoft Docs"
description: "Saiba como criar uma máquina virtual Linux com a CLI do Azure 1.0 utilizando o modelo de implementação clássica"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 6e8f7e7e4bf26ac83612a73888cd0aea9eaa4c90
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a>Como criar uma VM com Linux clássico com a CLI do Azure 1.0
> [!IMPORTANT] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássico](../../../resource-manager-deployment-model.md). Este artigo abrange utilizando o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Para a versão do Gestor de recursos, consulte [aqui](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Este tópico descreve como criar uma máquina virtual (VM) do Linux com a CLI do Azure 1.0 utilizando o modelo de implementação clássica. Utilizamos uma imagem de Linux do disponíveis **imagens** no Azure. Os comandos da CLI do Azure 1.0 conceda as seguintes opções de configuração, entre outras pessoas:

* Ligar a VM a uma rede virtual
* Adicionar a VM num serviço em nuvem existente
* Adicionar a VM a uma conta de armazenamento existente
* Adicionar a VM para um conjunto de disponibilidade ou localização

> [!IMPORTANT]
> Se pretender que a VM para utilizar uma rede virtual para que possa ligar ao mesmo diretamente pelo nome de anfitrião ou configurar ligações entre locais, certifique-se de que especificar a rede virtual, quando criar a VM. Uma VM pode ser configurada para associar uma rede virtual, apenas quando criar a VM. Para obter detalhes sobre redes virtuais, consulte [descrição geral de rede Virtual do Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Como criar uma VM com Linux utilizando o modelo de implementação clássica
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

