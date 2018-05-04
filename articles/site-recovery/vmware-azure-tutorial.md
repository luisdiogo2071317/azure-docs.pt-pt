---
title: Configurar a recuperação após desastre para o Azure das VMs VMware no local com o Azure Site Recovery | Microsoft Docs
description: Saiba como configurar a recuperação após desastre para o Azure das VMs VMware no local com o Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6c86a98dd819b91608be04f1466dc1e6764ee4b9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Configurar a recuperação após desastre para o Azure das VMs VMware no local

Este tutorial mostra como configurar a recuperação após desastre para o Azure das VMs VMware no local, com o Windows. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Introduza a origem e o destino da replicação.
> * Configure o ambiente de replicação de origem, incluindo os componentes do Azure Site Recovery no local e o ambiente de replicação de destino.
> * Criar uma política de replicação.
> * Ativar a replicação para uma VM.

Este tutorial é o terceiro de uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

* [Preparar o Azure](tutorial-prepare-azure.md). Este tutorial descreve como configurar uma conta de armazenamento do Azure e a rede, assegurar que a sua conta do Azure tem as permissões corretas e criar um cofre dos Serviços de Recuperação.
* [Preparar o VMware no local](vmware-azure-tutorial-prepare-on-premises.md). Neste tutorial, vai preparar contas para que o Site Recovery possa aceder aos servidores do VMware para detetar VMs e, opcionalmente, executar uma instalação push do componente do serviço de mobilidade do Site Recovery quando ativar a replicação para uma VM. Também vai assegurar que os servidores do VMware e as VMs estão em conformidade com os requisitos do Site Recovery.

Antes de começar, é útil [rever a arquitetura](vmware-azure-architecture.md) dos cenários de recuperação após desastre.


## <a name="select-a-replication-goal"></a>Selecionar um objetivo de replicação

1. Em **Cofres dos Serviços de Recuperação**, selecione o nome do cofre, **ContosoVMVault**.
2. Em **Introdução**, selecione Site Recovery. Em seguida, selecione **Preparar Infraestrutura**.
3. Em **Objetivo de proteção** > **Onde estão localizadas as máquinas virtuais**, selecione **No local**.
4. Em **Para onde pretende replicar as máquinas**, selecione **Para o Azure**.
5. Em **As suas máquinas estão virtualizadas**, selecione **Sim, com o VMware vSphere Hypervisor**. Em seguida, selecione **OK**.

## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem


Para configurar o ambiente de origem, precisará de uma única máquina de elevada disponibilidade no local para alojar componentes do Site Recovery no local. Os componentes incluem o servidor de configuração, o servidor de processos e o servidor de destino principal:

- O servidor de configuração coordena as comunicações entre o local e o Azure, e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure. O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.
- O servidor de destino principal processa dados de replicação durante a reativação pós-falha a partir do Azure.

Para configurar o servidor de configuração como uma VM VMware de elevada disponibilidade, transfira um modelo OVF (Open Virtualization Format) preparado e importe o modelo para o VMware, para criar a VM. Depois de configurar o servidor de configuração, registe-o no cofre. Após o registo, o Site Recovery deteta as VMs VMware no local.

> [!TIP]
> Este tutorial utiliza um modelo OVF para criar o servidor de configuração da VM de VMware. Se não for possível efetuar este procedimento, pode executar a [configuração manual](physical-manage-configuration-server.md). 


### <a name="download-the-vm-template"></a>Transferir o modelo de VM

1. No cofre, vá para **Preparar Infraestrutura** > **Origem**.
2. Em **Preparar origem**, selecione **+Servidor de configuração**.
3. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
4. Transfira o modelo OVF para o servidor de configuração.

  > [!TIP]
  Pode transferir a versão mais recente do modelo do servidor de configuração diretamente do [Centro de Transferências da Microsoft](https://aka.ms/asrconfigurationserver).

## <a name="import-the-template-in-vmware"></a>Importar o modelo no VMware

1. Inicie sessão no servidor VMware vCenter ou no anfitrião vSphere ESXi, utilizando o VMWare vSphere Client.
2. No menu **Ficheiro**, selecione **Implementar o Modelo OVF**, para iniciar o assistente para Implementar o Modelo OVF. 

     ![Modelo OVF](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. Em **Selecionar origem**, introduza a localização do OVF transferido.
4. Em **Rever detalhes**, selecione **Seguinte**.
5. Em **Selecionar nome e pasta** e **Selecionar configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Thick Provision Eager Zeroed** em **Selecionar formato de disco virtual**.
7. Nas restantes páginas do assistente, aceite as predefinições.
8. Em **Pronto para concluir**:

    * Para configurar a VM com as predefinições, selecione **Ligar após a implementação** > **Concluir**.

    * Se pretender adicionar outra interface de rede, desmarque **Ligar após a implementação**. Em seguida, selecione **Concluir**. Por predefinição, o modelo do servidor de configuração é implementado com um único NIC. Pode adicionar mais NICs após a implementação.

## <a name="add-an-additional-adapter"></a>Adicionar outro adaptador

Para adicionar outro NIC ao servidor de configuração, adicione-o antes de registar o servidor no cofre. Adicionar mais adaptadores não é suportado após o registo.

1. No inventário do vSphere Client, clique na VM com o botão direito do rato e selecione **Editar Definições**.
2. Em **Hardware**, selecione **Adicionar** > **Adaptador Ethernet**. Em seguida, selecione **Seguinte**.
3. Selecione um tipo de adaptador e uma rede. 
4. Para ligar o NIC virtual quando a VM estiver ligada, selecione **Estabelecer ligação ao ligar**. Selecione **Seguinte** > **Concluir**. Em seguida, selecione **OK**.


## <a name="register-the-configuration-server"></a>Registar o servidor de configuração 

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão na VM como administrador.
4. Da primeira vez que iniciar sessão, a Ferramenta de Configuração do Azure Site Recovery é iniciada.
5. Introduza um nome que será utilizado para registar o servidor de configuração no Site Recovery. Em seguida, selecione **Seguinte**.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois de a ligação estar estabelecida, selecione **Iniciar sessão** para iniciar sessão na sua subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.
7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Inicie sessão na máquina novamente. O assistente de gestão do servidor de configuração inicia automaticamente.

### <a name="configure-settings-and-add-the-vmware-server"></a>Configurar as definições e adicionar o servidor VMware

1. No assistente de gestão do servidor de configuração, selecione **Configurar a conectividade** e, em seguida, selecione o NIC para receber o tráfego de replicação. Em seguida, selecione **Guardar**. Não é possível alterar esta definição depois de estar configurada.
2. Em **Selecionar cofre dos Serviços de Recuperação**, selecione a sua subscrição do Azure e o grupo de recursos e cofre relevantes.
3. Em **Instalar software de terceiros**, aceite o contrato de licença. Selecione **Transferir e Instalar** para instalar o Servidor MySQL.
4. Selecione **Instalar VMware PowerCLI**. Certifique-se de que todas as janelas do browser estão fechadas antes de efetuar este procedimento. Em seguida, selecione **Continuar**.
5. Em **Validar configuração da aplicação**, os pré-requisitos são verificados antes de continuar.
6. Em **Configurar servidor vCenter Server/vSphere ESXi**, introduza o FQDN ou endereço IP do servidor vCenter ou anfitrião vSphere onde estão localizadas as VMs que pretende replicar. Introduza a porta em que o servidor está a escutar. Introduza um nome amigável a utilizar para o servidor VMware no cofre.
7. Introduza as credenciais que o servidor de configuração irá utilizar para ligar ao servidor VMware. O Site Recovery utiliza estas credenciais para detetar automaticamente as VMs VMware que estão disponíveis para replicação. Selecione **Adicionar** e, em seguida, selecione **Continuar**.
8. Em **Configurar as credenciais da máquina virtual**, introduza o nome de utilizador e a palavra-passe a utilizar para instalar automaticamente o Serviço de Mobilidade nas máquinas, quando a replicação estiver ativada. Para computadores Windows, a conta precisa de privilégios de administrador local nos computadores que pretende replicar. Para o Linux, forneça detalhes para a conta raiz.
9. Selecione **Finalizar configuração** para concluir o registo. 
10. Após a conclusão do registo, no portal do Azure, certifique-se de que o servidor de configuração e o servidor VMware estão listados na página **Origem** do cofre. Em seguida, selecione **OK** para configurar as definições de destino.


O Site Recovery liga-se aos servidores VMware com as definições especificadas e deteta as VMs.

> [!NOTE]
> Pode demorar 15 minutos ou mais para o nome da conta aparecer no portal. Para atualizar imediatamente, selecione **Servidores de Configuração** > ***nome do servidor*** > **Atualizar Servidor**.

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e verifique os recursos de destino.

1. Selecione **Preparar infraestrutura** > **Destino**. Selecione a subscrição do Azure que pretende utilizar.
2. Especifique se o seu modelo de implementação de destino é baseado no Azure Resource Manager ou se é clássico.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.

   ![Separador Destino](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Criar uma política de replicação

1. Abra o [portal do Azure](https://portal.azure.com) e selecione **Todos os recursos**.
2. Selecione o cofre do Serviço de Recuperação com o nome **ContosoVMVault**.
3. Para criar uma política de replicação, selecione **Infraestrutura do Site Recovery** > **Políticas de Replicação** > **+Política de Replicação**.
4. Em **Criar política de replicação**, introduza o nome de política **VMwareRepPolicy**.
5. Em **Limiar de RPO**, utilize a predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
6. Em **Retenção do ponto de recuperação**, utilize a predefinição de 24 horas como a duração da janela de retenção para cada ponto de recuperação. Para este tutorial, utilize 72 horas. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
7. Em **Frequência de instantâneos consistentes com a aplicação**, utilize a predefinição de 60 minutos para a frequência com que os instantâneos consistentes com a aplicação são criados. Selecione **OK** para criar a política.

   ![Criar política de replicação](./media/vmware-azure-tutorial/replication-policy.png)

A política é associada automaticamente ao servidor de configuração. Uma política correspondente é criada automaticamente para reativação pós-falha por predefinição. Por exemplo, se a política de replicação for **rep-policy**, a política de reativação pós-falha é **rep-policy-failback**. Esta política não é utilizada depois de iniciar uma reativação pós-falha a partir do Azure.

## <a name="enable-replication"></a>Ativar a replicação

O Site Recovery instala o Serviço de Mobilidade quando a replicação está ativada para uma VM. Pode demorar 15 minutos ou mais tempo para as alterações produzirem efeitos e aparecerem no portal.

Ative a replicação da seguinte forma:

1. Selecione **Replicar aplicação** > **Origem**.
2. Em **Origem**, selecione o servidor de configuração.
3. Em **Tipo de máquina**, selecione **Máquinas Virtuais**.
4. Em **vCenter/vSphere Hypervisor**, selecione o servidor vCenter que gere o anfitrião vSphere ou selecione o anfitrião.
5. Selecione o servidor de processos (servidor de configuração). Em seguida, selecione **OK**.
6. Em **Destino**, selecione a subscrição e o grupo de recursos no qual pretende criar as VMs de ativação pós-falha. Selecione o modelo de implementação que pretende utilizar no Azure (clássico ou Resource Manager) para as VMs de ativação pós-falha.
7. Selecione a conta de armazenamento do Azure que pretende utilizar para replicar os dados.
8. Selecione a rede e a sub-rede do Azure às quais as VMs do Azure se ligam quando forem criadas após a ativação pós-falha.
9. Selecione **Configurar agora para as máquinas selecionadas** para aplicar a definição de rede para todas as máquinas selecionadas para proteção. Selecione **Configurar mais tarde** para selecionar a rede do Azure por máquina.
10. Em **Máquinas Virtuais** > **Selecionar máquinas virtuais**, selecione cada máquina que pretende replicar. Só pode selecionar máquinas para as quais a replicação pode ser ativada. Em seguida, selecione **OK**.
11. Em **Propriedades** > **Configurar propriedades**, selecione a conta que o servidor de processos irá utilizar para instalar automaticamente o Serviço de Mobilidade na máquina.
12. Em **Definições de replicação** > **Configurar as definições de replicação**, certifique-se de que a política de replicação correta está selecionada.
13. Selecione **Ativar Replicação**.

Pode controlar o progresso da tarefa **Ativar Proteção** em **Definições** > **Tarefas** > **Tarefas do Site Recovery**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.

Para monitorizar as VMs que adiciona, verifique a última hora de deteção de VMs em **Servidores de Configuração** > **Último Contacto Em**. Para adicionar VMs sem aguardar a deteção agendada, realce o servidor de configuração (não o selecione) e selecione **Atualizar**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Executar um teste de recuperação após desastre](site-recovery-test-failover-to-azure.md)
