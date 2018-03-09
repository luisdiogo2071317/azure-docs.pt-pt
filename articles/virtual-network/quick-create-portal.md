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
ms.openlocfilehash: 3c040f677aa25656148081d533e87cc55f1e22e7
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Criar uma rede virtual com o portal do Azure

Neste artigo, irá aprender a criar uma rede virtual. Depois de criar uma rede virtual, implementar duas máquinas virtuais para a rede virtual para testar a comunicação de rede privada entre eles.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Como é mostrado na imagem seguinte, introduza *myVirtualNetwork* para **nome**, *myResourceGroup* para **grupo de recursos**, selecione um  **Localização** e os seus **subscrição**, aceite as restantes predefinições e, em seguida, selecione **criar**. 

    ![Introduza as informações básicas sobre a sua rede virtual](./media/quick-create-portal/virtual-network.png)

    O **espaço de endereços** é especificada em notação CIDR. O endereço espaço 10.0.0.0/16 abrange 10.0.0.0-10.0.255.254. Uma rede virtual contém zero ou mais sub-redes. A sub-rede especificada **intervalo de endereços** inclui 10.0.0.0-10.0.0.255 de endereços IP. 10.0.0.4-10.0.0.254 só estão disponíveis no entanto, porque o Azure reserva-se os endereços de quatro primeiras (0-3) e o último endereço em cada sub-rede. Os endereços IP disponíveis são atribuídos aos recursos implementados dentro de uma rede virtual.

## <a name="test-network-communication"></a>Comunicação de rede de teste

Uma rede virtual permite que vários tipos de recursos do Azure em privado comuniquem entre si. Um tipo de recurso que pode implementar numa rede virtual é uma máquina virtual. Crie duas máquinas virtuais na rede virtual para que possa validar privada comunicação entre os mesmos num passo posterior.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. Selecione **+ criar um recurso** encontrado no canto superior, à esquerda do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**.
3. Introduza as informações da máquina virtual mostradas na imagem que se segue. O **nome de utilizador** e **palavra-passe** introduzir são utilizadas para iniciar sessão máquina virtual num passo posterior. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). Selecione o **subscrição**, optar por utilizar o existente *myResourceGroup* recursos de grupo e certifique-se de que o **localização** selecionada é a mesma localização que criou o rede virtual no. Quando terminar, selecione **OK**.

    ![Introduza as informações básicas sobre uma máquina virtual](./media/quick-create-portal/virtual-machine-basics.png)
4. Selecione um tamanho da máquina virtual e, em seguida, selecione **selecione**. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Os tamanhos que são apresentados para podem ser diferentes do exemplo seguinte: 

    ![Selecione um tamanho de uma máquina virtual](./media/quick-create-portal/virtual-machine-size.png)
5. Em **definições**, *myVirtualNetwork* já deve estar selecionada para **rede Virtual**, mas se não for, selecione **rede Virtual** , em seguida, selecione *myVirtualNetwork*. Deixe *predefinido* selecionado para **sub-rede**e, em seguida, selecione **OK**.

    ![Selecione uma rede virtual](./media/quick-create-portal/virtual-machine-network-settings.png)
6. No **resumo** página, selecione **criar** para iniciar a implementação da máquina virtual. 
7. A máquina virtual demora alguns minutos a criar. Após a criação, a máquina virtual está afixada ao dashboard do portal do Azure e o resumo de máquina virtual abre automaticamente. Selecione **redes**.

    ![Informações de rede de máquina virtual](./media/quick-create-portal/virtual-machine-networking.png)

    Verá que o **IP privado** endereço é *10.0.0.4*. No passo 5, em **definições**, selecionando o *myVirtualNetwork* rede virtual e aceite a sub-rede com o nome *predefinido* para **sub-rede**. Quando lhe [criada a rede virtual](#create-a-virtual-network), aceite o valor predefinido de 10.0.0.0/24 para a sub-rede **intervalo de endereços**. Servidor DHCP do Azure atribui o primeiro endereço disponível para a sub-rede que selecionar para a máquina virtual. Uma vez que o Azure reserva de quatro primeiras endereços (0-3) de cada sub-rede, 10.0.0.4 é o primeiro endereço IP disponível disponível para a sub-rede.

    O **IP público** endereço atribuído é diferente do endereço atribuído à máquina virtual. Azure atribui um endereço público, para o Internet encaminhável IP para cada máquina virtual, por predefinição. O endereço IP público é atribuído à máquina virtual de um [conjunto de endereços atribuídos a cada região do Azure](https://www.microsoft.com/download/details.aspx?id=41653). Enquanto o Azure sabe qual o endereço IP público é atribuído a uma máquina virtual, o sistema operativo executado numa máquina virtual tem não conhecimento de qualquer endereço IP público atribuído.
8. Concluir os passos 1 a 7 novamente, mas no passo 3, nome da máquina virtual *myVm2*. 
9. Depois da máquina virtual é criada, selecione **redes**, uma vez que lhe no passo 7. Pode ver o **IP privado** endereço é *10.0.0.5*. Uma vez que o Azure anteriormente atribuído o primeiro endereço utilizável de *10.0.0.4* da sub-rede para o *myVm1* máquina virtual, que lhe atribuída *10.0.0.5* para o  *myVm2* máquina virtual, porque foi o seguinte endereço disponível na sub-rede.

### <a name="connect-to-a-virtual-machine"></a>Ligar a uma máquina virtual

1. Ligar de forma remota a *myVm1* máquina virtual. Na parte superior do portal do Azure, introduza *myVm1*. Quando **myVm1** aparece nos resultados da pesquisa, selecionados-lo. Selecione o **Connect** botão.

    ![Descrição geral de máquina virtual](./media/quick-create-portal/virtual-machine-overview.png)
2. Depois de selecionar o **Connect** botão, um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado e será transferido para o seu computador.  
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Connect**. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual e, em seguida, selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** ou **continuar** para continuar com a ligação.

### <a name="validate-communication"></a>Validar a comunicação

A tentar enviar um ping uma falha de máquina virtual do Windows, porque não é permitido ping através da firewall do Windows, por predefinição. Para permitir que um ping para *myVm1*, introduza o seguinte comando numa linha de comandos:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Para validar a comunicação com *myVm2*, introduza o seguinte comando numa linha de comandos *myVm1* máquina virtual. Forneça as credenciais que utilizou quando criou a máquina virtual e, em seguida, conclua a ligação:

```
mstsc /v:myVm2
```

A ligação de ambiente de trabalho remota é efetuada com êxito porque as duas máquinas virtuais têm endereços IP privados atribuídos a partir do *predefinido* sub-rede e porque o ambiente de trabalho remoto está aberta através da firewall do Windows, por predefinição. É possível ligar ao *myVm2* pelo nome de anfitrião porque o Azure oferece automaticamente resolução do nome DNS de todos os anfitriões numa rede virtual. Numa linha de comandos, executar o ping *myVm1*, de *myVm2*.

```
ping myvm1
```

Ping for bem sucedido porque permitido através da firewall do Windows no *myVm1* máquina virtual num passo anterior. Para confirmar a comunicação de saída à Internet, introduza o seguinte comando:

```
ping bing.com
```

Receber quatro respostas bing.com. Por predefinição, qualquer máquina virtual numa rede virtual podem comunicar saída à Internet. 

Sair da sessão de ambiente de trabalho remota.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém:

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, implementou uma predefinição a rede virtual com uma sub-rede. Para saber como criar uma rede virtual personalizada com várias sub-redes, avance para o tutorial para criar uma rede virtual personalizada.

> [!div class="nextstepaction"]
> [Criar uma rede virtual personalizada](virtual-networks-create-vnet-arm-pportal.md)
