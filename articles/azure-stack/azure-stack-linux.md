---
title: Adicionar imagens do Linux no Azure Stack
description: Saiba como adicionar imagens do Linux no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: db52d145c3bfbd9415072be13ccb502969f07374
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077652"
---
# <a name="add-linux-images-to-azure-stack"></a>Adicionar imagens do Linux no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode implementar máquinas virtuais do Linux (VMs) no Azure Stack ao adicionar uma imagem baseada em Linux no Azure Marketplace, pilha. A maneira mais fácil para adicionar uma imagem do Linux no Azure Stack é através da gestão do Marketplace. Estas imagens tem sido preparadas e testadas quanto à compatibilidade com o Azure Stack.

## <a name="marketplace-management"></a>Gestão do Marketplace

Para transferir imagens do Linux no Azure Marketplace, utilize os procedimentos no seguinte artigo. Selecione as imagens do Linux que deseja oferecer aos utilizadores no seu Azure Stack. 

[Transferir itens do marketplace do Azure para o Azure Stack](azure-stack-download-azure-marketplace-item.md).

Tenha em atenção que existem atualizações freqüentes para estas imagens, por isso, verifique a gestão de Marketplace, muitas vezes, para manter atualizados.

## <a name="prepare-your-own-image"></a>Preparar a sua própria imagem

 Sempre que possível, transfira as imagens disponíveis por meio do gerenciamento de mercado que tenham sido preparado e testado para o Azure Stack. 
 
 O agente Linux do Azure (normalmente denominados `WALinuxAgent` ou `walinuxagent`) é necessário, e nem todas as versões do agente irão funcionar no Azure Stack. Deve usar a versão 2.2.18 ou posterior se criar sua própria imagem. Tenha em atenção que [cloud-init](https://cloud-init.io/) , neste momento, não é suportado no Azure Stack.

 Pode se preparar a sua própria imagem de Linux utilizar as instruções seguintes:

   * [Distribuições baseada em centOS](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

    
## <a name="add-your-image-to-the-marketplace"></a>Adicionar a sua imagem no Marketplace
 
Siga [adicionar a imagem no Marketplace](azure-stack-add-vm-image.md). Certifique-se de que o `OSType` parâmetro estiver definido como `Linux`.

Depois de adicionar a imagem no Marketplace, é criado um item do Marketplace e os utilizadores podem implementar uma máquina virtual Linux.
