---
title: "Implementar o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como implementar um servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: 3b09c11d76d5c88b904afa3c6d256bc475992339
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="deploy-a-configuration-server"></a>Implementar um servidor de configuração

Implementar um servidor de configuração no local ao utilizar [do Azure Site Recovery](site-recovery-overview.md) para recuperação após desastre de VMs de VMware e servidores físicos para o Azure. As comunicações de coordenadas do servidor de configuração no local VMware e o Azure. Também gere a replicação de dados. Este artigo explica os passos necessários para implementar o servidor de configuração.

## <a name="prerequisites"></a>Pré-requisitos

Recomendamos que implemente o servidor de configuração como uma VM de VMware altamente disponível. Para a replicação do servidor físico, pode configurar o servidor de configuração no computador físico. Requisitos mínimos de hardware estão resumidos na tabela seguinte.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Planeamento de capacidade

Os requisitos de dimensionamento para o servidor de configuração dependem a taxa de alteração de dados possíveis. Utilize esta tabela como guia.

| **CPU** | **Memória** | **Tamanho da cache do disco** | **Taxa de alteração de dados** | **Máquinas protegidas** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 sockets * 4 núcleos @ 2,5 GHz) |16 GB |300 GB |500 GB ou inferior |Replicar menos de 100 máquinas. |
| 12 vCPUs (2 sockets * 6 núcleos @ 2,5 GHz) |18 GB |600 GB |500 GB a 1 TB |Replicar 100 150 máquinas. |
| 16 vCPUs (2 sockets * 8 núcleos @ 2,5 GHz) |32 GB |1 TB |1 TB para 2 TB |Replicar máquinas 150 200. |


Se estiver a replicar VMs de VMware, leia mais sobre [considerações de planeamento de capacidade](/site-recovery-plan-capacity-vmware.md). Execute o [ferramenta de implementação do planner](site-recovery-deployment-planner.md) para replicação de VMWare.



## <a name="download-the-template"></a>Transferir o modelo

Recuperação de sites fornece um modelo transferível para configurar o servidor de configuração como uma VM de VMware altamente disponível. 

1. No cofre, vá para **Preparar Infraestrutura** > **Origem**.
2. No **preparar a origem**, selecione **+ o servidor de configuração**.
3. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
4. Transfira o modelo OVF (Open Virtualization Format) para o servidor de configuração.

  > [!TIP]
  Pode transferir a versão mais recente do modelo de servidor de configuração diretamente a partir de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-template-in-vmware"></a>Importar o modelo no VMware


1. Iniciar sessão para o servidor VMware vCenter ou vSphere ESXi utilizando o VMWare vSphere Client.
2. No **ficheiro** menu, selecione **implementar o modelo de OVF** para iniciar o Assistente de implementar o modelo de OVF.

     ![Modelo OVF](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. No **selecionar origem**, introduza a localização do OVF transferido.
4. No **rever detalhes**, selecione **seguinte**.
5. No **Selecionar nome e a pasta** e **selecione configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Aprovisionamento Intenso Diligente Posto a Zero** em **Selecionar formato de disco virtual**.
4. No resto das páginas do assistente, aceite as predefinições.
5. Em **Pronto para concluir**:

    * Para configurar a VM com as predefinições, selecione **Ligar após a implementação** > **Concluir**.

    * Para adicionar uma interface de rede adicionais, desmarque **ligar após a implementação**e, em seguida, selecione **concluir**. Por predefinição, o modelo de servidor de configuração é implementado com um NIC único. Pode adicionar NICs adicionais após a implementação.


## <a name="add-an-additional-adapter"></a>Adicionar outro adaptador

Se pretender adicionar uma NIC adicional para o servidor de configuração, adicione-o antes de registar o servidor no cofre. Adicionar mais adaptadores não é suportado após o registo.

1. No inventário do vSphere Client, clique na VM com o botão direito do rato e selecione **Editar Definições**.
2. No **Hardware**, selecione **adicionar** > **adaptador Ethernet**. Em seguida, selecione **Seguinte**.
3. Selecione um tipo de adaptador e uma rede. 
4. Para ligar o NIC virtual quando a VM estiver ligada, selecione **Estabelecer ligação ao ligar**. Em seguida, selecione **seguinte** > **concluir** > **OK**.
 

## <a name="register-the-configuration-server"></a>Registar o servidor de configuração 

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceitar o contrato de licença e introduza uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão para a VM como o administrador.
4. Na primeira vez que iniciar sessão, inicia a ferramenta de configuração do Azure Site Recovery.
5. Introduza um nome que é utilizado para registar o servidor de configuração com a recuperação de Site. Em seguida, selecione **Seguinte**.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois da ligação for estabelecida, selecione **sessão** para iniciar sessão sua subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.
7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Iniciar sessão máquina novamente. O Assistente de gestão do servidor de configuração é iniciado automaticamente.

### <a name="configure-settings"></a>Configurar definições

1. No Assistente de gestão do servidor de configuração, selecione **configuração conectividade**. Selecione o NIC para receber o tráfego de replicação e, em seguida, selecione **guardar**. Não é possível alterar esta definição depois de estar configurada.
2. No **selecionar serviços de recuperação cofre**, selecione a sua subscrição do Azure e o grupo de recursos relevantes e do cofre.
3. No **instalar software de terceiros**, aceite o contrato de licença. Selecione **transferir e instalar** para instalar o servidor de MySQL.
4. Selecione **instalar VMware PowerLCI**. Certifique-se de que todas as janelas do browser estão fechadas antes de efetuar este passo. Em seguida, selecione **continuar**.
5. No **validar configuração de aplicação**, verificar pré-requisitos antes de continuar.
6. No **configurar servidor de ESXi vSphere/servidor vCenter**, introduza o FQDN ou endereço IP do servidor vCenter ou anfitriões vSphere, onde estão localizadas as VMs que pretende replicar. Introduza a porta que o servidor está a escutar e um nome amigável para o servidor do VMware no cofre.
7. Introduza as credenciais para ser utilizado pelo servidor de configuração para ligar ao servidor do VMware. O Site Recovery utiliza estas credenciais para detetar automaticamente as VMs VMware que estão disponíveis para replicação. Selecione **adicionar**e, em seguida, selecione **continuar**.
8. No **configurar as credenciais de máquina virtual**, introduza o nome de utilizador e palavra-passe para ser utilizado para instalar automaticamente o serviço de mobilidade do Azure Site Recovery nas máquinas, quando a replicação está ativada. Para computadores Windows, a conta precisa de privilégios de administrador local nos computadores que pretende replicar. Para o Linux, forneça detalhes para a conta raiz.
9. Selecione **Finalize configuração** para concluir o registo. 
10. Após a conclusão do registo, no portal do Azure, certifique-se de que o servidor de configuração e o servidor VMware estão listados na página **Origem** do cofre. Em seguida, selecione **OK** para configurar as definições de destino.


## <a name="troubleshoot-deployment-issues"></a>Resolver problemas de implementação

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Passos Seguintes

Reveja os tutoriais sobre como configurar a recuperação após desastre de [VMs de VMware](tutorial-vmware-to-azure.md) e [servidores físicos](tutorial-physical-to-azure.md) para o Azure.
