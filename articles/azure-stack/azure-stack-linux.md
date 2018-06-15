---
title: Adicionar imagens de Linux a pilha do Azure
description: Saiba como adicionar imagens de Linux com pilha do Azure.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 64a860bc925b9c7499363c1fe39d03df88a9a51d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33935721"
---
# <a name="add-linux-images-to-azure-stack"></a>Adicionar imagens de Linux a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode implementar máquinas de virtuais (VMs) do Linux na pilha do Azure através da adição de uma imagem baseado em Linux no Azure Marketplace pilha. É a forma mais fácil para adicionar uma imagem de Linux a pilha do Azure através da gestão do Marketplace. Estas imagens tenham sido preparadas e testadas relativamente à compatibilidade com a pilha do Azure.

## <a name="marketplace-management"></a>Gestão do Marketplace

Para transferir as imagens de Linux no Azure Marketplace, utilize os procedimentos no seguinte artigo. Selecione as imagens de Linux que pretende oferecem aos utilizadores na sua pilha do Azure. 

[Transferir itens do marketplace do Azure para Azure pilha](azure-stack-download-azure-marketplace-item.md).

Tenha em atenção que existem atualizações frequentes para estas imagens, por isso, verifique a gestão de Marketplace, muitas vezes, para manter atualizados.

## <a name="prepare-your-own-image"></a>Preparar a sua própria imagem

 Sempre que possível, transfira as imagens disponíveis através da gestão de Marketplace que tenham sido preparado e testado relativamente à pilha do Azure. 
 
 O agente Linux do Azure (normalmente designado `WALinuxAgent` ou `walinuxagent`) é necessário, e nem todas as versões do agente irão funcionar na pilha do Azure. Deve utilizar a versão 2.2.18 ou posterior, se criar a sua própria imagem. Tenha em atenção que [nuvem init](https://cloud-init.io/) , neste momento, não é suportada na pilha do Azure.

 Preparar a sua própria imagem Linux utilizando as instruções seguintes:

   * [Distribuições baseado em centOS](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](azure-stack-redhat-create-upload-vhd.md)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu Server](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

    
## <a name="add-your-image-to-the-marketplace"></a>Adicionar a imagem do Marketplace
 
Siga [adicionar a imagem para o mercado](azure-stack-add-vm-image.md). Certifique-se de que o `OSType` parâmetro está definido como `Linux`.

Depois de adicionar a imagem do Marketplace, é criado um item do Marketplace e os utilizadores podem implementar uma máquina virtual Linux.
