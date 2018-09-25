---
title: Criar uma VM do Linux clássica com a CLI clássica do Azure | Documentos da Microsoft
description: Saiba como criar uma máquina virtual Linux com a CLI clássica do Azure com o modelo de implementação clássico
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
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
ms.author: cynthn
ms.openlocfilehash: 507d9e12a37d7bf187a3e56b04cb47ac0104773d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982046"
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-classic-cli"></a>Como criar uma VM do Linux clássica com a CLI clássica do Azure
> [!IMPORTANT] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Para a versão do Resource Manager, consulte [aqui](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Este tópico descreve como criar uma máquina virtual (VM) do Linux com a CLI clássica do Azure com o modelo de implementação clássico. Utilizamos uma imagem do Linux da disponíveis **imagens** no Azure. Os comandos da CLI clássicos do Azure oferecem as seguintes opções de configuração, entre outras:

* Ligar a VM a uma rede virtual
* Adicionar a VM para um serviço cloud existente
* Adicionar a VM a uma conta de armazenamento existente
* Adicionar a VM ao conjunto de disponibilidade ou localização

> [!IMPORTANT]
> Se pretender que a VM para utilizar uma rede virtual para que possam ligar à mesma diretamente por nome de anfitrião ou configuração de ligações entre locais, certifique-se de que especificar a rede virtual, ao criar a VM. Uma VM pode ser configurada para associar uma rede virtual apenas quando criar a VM. Para obter detalhes sobre redes virtuais, consulte [descrição geral de rede Virtual do Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Como criar uma VM do Linux utilizar o modelo de implementação clássica
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

