---
title: Configurar a origem ambiente e a configuração serverfor recuperação após desastre de VMs de VMware para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como configurar o seu ambiente no local e o servidor de configuração para recuperação após desastre de VMs de VMware para o Azure com o Azure Site Recovery.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: e09e93ef85449c51e35b8da7ad93ee7088a0e7b4
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566317"
---
# <a name="set-up-the-source-environment-and-configuration-server"></a>Configurar o servidor de configuração e de ambiente de origem

Este artigo descreve como configurar o ambiente no local de origem para recuperação após desastre de VMs de VMware para o Azure com [do Azure Site Recovery](site-recovery-overview.md). Ele inclui passos para configurar uma máquina no local como o servidor de configuração do Site Recovery e a detetar automaticamente VMs no local. 

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que fez o seguinte

- Planejado sua recuperação de desastres implementação utilizando o [do Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md). O que ajuda a garantir que tem recursos suficientes e largura de banda para recuperação após desastre e a ponto de recuperação objetivo (RPO) na sua organização.
- [Configurar recursos do Azure](tutorial-prepare-azure.md) no [portal do Azure](http://portal.azure.com).
- [Configurar o VMware no local](vmware-azure-tutorial-prepare-on-premises.md) recursos, incluindo uma conta dedicada para a deteção automática de VMs de VMware no local.
- Pode [rever as perguntas mais comuns](vmware-azure-common-questions.md#configuration-server) sobre a implementação de servidor de configuração e gestão.


## <a name="choose-protection-goals"></a>Selecione os objetivos de proteção

1. Em **Cofres dos Serviços de Recuperação**, selecione o nome do cofre. 
2. Em **Introdução**, selecione Site Recovery. Em seguida, selecione **Preparar Infraestrutura**.
3. Em **Objetivo de proteção** > **Onde estão localizadas as máquinas virtuais**, selecione **No local**.
4. Em **Para onde pretende replicar as máquinas**, selecione **Para o Azure**.
5. Em **As suas máquinas estão virtualizadas**, selecione **Sim, com o VMware vSphere Hypervisor**. Em seguida, selecione **OK**.

## <a name="about-the-configuration-server"></a>Sobre o servidor de configuração

Implementar um servidor de configuração no local ao configurar a recuperação após desastre de VMs de VMware para o Azure.

- As comunicações de coordenadas do servidor de configuração entre locais VMware e o Azure. Ele também gerencia a replicação de dados.
- Implementar o servidor de configuração como uma VM de VMware.
- Recuperação de sites fornece um modelo de OVA que transferir a partir do portal do Azure e importar para o vCenter Server para configurar a VM do servidor de configuração.
- [Saiba mais](vmware-azure-architecture.md) sobre os componentes de servidor de configuração e os processos.


>[!NOTE]
Não utilize estas instruções se estiver a implementar o servidor de configuração para recuperação após desastre de computadores físicos no local para o Azure. Para este cenário, siga [este artigo](physical-azure-set-up-source.md).


## <a name="before-you-deploy-the-configuration-server"></a>Antes de implementar o servidor de configuração

Se instalar o servidor de configuração como uma VM do VMware com o modelo de OVA, a VM será instalada com todos os pré-requisitos. Se pretende rever os pré-requisitos, utilize os seguintes artigos.

- [Saiba mais sobre](vmware-azure-configuration-server-requirements.md) o hardware, software e requisitos de capacidade para o servidor de configuração.
- Se estiver a replicar várias VMs de VMware, deve rever o [considerações de planeamento de capacidade](site-recovery-plan-capacity-vmware.md)e execute o [do Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md) ferramenta para replicação de VMWare.
- A licença do Windows para o modelo de OVA é uma licença de avaliação é válida por 180 dias. Após esse período, terá de ativar o Windows com uma licença válida. 
- O modelo de OVA fornece uma forma simples de configurar o servidor de configuração como uma VM de VMware. Se por algum motivo, precisa configurar a VM de VMware sem o modelo, reveja os pré-requisitos e siga as instruções manuais [este artigo](physical-azure-set-up-source.md).
- Sua conta do Azure necessita de permissões para criar aplicações do Azure AD.


>[!IMPORTANT]
O servidor de configuração tem de ser implementado, tal como descrito neste artigo. Copiar ou clonagem de um servidor de configuração existente não é suportada.

### <a name="set-up-azure-account-permissions"></a>Configurar permissões de conta do Azure

Seja um administrador global/inquilino pode atribuir permissões para criar aplicações do Azure AD para a conta, ou atribuir a função de desenvolvedor do aplicativo (que tem as permissões) à conta.


O administrador de inquilino/global pode conceder permissões para a conta da seguinte forma:

1. No Azure AD, o administrador global/inquilino deve acessar **do Azure Active Directory** > **utilizadores** > **as definições de utilizador**.
2. O administrador deve definir **registos de aplicações** ao **Sim**.

 > [!NOTE]
 > Esta é uma definição de predefinição que não seja confidencial. [Saiba mais](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Em alternativa o administrador de inquilino/global tem permissões para atribuir a função para a conta. [Saiba mais](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

 


## <a name="download-the-ova-template"></a>Transferir o modelo de OVA

1. No cofre, vá para **Preparar Infraestrutura** > **Origem**.
2. Em **Preparar origem**, selecione **+Servidor de configuração**.
3. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
4. Transfira o modelo de aplicativo de Virtualização aberto (OVA) para o servidor de configuração.

  > [!TIP]
>Também pode transferir a versão mais recente do modelo de servidor de configuração do [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-ova-template-in-vmware"></a>Importar o modelo de OVA no VMware

1. Inicie sessão no servidor VMware vCenter ou no anfitrião vSphere ESXi, com o VMWare vSphere Client.
2. No menu **Ficheiro**, selecione **Implementar o Modelo OVF**, para iniciar o assistente para Implementar o Modelo OVF.

     ![Modelo OVF](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. Na **selecionar origem**, introduza a localização do OVF transferido.
4. Na **rever detalhes**, selecione **próxima**.
5. Na **selecione o nome e a pasta** e **selecionar configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Aprovisionamento Intenso Diligente Posto a Zero** em **Selecionar formato de disco virtual**.
7. No resto das páginas do assistente, aceite as predefinições.
8. Na **pronto para concluir**, para configurar uma VM com as configurações padrão, selecionadas **ligar após a implementação** > **concluir**.
9. Por predefinição, a VM é implementada com um único NIC. Se pretender adicionar uma adição NIC, desmarque **ligar após a implementação**e clique em **concluir**. Em seguida, siga o procedimento seguinte.

## <a name="add-an-adapter-to-the-configuration-server"></a>Adicionar um adaptador para o servidor de configuração

Se pretender adicionar outro NIC ao servidor de configuração, adicione-o antes de registar o servidor no cofre. Adicionar mais adaptadores não é suportado após o registo.

1. No inventário do vSphere Client, clique na VM com o botão direito do rato e selecione **Editar Definições**.
2. Em **Hardware**, selecione **Adicionar** > **Adaptador Ethernet**. Em seguida, selecione **Seguinte**.
3. Selecione um tipo de adaptador e uma rede.
4. Para ligar o NIC virtual quando a VM estiver ativada, selecione **Connect em ativação**. Em seguida, selecione **próxima** > **concluir** > **OK**.

## <a name="register-the-configuration-server"></a>Registar o servidor de configuração 
Ative a VM e registar o servidor de configuração no cofre do Site Recovery.

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca no experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão na VM como administrador.



## <a name="set-up-the-configuration-server"></a>Configurar o servidor de configuração

Como parte da implementação, terá de instalar o MySQL no servidor de configuração de VM. Pode fazê-lo de duas formas:

- Deixe que o Site Recovery, transferir e instalar o MySQL. Se pretender utilizar esta opção, não precisa de fazer nada antes de iniciar a configuração do servidor de configuração.
- Transferir e instalar manualmente o MySQL: antes de configurar o servidor de configuração, transfira o instalador do MySQL e colocá-la a **C:\Temp\ASRSetup** pasta. Agora a instalar o MySQL. 
- Transferir manualmente e permita que a recuperação de Site que instalar. Antes de configurar o servidor de configuração, transfira o instalador do MySQL e colocá-la a **C:\Temp\ASRSetup** pasta.


1. Na primeira vez que iniciar sessão na VM, a ferramenta de configuração do Azure Site Recovery é iniciada.
2. Especifique um nome utilizado para registar o servidor de configuração no cofre do Site Recovery. Em seguida, selecione **Seguinte**.
3. A ferramenta verifica se a VM pode ligar ao Azure. Depois de a ligação estar estabelecida, selecione **Iniciar sessão** para iniciar sessão na sua subscrição do Azure. Tenha em atenção que a conta tem de ter acesso ao Cofre no qual pretende registar o servidor de configuração.
4. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
5. Inicie sessão na máquina novamente. O Assistente de gestão de servidor de configuração é iniciado automaticamente dentro de alguns segundos.
6. No assistente, selecione **configurar a conectividade**.
7. Selecione o NIC que o servidor de processos (em execução por predefinição no servidor de configuração) utiliza para receber o tráfego de replicação de VMs.
8. Em seguida, selecione **Guardar**. Tenha em atenção que não é possível alterar as definições do cofre depois do servidor de configuração está registado. 
9. Na **cofre dos serviços de recuperação selecione**, inicie sessão no Microsoft Azure, selecione a sua subscrição do Azure e o grupo de recursos relevantes e o cofre. 
10. Na **instalar o software de terceiros**, instalar o MySQL:
     - Se o Site Recovery está a processar a MySQL transferência e instalação, clique em **transferir e instalar**. Aguarde que a instalação seja concluída e, em seguida, continuar com o assistente.
     - Se baixou o MySQL e Site Recovery irá instalá-lo, clique em **transferir e instalar**. Aguarde a conclusão da instalação e prosseguir com o assistente.
     - Se manualmente, baixado e instalado o MySQL, clique em **transferir e instalar**. A aplicação mostra como **já instalado**. Continue com o assistente.
11. Na **validar configuração da aplicação**, programa de configuração verifica os pré-requisitos antes de continuar. 
12. Na **configurar servidor vCenter Server/vSphere ESXi**, introduza o FQDN ou endereço IP do servidor vCenter ou anfitrião vSphere, no qual as VMs que pretende replicar estão localizados. Introduza a porta em que o servidor está a escutar e especifique um nome amigável para o servidor VMware no cofre.
13. Introduza as credenciais que serão utilizadas pelo servidor de configuração para se ligar ao servidor VMware e detetar automaticamente as VMs de VMware que estão disponíveis para replicação. Selecione **adicione** >  **continuar**. As credenciais são guardadas localmente.
14. Na **Configure as credenciais da máquina virtual**, especifique as credenciais que o Site Recovery irá utilizar para instalar automaticamente o serviço de mobilidade, quando ativa a replicação para uma VM.
    - Para computadores Windows, a conta precisa de privilégios de administrador local nos computadores que pretende replicar.
    - Para o Linux, forneça detalhes para a conta raiz.
15. Selecione **Finalizar configuração** para concluir o registo.
16. Após a conclusão do registo, abra o portal do Azure e verifique se o servidor de configuração e o servidor VMware aparecem **cofre dos serviços de recuperação** > **gerir**  >   **Infraestrutura do site Recovery** > **servidores de configuração**.


## <a name="exclude-antivirus-on-the-configuration-server"></a>Excluir antivírus no servidor de configuração

Se estiver a executar software antivírus no servidor de configuração de VM, certifique-se de que as seguintes pastas são excluídas da operações de antivírus. Isso garante que a replicação e conectividade funcionar conforme esperado: 

- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\Program Files\Microsoft Azure Site Recovery Provider
- C:\Programas\Microsoft Azure Site Recovery Configuration Manager
- Ferramenta de coleção de erro do C:\Program Files\Microsoft Azure Site Recovery
- C:\thirdparty
- C:\Temp
- C:\strawberry
- C:\ProgramData\MySQL
- C:\Program ficheiros (x86) \MySQL
- C:\ProgramData\ASR
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\Inetpub
- Diretório de instalação do site Recovery. Por exemplo: E:\Program ficheiros (x86) \Microsoft Azure Site Recovery


## <a name="next-steps"></a>Passos Seguintes
- Caso se depare com dificuldades, reveja [perguntas comuns](vmware-azure-common-questions.md#configuration server) e [resolução de problemas](vmware-azure-troubleshoot-configuration-server.md) para o servidor de configuração.
- Agora, [configurar o ambiente de destino](./vmware-azure-set-up-target.md) no Azure.
