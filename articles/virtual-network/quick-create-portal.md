---
title: Criar uma rede virtual - início rápido - Portal do Azure | Microsoft Docs
description: Neste início rápido, vai aprender a criar uma rede virtual com o Portal do Azure. Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais, comuniquem em privado entre si e com a Internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 7107dc72686004141d8bea0083089cba065a9f4c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841406"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Início Rápido: criar uma rede virtual com o Portal do Azure

Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais (VM), comuniquem em privado entre si e com a Internet. Neste início rápido, vai aprender a criar uma rede virtual. Depois de criar uma rede virtual, vai implementar duas VMs na rede virtual. Em seguida, vai ligar a uma VM a partir da Internet e comunicar em privado entre as duas VMs.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

1. Selecione **+ Criar um recurso** no canto superior esquerdo do Portal do Azure.
2. Selecione **Redes** e, em seguida, selecione **Rede virtual**.
3. Introduza ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **Criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos| Selecione **Criar novo** e introduza *myResourceGroup*.|
    |Localização| Selecione **E.U.A. Leste**.|

    ![Introduza as informações básicas sobre a sua rede virtual](./media/quick-create-portal/create-virtual-network.png)

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual:

### <a name="create-the-first-vm"></a>Criar a primeira VM

1. Selecione **+ Criar um recurso**, disponível no canto superior esquerdo do Portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**.
3. Introduza ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Nome|myVm1|
    |Nome de utilizador| Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe| Introduza uma palavra-passe à sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos| Selecione **Utilizar existente** e selecione **myResourceGroup**.|
    |Localização| Selecione **E.U.A. Leste**|

    ![Informações básicas da máquina virtual](./media/quick-create-portal/virtual-machine-basics.png)

4. Escolha um tamanho para a VM e selecione **Selecionar**.
5. Em **Definições**, aceite todas as predefinições e, em seguida, selecione **OK**.

    ![Definições da máquina virtual](./media/quick-create-portal/virtual-machine-settings.png)

6. Em **Criar** no **Resumo**, selecione **Criar** para iniciar a implementação da VM. A implementação da VM demora alguns minutos. 

### <a name="create-the-second-vm"></a>Criar a segunda VM

Conclua os passos 1 a 6 novamente mas, no passo 3, dê à VM o nome *myVm2*.

## <a name="connect-to-a-vm-from-the-internet"></a>Ligar a uma VM a partir da Internet

1. Depois de a VM *myVm1* estar criada, ligue à mesma. Na parte superior do Portal do Azure, introduza *myVm1*. Quando a **myVm1** aparecer nos resultados da pesquisa, selecione-a. Selecione o botão **Ligar**.

    ![Ligar a uma máquina virtual](./media/quick-create-portal/connect-to-virtual-machine.png)

2. Depois de selecionar o botão **Ligar**, é criado e transferido um ficheiro Remote Desktop Protocol (.rdp) para o computador.  
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Ligar**. Introduza o nome de utilizador e a palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **Mais opções** e **Utilizar uma conta diferente** para especificar as credenciais que introduziu quando criou a VM. 
4. Selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **Continuar** para prosseguir com a ligação.

## <a name="communicate-between-vms"></a>Comunicar entre VMs

1. No PowerShell, introduza `ping myvm2`. O ping falha porque o ping utiliza o protocolo ICMP (Internet Control Message Protocol), e o ICMP não é permitido através da firewall do Windows, por predefinição.
2. Para permitir à *myVm2* enviar um ping à *myVm1* num passo posterior, introduza o seguinte comando do PowerShell, que permite a entrada de ICMP através da firewall do Windows:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

3. Feche a ligação de ambiente de trabalho remoto à *myVm1*. 

4. Conclua novamente os passos em [Ligar a uma VM a partir da Internet](#connect-to-a-vm-from-the-internet), mas ligue à *myVm2*. Numa linha de comandos, introduza `ping myvm1`.

    Vai receber respostas da *myVm1*porque permitiu o ICMP através da firewall do Windows na VM *myVm1* num passo anterior.

5. Feche a ligação de ambiente de trabalho remoto à *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos e todos os recursos contidos no mesmo:

1. Introduza *myResourceGroup* na caixa **Pesquisar** na parte superior do portal. Quando vir o **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* em **ESCREVER O NOME DO GRUPO DE RECURSOS:** e selecione **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou uma rede virtual predefinida e duas VMs. Ligou a uma VM a partir da Internet e comunicou em privado entre a VM e outra VM. Para saber mais sobre as definições de rede virtual, veja [Gerir uma rede virtual](manage-virtual-network.md).

Por predefinição, o Azure permite a comunicação privada sem restrições entre máquinas virtuais, mas só permite ligações de ambiente de trabalho remoto de entrada a VMs do Windows a partir da Internet. Para saber como permitir ou restringir diferentes tipos de comunicação de rede de e para as VMs, avance para o tutorial [Filtrar tráfego de rede](tutorial-filter-network-traffic.md).