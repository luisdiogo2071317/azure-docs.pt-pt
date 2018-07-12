---
title: Utilizar o Azure Backup Server para fazer cópias de segurança de cargas de trabalho para o Azure
description: Utilize o Azure Backup Server para proteger ou cópia de segurança de cargas de trabalho para o portal do Azure.
services: backup
author: PVRK
manager: shivamg
keywords: servidor de cópia de segurança do Azure. proteger cargas de trabalho; fazer cópias de segurança de cargas de trabalho
ms.service: backup
ms.topic: conceptual
ms.date: 7/10/2018
ms.author: adigan
ms.openlocfilehash: 67243aca9f5f578402ff79422783148af53798c6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38546022"
---
# <a name="install-and-configure-azure-backup-server"></a>Instalar e configurar o servidor de cópia de segurança do Azure
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Este artigo explica como preparar o ambiente para fazer cópias de segurança de cargas de trabalho com o Azure Backup Server. Com o servidor de cópia de segurança do Azure, pode proteger cargas de trabalho de aplicação, como VMs de Hyper-V, Microsoft SQL Server, SharePoint Server, Microsoft Exchange e clientes Windows de um único console.

> [!NOTE]
> O Azure Backup Server agora pode proteger as VMs VMware e fornece capacidades de segurança melhorada. Instalar o produto, conforme explicado nas secções abaixo; aplica atualização 1 e o agente de cópia de segurança mais recente do Azure. Para saber mais sobre como fazer backup de servidores de VMware no servidor de cópia de segurança do Azure, veja o artigo [utilizar o Azure Backup Server para fazer backup de um servidor VMware](backup-azure-backup-server-vmware.md). Para saber mais sobre as capacidades de segurança, consulte [documentação de funcionalidades de segurança de cópia de segurança do Azure](backup-azure-security-feature.md).
>
>

Também pode proteger a infraestrutura como um cargas de trabalho do serviço (IaaS), como VMs no Azure.

> [!NOTE]
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e clássica](../azure-resource-manager/resource-manager-deployment-model.md). Este artigo fornece as informações e procedimentos para restaurar VMs implementadas com o modelo do Resource Manager.
>
>

O Azure Backup Server herda grande parte da funcionalidade de cópia de segurança da carga de trabalho do Data Protection Manager (DPM). Este artigo liga para documentação do DPM para explicar algumas das funcionalidades compartilhadas. No entanto, servidor de cópia de segurança do Azure partilha grande parte a mesma funcionalidade que o DPM. O Azure Backup Server não fazer cópias de segurança em banda, nem é feita a integração com o System Center.

## <a name="choose-an-installation-platform"></a>Escolha uma plataforma de instalação
A primeira etapa para colocar o servidor de cópia de segurança do Azure em funcionamento é configurar um servidor do Windows. O servidor pode ser no Azure ou no local.

### <a name="using-a-server-in-azure"></a>Utilizar um servidor no Azure
Ao escolher um servidor para executar o servidor de cópia de segurança do Azure, recomenda-se que começar com uma imagem da galeria do Datacenter do Windows Server 2012 R2 ou Windows Server 2016 Datacenter. O artigo [criar a sua primeira máquina virtual do Windows no portal do Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), fornece um tutorial para começar a trabalhar com o recomendado da máquina virtual no Azure, mesmo se nunca utilizou o Azure. Os requisitos mínimos recomendados para a máquina do servidor virtual (VM) devem ser: A2 padrão com dois núcleos e 3,5 GB de RAM.

Proteger cargas de trabalho com o Azure Backup Server tem muitos nuances. O artigo [instalar o DPM como máquina virtual do Azure](https://technet.microsoft.com/library/jj852163.aspx), ajuda a explica tais nuances. Antes de implementar a máquina, leia este artigo completamente.

### <a name="using-an-on-premises-server"></a>Utilizar um servidor no local
Se não pretender executar o servidor de base no Azure, pode executar o servidor numa VM de Hyper-V, uma VM de VMware ou num host físico. Os requisitos mínimos recomendados para o hardware de servidor são dois núcleos e 4 GB de RAM. Os sistemas operativos suportados estão listados na tabela a seguir:

| Sistema Operativo | Plataforma | SKU |
|:--- | --- |:--- |
| Windows Server 2016 e SPs mais recentes |64 bits |Standard, Datacenter, Essentials (e posteriores para o MABS v2) |
| Windows Server 2012 R2 SPs mais recentes |64 bits |Standard, Datacenter, Foundation |
| Windows Server 2012 e SPs mais recentes |64 bits |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 e SPs mais recentes |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 e SPs mais recentes |64 bits |Standard, Workgroup |

Pode eliminar duplicados no armazenamento do DPM com a eliminação de duplicados do Windows Server. Saiba mais sobre como [DPM e eliminação de duplicados](https://technet.microsoft.com/library/dn891438.aspx) funcionam em conjunto, quando implementada nas VMs de Hyper-V.

> [!NOTE]
> O Azure Backup Server foi concebido para ser executado num servidor dedicado e especializado. Não é possível instalar o servidor de cópia de segurança do Azure em:
> - Um computador em execução como um controlador de domínio
> - Um computador no qual a função de Servidor de Aplicações está instalada
> - Um computador que é um servidor de gestão do System Center Operations Manager
> - Um computador no qual o Exchange Server está em execução
> - Um computador que é um nó de um cluster

Sempre associe o servidor de cópia de segurança do Azure a um domínio. Se planear mover o servidor para um domínio diferente, instalar o servidor de cópia de segurança do Azure pela primeira vez, em seguida, associe o servidor para o novo domínio. Mover uma máquina do servidor de cópia de segurança do Azure existente para um novo domínio após a implantação *nepodporuje*.

Se envia dados de cópia de segurança para o Azure ou mantê-lo localmente, o servidor de cópia de segurança do Azure tem de estar registado com um cofre dos serviços de recuperação.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Definir Replicação de Armazenamento
A opção de replicação de armazenamento permite-lhe escolher entre o armazenamento georredundante e o armazenamento localmente redundante. Por predefinição, os cofres dos serviços de recuperação utilizam o armazenamento georredundante. Se este cofre é o Cofre primário, deixe a opção de armazenamento definida para armazenamento georredundante. Escolha armazenamento localmente redundante se pretende uma opção mais barata que não é tão durável. Leia mais sobre as opções de armazenamento [georredundante](../storage/common/storage-redundancy-grs.md) e [localmente redundante](../storage/common/storage-redundancy-lrs.md) na [descrição geral da replicação de Armazenamento do Azure](../storage/common/storage-redundancy.md).

Para editar a definição de replicação de armazenamento:

1. Selecione o cofre para abrir o dashboard do cofre e o menu de definições. Se o **configurações** menu não abre, clique em **todas as definições** no dashboard do cofre.
2. Na **definições** menu, clique em **infraestrutura de cópia de segurança** > **configuração de cópia de segurança** para abrir o **configuração de cópia de segurança**painel. Sobre o **configuração de cópia de segurança** menu, escolha a opção de replicação de armazenamento para o cofre.

    ![Lista de cofres de cópia de segurança](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Após escolher a opção de armazenamento para o cofre, está pronto para associar a VM com o cofre. Para começar a associação, detete e registe as máquinas virtuais do Azure.

## <a name="software-package"></a>Pacote de software
### <a name="downloading-the-software-package"></a>Transferir o pacote de software
1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Se já tiver um cofre de serviços de recuperação aberto, avance para o passo 3. Se não tiver um cofre de serviços de recuperação aberto, mas estão no portal do Azure, no menu principal, clique em **procurar**.

   * Na lista de recursos, escreva **Serviços de Recuperação**.
   * À medida que começa a escrever, irá filtrar a lista com base na sua entrada. Quando vir **Cofres dos Serviços de Recuperação**, clique no mesmo.

     ![Passo 1 da Criação de um Cofre dos Serviços de Recuperação](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     É apresentada a lista dos cofres dos Serviços de Recuperação.
   * Na lista dos cofres dos Serviços de Recuperação, selecione um cofre.

     O dashboard do cofre selecionado é aberto.

     ![Abrir painel do cofre](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. O **definições** painel abre-se por predefinição. Se estiver fechado, clique em **definições** para abrir o painel de definições.

    ![Abrir painel do cofre](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Clique em **cópia de segurança** para abrir o Assistente de introdução.

    ![Cópia de segurança de introdução](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    Na **introdução à cópia de segurança** painel que aparece, **objetivos de cópia de segurança** será selecionada automaticamente.

    ![Aberto de cópia de segurança objetivos padrão](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. Na **objetivo de cópia de segurança** painel, da **em que a sua carga de trabalho é executado** menu, selecione **locais**.

    ![cargas de trabalho como objetivos e no local](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    Do **o que fazer quiser a cópia de segurança?** menu pendente, selecione as cargas de trabalho que pretende proteger com o Azure Backup Server e, em seguida, clique em **OK**.

    O **introdução à cópia de segurança** comutadores de assistente a **preparar a infraestrutura** opção para efetuar cópias de segurança de cargas de trabalho para o Azure.

   > [!NOTE]
   > Se pretender apenas fazer uma cópia de segurança de ficheiros e pastas, recomendamos utilizar o agente de cópia de segurança do Azure e seguir as orientações neste artigo, [primeiras impressões: efetuar cópias de segurança ficheiros e pastas](backup-try-azure-backup-in-10-mins.md). Se pretender proteger mais de ficheiros e pastas, ou se estiver a planear expandir as necessidades de proteção no futuro, selecione essas cargas de trabalho.
   >
   >

    ![Introdução a alteração do Assistente de introdução](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. Na **preparar infraestrutura** painel apresentado, clique no **transferir** ligações para instalar o Azure Backup Server e as credenciais do Cofre de Download. Utilize as credenciais do cofre durante o registo do servidor de cópia de segurança do Azure para o Cofre dos serviços de recuperação. As ligações levá-lo ao centro de Download, onde o pacote de software pode ser baixado.

    ![Preparar infraestrutura para o servidor de cópia de segurança do Azure](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Selecione todos os ficheiros e clique em **seguinte**. Transferir todos os ficheiros que chegam da página de download do Microsoft Azure Backup e coloque todos os ficheiros na mesma pasta.

    ![1 do Centro de transferências](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Uma vez que o tamanho do download de todos os arquivos em conjunto é > 3G, num 10 Mbps ligação de transferência pode demorar até 60 minutos para a transferência seja concluída.

### <a name="extracting-the-software-package"></a>Extrair o pacote de software
Depois de baixar todos os ficheiros, clique em **MicrosoftAzureBackupInstaller.exe**. Isso iniciará o **Assistente de configuração de cópia de segurança do Microsoft Azure** para extrair os ficheiros de configuração para uma localização especificada por si. Continuar através do assistente e clique nas **extrair** botão para iniciar o processo de extração.

> [!WARNING]
> Pelo menos 4GB de espaço livre é necessário para extrair os ficheiros de configuração.
>
>

![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Depois do processo de extração completo, marque a caixa para iniciar o recentemente extraídos *setup.exe* para começar a instalar o Microsoft Azure Backup Server e clique nas **concluir** botão.

### <a name="installing-the-software-package"></a>Instalar o pacote de software
1. Clique em **Microsoft Azure Backup** para iniciar o Assistente de configuração.

    ![Assistente de configuração de cópia de segurança do Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. No ecrã de boas-vindos, clique nas **seguinte** botão. Isto leva-o para o *verificações de pré-requisitos* secção. Neste ecrã, clique em **verificar** para determinar se foram cumpridos os pré-requisitos de hardware e software para o servidor de cópia de segurança do Azure. Se todos os pré-requisitos são cumpridos com êxito, verá uma mensagem a indicar que o computador cumpre os requisitos. Clique nas **seguinte** botão.

    ![Servidor de cópia de segurança do Azure - pré-requisitos e bem-vindo de verificação](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup Server requer o SQL Server Standard. Além disso, o pacote de instalação do servidor de cópia de segurança do Azure inclui integrados nos binários do SQL Server apropriados necessários se não pretender utilizar o seu próprio SQL. Ao iniciar com uma nova instalação do servidor de cópia de segurança do Azure, deve escolher a opção **instalar nova instância do SQL Server com esta configuração** e clique nas **verificar e instalar** botão. Assim que os pré-requisitos estão instalados com êxito, clique em **seguinte**.

    ![Servidor de cópia de segurança do Azure - verificação SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Se ocorrer uma falha com uma recomendação para reiniciar a máquina, fazê-lo e clique em **verificar novamente**.

   > [!NOTE]
   > O Azure Backup Server não irá funcionar com uma instância remota do SQL Server. A instância que está a ser utilizada pelo servidor de cópia de segurança do Azure tem de ser local.
   >
   >
4. Forneça uma localização para a instalação dos ficheiros do servidor de cópia de segurança do Microsoft Azure e clique em **seguinte**.

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    A localização de rascunho é um requisito para a cópia no Azure. Certifique-se de que a localização de rascunho é, pelo menos, 5% dos dados em planeadas a cópia de segurança para a cloud. Para a proteção de disco, discos separados tem de ser configurado depois de concluída a instalação. Para obter mais informações sobre agrupamentos de armazenamento, consulte [configurar agrupamentos de armazenamento e armazenamento no disco](https://technet.microsoft.com/library/hh758075.aspx).
5. Forneça uma palavra-passe forte para contas de usuário local restritas e clique em **seguinte**.

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Selecione se pretende utilizar *Microsoft Update* para verificar a existência de atualizações e clique em **próxima**.

   > [!NOTE]
   > Recomendamos ter a atualização do Windows redirecionar para o Microsoft Update, que oferece segurança e atualizações importantes para o Windows e outros produtos, como o Microsoft Azure Backup Server.
   >
   >

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Reveja os *resumo de definições* e clique em **instalar**.

    ![PreReq2 de cópia de segurança do Microsoft Azure](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. A instalação ocorre em fases. Na primeira fase, o agente de serviços de recuperação do Microsoft Azure está instalado no servidor. O assistente também verifica a existência de conectividade à Internet. Se a ligação à Internet pode continuar com a instalação; caso contrário, tem de fornecer detalhes do proxy para estabelecer ligação à Internet.

    A próxima etapa é configurar o agente de serviços de recuperação do Microsoft Azure. Como parte da configuração, terá de fornecer suas credenciais do cofre para registar a máquina para o Cofre dos serviços de recuperação. Também deve fornecer uma frase de acesso para encriptar/desencriptar os dados enviados entre o Azure e o local. Automaticamente pode gerar uma frase de acesso ou fornecer seu próprio frase de acesso mínimo 16 carateres. Continue o assistente até que o agente foi configurado.

    ![PreReq2 Server de cópia de segurança do Azure](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Depois de concluir com êxito o registo do servidor de cópia de segurança do Microsoft Azure, o Assistente de configuração geral prossegue para a instalação e configuração do SQL Server e os componentes de servidor de cópia de segurança do Azure. Depois de concluída a instalação de componentes do SQL Server, os componentes de servidor de cópia de segurança do Azure estão instalados.

    ![Servidor do Backup do Azure](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Quando tiver concluído o passo de instalação, ícones de área de trabalho do produto foi criados também. Basta clicar duas vezes no ícone para iniciar o produto.

### <a name="add-backup-storage"></a>Adicionar armazenamento de cópia de segurança
A primeira cópia de segurança é mantida no armazenamento ligado à máquina do servidor de cópia de segurança do Azure. Para obter mais informações sobre como adicionar discos, consulte [configurar agrupamentos de armazenamento e armazenamento no disco](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Tem de adicionar armazenamento de cópia de segurança, mesmo que pretenda enviar dados para o Azure. A arquitetura atual do servidor de cópia de segurança do Azure, o Cofre de cópias de segurança do Azure mantém os *segundo* cópia dos dados enquanto o armazenamento local contém a cópia de segurança do primeira (e obrigatória).
>
>

## <a name="network-connectivity"></a>Conectividade de rede
O Azure Backup Server precisa de conectividade para o serviço de cópia de segurança do Azure para o produto a funcionar com êxito. Para validar se o computador tem conectividade para o Azure, utilize o ```Get-DPMCloudConnection``` cmdlet na consola do PowerShell de servidor de cópia de segurança do Azure. Se a saída do cmdlet for TRUE, em seguida, existe a conectividade, caso contrário não existe nenhuma conectividade.

Ao mesmo tempo, a subscrição do Azure tem de estar em bom estado de funcionamento. Para obter informações sobre o estado da sua subscrição e geri-lo, inicie sessão para o [portal de subscrições](https://account.windowsazure.com/Subscriptions).

Sabe o estado da conetividade do Azure e da subscrição do Azure, pode utilizar a tabela abaixo para obter informações sobre o impacto sobre a funcionalidade de cópia de segurança/restauro fornecida.

| Estado de conectividade | Subscrição do Azure | Criar uma cópia de segurança do Azure | Criar cópias de segurança em disco | Restaurar a partir do Azure | Restaurar a partir de disco |
| --- | --- | --- | --- | --- | --- |
| Ligado |Ativa |Permitido |Permitido |Permitido |Permitido |
| Ligado |Fora do prazo |Parada |Parada |Permitido |Permitido |
| Ligado |Desaprovisionamento |Parada |Parada |Pontos de recuperação de parada e do Azure eliminados |Parada |
| Perda de conectividade > 15 dias |Ativa |Parada |Parada |Permitido |Permitido |
| Perda de conectividade > 15 dias |Fora do prazo |Parada |Parada |Permitido |Permitido |
| Perda de conectividade > 15 dias |Desaprovisionamento |Parada |Parada |Pontos de recuperação de parada e do Azure eliminados |Parada |

### <a name="recovering-from-loss-of-connectivity"></a>Recuperar da perda de conectividade
Se tiver uma firewall ou um proxy que está a impedir o acesso para o Azure, terá de lista de permissões os seguintes endereços de domínio no perfil de firewall/proxy:

* `http://www.msftncsi.com/ncsi.txt`
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Depois de conectividade para o Azure foi restaurada para a máquina do servidor de cópia de segurança do Azure, as operações que podem ser executadas são determinadas pelo Estado de subscrição do Azure. A tabela acima tem detalhes sobre as operações permitidas uma vez que a máquina for "ligada".

### <a name="handling-subscription-states"></a>Manipulação de Estados de subscrição
É possível tirar uma subscrição do Azure a partir de um *expirado* ou *desaprovisionada* estado para o *Active* estado. No entanto isso tem algumas implicações no comportamento do produto, enquanto o estado não é *Active Directory*:

* R *desaprovisionada* subscrição perde a funcionalidade para o período em que é desaprovisionada. No decisivo *Active Directory*, a funcionalidade de produto de cópia de segurança/restauro é reativada. Os dados de cópia de segurança no disco local também podem ser obtidos se ele foi mantido com um período de retenção suficientemente grande. No entanto, os dados de cópia de segurança no Azure são irremediavelmente perdidos depois de insere a subscrição do *desaprovisionada* estado.
* Uma *expirado* subscrição apenas perde a funcionalidade para até que ele se tornou *Active Directory* novamente. Quaisquer cópias de segurança agendadas para o período de que a subscrição foi *expirado* não será executado.

## <a name="troubleshooting"></a>Resolução de problemas
Se o Microsoft Azure Backup server falhar com erros durante a fase de configuração (ou cópia de segurança ou restauro), consulte este [documento de códigos de erro](https://support.microsoft.com/kb/3041338) para obter mais informações.
Também pode consultar [FAQs relacionados com a cópia de segurança do Azure](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Passos Seguintes
Pode obter informações detalhadas [preparar o ambiente para o DPM](https://technet.microsoft.com/library/hh758176.aspx) no site do Microsoft TechNet. Também contém informações sobre configurações suportadas em que servidor de cópia de segurança do Azure podem ser implementado e utilizado.

Pode utilizar estes artigos para obter uma compreensão mais aprofundada de proteção de carga de trabalho usando o Microsoft Azure Backup server.

* [Cópia de segurança do SQL Server](backup-azure-backup-sql.md)
* [Cópia de segurança do SharePoint server](backup-azure-backup-sharepoint.md)
* [Cópia de segurança do servidor alternativo](backup-azure-alternate-dpm-server.md)
