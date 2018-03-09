---
title: "Implementar o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como implementar um servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 99b368ca364bd7c5bebfc00c2df0f04333293388
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="deploy-a-configuration-server"></a>Implementar um servidor de configuração

Implementar um servidor de configuração no local ao utilizar [do Azure Site Recovery](site-recovery-overview.md) para recuperação após desastre de VMs de VMware e servidores físicos para o Azure. As comunicações de coordenadas do servidor de configuração no local VMware e o Azure. Também gere a replicação de dados. Este artigo explica os passos necessários para implementar o servidor de configuração quando está a replicar VMware VMs para o Azure. [Siga este artigo](physical-azure-set-up-source.md) se precisar de configurar um servidor de configuração para replicação do servidor físico.

## <a name="prerequisites"></a>Pré-requisitos

Recomendamos que implemente o servidor de configuração como uma VM de VMware altamente disponível. Requisitos mínimos de hardware estão resumidos na tabela seguinte.

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
2. Em **Preparar origem**, selecione **+Servidor de configuração**.
3. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
4. Transfira o modelo OVF (Open Virtualization Format) para o servidor de configuração.

  > [!TIP]
  Pode transferir a versão mais recente do modelo de servidor de configuração diretamente a partir de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-template-in-vmware"></a>Importar o modelo no VMware


1. Inicie sessão no servidor VMware vCenter ou no anfitrião vSphere ESXi, utilizando o VMWare vSphere Client.
2. No menu **Ficheiro**, selecione **Implementar o Modelo OVF**, para iniciar o assistente para Implementar o Modelo OVF.

     ![Modelo OVF](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. No **selecionar origem**, introduza a localização do OVF transferido.
4. No **rever detalhes**, selecione **seguinte**.
5. No **Selecionar nome e a pasta** e **selecione configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Aprovisionamento Intenso Diligente Posto a Zero** em **Selecionar formato de disco virtual**.
4. No resto das páginas do assistente, aceite as predefinições.
5. Em **Pronto para concluir**:

    * Para configurar a VM com as predefinições, selecione **Ligar após a implementação** > **Concluir**.

    * Para adicionar uma interface de rede adicionais, desmarque **ligar após a implementação**e, em seguida, selecione **concluir**. Por predefinição, o modelo do servidor de configuração é implementado com um único NIC. Pode adicionar mais NICs após a implementação.


## <a name="add-an-additional-adapter"></a>Adicionar outro adaptador

Se pretender adicionar uma NIC adicional para o servidor de configuração, adicione-o antes de registar o servidor no cofre. Adicionar mais adaptadores não é suportado após o registo.

1. No inventário do vSphere Client, clique na VM com o botão direito do rato e selecione **Editar Definições**.
2. Em **Hardware**, selecione **Adicionar** > **Adaptador Ethernet**. Em seguida, selecione **Seguinte**.
3. Selecione um tipo de adaptador e uma rede. 
4. Para ligar o NIC virtual quando a VM estiver ligada, selecione **Estabelecer ligação ao ligar**. Em seguida, selecione **seguinte** > **concluir** > **OK**.
 

## <a name="register-the-configuration-server"></a>Registar o servidor de configuração 

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão na VM como administrador.
4. Da primeira vez que iniciar sessão, a Ferramenta de Configuração do Azure Site Recovery é iniciada.
5. Introduza um nome que será utilizado para registar o servidor de configuração no Site Recovery. Em seguida, selecione **Seguinte**.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois de a ligação estar estabelecida, selecione **Iniciar sessão** para iniciar sessão na sua subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.
7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Inicie sessão na máquina novamente. O assistente de gestão do servidor de configuração inicia automaticamente.

### <a name="configure-settings"></a>Configurar definições

1. No Assistente de gestão do servidor de configuração, selecione **configuração conectividade**. Selecione o NIC para receber o tráfego de replicação e, em seguida, selecione **guardar**. Não é possível alterar esta definição depois de estar configurada.
2. Em **Selecionar cofre dos Serviços de Recuperação**, selecione a sua subscrição do Azure e o grupo de recursos e cofre relevantes.
3. Em **Instalar software de terceiros**, aceite o contrato de licença. Selecione **Transferir e Instalar** para instalar o Servidor MySQL.
4. Selecione **instalar VMware PowerLCI**. Certifique-se de que todas as janelas do browser estão fechadas antes de efetuar este passo. Em seguida, selecione **Continuar**.
5. Em **Validar configuração da aplicação**, os pré-requisitos são verificados antes de continuar.
6. Em **Configurar servidor vCenter Server/vSphere ESXi**, introduza o FQDN ou endereço IP do servidor vCenter ou anfitrião vSphere onde estão localizadas as VMs que pretende replicar. Introduza a porta que o servidor está a escutar e um nome amigável para o servidor do VMware no cofre.
7. Introduza as credenciais que o servidor de configuração irá utilizar para ligar ao servidor VMware. O Site Recovery utiliza estas credenciais para detetar automaticamente as VMs VMware que estão disponíveis para replicação. Selecione **Adicionar** e, em seguida, selecione **Continuar**.
8. No **configurar as credenciais de máquina virtual**, introduza o nome de utilizador e palavra-passe para ser utilizado para instalar automaticamente o serviço de mobilidade do Azure Site Recovery nas máquinas, quando a replicação está ativada. Para computadores Windows, a conta precisa de privilégios de administrador local nos computadores que pretende replicar. Para o Linux, forneça detalhes para a conta raiz.
9. Selecione **Finalize configuração** para concluir o registo. 
10. Após a conclusão do registo, no portal do Azure, certifique-se de que o servidor de configuração e o servidor VMware estão listados na página **Origem** do cofre. Em seguida, selecione **OK** para configurar as definições de destino.


## <a name="troubleshoot-deployment-issues"></a>Resolver problemas de implementação

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Passos Seguintes

Configure a recuperação de desastre do [VMs de VMware](vmware-azure-tutorial.md) para o Azure.
