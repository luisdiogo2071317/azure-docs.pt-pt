---
title: Instalar o servidor de cópia de segurança do Azure na pilha do Azure | Microsoft Docs
description: Utilize o servidor de cópia de segurança do Azure para proteger ou cópia de segurança de cargas de trabalho na pilha do Azure.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: servidor do backup do Azure; proteger cargas de trabalho; cópia de segurança de cargas de trabalho
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/5/2018
ms.author: markgal
ms.openlocfilehash: f39f8571d4256a14f64ee2a66788cac8fa524eec
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248899"
---
# <a name="install-azure-backup-server-on-azure-stack"></a>Instalar Azure Backup Server no Azure Stack

Este artigo explica como instalar o servidor de cópia de segurança do Azure na pilha do Azure. Com o servidor de cópia de segurança do Azure, pode proteger a infraestrutura como um cargas de trabalho do serviço (IaaS) como máquinas virtuais em execução na pilha do Azure. Uma vantagem de utilizar o servidor de cópia de segurança do Azure para proteger as cargas de trabalho é que pode gerir todos os proteção de carga de trabalho a partir de uma única consola.

> [!NOTE]
> Para saber mais sobre as capacidades de segurança, consulte [documentação de funcionalidades de segurança da cópia de segurança do Azure](backup-azure-security-feature.md).
>

## <a name="azure-backup-server-protection-matrix"></a>Matriz de proteção do Azure Backup Server
Servidor de cópia de segurança do Azure protege as seguintes cargas de trabalho de máquina virtual de pilha do Azure.

| Origem de dados protegida | Proteção e recuperação |
| --------------------- | ----------------------- |
| Canal de anual do Windows Server Semiestruturados - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| Windows Server 2016 - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| Windows Server 2012 R2 - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| Windows Server 2012 – Datacenter/Entprise/Standard | Volumes, ficheiros, pastas |
| Windows Server 2008 R2 - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| SQL Server 2016 | Base de Dados |
| SQL Server 2014 | Base de Dados |
| SQL Server 2012 SP1 | Base de Dados |
| SharePoint 2013 | Farm, base de dados, front-end, servidor web |
| O SharePoint 2010 | Farm, base de dados, front-end, servidor web |


### <a name="host-vs-guest-backup"></a>Vs convidado cópia de segurança do anfitrião

Servidor de cópia de segurança do Azure efetua anfitrião ou ao nível do convidado cópias de segurança de máquinas virtuais. Ao nível do anfitrião, o agente do Backup do Azure está instalado na máquina virtual ou cluster e protege a toda a máquina virtual e ficheiros de dados em execução no anfitrião. Ao nível do convidado, o agente do Backup do Azure está instalado em cada máquina virtual e protege a carga de trabalho presente nessa máquina.

Ambos os métodos de ter os respetivos os profissionais de TI e contras:

   * Cópias de segurança ao nível do anfitrião de trabalho, independentemente do SO em execução em máquinas de convidados e não necessitam do agente do Backup do Azure ser instalado em cada VM. Se implementar cópias de segurança ao nível do anfitrião, recuperar uma máquina virtual inteira, ou ficheiros e pastas (recuperação ao nível do item).
   * Cópia de segurança ao nível do convidado é vantajoso para proteger cargas de trabalho específicas em execução numa máquina virtual. Ao nível do anfitrião, pode recuperar toda a VM ou ficheiros específicos, mas não recuperar os dados no contexto de uma aplicação específica. Por exemplo, para recuperar ficheiros específicos de SharePoint a partir de uma máquina virtual protegida, terá de proteger a VM ao nível do convidado. Se pretender proteger os dados armazenados em discos pass-through, tem de utilizar cópias de segurança ao nível do convidado. Pass-through permite que a máquina virtual para aceder diretamente ao armazenamento e não armazena dados de volumes virtuais num ficheiro VHD.

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Pré-requisitos para o ambiente de servidor de cópia de segurança do Azure

Quando instalar o servidor de cópia de segurança do Azure no seu ambiente de pilha do Azure, considere as recomendações nesta secção. O instalador do servidor de cópia de segurança do Azure verifica que o seu ambiente tem os pré-requisitos necessários, mas irá poupar tempo ao preparar antes de instalar.

### <a name="determining-size-of-virtual-machine"></a>Determinar o tamanho da máquina virtual
Para executar o servidor de cópia de segurança do Azure numa máquina virtual do Azure pilha, utilize tamanho A2 ou superior. Para obter ajuda na escolha de um tamanho de máquina virtual, transfira o [Calculadora do tamanho da VM do Azure pilha](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Redes virtuais em máquinas virtuais de pilha do Azure
Todas as máquinas virtuais utilizadas numa carga de trabalho de pilha do Azure têm de pertencer à mesma rede virtual do Azure e subscrição do Azure.

### <a name="azure-backup-server-vm-performance"></a>Desempenho de VM do servidor de cópia de segurança do Azure
Se partilhado com outras máquinas virtuais, o armazenamento e de contas tamanho impacto de limites IOPS desempenho de VM de servidor de cópia de segurança do Azure. Por este motivo, deve utilizar uma conta de armazenamento separada para a máquina virtual do servidor de cópia de segurança do Azure. O agente de cópia de segurança do Azure em execução no servidor de cópia de segurança do Azure necessita de armazenamento temporário para:
- uso próprio (uma localização da cache),
- dados restaurados a partir da nuvem (área de transição local)

### <a name="configuring-azure-backup-temporary-disk-storage"></a>Configurar o armazenamento de disco temporário de cópia de segurança do Azure
Cada máquina virtual de pilha do Azure é fornecido com o armazenamento em disco temporário, que está disponível para o utilizador como volume `D:\`. A área de transição local necessária para a cópia de segurança do Azure pode ser configurada para estar situada em `D:\`, e a localização da cache pode ser colocada em `C:\`. Desta forma, nenhum armazenamento tem de ser carved na direção oposta aos discos de dados ligados à máquina virtual do servidor de cópia de segurança do Azure.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Armazenar dados de cópia de segurança no disco local e no Azure
Servidor do Backup do Azure armazena os dados de cópia de segurança nos discos Azure ligados à máquina virtual, para recuperação operacional. Assim que o espaço de armazenamento e discos estão anexados à máquina virtual, o servidor de cópia de segurança do Azure gere o armazenamento para si. A quantidade de armazenamento de dados de cópia de segurança depende do número e tamanho dos discos anexados a cada [máquina virtual de pilha do Azure](../azure-stack/user/azure-stack-storage-overview.md). Cada tamanho da VM de pilha do Azure tem um número máximo de discos que podem ser anexados à máquina virtual. Por exemplo, A2 é quatro discos. A3 é oito discos. A4 é 16 discos. Novamente, o tamanho e número de discos determina o agrupamento de armazenamento de cópia de segurança total.

> [!IMPORTANT]
> Deve **não** manter os dados de recuperação operacional (cópia de segurança) nos discos ligados por servidor de cópia de segurança do Azure mais de cinco dias.
>

Armazenar dados de cópia de segurança no Azure reduz a infraestrutura de cópia de segurança na pilha do Azure. Se os dados são mais de cinco dias, devem ser armazenado no Azure.

Para armazenar dados de cópia de segurança no Azure, criar ou utilizar um cofre dos serviços de recuperação. Quando preparar a cópia de segurança a carga de trabalho do servidor de cópia de segurança do Azure, [configurar o Cofre dos serviços de recuperação](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault). Depois de configurar, sempre que é executada uma tarefa de cópia de segurança, é criado um ponto de recuperação no cofre. Cada cofre dos serviços de recuperação contém até 9999 pontos de recuperação. Dependendo do número de pontos de recuperação criados e quanto são mantidos, pode manter os dados de cópia de segurança para muitos anos. Por exemplo, pode criar pontos de recuperação mensal e manter os mesmos cinco anos.
 
### <a name="using-sql-server"></a>Utilizar o SQL Server
Se pretender utilizar um SQL Server remoto para a base de dados do servidor de cópia de segurança do Azure, selecione apenas uma VM de pilha do Azure com o SQL Server.

### <a name="scaling-deployment"></a>Implementação de dimensionamento
Se pretender dimensionar a sua implantação, tem as seguintes opções:
  - Aumentar verticalmente - aumentar o tamanho da máquina virtual do servidor de cópia de segurança do Azure, de uma série a série de D e aumentar o armazenamento local [pelas instruções de máquina virtual do Azure pilha](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Descarregar dados - envia os dados antigos para servidor de cópia de segurança do Azure e mantém apenas os dados mais recentes no armazenamento ligado ao servidor de cópia de segurança do Azure.
  - Ampliar - adicionar mais servidores de cópia de segurança do Azure para proteger as cargas de trabalho.

### <a name="net-framework"></a>.NET framework

.NET framework 3.5 SP1 ou superior tem de estar instalado na máquina virtual.

### <a name="joining-a-domain"></a>Associação a um domínio

A máquina virtual do servidor de cópia de segurança do Azure tem de ser associada a um domínio. Um utilizador de domínio com privilégios de administrador tem de instalar servidor de cópia de segurança do Azure na máquina virtual.

## <a name="using-an-iaas-vm-in-azure-stack"></a>Utilizar uma VM do IaaS na pilha do Azure

Ao escolher um servidor de cópia de segurança do servidor do Azure, começar com uma imagem de galeria do Windows Server 2012 R2 Datacenter ou Datacenter do Windows Server 2016. O artigo [criar a sua primeira máquina virtual do Windows no portal do Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), fornece um tutorial para começar a trabalhar com o recomendado da máquina virtual. Os requisitos mínimos recomendados para a máquina virtual (VM) do servidor devem ser: A2 padrão com dois núcleos e 3.5 GB de RAM.

Proteger cargas de trabalho com o servidor de cópia de segurança do Azure tem muitas nuances. O artigo [instalar o DPM como uma máquina virtual do Azure](https://technet.microsoft.com/library/jj852163.aspx), ajuda a explica estes nuances. Antes de implementar a máquina, leia este artigo completamente.

> [!NOTE]
> Servidor de cópia de segurança do Azure foi concebido para ser executado numa máquina virtual dedicada, de finalidade única. Não é possível instalar o servidor de cópia de segurança do Azure em:
> - Um computador em execução como um controlador de domínio
> - Um computador no qual a função de Servidor de Aplicações está instalada
> - Um computador no qual o Exchange Server está em execução
> - Um computador que é um nó de um cluster

Sempre associe o servidor de cópia de segurança do Azure a um domínio. Se precisar de mover o servidor de cópia de segurança do Azure para um domínio diferente, primeiro instalar o servidor de cópia de segurança do Azure, em seguida, associar ao novo domínio. Depois de implementar o servidor de cópia de segurança do Azure, não é possível movê-la para um novo domínio.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Definir Replicação de Armazenamento

A opção de replicação de armazenamento de cofre dos serviços de recuperação permite-lhe escolher entre o armazenamento georredundante e localmente redundante. Por predefinição, os cofres dos serviços de recuperação utilizam o armazenamento georredundante. Se este cofre é o seu Cofre primário, deixe a opção de armazenamento definida para o armazenamento georredundante. Escolha armazenamento localmente redundante se pretende uma opção mais barata que seja inferior durável. Leia mais sobre as opções de armazenamento [georredundante](../storage/common/storage-redundancy-grs.md) e [localmente redundante](../storage/common/storage-redundancy-lrs.md) na [descrição geral da replicação de Armazenamento do Azure](../storage/common/storage-redundancy.md).

Para editar a definição de replicação de armazenamento:

1. Selecione o cofre para abrir o dashboard do cofre e o menu definições. Se o **definições** menu não abrir, clique em **todas as definições** no dashboard do cofre.
2. No **definições** menu, clique em **infraestrutura de cópia de segurança** > **configuração de cópia de segurança** para abrir o **configuração de cópia de segurança**menu. No **configuração de cópia de segurança** menu, escolha a opção de replicação de armazenamento para o cofre.

    ![Lista de cofres de cópia de segurança](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

## <a name="download-azure-backup-server-installer"></a>Transferir o instalador do servidor de cópia de segurança do Azure

Existem duas formas para transferir o instalador do servidor de cópia de segurança do Azure. Pode transferir o instalador do servidor de cópia de segurança do Azure do [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55269). Também pode transferir o instalador do servidor de cópia de segurança do Azure como a configuração de um cofre dos serviços de recuperação. Os seguintes passos guiá-lo a transferir o instalador do portal do Azure ao configurar um cofre dos serviços de recuperação.

1. A sua máquina virtual de pilha do Azure, [iniciar sessão na sua subscrição do Azure no portal do Azure](https://portal.azure.com/).
2. No menu da esquerda, selecione **todos os serviços**.

    ![Escolha a opção de todos os serviços no menu principal](./media/backup-mabs-install-azure-stack/click-all-services.png)

3. No **todos os serviços** caixa de diálogo, escreva *dos serviços de recuperação*. À medida que começa a escrever, a sua entrada filtra a lista de recursos. Depois de o vir, selecione **cofres dos serviços de recuperação**.

    ![Tipo dos serviços de recuperação na caixa de diálogo todos os serviços](./media/backup-mabs-install-azure-stack/all-services.png)

    É apresentada a lista de cofres dos serviços de recuperação na subscrição.

4. Na lista de cofres dos serviços de recuperação, selecione o cofre para abrir o dashboard.

    ![Tipo dos serviços de recuperação na caixa de diálogo todos os serviços](./media/backup-mabs-install-azure-stack/rs-vault-dashboard.png)

5. No menu começar a utilizar o cofre, clique em **cópia de segurança** para abrir o Assistente de introdução.

    ![Introdução de cópia de segurança](./media/backup-mabs-install-azure-stack/getting-started-backup.png)

    Abre o menu de cópia de segurança.

    ![Abrir de cópia de segurança objetivos predefinido](./media/backup-mabs-install-azure-stack/getting-started-menu.png)

6. No menu de cópia de segurança, do **onde está a carga de trabalho em execução** menu, selecione **no local**. Do **que itens pretende cópia de segurança?** menu pendente, selecione as cargas de trabalho que pretende proteger os utilizando o servidor de cópia de segurança do Azure. Se não tem a certeza que cargas de trabalho para selecionar, escolha **máquinas virtuais Hyper-V** e, em seguida, clique em **preparar infraestrutura**.

    ![no local e cargas de trabalho como objetivos](./media/backup-mabs-install-azure-stack/getting-started-menu-onprem-hyperv.png)

    O **preparar a infraestrutura** é aberto o menu.

7. No **preparar a infraestrutura** menu, clique em **transferir** para abrir uma página web para transferir os ficheiros de instalação do servidor de cópia de segurança do Azure.

    ![Introdução ao Assistente de introdução de alterações](./media/backup-mabs-install-azure-stack/prepare-infrastructure.png)

    A página do Microsoft web que aloja os ficheiros transferíveis para o servidor de cópia de segurança do Azure, é aberto.

8. Na página de transferência do servidor de cópia de segurança do Microsoft Azure, selecione um idioma e clique em **transferir**.

    ![É aberto o Centro de transferência](./media/backup-mabs-install-azure-stack/mabs-download-center-page.png)

9. O instalador do servidor de cópia de segurança do Azure é composto de oito ficheiros - um programa de instalação e sete. bin ficheiros. Verifique **nome de ficheiro** para selecionar todos os ficheiros necessários e clique em **seguinte**. Transferir todos os ficheiros na mesma pasta.

    ![1 do Centro de transferências](./media/backup-mabs-install-azure-stack/download-center-selected-files.png)

    O tamanho de transferência de todos os ficheiros de instalação é superior a 3 GB. Durante a uma transferência de 10 Mbps ligação, transferir todos os ficheiros de instalação pode demorar até 60 minutos. Transfiram os ficheiros para a localização de transferência especificada.

## <a name="extract-azure-backup-server-install-files"></a>Extraia os ficheiros de instalação do servidor de cópia de segurança do Azure

Depois de transferir todos os ficheiros para a máquina virtual de pilha do Azure, aceda à localização de transferência. É a primeira fase de instalação do servidor de cópia de segurança do Azure para extrair os ficheiros.

![1 do Centro de transferências](./media/backup-mabs-install-azure-stack/download-mabs-installer.png)

1. Para iniciar a instalação, na lista de ficheiros transferidos, clique em **MicrosoftAzureBackupserverInstaller.exe**.

    > [!WARNING]
    > É necessário, pelo menos, 4GB de espaço livre para extrair os ficheiros de configuração.
    >

2. No Assistente do servidor de cópia de segurança do Azure, clique em **seguinte** para continuar.

    ![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wiz-1.png)

3. Escolha o caminho para os ficheiros do servidor de cópia de segurança do Azure e clique em **seguinte**.

   ![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-select-destination-1.png)

4. Verifique a localização de extração e clique em **extrair**.

   ![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-extract-2.png)

5. O assistente extrai os ficheiros e readies o processo de instalação.

   ![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-install-3.png)

6. Depois de ter concluído o processo de extração, clique em **concluir**. Por predefinição, **executar setup.exe** está selecionada. Ao clicar em **concluir**, Setup.exe instala o servidor de cópia de segurança do Microsoft Azure para a localização especificada.

   ![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-finish-4.png)

## <a name="install-the-software-package"></a>Instalar o pacote de software

No passo anterior, em que clicou **concluir** para a fase de extração de sair e iniciar o Assistente de configuração do servidor de cópia de segurança do Azure.

![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5.png)

Servidor de cópia de segurança do Azure partilha código com o Data Protection Manager. Verá referências a Data Protection Manager e o DPM no instalador do servidor de cópia de segurança do Azure. Apesar do servidor de cópia de segurança do Azure e do Data Protection Manager são separados produtos, estes produtos estão estreitamente relacionadas com. Na documentação do servidor de cópia de segurança do Azure, todas as referências ao Data Protection Manager e o DPM se aplicam ao servidor de cópia de segurança do Azure.

1. Para iniciar o Assistente de configuração, clique em **servidor de cópia de segurança do Microsoft Azure**.

   ![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5b.png)

2. No **boas-vindas** ecrã, clique em **seguinte**.

    ![Verificação do servidor de cópia de segurança do Azure - boas-vindas e pré-requisitos](./media/backup-mabs-install-azure-stack/mabs-install-wizard-setup-6.png)

3. No **verificações de pré-requisitos** ecrã, clique em **verifique** para determinar se tiverem sido cumpridos os pré-requisitos de hardware e software para o servidor de cópia de segurança do Azure.

    ![Verificação do servidor de cópia de segurança do Azure - boas-vindas e pré-requisitos](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-7.png)

    Se o seu ambiente tem os pré-requisitos necessários, verá uma mensagem a indicar que o computador cumpre os requisitos. Clique em **Seguinte**.  

    ![Servidor de cópia de segurança do Azure - passada na verificação de pré-requisitos](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-passed-8.png)

    Se o ambiente não cumprir os pré-requisitos necessários, os problemas serão especificados. Os pré-requisitos não foram cumpridos também constam de DpmSetup.log. Resolva os erros de pré-requisitos e, em seguida, execute **verifique novamente**. Não é possível continuar a instalação até que todos os pré-requisitos são cumpridos.

    ![Servidor de cópia de segurança do Azure - os pré-requisitos da instalação não foram cumpridos](./media/backup-mabs-install-azure-stack/installation-errors.png)

4. Servidor de cópia de segurança do Microsoft Azure necessita do SQL Server. O pacote de instalação do servidor de cópia de segurança do Azure vem agrupado com os binários adequados do SQL Server. Se pretender utilizar a seus próprios instalação do SQL Server, pode. No entanto, a opção recomendada é permitir que o instalador adicionar uma nova instância do SQL Server. Certifique-se à sua escolha funciona com o seu ambiente, clique em **verificar e instalar**.

   > [!NOTE]
   > Servidor de cópia de segurança do Azure não irá funcionar com uma instância remota do SQL Server. A instância utilizada pelo servidor de cópia de segurança do Azure tem de ser local.
   >

    ![Verificação do servidor de cópia de segurança do Azure - boas-vindas e pré-requisitos](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-install-9.png)

    Após verificar, se a máquina virtual tem os pré-requisitos necessários para instalar o servidor de cópia de segurança do Azure, clique em **seguinte**.

    ![Verificação do servidor de cópia de segurança do Azure - boas-vindas e pré-requisitos](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-ready-10.png)

    Se ocorrer uma falha com uma recomendação para reiniciar a máquina, em seguida, reinicie a máquina. Depois de reiniciar o computador, reinicie o programa de instalação, e quando chegar ao **as definições de SQL** ecrã, clique em **verifique novamente**.

5. No **definições de instalação**, forneça uma localização para a instalação de ficheiros do servidor de cópia de segurança do Microsoft Azure e clique em **seguinte**.

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-settings-11.png)

    A localização scratch é necessário para fazer cópias de segurança do Azure. Certifique-se de que o tamanho da localização scratch é equivalente a, pelo menos, 5% dos dados a ser planeados a cópia de segurança para o Azure. Para a proteção de disco, discos separados tem de ser configurado depois de concluída a instalação. Para obter mais informações sobre agrupamentos de armazenamento, consulte [configurar agrupamentos de armazenamento e armazenamento em disco](https://technet.microsoft.com/library/hh758075.aspx).

6. No **definições de segurança** ecrã, forneça uma palavra-passe segura para contas de utilizador locais restritas e clique em **seguinte**.

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-security-12.png)

7. No **Microsoft Update recusa-In** ecrã, selecione se pretende utilizar *Microsoft Update* para procurar atualizações e clique em **seguinte**.

   > [!NOTE]
   > Recomenda-se ter o Windows Update redirecionar para o Microsoft Update oferece segurança e atualizações importantes para o Windows e outros produtos, como o servidor de cópia de segurança do Microsoft Azure.
   >

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-update-13.png)

8. Reveja o *resumo de definições* e clique em **instalar**.

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-summary-14.png)

    Quando o servidor de cópia de segurança do Azure conclusão da instalação, o instalador inicia imediatamente o instalador do agente dos serviços de recuperação do Microsoft Azure.

9. O Microsoft Azure Recovery Services Agent instalador abre e verifica a existência de conectividade à Internet. Se a conectividade à Internet está disponível, prossiga com a instalação. Se não houver sem conectividade, forneça os detalhes do proxy para estabelecer ligação à Internet. Assim que tiver de especificar as definições de proxy, clique em **seguinte**.

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-proxy-15.png)

10. Para instalar o Microsoft Azure Recovery Services Agent, clique em **instalar**.

    ![PreReq2 do servidor de cópia de segurança do Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-mars-agent-16.png)

    O agente dos serviços de recuperação do Microsoft Azure, também denominado o agente de cópia de segurança do Azure, configura o servidor de cópia de segurança do Azure para o Cofre dos serviços de recuperação. Depois de configurar, servidor de cópia de segurança do Azure irá sempre efetuar cópia de segurança dados no mesmo Cofre de serviços de recuperação.

11. Depois do agente dos serviços de recuperação do Microsoft Azure conclusão da instalação, clique em **seguinte** para iniciar a fase seguinte: registar o servidor de cópia de segurança do Azure com o Cofre dos serviços de recuperação.

    ![PreReq2 do servidor de cópia de segurança do Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-complete-16.png)

    O instalador inicia o **Assistente de registo do servidor**.

12. Mudar para a sua subscrição do Azure e o seu Cofre de serviços de recuperação. No **preparar infraestrutura** menu, clique em **transferir** para transferir as credenciais do cofre. Se o **transferir** botão no passo 2 não está ativa, selecione **Already transferido ou utilizando a instalação de servidor de cópia de segurança do Azure mais recente** para ativar o botão. Transferem as credenciais do cofre para a localização onde armazenar as transferências. Tenha em atenção desta localização porque terá para o passo seguinte.

    ![PreReq2 do servidor de cópia de segurança do Azure](./media/backup-mabs-install-azure-stack/download-mars-credentials-17.png)

13. No **cofre identificação** menu, clique em **procurar** para encontrar as credenciais do cofre dos serviços de recuperação.

    ![PreReq2 do servidor de cópia de segurança do Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-vault-id-18.png)

    No **Selecione as credenciais do cofre** caixa de diálogo, vá para a localização de transferência, selecione as suas credenciais do cofre e clique em **abra**.

    O caminho para as credenciais que é apresentado no menu de identificação do cofre. Clique em **seguinte** para avançar para a definição de encriptação.

14. No **definição de encriptação** caixa de diálogo, forneça uma frase de acesso para a encriptação de cópias de segurança e uma localização para armazenar o frase de acesso e clique em **seguinte**.

    ![PreReq2 do servidor de cópia de segurança do Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-encryption-19.png)

    Pode fornecer a suas próprias frase de acesso ou utilizar o gerador de frase de acesso para criar uma por si. A frase de acesso é mesmo seu e da Microsoft não guarde ou gerir esta frase de acesso. Para preparar um desastre, guarde a frase de acesso para uma localização acessível.

    Assim que clicar em **seguinte**, o servidor de cópia de segurança do Azure está registado com o Cofre dos serviços de recuperação. O instalador continua a instalar o SQL Server e o servidor de cópia de segurança do Azure.

    ![PreReq2 do servidor de cópia de segurança do Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-still-installing-20.png)

15. Quando o instalador concluir, o estado mostra de que todo o software foi instalado com êxito.

    ![PreReq2 do servidor de cópia de segurança do Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-done-22.png)

    Quando concluir a instalação, a consola do servidor de cópia de segurança do Azure e os ícones do PowerShell do servidor de cópia de segurança do Azure são criados no ambiente de trabalho do servidor.

## <a name="add-backup-storage"></a>Adicionar armazenamento de cópia de segurança

A primeira cópia de segurança é mantida no armazenamento ligado à máquina do servidor de cópia de segurança do Azure. Para obter mais informações sobre a adição de discos, consulte [configurar agrupamentos de armazenamento e armazenamento em disco](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Terá de adicionar o armazenamento de cópia de segurança mesmo que se pretende enviar dados para o Azure. A arquitetura de servidor de cópia de segurança do Azure, os serviços de recuperação cofre detém o *segundo* cópia dos dados enquanto o armazenamento local contém a cópia de segurança primeiro (e obrigatória).
>
>

## <a name="network-connectivity"></a>Conectividade de rede

Servidor de cópia de segurança do Azure necessita de conectividade para o serviço de cópia de segurança do Azure para o produto a funcionar com êxito. Para validar se o computador tem conectividade para o Azure, utilize o ```Get-DPMCloudConnection``` cmdlet na consola do PowerShell de servidor de cópia de segurança do Azure. Se o resultado do cmdlet é verdadeiro, então existe conectividade, caso contrário há sem conectividade.

Ao mesmo tempo, a subscrição do Azure tem de estar em bom estado de funcionamento. Para saber o estado da sua subscrição e geri-lo, inicie sessão para o [portal subscrição](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

Depois de saber o estado da conetividade do Azure e da subscrição do Azure, pode utilizar a tabela abaixo para descobrir o impacto sobre a funcionalidade de cópia de segurança/restauro fornecida.

| Estado de conectividade | Subscrição do Azure | Criar uma cópia de segurança do Azure | Cópia de segurança para disco | Restaurar a partir do Azure | Restaurar a partir do disco |
| --- | --- | --- | --- | --- | --- |
| Ligado |Ativa |Permitido |Permitido |Permitido |Permitido |
| Ligado |Fora do prazo |Parada |Parada |Permitido |Permitido |
| Ligado |Desaprovisionada |Parada |Parada |Pontos de recuperação de paragem e o Azure eliminados |Parada |
| Conectividade perdida > 15 dias |Ativa |Parada |Parada |Permitido |Permitido |
| Conectividade perdida > 15 dias |Fora do prazo |Parada |Parada |Permitido |Permitido |
| Conectividade perdida > 15 dias |Desaprovisionada |Parada |Parada |Pontos de recuperação de paragem e o Azure eliminados |Parada |

### <a name="recovering-from-loss-of-connectivity"></a>Recuperar a partir de perda de conectividade

Se uma firewall ou um proxy está a impedir o acesso ao Azure, a lista branca domínio seguinte endereços no perfil da firewall/proxy:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

Depois de conectividade para o Azure é restaurada para o servidor de cópia de segurança do Azure, o estado de subscrição do Azure determina as operações que podem ser executadas. Assim que o servidor estiver **ligado**, utilize a tabela em [conectividade de rede](backup-mabs-install-azure-stack.md#network-connectivity) para ver as operações disponíveis.

### <a name="handling-subscription-states"></a>Processamento de Estados de subscrição

É possível alterar uma subscrição do Azure de *expirado* ou *Deprovisioned* estado para *Active Directory* estado. Enquanto o estado de subscrição não é *Active Directory*:

- Enquanto é uma subscrição *Deprovisioned*, perder funcionalidade. A subscrição para o restauro *Active Directory*, revives a funcionalidade de cópia de segurança/restauro. Se os dados de cópia de segurança no disco local foi mantidos com um período de retenção suficientemente grande, o que é possível obter dados de cópia de segurança. No entanto, os dados de cópia de segurança no Azure são irretrievably perdidos depois da subscrição introduz o *Deprovisioned* estado.
- Enquanto é uma subscrição *expirado*, perder funcionalidade. Cópias de segurança agendadas não são executados enquanto é uma subscrição *expirado*.

## <a name="troubleshooting"></a>Resolução de problemas

Se o servidor de cópia de segurança do Microsoft Azure falhar com erros durante a fase de configuração (ou cópia de segurança ou restauro), consulte o [documento de códigos de erro](https://support.microsoft.com/kb/3041338).
Também pode consultar [perguntas mais frequentes relacionadas com a cópia de segurança do Azure](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Passos Seguintes

O artigo [preparar o ambiente para o DPM](https://technet.microsoft.com/library/hh758176.aspx), contém informações sobre as configurações suportadas do servidor de cópia de segurança do Azure.

Pode utilizar os seguintes artigos para obter uma compreensão mais aprofundada da proteção de carga de trabalho com o servidor de cópia de segurança do Microsoft Azure.

- [Cópia de segurança do SQL Server](backup-azure-backup-sql.md)
- [Cópia de segurança do SharePoint server](backup-azure-backup-sharepoint.md)
- [Cópia de segurança do servidor alternativo](backup-azure-alternate-dpm-server.md)
