---
title: Criar uma máquina virtual Linux utilizando a CLI do Azure na pilha do Azure | Microsoft Docs
description: Crie uma máquina virtual Linux com a CLI na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 21F7D599-1FEC-4827-A5C3-06495C5F53A4
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 90b36183ba32e75e06d434098d26cb10f3736373
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32156696"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Início rápido: criar uma máquina de virtual do Linux, utilizando a CLI do Azure na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode criar uma máquina virtual do Ubuntu Server 16.04 LTS, utilizando a CLI do Azure. Siga os passos neste artigo para criar e utilizar uma máquina virtual. Este artigo também dá-lhe os passos para:

* Ligar à máquina virtual com um cliente remoto.
* Instalar o servidor de web NGINX e ver a home page predefinida.
* Limpe os recursos não utilizados.

## <a name="prerequisites"></a>Pré-requisitos

* **Uma imagem do Linux no marketplace pilha do Azure**

   O marketplace de pilha do Azure não contém uma imagem de Linux por predefinição. Obter o operador de pilha do Azure para fornecer o **Ubuntu Server 16.04 LTS** imagem precisa. O operador pode utilizar os passos descritos no [transferir itens do marketplace do Azure para Azure pilha](../azure-stack-download-azure-marketplace-item.md) artigo.

* Pilha do Azure requer uma versão específica da CLI do Azure para criar e gerir os recursos. Se não tiver a CLI do Azure configurada para a pilha do Azure, inicie sessão no [kit de desenvolvimento](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou um baseados em Windows externo cliente se [ligado através de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) e siga os passos para [ instalar e configurar a CLI do Azure](azure-stack-version-profiles-azurecli2.md).

* Uma chave SSH pública com o id_rsa.pub nome guardado no diretório. SSH do seu perfil de utilizador do Windows. Para obter informações detalhadas sobre a criação de chaves SSH, consulte [criar SSH chaves no Windows](../../virtual-machines/linux/ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico onde pode implementar e gerir recursos de pilha do Azure. O kit de desenvolvimento ou a pilha do Azure integrado no sistema, execute o [criar grupo az](/cli/azure/group#az_group_create) comando para criar um grupo de recursos.

>[!NOTE]
 Os valores são atribuídos para todas as variáveis nos exemplos de código. No entanto, pode atribuir novos valores, se pretender.

O exemplo seguinte cria um grupo de recursos denominado myResourceGroup na localização local.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Criar uma máquina virtual utilizando o [az vm criar](/cli/azure/vm#az_vm_create) comando. O exemplo seguinte cria uma VM com o nome myVM. Este exemplo utiliza Demouser para um nome de utilizador administrativo e Demouser@123 como a palavra-passe do utilizador. Altere estes valores para algo que é adequado para o seu ambiente.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "UbuntuLTS" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

O endereço IP público é devolvido no **PublicIpAddress** parâmetro. Anote este endereço porque precisa de aceder à máquina virtual.

## <a name="open-port-80-for-web-traffic"></a>Abrir a porta 80 para o tráfego da Web

Porque esta máquina virtual vai executar o servidor web do IIS, terá de abrir a porta 80 para tráfego de Internet. Utilize o comando [az vm open-port](/cli/azure/vm#open-port) para abrir a porta pretendida.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="use-ssh-to-connect-to-the-virtual-machine"></a>Utilizar o SSH para ligar à máquina virtual

Num computador cliente com SSH instalado, ligar à máquina virtual. Se estiver a trabalhar num cliente Windows, utilize [Putty](http://www.putty.org/) para criar a ligação. Para ligar à máquina virtual, utilize o seguinte comando:

```bash
ssh <publicIpAddress>
```

## <a name="install-the-nginx-web-server"></a>Instalar o servidor de web NGINX

Para recursos do pacote de atualização e instalar o pacote NGINX mais recente, execute o seguinte script:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>Ver a página de boas-vindas do NGINX

Com NGINX instalado e a porta 80 aberto na sua máquina virtual, que pode aceder ao servidor de web utilizando o endereço IP público da máquina virtual. Abra um browser e navegue para ```http://<public IP address>```.

![Página de boas-vindas NGINX web server](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Limpar recursos

Limpe os recursos que não precisa de já. Pode utilizar o [eliminação do grupo de az](/cli/azure/group#az_group_delete) comando para remover estes recursos. Para eliminar o grupo de recursos e todos os respetivos recursos, execute o seguinte comando:

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Este guia de introdução implementada uma máquina virtual do servidor Linux básica com um servidor web. Para saber mais sobre as máquinas virtuais de pilha do Azure, avance para [considerações para máquinas virtuais no Azure pilha](azure-stack-vm-considerations.md).
