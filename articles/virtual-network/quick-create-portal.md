---
title: Criar uma rede Virtual do Azure - Portal | Microsoft Docs
description: "Saiba mais rapidamente criar uma rede virtual com o portal do Azure. Uma rede virtual permite que os recursos do Azure, tais como as máquinas virtuais, em privado comunicar entre si e com a internet."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c8f2cbe6b7377772e019a4ff90f91355ba0815ae
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Criar uma rede virtual com o portal do Azure

Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais (VM), em privado comunicar entre si e com a internet. Neste artigo, irá aprender a criar uma rede virtual. Depois de criar uma rede virtual, implementar duas VMs para a rede virtual. Em seguida, ligar a uma VM a partir da internet e comunicar em privado entre as duas VMs.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Introduza, ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos| Selecione **criar nova** e introduza *myResourceGroup*.|
    |Localização| Selecione **EUA Leste**.|

    ![Introduza as informações básicas sobre a sua rede virtual](./media/quick-create-portal/create-virtual-network.png)

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual:

### <a name="create-the-first-vm"></a>Criar a primeira VM

1. Selecione **+ criar um recurso** encontrado no canto superior, à esquerda do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**.
3. Introduza, ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Nome|myVm1|
    |Nome de utilizador| Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe| Introduza uma palavra-passe da sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos| Selecione **utilizar existente** e selecione **myResourceGroup**.|
    |Localização| Selecione **EUA leste**|

    ![Noções básicas de máquina virtual](./media/quick-create-portal/virtual-machine-basics.png)

4. Selecione um tamanho para a VM e, em seguida, selecione **selecione**.
5. Em **definições**, aceite todas as predefinições e, em seguida, selecione **OK**.

    ![Definições da máquina virtual](./media/quick-create-portal/virtual-machine-settings.png)

6. Em **criar** do **resumo**, selecione **criar** para iniciar a implementação de VM. A VM demora alguns minutos a implementar. 

### <a name="create-the-second-vm"></a>Criar a VM segundo

Concluir os passos 1-6 novamente, mas no passo 3, nome da VM *myVm2*.

## <a name="connect-to-a-vm-from-the-internet"></a>Ligar a uma VM a partir da internet

1. Depois de *myVm1* é criado, ligar ao mesmo. Na parte superior do portal do Azure, introduza *myVm1*. Quando **myVm1** aparece nos resultados da pesquisa, selecionados-lo. Selecione o **Connect** botão.

    ![Ligar a uma máquina virtual](./media/quick-create-portal/connect-to-virtual-machine.png)

2. Depois de selecionar o **Connect** botão, um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado e será transferido para o seu computador.  
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Connect**. Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM. 
4. Selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **continuar**, prossiga com a ligação.

## <a name="communicate-privately-between-vms"></a>Comunicar modo privado entre VMs

1. A partir do PowerShell, introduza `ping myvm2`. Ping falhar, porque o ping utiliza o protocolo de mensagem de controlo de internet (ICMP) e ICMP não é permitido através da firewall do Windows, por predefinição.
2. Para permitir *myVm2* para enviar um ping *myVm1* num passo posterior, introduza o seguinte comando do PowerShell, que permite ICMP entrada através da firewall do Windows:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

3. Fechar a ligação de ambiente de trabalho remota para *myVm1*. 

4. Execute os passos do [ligar a uma VM a partir da internet](#connect-to-a-vm-from-the-internet) novamente, mas a ligação à *myVm2*. Numa linha de comandos, introduza `ping myvm1`.

    Receber respostas do *myVm1*porque permitido ICMP através da firewall do Windows no *myVm1* VM num passo anterior.

5. Fechar a ligação de ambiente de trabalho remota para *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém:

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma rede virtual predefinido e duas VMs. Ligado a uma VM a partir da Internet e comunidades de forma clara modo privado entre a VM e outra VM. Para saber mais sobre as definições de rede virtual, consulte [gerir uma rede virtual](manage-virtual-network.md).

Por predefinição, o Azure permite a comunicação privada sem restrições entre máquinas virtuais, mas só permite ligações de ambiente de trabalho remotas entrada para VMs do Windows através da Internet. Para saber como permitir ou restringir a diferentes tipos de comunicação de rede de e para as VMs, avançar para o próximo tutorial.

> [!div class="nextstepaction"]
> [Filtrar o tráfego de rede](virtual-networks-create-nsg-arm-pportal.md)