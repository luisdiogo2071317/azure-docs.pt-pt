---
title: Criar uma rede virtual no Azure - Portal | Microsoft Docs
description: "Saiba mais rapidamente criar uma rede virtual com o portal do Azure. Uma rede virtual permite que vários tipos de recursos do Azure em privado comuniquem entre si."
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
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 8b6a4abdb7677417462392feade0c7cfdf99246f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Criar uma rede virtual com o portal do Azure

Neste artigo, irá aprender a criar uma rede virtual. Depois de criar uma rede virtual, implementar duas máquinas virtuais numa rede virtual e comunicar em privado entre eles e com a internet.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Introduza, ou selecione as seguintes informações e, em seguida, selecione **criar**:
    - **Name**: *myVirtualNetwork*
    - **Espaço de endereços**: aceite a predefinição. Foi especificado o espaço de endereços em notação CIDR.
    - **Subscrição**: selecione a sua subscrição.
    - **Grupo de recursos**: selecione **criar nova** e introduza *myResourceGroup*.
    - **Localização**: selecionar * e.u. a leste * *.
    - **Sub-rede, nome**: aceite a predefinição.
    - **Sub-rede, o intervalo de endereços**: aceite a predefinição.
    - **Pontos finais de serviço**: aceite a predefinição.

    ![Introduza as informações básicas sobre a sua rede virtual](./media/quick-create-portal/virtual-network.png)

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Uma rede virtual permite que vários tipos de recursos do Azure em privado comuniquem entre si e com a internet. Um tipo de recurso que pode implementar numa rede virtual é uma máquina virtual.

1. Selecione **+ criar um recurso** encontrado no canto superior, à esquerda do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**.
3. Introduza, ou selecione as seguintes informações e, em seguida, selecione **OK**:
    - **Name**: *myVm1*
    - **Nome de utilizador**: introduza um nome de utilizador à sua escolha.
    - **Palavra-passe**: introduza uma palavra-passe da sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).
    - **Subscrição**: selecione a sua subscrição.
    - **Grupo de recursos**: selecione **utilizar existente** e selecione **myResourceGroup**.
    - **Localização**: selecione *EUA Leste*.

    ![Introduza as informações básicas sobre uma máquina virtual](./media/quick-create-portal/virtual-machine-basics.png)
4. Selecione um tamanho da máquina virtual e, em seguida, selecione **selecione**.
5. Em **definições**, *myVirtualNetwork* já deve estar selecionada para **rede Virtual**, mas se não for, selecione **rede Virtual** , em seguida, selecione *myVirtualNetwork*. Deixe *predefinido* selecionado para **sub-rede**e, em seguida, selecione **OK**.

    ![Selecione uma rede virtual](./media/quick-create-portal/virtual-machine-network-settings.png)
6. No **resumo** página, selecione **criar** para iniciar a implementação da máquina virtual. A máquina virtual demora alguns minutos a implementar. 
7. Concluir os passos 1-6 novamente, mas no passo 3, nome da máquina virtual *myVm2*.

## <a name="connect-to-a-virtual-machine"></a>Ligar a uma máquina virtual

1. Depois de *myVm1* é criado, remotamente ligar ao mesmo. Na parte superior do portal do Azure, introduza *myVm1*. Quando **myVm1** aparece nos resultados da pesquisa, selecionados-lo. Selecione o **Connect** botão.

    ![Descrição geral de máquina virtual](./media/quick-create-portal/virtual-machine-overview.png)

2. Depois de selecionar o **Connect** botão, um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado e será transferido para o seu computador.  
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Connect**. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais introduzidas quando a criar a máquina virtual), em seguida, selecione OK. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** ou **continuar** para continuar com a ligação.

## <a name="communicate-between-vms"></a>Comunicar entre VMs

1. Numa linha de comandos, introduza `ping myvm2`. Ping falhar porque utiliza de ping ICMP e ICMP não é permitido através do Windows firewall, por predefinição. Para permitir *myVm2* para enviar um ping *myVm1* num passo posterior, introduza o seguinte comando numa linha de comandos:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

2. Fechar a ligação de ambiente de trabalho remota para *myVm1*. 

3. Execute os passos do [ligar a uma máquina virtual](#connect-to-a-virtual-machine), mas a ligação à *myVm2*. Numa linha de comandos, introduza `ping myvm1`.

    É capaz de executar o ping com êxito o *myVm1* máquina virtual a partir de *myVm2* máquina virtual porque:

    - permitido ICMP através da firewall do Windows no *myVm1* máquina virtual no passo anterior.
    - Por predefinição, o Azure permite que todo o tráfego de rede entre os recursos na mesma rede virtual.

## <a name="communicate-to-the-internet"></a>Comunicar com a internet

1. Enquanto ainda ligada para o *myVm2* máquina virtual, numa linha de comandos, introduza `ping bing.com`.

    Receber quatro respostas bing.com. 

    Conseguir ping com êxito a um recurso de internet do *myVm2* máquina virtual, porque qualquer máquina virtual pode comunicar saída à internet, por predefinição.

2. Sair da sessão de ambiente de trabalho remota.

## <a name="communicate-from-the-internet"></a>Comunicar através da internet

1. Obter o endereço IP público do *myVm1* máquina virtual. Na imagem mostrada no passo 1 na [ligar a uma máquina virtual](#connect-to-a-virtual-machine), verá um endereço IP público. Na imagem, o endereço é *13.90.241.247*. O endereço para a máquina virtual é diferente. 

2. Do seu computador, ping o endereço IP público do seu *myVm1* máquina virtual. Ping falhar, apesar de ICMP está aberta através da firewall do Windows.

    Ping falhar porque todo o tráfego para máquinas virtuais do Windows, exceto as ligações de ambiente de trabalho remotas através da porta 3389, é negado pelo Azure, por predefinição. 

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém:

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, implementou uma predefinição a rede virtual com uma sub-rede. Para saber como criar uma rede virtual personalizada com várias sub-redes, avance para o tutorial para criar uma rede virtual personalizada.

> [!div class="nextstepaction"]
> [Criar uma rede virtual personalizada](virtual-networks-create-vnet-arm-pportal.md)
