---
title: "Configure a recuperação de desastre para o Azure para as VMs de VMware no local com o Azure Site Recovery | Microsoft Docs"
description: "Saiba como configurar a recuperação após desastre para o Azure para as VMs de VMware no local com o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8acc8deff8b635c97e8722d65a728aebf0e49bb3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Configure a recuperação de desastre para o Azure para as VMs de VMware no local

Este tutorial mostra como configurar a recuperação após desastre para o Azure para as VMs de VMware no local com o Windows. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Especifique a origem de replicação e de destino.
> * Configure o ambiente de replicação de origem, incluindo os componentes da recuperação de Site no local e o ambiente de replicação de destino.
> * Criar uma política de replicação
> * Ativar a replicação para uma VM

Este é o terceiro tutorial uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Preparar o VMware no local](tutorial-prepare-on-premises-vmware.md)

Antes de começar, é útil [rever a arquitetura](concepts-vmware-to-azure-architecture.md) para cenário de recuperação após desastre.


## <a name="select-a-replication-goal"></a>Selecione um objetivo de replicação

1. No **cofres dos serviços de recuperação**, clique no nome do cofre, **ContosoVMVault**.
2. No **introdução**, clique em recuperação de sites. Em seguida, clique em **preparar infraestrutura**.
3. No **objetivo de proteção** > **onde estão as máquinas localizadas**, selecione **no local**.
4. No * * onde pretende replicar as máquinas, selecione **para o Azure**.
5. No **são as máquinas virtualizadas**, selecione **Sim, com o VMware vSphere hipervisor**. Em seguida, clique em **OK**.

## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Para configurar o ambiente de origem, terá de um único de elevada disponibilidade, máquina no local para componentes de recuperação de sites no local do anfitrião. Componentes incluem o servidor de configuração, o servidor de processos e o servidor de destino principal.

- O servidor de configuração coordena as comunicações entre o local e o Azure, e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação, otimiza-os com a colocação em cache, compressão e encriptação, e envia-os para o armazenamento do Azure. O servidor de processos também instala o serviço de mobilidade em VMs que pretende replicar, e efetua a descoberta automática de VMs de VMware no local.
- O servidor de destino mestre processa dados de replicação durante a reativação pós-falha a partir do Azure.

Para configurar o servidor de configuração como uma VM de VMware altamente disponível, transferir um modelo OVF preparado e importar o modelo para VMware para criar a VM. Depois de configurar o servidor de configuração, registá-lo no cofre. Após o registo, a recuperação de Site Deteta as VMs de VMware no local.

### <a name="download-the-vm-template"></a>Transferir o modelo VM

1. O cofre, aceda a **preparar infraestrutura** > **origem**.
2. No **preparar a origem**, clique em **+ o servidor de configuração**.
3. No **Adicionar servidor**, verifique se **servidor de configuração para VMware** aparece no **tipo de servidor**.
4. Transferir o modelo de abrir virtualização formato OVF () para o servidor de configuração.

  > [!TIP]
  Pode ser transferida a versão mais recente do modelo de servidor de configuração diretamente a partir do [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

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

### <a name="configure-settings-and-connect-to-vmware"></a>Configurar as definições e ligue-se para VMware

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


Recuperação de sites liga-se aos servidores do VMware com as definições especificadas e Deteta as VMs.

> [!NOTE]
> Pode demorar 15 minutos ou mais para o nome da conta aparecer no portal. Para atualizar imediatamente, clique em **Servidores de Configuração** > ***nome do servidor*** > **Atualizar Servidor**.

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e certifique-se de recursos de destino.

1. Clique em **Preparar a infraestrutura** > **Destino** e selecione a subscrição do Azure que pretende utilizar.
2. Especifique se o seu modelo de implementação de destino é baseado no Resource Manager ou clássico.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.

   ![Alvo](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Criar uma política de replicação

1. Abra o [portal do Azure](https://portal.azure.com) e clique em **todos os recursos**.
2. Clique no cofre do serviço de recuperação com o nome **ContosoVMVault**.
3. Para criar uma política de replicação, clique em **infraestrutura de recuperação de Site** > **políticas de replicação** > **+ política de replicação**.
4. No **criar política de replicação**, especifique um nome de política **VMwareRepPolicy**.
5. No **limiar RPO**, utilizar a predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. É gerado um alerta se a replicação contínua excede este limite.
6. No **retenção do ponto de recuperação**, utilizar a predefinição de 24 horas para o período de retenção da janela de retenção para cada ponto de recuperação. Para este tutorial, iremos selecionar 72 horas. VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
7. No **frequência de instantâneos consistentes com aplicação**, utilizar a predefinição de 60 minutos para a frequência de que os instantâneos consistentes com aplicações foram criados. Clique em **OK** para criar uma política.

   ![Política](./media/tutorial-vmware-to-azure/replication-policy.png)

A política é automaticamente associada ao servidor de configuração. Por predefinição, uma política correspondente é criada automaticamente para reativação pós-falha. Por exemplo, se a política de replicação é **rep política** a política de reativação pós-falha será **rep-política-reativação pós-falha**. Esta política não é utilizada depois de iniciar uma reativação pós-falha a partir do Azure.

## <a name="enable-replication"></a>Ativar a replicação

Recuperação de site instala o serviço de mobilidade quando a replicação está ativada para uma VM. Pode demorar 15 minutos ou já para as alterações que tenha efeito e aparecer no portal.

Ative a replicação da seguinte forma:

1. Clique em **replicar aplicação** > **origem**.
2. No **origem**, selecione o servidor de configuração.
3. No **máquina tipo**, selecione **máquinas virtuais**.
4. No **vCenter/vSphere hipervisor**, selecione o servidor vCenter que gere o anfitrião do Vcenter ou selecione o anfitrião.
5. Selecione o servidor de processos (servidor de configuração). Clique IThen **OK**.
6. No **destino**, selecione a subscrição e o grupo de recursos no qual pretende criar a ativação pós-falha das VMs. Escolha o modelo de implementação que pretende utilizar no Azure (clássica ou recurso management), para a ativação pós-falha das VMs com falhas.
7. Selecione a conta de armazenamento do Azure que pretende utilizar para replicar os dados.
8. Selecione a rede e a sub-rede do Azure às quais as VMs do Azure se vão ligar quando forem criadas após a ativação pós-falha.
9. Selecione **Configurar agora para as máquinas selecionadas** para aplicar a definição de rede a todas as máquinas selecionadas para proteção. Selecione **Configurar mais tarde** para selecionar a rede do Azure por máquina.
10. Em **Máquinas Virtuais** > **Selecionar máquinas virtuais**, clique e selecione cada máquina que pretende replicar. Só pode selecionar máquinas para as quais a replicação pode ser ativada. Em seguida, clique em **OK**.
11. No **propriedades** > **configurar propriedades**, selecione a conta que será utilizada pelo servidor de processos para instalar automaticamente o serviço de mobilidade na máquina.
12. No **as definições de replicação** > **configurar as definições de replicação**, certifique-se de que a política de replicação correto está selecionada.
13. Clique em **ativar a replicação**.

Pode controlar o progresso do **ativar proteção** da tarefa no **definições** > **tarefas** > **tarefas de recuperação de Site**. Depois da tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.

Para monitorizar as VMs que adiciona, pode verificar a última vez detetada para VMs no **servidores de configuração**
> **Último contacto em**. Para adicionar VMs sem aguardar que a deteção agendada, realce o servidor de configuração (não clique nele) e clique em **atualizar**.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Executar um teste de recuperação após desastre](site-recovery-test-failover-to-azure.md)
