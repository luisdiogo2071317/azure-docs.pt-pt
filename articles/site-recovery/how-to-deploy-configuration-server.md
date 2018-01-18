---
title: " Implementar o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como implementar um servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: e257ede08ac46ad863b4883b10399058e6f59f1f
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="deploy-a-configuration-server"></a>Implementar um servidor de configuração

Implementar um servidor de configuração no local ao utilizar o [do Azure Site Recovery](site-recovery-overview.md) serviço recuperação de desastres de VMs de VMware e servidores físicos para o Azure. Servidor de configuração WThe coordena as comunicações entre no local VMware e o Azure e gere a replicação de dados. Este artigo explica os passos necessários para implementar o servidor de configuração.

## <a name="prerequisites"></a>Pré-requisitos

Recomendamos que implemente o servidor de configuração como uma VM de VMware altamente disponível. Para a replicação do servidor físico, pode configurar o servidor de configuração no computador físico. Requisitos mínimos de hardware estão resumidos na tabela seguinte.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Planeamento de capacidade

Os requisitos de dimensionamento para o servidor de configuração dependem a taxa de alteração de dados possíveis. Utilize esta tabela como guia.

| **CPU** | **Memória** | **Tamanho da cache do disco** | **Taxa de alteração de dados** | **Máquinas protegidas** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 sockets * 4 núcleos @ 2,5 GHz) |16 GB |300 GB |500 GB ou inferior |Replicar menos de 100 máquinas. |
| 12 vCPUs (2 sockets * 6 núcleos @ 2,5 GHz) |18 GB |600 GB |500 GB a 1 TB |Replicar entre 100 150 máquinas. |
| 16 vCPUs (2 sockets * 8 núcleos @ 2,5 GHz) |32 GB |1 TB |1 TB para 2 TB |Replicar entre 150 200 máquinas. |


Se estiver a replicar VMs de VMware, leia mais sobre [considerações de planeamento de capacidade](/site-recovery-plan-capacity-vmware.md)e execute o [ferramenta de implementação do planner](site-recovery-deployment-planner.md) para replicação de VMWare.



## <a name="download-the-template"></a>Transferir o modelo

Recuperação de sites fornece um modelo transferível para configurar o servidor de configuração como uma VM de VMware altamente disponível. 

1. O cofre, aceda a **preparar infraestrutura** > **origem**.
2. No **preparar a origem**, clique em **+ o servidor de configuração**.
3. No **Adicionar servidor**, verifique se **servidor de configuração para VMware** aparece no **tipo de servidor**.
4. Transferir o modelo de abrir virtualização formato OVF () para o servidor de configuração.

  > [!TIP]
  Pode ser transferida a versão mais recente do modelo de servidor de configuração diretamente a partir do [Microsoft Download Center](https://aka.ms/asrconfigurationserver)


## <a name="import-the-template-in-vmware"></a>Importar o modelo do VMware


1. Inicie sessão no VMware vCenter server ou vSphere ESXi anfitrião, utilizando o VMWare vSphere Client.
2. No **ficheiro** menu, selecione **implementar o modelo de OVF**, para iniciar o Assistente de implementar o modelo de OVF.  

     ![Modelo OVF](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. No **selecionar origem**, especifique a localização do OVF transferido.
4. No **rever detalhes**, clique em **seguinte**.
5. No **Selecionar nome e a pasta**, e **selecione configuração**, aceite as predefinições.
6. No **selecionar armazenamento**, para selecionar o melhor desempenho **espesso aprovisionar Eager Zeroed** no **formato de disco virtual selecione**.
4. O resto das páginas do assistente, aceite as predefinições.
5. No **preparado para concluir**:
  - Para configurar a VM com as predefinições, selecione **ligar após a implementação** > **concluir**.
  - Se pretender adicionar uma interface de rede adicionais, desmarque **ligar após a implementação**e, em seguida, selecione **concluir**. Por predefinição, o modelo de servidor de configuração é implementado com um único NIC, mas pode adicionar NICs adicionais após a implementação.


## <a name="add-an-additional-adapter"></a>Adicionar um adaptador adicional

Se pretender adicionar uma NIC adicional para o servidor de configuração, fazê-lo antes de registar o servidor no cofre. Adicionar adaptadores adicionais não é suportada após o registo.

1. No inventário de cliente vSphere, a VM com o botão direito e selecione **editar definições de**.
2. No **Hardware**, clique em **adicionar** > **adaptador Ethernet**. Clique depois em **Seguinte**.
3. Selecione e o tipo de adaptador e uma rede. 
4. Para ligar o NIC virtual quando a VM estiver ativada, selecione **ligar ao power no**. Clique em **seguinte** > **concluir**e, em seguida, clique em **OK**.
 

## <a name="register-the-configuration-server"></a>Registar o servidor de configuração 

1. VMWare vSphere cliente na consola do, ative a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceitar o contrato de licença e especificar uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão para a VM como o administrador.
4. Na primeira vez que iniciar sessão, inicia a ferramenta de configuração do Azure Site Recovery.
5. Especifique um nome que é utilizado para registar o servidor de configuração com a recuperação de Site. Clique depois em **Seguinte**.
6. A ferramenta verifica se a VM pode ligar para o Azure. Depois da ligação for estabelecida, clique em **sessão**, para iniciar sessão na sua subscrição do Azure. As credenciais tem de ter acesso ao Cofre no qual pretende registar o servidor de configuração.
7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciado.
8. Inicie sessão na máquina novamente. O Assistente de gestão do servidor de configuração irá iniciar automaticamente.

### <a name="configure-settings"></a>Configurar definições

1. No Assistente de gestão do servidor de configuração > **configuração conectividade**, selecione o NIC que irão receber o tráfego de replicação. Em seguida, clique em **Guardar**. Não é possível alterar esta definição depois de estar configurada.
2. No **selecionar serviços de recuperação cofre**, selecione a sua subscrição do Azure e o grupo de recursos relevantes e do cofre.
3. No **instalar software de terceiros**, aceite o agreeemtn de licença e clique em **transferir e instalar**, para instalar o servidor de MySQL.
4. Clique em **instalar VMware PowerLCI**. Certifique-se de que todas as janelas do browser estão fechadas antes de efetuar este procedimento. Em seguida, clique em **continuar**
5. No **validar configuração de aplicação**, serão possível verificar os pré-requisitos antes de continuar.
6. No **configurar servidor de ESXi vSphere/servidor vCenter**, especifique o FQDN ou endereço IP do servidor vCenter ou anfitriões vSphere, no qual as VMs que pretende replicar estão localizados. Especifique a porta em que o servidor está a escutar e um nome amigável para ser utilizado para o servidor do VMware no cofre.
7. Especifique as credenciais que serão utilizadas pelo servidor de configuração para ligar ao servidor do VMware. Recuperação de site utiliza estas credenciais para detetar automaticamente as VMs de VMware que estão disponíveis para replicação. Clique em **adicionar**e, em seguida, clique em **continuar**.
8. No **configurar as credenciais de máquina virtual**, especifique o nome de utilizador e palavra-passe que será utilizado para instalar automaticamente o serviço de mobilidade em máquinas, quando a replicação está ativada. Para máquinas do Windows, a conta tem privilégios de administrador local nos computadores que pretende replicar. Para Linux fornecem detalhes para a conta raiz.
9. Clique em **Finalize configuração** para concluir o registo. 
10. Após a conclusão de registo, no portal do Azure, certifique-se de que o servidor de configuração e o servidor do VMware estão listadas no **origem** página no cofre. Em seguida, clique em **OK** para configurar as definições de destino.


## <a name="troubleshoot-deployment-issues"></a>Resolver problemas de implementação

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Passos Seguintes

Reveja os tutoriais para configurar a recuperação após desastre de [VMs de VMware](tutorial-vmware-to-azure.md) e [servidores físicos](tutorial-physical-to-azure.md) para o Azure.
