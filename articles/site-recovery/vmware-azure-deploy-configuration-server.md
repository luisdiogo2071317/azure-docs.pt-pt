---
title: Implementar o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como implementar um servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/11/2018
ms.author: mayg
ms.openlocfilehash: 1efbd6bfb6f3bc3e5deae058b542f665b3153cdb
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794359"
---
# <a name="deploy-a-configuration-server"></a>Implementar um servidor de configuração

Implementar um servidor de configuração no local quando utiliza [do Azure Site Recovery](site-recovery-overview.md) para recuperação após desastre de VMs de VMware e servidores físicos para o Azure. As comunicações de coordenadas do servidor de configuração entre locais VMware e o Azure. Ele também gerencia a replicação de dados. Este artigo orienta-o pelos passos necessários para implementar o servidor de configuração quando está a replicar VMs de VMware para o Azure. [Siga este artigo](physical-azure-set-up-source.md) se precisar de configurar um servidor de configuração para a replicação de servidor físico.

>[!TIP]
Pode aprender sobre a função do servidor de configuração como parte da arquitetura do Azure Site Recovery [aqui](vmware-azure-architecture.md).

## <a name="deployment-of-configuration-server-through-ova-template"></a>Implementação do servidor de configuração através do modelo de OVA

Servidor de configuração tem de ser definido como uma VM de VMware altamente disponível com determinados mínimos de hardware e requisitos de dimensionamento. Para a implementação de cómodo e fácil, o Site Recovery fornece um modelo de OVA (aplicativo de Virtualização aberto) que pode ser baixado para configurar o servidor de configuração que está em conformidade com todos os requisitos estabelecidos listados abaixo.

## <a name="prerequisites"></a>Pré-requisitos

Requisitos mínimos de hardware para um servidor de configuração estão resumidos na tabela seguinte.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="capacity-planning"></a>Planeamento de capacidade

Os requisitos de dimensionamento para o servidor de configuração dependem da taxa de alteração de dados possíveis. Utilize esta tabela como guia.

| **CPU** | **Memória** | **Tamanho do disco de cache** | **Taxa de alteração de dados** | **Máquinas protegidas** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 sockets * 4 núcleos \@ 2,5 GHz) |16 GB |300 GB |500 GB ou menos |Replicar máquinas menos de 100. |
| 12 vCPUs (2 sockets * 6 núcleos \@ 2,5 GHz) |18 GB |600 GB |500 GB a 1 TB |Replicar máquinas de 100 150. |
| 16 vCPUs (2 sockets * 8 núcleos \@ 2,5 GHz) |32 GB |1 TB |1 TB para 2 TB |Replicar máquinas de 150 a 200. |

Se estiver a replicar mais do que uma VM de VMware, leia [considerações de planeamento de capacidade](site-recovery-plan-capacity-vmware.md). Executar o [ferramenta Planeador de implementações](site-recovery-deployment-planner.md) para a replicação de VMWare.

## <a name="download-the-template"></a>Transferir o modelo

1. No cofre, vá para **Preparar Infraestrutura** > **Origem**.
2. Em **Preparar origem**, selecione **+Servidor de configuração**.
3. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
4. Transfira o modelo de aplicativo de Virtualização aberto (OVA) para o servidor de configuração.

  > [!TIP]
>Também pode transferir a versão mais recente do modelo de servidor de configuração diretamente a partir [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

>[!NOTE]
A licença fornecida com o modelo de OVA é uma licença de avaliação válida por 180 dias. Publicar este período, cliente tem de ativar o windows com uma licença procured.

## <a name="import-the-template-in-vmware"></a>Importar o modelo no VMware

1. Inicie sessão no servidor VMware vCenter ou no anfitrião vSphere ESXi, utilizando o VMWare vSphere Client.
2. No menu **Ficheiro**, selecione **Implementar o Modelo OVF**, para iniciar o assistente para Implementar o Modelo OVF.

     ![Modelo OVF](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. Na **selecionar origem**, introduza a localização do OVF transferido.
4. Na **rever detalhes**, selecione **próxima**.
5. Na **selecione o nome e a pasta** e **selecionar configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Aprovisionamento Intenso Diligente Posto a Zero** em **Selecionar formato de disco virtual**.
7. No resto das páginas do assistente, aceite as predefinições.
8. Em **Pronto para concluir**:

    * Para configurar a VM com as predefinições, selecione **Ligar após a implementação** > **Concluir**.

    * Para adicionar outra interface de rede, desmarque **ligar após a implementação**e, em seguida, selecione **concluir**. Por predefinição, o modelo do servidor de configuração é implementado com um único NIC. Pode adicionar mais NICs após a implementação.

## <a name="add-an-additional-adapter"></a>Adicionar outro adaptador

Se pretender adicionar outro NIC ao servidor de configuração, adicione-o antes de registar o servidor no cofre. Adicionar mais adaptadores não é suportado após o registo.

1. No inventário do vSphere Client, clique na VM com o botão direito do rato e selecione **Editar Definições**.
2. Em **Hardware**, selecione **Adicionar** > **Adaptador Ethernet**. Em seguida, selecione **Seguinte**.
3. Selecione um tipo de adaptador e uma rede.
4. Para ligar o NIC virtual quando a VM estiver ativada, selecione **Connect em ativação**. Em seguida, selecione **próxima** > **concluir** > **OK**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Registar o servidor de configuração com os serviços do Azure Site Recovery

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão na VM como administrador.
4. Na primeira vez que iniciar sessão, dentro de alguns segundos a ferramenta de configuração do Azure Site Recovery é iniciada.
5. Introduza um nome que será utilizado para registar o servidor de configuração no Site Recovery. Em seguida, selecione **Seguinte**.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois de a ligação estar estabelecida, selecione **Iniciar sessão** para iniciar sessão na sua subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.
7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Inicie sessão na máquina novamente. O Assistente de gestão de servidor de configuração é iniciado **automaticamente** em alguns segundos.

### <a name="configure-settings"></a>Configurar definições

1. No assistente de gestão do servidor de configuração, selecione **Configurar a conectividade** e, em seguida, selecione o NIC que o servidor de processos utiliza para receber o tráfego de replicação das VMs. Em seguida, selecione **Guardar**. Não é possível alterar esta definição depois de estar configurada. Recomenda-se vivamente que não altere o endereço IP de um servidor de configuração. Certifique-se de que o IP atribuído ao servidor de configuração é o IP estático e não os IP de DHCP.
2. Na **cofre dos serviços de recuperação selecione**, inicie sessão no Microsoft Azure, selecione a sua subscrição do Azure e o grupo de recursos relevantes e o cofre.

    > [!NOTE]
    > Depois de registado, não há nenhum flexibilidade para alterar o Cofre dos serviços de recuperação.

3. Na **instalar o software de terceiros**,

    |Cenário   |Passos a seguir  |
    |---------|---------|
    |Pode transferir e instalar manualmente o MySQL?     |  Sim. Transferir a aplicação do MySQL e coloque-o na pasta **C:\Temp\ASRSetup**, em seguida, instale manualmente. Agora, quando aceitar os termos de > clique em **transfira e instale**, o portal indica *já instalada*. Pode avançar para o passo seguinte.       |
    |Posso evitar a transferência do MySQL online?     |   Sim. Coloque a sua aplicação de instalador do MySQL na pasta **C:\Temp\ASRSetup**. Aceite os termos > clique em **transferir e instalar**, o portal irá utilizar o instalador adicionado por e instalará o aplicativo. Pode avançar para a instalação de postagem de passo seguinte.    |
    |Eu gostaria de transferir e instalar o MySQL através do Azure Site Recovery     |  Aceitar o contrato de licença e clique em **transferir e instalar**. Em seguida, pode avançar para a instalação de postagem de passo seguinte.       |
4. Em **Validar configuração da aplicação**, os pré-requisitos são verificados antes de continuar.
5. Em **Configurar servidor vCenter Server/vSphere ESXi**, introduza o FQDN ou endereço IP do servidor vCenter ou anfitrião vSphere onde estão localizadas as VMs que pretende replicar. Introduza a porta em que o servidor está a escutar. Introduza um nome amigável a utilizar para o servidor VMware no cofre.
6. Introduza as credenciais que o servidor de configuração irá utilizar para ligar ao servidor VMware. O Site Recovery utiliza estas credenciais para detetar automaticamente as VMs VMware que estão disponíveis para replicação. Selecione **adicione**e, em seguida **continuar**. As credenciais introduzidas aqui localmente são guardadas.
7. Na **Configure as credenciais da máquina virtual**, introduza o nome de utilizador e palavra-passe de máquinas virtuais para instalar automaticamente o serviço de mobilidade durante a replicação. Para **Windows** máquinas, a conta tem privilégios de administrador local nas máquinas que pretende replicar. Para **Linux**, forneça detalhes da conta de raiz.
8. Selecione **Finalizar configuração** para concluir o registo.
9. Após a conclusão do registo, abra o portal do Azure, certifique-se de que o servidor de configuração e o servidor VMware estão listados na **cofre dos serviços de recuperação** > **gerir**  >  **Infraestrutura do site Recovery** > **servidores de configuração**.

## <a name="upgrade-the-configuration-server"></a>Atualizar o servidor de configuração

Para atualizar o servidor de configuração para a versão mais recente, siga estes [passos](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).

## <a name="manage-the-configuration-server"></a>Gerir o servidor de configuração

Para evitar interrupções na replicação em curso, certifique-se de que o endereço IP do servidor de configuração não é alterada depois do servidor de configuração foi registado para um cofre. Pode saber mais sobre tarefas de gestão de servidor de configuração comuns [aqui](vmware-azure-manage-configuration-server.md).

## <a name="faq"></a>FAQ

1. Pode utilizar a VM, onde está instalado o servidor de configuração, para fins diferentes?

    **Não**, recomendamos que utilize a VM para o único propósito de servidor de configuração. Certifique-se de que segue todas as especificações mencionadas na [pré-requisitos](#prerequisites) para a gestão eficiente de recuperação após desastre.
2. Pode alternar o Cofre já está registado no servidor de configuração com um cofre recém-criado?

    **Não**, depois de um cofre está registado no servidor de configuração, não pode ser alterado.
3. Pode utilizar o mesmo servidor de configuração para proteger máquinas virtuais e físicas?

    **Sim**, o mesmo servidor de configuração pode ser utilizado para replicar máquinas físicas e virtuais. No entanto, o computador físico pode ser falha back apenas a uma VM de VMware.
4. Qual é a finalidade de um servidor de configuração e em que é utilizada?

    Consulte a [arquitetura de replicação do Azure de VMware para](vmware-azure-architecture.md) para saber mais sobre o servidor de configuração e as funcionalidades do mesmo.
5. Onde posso encontrar a versão mais recente do servidor de configuração?

    Para obter os passos atualizar o servidor de configuração através do portal, consulte [atualizar o servidor de configuração](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server). Pode baixá-lo de também diretamente [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
6. Onde posso transferir a frase de acesso para o servidor de configuração?

    Consulte a [este artigo](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) para transferir a frase de acesso.
7. Onde posso transferir as chaves de registo do Cofre?

    Na **Cofre de serviços de recuperação**, **gerir** > **infraestrutura do Site Recovery** > **deservidoresdeconfiguração**. Nos servidores, selecione **chave de registo do Download** para transferir o ficheiro de credenciais do cofre.
8. Pode clonar um servidor de configuração existente e utilizá-lo para a orquestração da replicação?

    **Não**, utilização de um componente de servidor de configuração clonado não é suportada.

9. Pode alterar o IP do servidor de configuração?

    **Não**, é vivamente recomendado para não alterar o endereço IP de um servidor de configuração. Certifique-se de que todos os IPs atribuídos ao servidor de configuração são IPs ESTÁTICOS e não os IPs de DHCP.

## <a name="troubleshoot-deployment-issues"></a>Resolver problemas de implementação

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Passos Seguintes

Configurar a recuperação após desastre de [VMs de VMware](vmware-azure-tutorial.md) para o Azure.