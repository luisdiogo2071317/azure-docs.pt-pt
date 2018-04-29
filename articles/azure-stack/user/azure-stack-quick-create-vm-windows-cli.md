---
title: Criar uma máquina virtual do Windows na pilha do Azure utilizando a CLI do Azure | Microsoft Docs
description: Saiba como criar uma VM do Windows na pilha do Azure utilizando a CLI do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: E26B246E-811D-44C9-9BA6-2B3CE5B62E83
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/23/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 381c1c37b0675d97adc058979a5d9b5c4fd2cc8b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-windows-server-virtual-machine-by-using-azure-cli-in-azure-stack"></a>Início rápido: criar uma máquina virtual do Windows, utilizando a CLI do Azure na pilha do Azure

‎*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode criar uma máquina virtual do Windows Server 2016 com a CLI do Azure. Siga os passos neste artigo para criar e utilizar uma máquina virtual. Este artigo também dá-lhe os passos seguintes:

* Ligar à máquina virtual com um cliente remoto.
* Instalar o servidor de web IIS e ver a home page predefinida.
* Limpe os seus recursos.

## <a name="prerequisites"></a>Pré-requisitos

* Certifique-se de que o operador de pilha do Azure adicionadas a **Windows Server 2016** imagens no Marketplace de pilha do Azure.

* Pilha do Azure requer uma versão específica do CLI do Azure para criar e gerir os recursos. Se não tiver a CLI do Azure configurada para a pilha do Azure, siga os passos para [instalar e configurar a CLI do Azure](azure-stack-version-profiles-azurecli2.md).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico onde pode implementar e gerir recursos de pilha do Azure. A partir do seu ambiente de pilha do Azure, execute o [criar grupo az](/cli/azure/group#az_group_create) comando para criar um grupo de recursos.

>[!NOTE]
 Os valores são atribuídos para todas as variáveis nos exemplos de código. No entanto, pode atribuir novos valores, se pretender.

O exemplo seguinte cria um grupo de recursos denominado myResourceGroup na localização local.

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Criar uma máquina virtual (VM), utilizando o [az vm criar](/cli/azure/vm#az_vm_create) comando. O exemplo seguinte cria uma VM com o nome myVM. Este exemplo utiliza Demouser para um nome de utilizador administrativo e Demouser@123 como a palavra-passe do utilizador. Altere estes valores para algo que é adequado para o seu ambiente.

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --use-unmanaged-disk \
  --location local
```

Quando é criada a VM, o **PublicIPAddress** parâmetro na saída contém o endereço IP público para a máquina virtual. Anote este endereço porque precisa de aceder à máquina virtual.

## <a name="open-port-80-for-web-traffic"></a>Abrir a porta 80 para o tráfego da Web

Porque esta VM vai executar o servidor web do IIS, terá de abrir a porta 80 para tráfego de Internet.

Utilize o [az vm open-porta](/cli/azure/vm#open-port) comando para abrir a porta 80.

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>Ligar à máquina virtual

Utilize o comando seguinte para criar uma ligação de ambiente de trabalho remoto para a máquina virtual. Substitua "Endereço IP público" com o endereço IP da sua máquina virtual. Quando lhe for pedido, introduza o nome de utilizador e palavra-passe que utilizou para a máquina virtual.

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>Instalar o IIS com o PowerShell

Agora que já iniciou sessão na máquina virtual, pode utilizar o PowerShell para instalar o IIS. Inicie o PowerShell na máquina virtual e execute o seguinte comando:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>Ver a página de boas-vindas do IIS

Pode utilizar um browser web à sua escolha para ver a página de boas-vindas do IIS predefinido. Utilize o endereço IP público documentado na secção anterior para visitar a página predefinida.

![Site predefinido do IIS](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>Limpar recursos

Limpe os recursos que não precisa de já. Utilize o [eliminação do grupo de az](/cli/azure/group#az_group_delete) comando para remover o grupo de recursos, a máquina virtual, e todos os recursos relacionados.

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Este guia de introdução implementada uma máquina de virtual básica do Windows Server. Para saber mais sobre as máquinas virtuais de pilha do Azure, avance para [considerações para máquinas virtuais no Azure pilha](azure-stack-vm-considerations.md).
