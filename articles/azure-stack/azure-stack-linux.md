---
title: Adicionar imagens de Linux a pilha do Azure
description: Saiba como adicionar imagens de Linux com pilha do Azure.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6e6f9ca3b314ee2f58d8007e7ddc93ddd213e361
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Adicionar imagens de Linux a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode implementar máquinas de virtuais (VMs) do Linux na pilha do Azure através da adição de uma imagem baseado em Linux no Azure Marketplace pilha. É a forma mais fácil para adicionar uma imagem de Linux a pilha do Azure através da gestão do Marketplace. Estas imagens tenham sido preparadas e testadas relativamente à compatibilidade com a pilha do Azure.

## <a name="marketplace-management"></a>Gestão do Marketplace

Para transferir as imagens de Linux no Azure Marketplace, utilize os procedimentos no seguinte artigo. Selecione as imagens de Linux que pretende oferecem aos utilizadores na sua pilha do Azure.

[Transferir itens do marketplace do Azure para Azure pilha](azure-stack-download-azure-marketplace-item.md).

## <a name="prepare-your-own-image"></a>Preparar a sua própria imagem

Preparar a sua própria imagem Linux utilizando um dos seguintes instruções:

   * [Distribuições baseado em centOS](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. Transfira e instale o [agente Linux do Azure](https://github.com/Azure/WALinuxAgent/).

    O agente Linux do Azure versão 2.2.2 ou superior, é necessário para aprovisionar a VM do Linux na pilha do Azure e algumas versões não funcionam (2.2.12 e 2.2.13, para obter exemplos). Muitas das distribuições listadas anteriormente já incluem uma versão do agente (normalmente designado `WALinuxAgent` ou `walinuxagent`). Se criar uma imagem personalizada, tem de instalar o agente manualmente. É possível determinar a versão instalada do nome do pacote ou executando `/usr/sbin/waagent -version` na VM.

    Siga as instruções abaixo para instalar manualmente - o agente Linux do Azure

   a. Em primeiro lugar, transfira o agente Linux do Azure mais recente do [GitHub](https://github.com/Azure/WALinuxAgent/releases), exemplo:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.21.tar.gz
   b. Descompacte o agente do Azure:

            # tar -vzxf v2.2.21.tar.gz
   c. Instalar o python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools
   d. Instale o agente do Azure:

            # cd WALinuxAgent-2.2.21
            # sudo python3 setup.py install --register-service

     Os sistemas com o Python 2. x e Python 3 instalado do lado do lado a lado poderão ter de executar o seguinte comando:

         sudo python3 setup.py install --register-service
     Para obter mais informações, consulte o agente Linux do Azure [Leia-me](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
2. [Adicionar a imagem para o mercado](azure-stack-add-vm-image.md). Certifique-se de que o `OSType` parâmetro está definido como `Linux`.
3. Depois de adicionar a imagem do Marketplace, é criado um item do Marketplace e os utilizadores podem implementar uma máquina virtual Linux.

## <a name="next-steps"></a>Passos Seguintes
[Descrição geral da oferta de serviços na pilha do Azure](azure-stack-offer-services-overview.md)
