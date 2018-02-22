---
title: "Configurar a recuperação após desastre para o Azure das VMs VMware no local com o Azure Site Recovery | Microsoft Docs"
description: "Saiba como configurar a recuperação após desastre para o Azure das VMs VMware no local, com o serviço Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 3d9248d2501c7fea0492bad2687b6bdfb0b903e8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Configurar a recuperação após desastre para o Azure das VMs VMware no local

Este tutorial mostra como configurar a recuperação após desastre para o Azure das VMs VMware no local, com o Windows. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Especificar a origem e o destino da replicação.
> * Configurar o ambiente de replicação de origem, incluindo os componentes do Site Recovery no local e o ambiente de replicação de destino.
> * Criar uma política de replicação
> * Ativar a replicação para uma VM

Este é o terceiro tutorial de uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Preparar o VMware no local](tutorial-prepare-on-premises-vmware.md)

Antes de começar, é útil [rever a arquitetura](concepts-vmware-to-azure-architecture.md) do cenário de recuperação após desastre.


## <a name="select-a-replication-goal"></a>Selecionar um objetivo de replicação

1. Nos **Cofres dos Serviços de Recuperação**, clique no nome do cofre, **ContosoVMVault**.
2. Em **Introdução**, clique em Site Recovery. Em seguida, clique em **Preparar Infraestrutura**.
3. Em **Objetivo de proteção** > **Onde estão localizadas as máquinas virtuais**, selecione **No local**.
4. Em **Para onde pretende replicar as máquinas**, selecione **Para o Azure**.
5. Em **As suas máquinas estão virtualizadas**, selecione **Sim, com o VMware vSphere Hypervisor**. Em seguida, clique em **OK**.

## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Para configurar o ambiente de origem,precisará de uma única máquina de elevada disponibilidade no local para alojar componentes do Site Recovery no local. Os componentes incluem o servidor de configuração, o servidor de processos e o servidor de destino principal.

- O servidor de configuração coordena as comunicações entre o local e o Azure, e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação, otimiza-os com a colocação em cache, compressão e encriptação, e envia-os para o armazenamento do Azure. O servidor de processos também instala o serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.
- O servidor de destino principal processa dados de replicação durante a reativação pós-falha a partir do Azure.

Para configurar o servidor de configuração como uma VM VMware de elevada disponibilidade, transfira um modelo OVF preparado e importe o modelo para o VMware, para criar a VM. Depois de configurar o servidor de configuração, registe-o no cofre. Após o registo, o Site Recovery deteta as VMs VMware no local.

### <a name="download-the-vm-template"></a>Transferir o modelo de VM

1. No cofre, vá para **Preparar Infraestrutura** > **Origem**.
2. Em **Preparar origem**, clique em **+Servidor de configuração**.
3. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
4. Transfira o modelo OVF (Open Virtualization Format) para o servidor de configuração.

  > [!TIP]
  A última versão do modelo do servidor de configuração pode ser transferida diretamente do [Centro de Transferências da Microsoft](https://aka.ms/asrconfigurationserver).

## <a name="import-the-template-in-vmware"></a>Importar o modelo no VMware

1. Inicie sessão no servidor VMware vCenter server ou no anfitrião vSphere ESXi, utilizando o VMWare vSphere Client.
2. No menu **Ficheiro**, selecione **implementar o Modelo OVF**, para iniciar o assistente para Implementar o Modelo OVF.  

     ![Modelo OVF](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. Em **Selecionar origem**, especifique a localização do OVF transferido.
4. Em **Rever detalhes**, clique em **Seguinte**.
5. Em **Selecionar nome e a pasta** e **Selecionar configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Aprovisionamento Intenso Diligente Posto a Zero** em **Selecionar formato de disco virtual**.
4. No resto das páginas do assistente, aceite as predefinições.
5. Em **Pronto para concluir**:
  - Para configurar a VM com as predefinições, selecione **Ligar após a implementação** > **Concluir**.
  - Se pretender adicionar outra interface de rede, desmarque **Ligar após a implementação** e, em seguida, selecione **Concluir**. Por predefinição, o modelo do servidor de configuração é implementado com um único NIC, mas pode adicionar mais NICs após a implementação.

  
## <a name="add-an-additional-adapter"></a>Adicionar outro adaptador

Se pretender adicionar outro NIC ao servidor de configuração, faça-o antes de registar o servidor no cofre. Adicionar mais adaptadores não é suportado após o registo.

1. No inventário do vSphere Client, clique na VM com o botão direito do rato e selecione **Editar Definições**.
2. Em **Hardware**, clique em **Adicionar** > **Adaptador Ethernet**. Clique depois em **Seguinte**.
3. Selecione e o tipo de adaptador e uma rede. 
4. Para ligar o NIC virtual quando a VM estiver ligada, selecione **Estabelecer ligação ao ligar**. Clique em **Seguinte** > **Concluir** e, em seguida, clique em **OK**.


## <a name="register-the-configuration-server"></a>Registar o servidor de configuração 

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e especifique uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão na VM como o administrador.
4. Da primeira vez que iniciar sessão, a Ferramenta de Configuração do Azure Site Recovery é iniciada.
5. Especifique um nome que será utilizado para registar o servidor de configuração no Site Recovery. Clique depois em **Seguinte**.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois de a ligação estar estabelecida, clique em **Iniciar sessão** para iniciar sessão na sua subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.
7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Inicie sessão na máquina novamente. O assistente de gestão do servidor de configuração irá iniciar automaticamente.

### <a name="configure-settings-and-connect-to-vmware"></a>Configurar as definições e ligar ao VMware

1. No assistente de gestão do servidor de configuração > **Configurar a conectividade**, selecione o NIC que irá receber o tráfego da replicação. Em seguida, clique em **Guardar**. Não é possível alterar esta definição depois de estar configurada.
2. Em **Selecionar cofre dos Serviços de Recuperação**, selecione a sua subscrição do Azure e o grupo de recursos e cofre relevantes.
3. Em **Instalar software de terceiros**, aceite o contrato de licença e clique em **Transferir e Instalar** para instalar o Servidor MySQL.
4. Clique em **Instalar VMware PowerCLI**. Certifique-se de que todas as janelas do browser estão fechadas antes de efetuar este procedimento. Em seguida, clique em **Continuar**
5. Em **Validar configuração da aplicação**, os pré-requisitos serão verificados antes de continuar.
6. Em **Configurar servidor vCenter Server/vSphere ESXi**, especifique o FQDN ou endereço IP do servidor vCenter ou anfitrião vSphere nos quais estão localizadas as VMs que pretende replicar. Especifique a porta na qual que o servidor está a escutar e um nome amigável para ser utilizado para o servidor VMware no cofre.
7. Especifique as credenciais que serão utilizadas pelo servidor de configuração para ligar ao servidor VMware. O Site Recovery utiliza estas credenciais para detetar automaticamente as VMs VMware que estão disponíveis para replicação. Clique em **Adicionar** e, em seguida, clique em **Continuar**.
8. Em **Configurar as credenciais da máquina virtual**, especifique o nome de utilizador e a palavra-passe que serão utilizados para instalar automaticamente o serviço de Mobilidade nos computadores, quando a replicação estiver ativada. Para computadores Windows, a conta precisa de privilégios de administrador local nos computadores que pretende replicar. Para o Linux, forneça detalhes para a conta raiz.
9. Clique em **Finalizar configuração** para concluir o registo. 
10. Após a conclusão do registo, no portal do Azure, certifique-se de que o servidor de configuração e o servidor VMware estão listados na página **Origem** do cofre. Em seguida, clique em **OK** para configurar as definições de destino.


O Site Recovery liga-se aos servidores VMware com as definições especificadas e deteta as VMs.

> [!NOTE]
> Pode demorar 15 minutos ou mais para o nome da conta aparecer no portal. Para atualizar imediatamente, clique em **Servidores de Configuração** > ***nome do servidor*** > **Atualizar Servidor**.

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e verifique os recursos de destino.

1. Clique em **Preparar a infraestrutura** > **Destino** e selecione a subscrição do Azure que pretende utilizar.
2. Especifique se o seu modelo de implementação de destino é baseado no Resource Manager ou clássico.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.

   ![Destino](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Criar uma política de replicação

1. Abra o [portal do Azure](https://portal.azure.com) e clique em **Todos os recursos**.
2. Clique no cofre do Serviço de Recuperação com o nome **ContosoVMVault**.
3. Para criar uma política de replicação, clique em **Infraestrutura do Site Recovery** > **Políticas de Replicação** > **+Política de Replicação**.
4. Em **Criar política de replicação**, especifique o nome de política **VMwareRepPolicy**.
5. Em **Limiar de RPO**, utilize a predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
6. Em **Retenção do ponto de recuperação**, utilize a predefinição de 24 horas como a duração da janela de retenção para cada ponto de recuperação. Para este tutorial, iremos selecionar 72 horas. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
7. Em **Frequência de instantâneos consistentes com a aplicação**, utilize a predefinição de 60 minutos para a frequência com que os instantâneos consistentes com a aplicação são criados. Clique em **OK** para criar a política.

   ![Política](./media/tutorial-vmware-to-azure/replication-policy.png)

A política é associada automaticamente ao servidor de configuração. Por predefinição, uma política correspondente é criada automaticamente para reativação pós-falha. Por exemplo, se a política de replicação for **rep-policy**, a política de reativação pós-falha será **rep-policy-failback**. Esta política não é utilizada depois de iniciar uma reativação pós-falha a partir do Azure.

## <a name="enable-replication"></a>Ativar a replicação

O Site Recovery instala o serviço de Mobilidade quando a replicação está ativada para uma VM. Pode demorar 15 minutos ou mais tempo para as alterações produzirem efeitos e aparecerem no portal.

Ative a replicação da seguinte forma:

1. Clique em **Replicar aplicação** > **Origem**.
2. Em **Origem**, selecione o servidor de configuração.
3. Em **Tipo de máquina**, selecione **Máquinas Virtuais**.
4. Em **vCenter/vSphere Hypervisor**, selecione o servidor vCenter que gere o anfitrião vSphere ou selecione o anfitrião.
5. Selecione o servidor de processos (servidor de configuração). Em seguida, clique em **OK**.
6. Em **Destino**, selecione a subscrição e o grupo de recursos em que pretende criar as VMs de ativação pós-falha. Selecione o modelo de implementação que pretende utilizar no Azure (clássico ou gestão de recursos) para as VMs de ativação pós-falha.
7. Selecione a conta de armazenamento do Azure que pretende utilizar para replicar os dados.
8. Selecione a rede e a sub-rede do Azure às quais as VMs do Azure se vão ligar quando forem criadas após a ativação pós-falha.
9. Selecione **Configurar agora para as máquinas selecionadas** para aplicar a definição de rede a todas as máquinas selecionadas para proteção. Selecione **Configurar mais tarde** para selecionar a rede do Azure por máquina.
10. Em **Máquinas Virtuais** > **Selecionar máquinas virtuais**, clique e selecione cada máquina que pretende replicar. Só pode selecionar máquinas para as quais a replicação pode ser ativada. Em seguida, clique em **OK**.
11. Em **Propriedades** > **Configurar propriedades**, selecione a conta que será utilizada pelo servidor de processos para instalar automaticamente o serviço de Mobilidade na máquina.
12. Em **Definições de replicação** > **Configurar as definições de replicação**, certifique-se de que a política de replicação correta está selecionada.
13. Clique em **Ativar Replicação**.

Pode controlar o progresso da tarefa **Ativar Proteção** em **Definições** > **Tarefas** > **Tarefas do Site Recovery**. Depois da tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.

Para monitorizar as VMs que adiciona, pode verificar a última hora de deteção de VMs em **Servidores de Configuração**
> **Último Contacto Em**. Para adicionar VMs sem aguardar a deteção agendada, realce o servidor de configuração (não clique nele) e clique em **Atualizar**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Executar um teste de recuperação após desastre](site-recovery-test-failover-to-azure.md)
