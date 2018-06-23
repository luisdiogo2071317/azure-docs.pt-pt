---
title: Implementar o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como implementar um servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/06/2018
ms.author: raynew
ms.openlocfilehash: 841176d8c5f215d18edf25b1f191792b37555fa9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318124"
---
# <a name="deploy-a-configuration-server"></a>Implementar um servidor de configuração

Implementar um servidor de configuração no local ao utilizar [do Azure Site Recovery](site-recovery-overview.md) para recuperação após desastre de VMs de VMware e servidores físicos para o Azure. As comunicações de coordenadas do servidor de configuração no local VMware e o Azure. Também gere a replicação de dados. Este artigo explica os passos necessários para implementar o servidor de configuração quando está a replicar VMware VMs para o Azure. [Siga este artigo](physical-azure-set-up-source.md) se precisar de configurar um servidor de configuração para replicação do servidor físico.

>[!TIP]
Pode saber sobre a função do servidor de configuração como parte da arquitetura do Azure Site Recovery [aqui](vmware-azure-architecture.md).

## <a name="deployment-of-configuration-server-through-ova-template"></a>Implementação do servidor de configuração através do modelo de OVA

Servidor de configuração tem de ser configurado como uma VM de VMware altamente disponível com determinados requisitos de dimensionamento e mínimos de hardware. Para implementação cómoda e fácil, a recuperação de sites fornece um modelo de OVA (abrir o Application Virtualization) transferível para configurar o servidor de configuração que está em conformidade com os requisitos de mandated listados abaixo.

## <a name="prerequisites"></a>Pré-requisitos

Requisitos mínimos de hardware para um servidor de configuração estão resumidos na tabela seguinte.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="capacity-planning"></a>Planeamento de capacidade

Os requisitos de dimensionamento para o servidor de configuração dependem a taxa de alteração de dados possíveis. Utilize esta tabela como guia.

| **CPU** | **Memória** | **Tamanho da cache do disco** | **Taxa de alteração de dados** | **Máquinas protegidas** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 sockets * 4 núcleos @ 2,5 GHz) |16 GB |300 GB |500 GB ou inferior |Replicar menos de 100 máquinas. |
| 12 vCPUs (2 sockets * 6 núcleos @ 2,5 GHz) |18 GB |600 GB |500 GB a 1 TB |Replicar 100 150 máquinas. |
| 16 vCPUs (2 sockets * 8 núcleos @ 2,5 GHz) |32 GB |1 TB |1 TB para 2 TB |Replicar máquinas 150 200. |

Se estiver a replicar mais do que uma VM de VMware, leia [considerações de planeamento de capacidade](/site-recovery-plan-capacity-vmware.md). Execute o [ferramenta de implementação do planner](site-recovery-deployment-planner.md) para replicação de VMWare.

## <a name="download-the-template"></a>Transferir o modelo

1. No cofre, vá para **Preparar Infraestrutura** > **Origem**.
2. Em **Preparar origem**, selecione **+Servidor de configuração**.
3. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
4. Transferir o modelo de aplicação de Virtualização abrir (OVA) para o servidor de configuração.

  > [!TIP]
>Também pode transferir a versão mais recente do modelo de servidor de configuração diretamente a partir de [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

>[!NOTE]
O licenciamento fornecido com o modelo de OVA é um licenciamento avaliação válido por 180 dias. Post este período, cliente tem de ativar os windows com um procured licenciamento.

## <a name="import-the-template-in-vmware"></a>Importar o modelo no VMware

1. Inicie sessão no servidor VMware vCenter ou no anfitrião vSphere ESXi, utilizando o VMWare vSphere Client.
2. No menu **Ficheiro**, selecione **Implementar o Modelo OVF**, para iniciar o assistente para Implementar o Modelo OVF.

     ![Modelo OVF](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. No **selecionar origem**, introduza a localização do OVF transferido.
4. No **rever detalhes**, selecione **seguinte**.
5. No **Selecionar nome e a pasta** e **selecione configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Aprovisionamento Intenso Diligente Posto a Zero** em **Selecionar formato de disco virtual**.
7. No resto das páginas do assistente, aceite as predefinições.
8. Em **Pronto para concluir**:

    * Para configurar a VM com as predefinições, selecione **Ligar após a implementação** > **Concluir**.

    * Para adicionar uma interface de rede adicionais, desmarque **ligar após a implementação**e, em seguida, selecione **concluir**. Por predefinição, o modelo do servidor de configuração é implementado com um único NIC. Pode adicionar mais NICs após a implementação.

## <a name="add-an-additional-adapter"></a>Adicionar outro adaptador

Se pretender adicionar uma NIC adicional para o servidor de configuração, adicione-o antes de registar o servidor no cofre. Adicionar mais adaptadores não é suportado após o registo.

1. No inventário do vSphere Client, clique na VM com o botão direito do rato e selecione **Editar Definições**.
2. Em **Hardware**, selecione **Adicionar** > **Adaptador Ethernet**. Em seguida, selecione **Seguinte**.
3. Selecione um tipo de adaptador e uma rede. 
4. Para ligar o NIC virtual quando a VM estiver ativada, selecione **Connect na ativação**. Em seguida, selecione **seguinte** > **concluir** > **OK**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Registar o servidor de configuração com os serviços do Azure Site Recovery

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão na VM como administrador.
4. É iniciado pela primeira vez que iniciar sessão, dentro de alguns segundos a ferramenta de configuração do Azure Site Recovery.
5. Introduza um nome que será utilizado para registar o servidor de configuração no Site Recovery. Em seguida, selecione **Seguinte**.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois de a ligação estar estabelecida, selecione **Iniciar sessão** para iniciar sessão na sua subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.
7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Inicie sessão na máquina novamente. O Assistente de gestão de servidor de configuração é iniciado **automaticamente** em alguns segundos.

### <a name="configure-settings"></a>Configurar definições

1. No assistente de gestão do servidor de configuração, selecione **Configurar a conectividade** e, em seguida, selecione o NIC que o servidor de processos utiliza para receber o tráfego de replicação das VMs. Em seguida, selecione **Guardar**. Não é possível alterar esta definição depois de estar configurada.
2. No **selecionar serviços de recuperação cofre**, inicie sessão no Microsoft Azure, selecione a sua subscrição do Azure e o grupo de recursos relevantes e cofre.
    >[!NOTE]
    > Depois de registado, não há nenhum flexibilidade para alterar o Cofre de serviços de recuperação.
3. No **instalar software de terceiros**,

    |Cenário   |Passos a seguir  |
    |---------|---------|
    |Pode transferir e instalar manualmente o MySQL?     |  Sim. Transferir a aplicação de MySQL & colocá-lo na pasta **C:\Temp\ASRSetup**, em seguida, instale manualmente. Agora, ao aceitar os termos > clique em **transferir e instalar**, indica o portal *já instalado*. Pode avançar para o passo seguinte.       |
    |Pode evitar a transferência de MySQL online?     |   Sim. Colocar a sua aplicação de instalador MySQL na pasta **C:\Temp\ASRSetup**. Aceitar os termos > clique em **transferir e instalar**, o portal irá utilizar o instalador adicionado por si e instala a aplicação. Pode avançar para o próximo passo posteriores à instalação.    |
    |Gostaria de transferir e instalar o MySQL através do Azure Site Recovery     |  Aceitar o contrato de licença e clique em **transferir e instalar**. Em seguida, pode avançar para o próximo passo posteriores à instalação.       |
4. Em **Validar configuração da aplicação**, os pré-requisitos são verificados antes de continuar.
5. Em **Configurar servidor vCenter Server/vSphere ESXi**, introduza o FQDN ou endereço IP do servidor vCenter ou anfitrião vSphere onde estão localizadas as VMs que pretende replicar. Introduza a porta em que o servidor está a escutar. Introduza um nome amigável a utilizar para o servidor VMware no cofre.
6. Introduza as credenciais que o servidor de configuração irá utilizar para ligar ao servidor VMware. O Site Recovery utiliza estas credenciais para detetar automaticamente as VMs VMware que estão disponíveis para replicação. Selecione **adicionar**e, em seguida, **continuar**. As credenciais introduzidas aqui são guardadas localmente.
7. No **configurar as credenciais de máquina virtual**, introduza o nome de utilizador e palavra-passe de máquinas virtuais para instalar automaticamente o serviço de mobilidade durante a replicação. Para **Windows** máquinas, a conta tem privilégios de administrador local nos computadores que pretende replicar. Para **Linux**, forneça os detalhes da conta de raiz.
8. Selecione **Finalizar configuração** para concluir o registo.
9. Após a conclusão de registo, abra o portal do Azure, certifique-se de que o servidor de configuração e o servidor do VMware estão listadas no **cofre dos serviços de recuperação** > **gerir**  >  **Infraestrutura de recuperação de sites** > **servidores de configuração**.

## <a name="faq"></a>FAQ

1. Pode utilizar a VM em que o servidor de configuração está instalado para diferentes fins? **Não**, o servidor de configuração tem de ser um servidor de finalidade única e utilizá-lo como um servidor partilhado não é suportado.
2. Pode alternar o Cofre já registado no servidor de configuração com o Cofre de recém-criado? **Não**, depois de um cofre está registado no servidor de configuração, não pode ser alterada.
3. Pode utilizar o mesmo servidor de configuração de proteção de máquinas virtuais e físicas? **Sim**, mesmo servidor de configuração pode ser utilizado para replicar máquinas virtuais e físicas. No entanto, não é suportada a reativação pós-falha para um computador físico.
4. Em que será utilizado o servidor de configuração? Consulte a nossa arquitetura do Azure Site Recovery [aqui](vmware-azure-architecture.md) para saber mais sobre o servidor de configuração e respetivas funcionalidades.
5. Onde posso encontrar a versão mais recente do servidor de configuração? Poderá transferi-lo de diretamente, [Microsoft Download Center](https://aka.ms/asrconfigurationserver). Consulte o artigo nos passos para o servidor de configuração de atualização [aqui](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).

## <a name="upgrade-the-configuration-server"></a>Atualizar o servidor de configuração

Para atualizar o servidor de configuração para a versão mais recente, leia os passos fornecidos [aqui](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)

## <a name="troubleshoot-deployment-issues"></a>Resolver problemas de implementação

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Passos Seguintes

Configure a recuperação de desastre do [VMs de VMware](vmware-azure-tutorial.md) para o Azure.
