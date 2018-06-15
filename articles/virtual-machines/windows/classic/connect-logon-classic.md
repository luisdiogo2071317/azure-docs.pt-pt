---
title: Inicie sessão numa VM do Azure clássico | Microsoft Docs
description: Utilize o portal do Azure para iniciar sessão na máquina virtual do Windows criada com o modelo de implementação clássica.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 87ecc65d2d4802ae826f3260b66b26e0bbe414e6
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012364"
---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a>Iniciar sessão numa máquina virtual do Windows através do portal do Azure
No portal do Azure, utilize o **Connect** botão para iniciar uma sessão de ambiente de trabalho remoto e inicie sessão numa VM do Windows.

Pretende ligar a uma VM com Linux? Consulte [como iniciar sessão para uma máquina virtual com Linux](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássico](../../../resource-manager-deployment-model.md). Este artigo abrange utilizando o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Para obter informações sobre como iniciar sessão a uma VM com o modelo do Resource Manager, consulte [aqui](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="connect-to-the-virtual-machine"></a>Ligar à máquina virtual
1. Inicie sessão no Portal do Azure.
2. Clique na máquina virtual que pretende aceder. O nome está listado no **todos os recursos** painel.

    ![Localizações de máquina virtual](./media/connect-logon/azureportaldashboard.png)

1. Clique em de **Connect** botão na página de propriedades da máquina virtual. 
2. No **ligar à máquina virtual** página, mantenha Selecione as opções adequadas e clique em **ficheiro RDP transferir**.
2. Abra o ficheiro RDP transferido e clique em **Connect** quando lhe for pedido. 
2. Recebe um aviso a informar que o ficheiro `.rdp` é de um publicador desconhecido. É uma situação normal. Na janela Ambiente de Trabalho Remoto, clique em **Ligar** para continuar.
   
    ![Captura de ecrã de um aviso sobre um publicador desconhecido.](./media/connect-logon/rdp-warn.png)
3. Na janela **Segurança do Windows**, selecione **Mais escolhas** e **Utilizar uma conta diferente**. Escreva as credenciais para uma conta da máquina virtual e clique em **OK**.
   
     **Conta local** – trata-se normalmente do nome de utilizador e da palavra-passe da conta local que especificou quando criou a máquina virtual. Neste caso, o domínio é o nome da máquina virtual e é introduzido como *nomedavm*&#92;*nomedeutilizador*.  
   
    **VM associada ao domínio** – se a VM pertence a um domínio, introduza o nome de utilizador no formato *Domínio*&#92;*Nomedeutilizador*. A conta também tem de estar no grupo Administradores ou terem sido concedidos privilégios de acesso remoto à VM.
   
    **Controlador de domínio** – se a VM for um controlador de domínio, escreva o nome de utilizador e a palavra-passe de uma conta de administrador para esse domínio.
4. Clique em **Sim** para confirmar a identidade da máquina virtual e concluir o início de sessão.
   
   ![Captura de ecrã que mostra uma mensagem sobre a confirmação da identidade da VM.](./media/connect-logon/cert-warning.png)

## <a name="next-steps"></a>Passos Seguintes
* Se o **Connect** botão está inativo ou tenha outros problemas com a ligação de ambiente de trabalho remoto, tente a repor a configuração. Clique em **reponha o acesso remoto** partir do dashboard de máquina virtual.

    ![Reposição-remota-acesso](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Para problemas com a palavra-passe, tente repor. Clique em **Repor palavra-passe** ao longo do limite esquerdo do virtual machine dashboard, em **suporte + resolução de problemas**.

    ![Reposição de palavra-passe](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Se estas sugestões não funcionam ou não são o que precisa de, consulte [ligações de resolução de problemas de ambiente de trabalho remoto para uma baseado no Windows Azure Máquina Virtual](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Este artigo orienta-o ao longo do diagnóstico e da resolução de problemas comuns.
