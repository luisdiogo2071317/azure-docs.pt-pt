---
title: Ligar a uma VM do Windows Server | Microsoft Docs
description: Saiba como ligar e iniciar sessão numa VM do Windows utilizando o Portal do Azure e o modelo de implementação Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: cynthn
ms.openlocfilehash: 7c495a74ccbcad3ee18147726742ee59b65f1c0e
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Como ligar e iniciar sessão numa máquina virtual do Azure a executar o Windows
Irá utilizar o botão **Ligar** no Portal do Azure para iniciar uma sessão de Ambiente de Trabalho Remoto (RDP) a partir de um ambiente de trabalho do Windows. Ligue primeiro à máquina virtual e, em seguida, inicie sessão.

Se estiver a tentar ligar a uma VM do Windows a partir de um Mac, terá de instalar um cliente RDP para Mac, como [o protocolo RDP (Remote Desktop Protocol) da Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Ligar à máquina virtual
1. Se ainda não o fez, inicie sessão no [Portal do Azure](https://portal.azure.com/).
2. No menu do lado esquerdo, clique em **Máquinas Virtuais**.
3. Selecione a máquina virtual na lista.
4. No topo da página para a máquina virtual, clique em do ![Imagem do botão para ligar.](./media/connect-logon/connect.png) Editar...
2. No **ligar à máquina virtual** página, selecione as opções adequadas e clique em **ficheiro RDP transferir**.
2. Abra o ficheiro RDP transferido e clique em **Connect** quando lhe for pedido. 
2. Recebe um aviso a informar que o ficheiro `.rdp` é de um publicador desconhecido. É uma situação normal. Na janela Ambiente de Trabalho Remoto, clique em **Ligar** para continuar.
   
    ![Captura de ecrã de um aviso sobre um publicador desconhecido.](./media/connect-logon/rdp-warn.png)
3. Na janela **Segurança do Windows**, selecione **Mais escolhas** e **Utilizar uma conta diferente**. Escreva as credenciais para uma conta da máquina virtual e clique em **OK**.
   
     **Conta local** – trata-se normalmente do nome de utilizador e da palavra-passe da conta local que especificou quando criou a máquina virtual. Neste caso, o domínio é o nome da máquina virtual e é introduzido como *nomedavm*&#92;*nomedeutilizador*.  
   
    **VM associada ao domínio** – se a VM pertence a um domínio, introduza o nome de utilizador no formato *Domínio*&#92;*Nomedeutilizador*. A conta também tem de estar no grupo Administradores ou terem sido concedidos privilégios de acesso remoto à VM.
   
    **Controlador de domínio** – se a VM for um controlador de domínio, escreva o nome de utilizador e a palavra-passe de uma conta de administrador para esse domínio.
4. Clique em **Sim** para confirmar a identidade da máquina virtual e concluir o início de sessão.
   
   ![Captura de ecrã que mostra uma mensagem sobre a confirmação da identidade da VM.](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Se o botão **Ligar** no portal estiver a cinzento e não estiver ligado ao Azure através de uma ligação [Express Route](../../expressroute/expressroute-introduction.md) ou [Rede de VPNs](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), terá de criar e atribuir um endereço IP público à VM antes de poder utilizar o RDP. Pode ler mais sobre [endereços IP públicos no Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 


## <a name="next-steps"></a>Passos Seguintes
Caso se depare com problemas ao tentar ligar, veja [Troubleshoot Remote Desktop connections (Resolução de Problemas de Ligações ao Ambiente de Trabalho Remoto)](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Este artigo orienta-o ao longo do diagnóstico e da resolução de problemas comuns.

