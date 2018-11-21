---
title: 'Cópia de segurança do Azure: Preparar para fazer uma cópia de segurança de máquinas virtuais'
description: Certifique-se de que o seu ambiente está preparado para fazer backup de máquinas virtuais no Azure.
services: backup
author: rayne-wiselman
manager: carmonm
keywords: cópias de segurança; cópias de segurança;
ms.service: backup
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: 086399f669b704a0ae2c9f719906e7efa672b5b1
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262514"
---
# <a name="prepare-to-back-up-azure-vms"></a>Preparar a cópia de segurança de VMs do Azure

Este artigo fornece os passos para preparar o ambiente para fazer uma cópia de segurança de uma máquina de virtual (VM) implementadas no Azure Resource Manager. Os passos apresentados nos procedimentos de utilizam o portal do Azure. Quando cria cópias de segurança de uma máquina virtual, os dados de cópia de segurança ou os pontos de recuperação, são armazenados num cofre de cópia de segurança do Recovery Services.



Antes de proteger (ou criar cópias de segurança) uma máquina de virtual implementadas no Resource Manager, certifique-se estes pré-requisitos existem:

* Crie ou identifique um cofre dos serviços de recuperação *na mesma região que a máquina virtual*.
* Selecionar um cenário, definir a política de cópia de segurança e definir os itens para proteger.
* Verificar a instalação de um agente de VM (extensão) na máquina virtual.
* Verifique a conectividade de rede.
* Para VMs do Linux, se desejar personalizar o seu ambiente de cópia de segurança para cópias de segurança consistentes com aplicações, siga os [passos para configurar os scripts anterior ao instantâneo e posterior ao instantâneo](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Se estas condições já existem no seu ambiente, avance para o [cópia de segurança as suas VMs](backup-azure-arm-vms.md) artigo. Se precisar de configurar ou verificar qualquer um destes pré-requisitos, este artigo orienta-o através dos passos.

## <a name="supported-operating-systems-for-backup"></a>Sistemas operativos suportados para a cópia de segurança

 * **Linux**: cópia de segurança do Azure suporta [uma lista de distribuições Azure endossa](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), exceto o CoreOS Linux e do sistema de operativo de 32 bits. Para obter a lista dos sistemas operativos Linux que suporta o restauro de ficheiros, consulte [recuperar ficheiros a partir de cópia de segurança da máquina virtual](backup-azure-restore-files-from-vm.md#for-linux-os).

    > [!NOTE]
    > Outras distribuições de bring-your-own-Linux podem funcionar, desde que o agente da VM está disponível na máquina virtual e haja suporte para Python. No entanto, esses distribuições não são suportadas.
    >
 * **Windows Server**, **cliente Windows**: as versões mais antigas do que o Windows Server 2008 R2 ou Windows 7, não são suportadas.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Limitações ao fazer backup e restaurar uma VM
Antes de preparar o seu ambiente, certifique-se de que compreender estas limitações:

* Cópias de máquinas virtuais com mais de 16 discos de dados não é suportada.
* Não é suportada para o backup das VMs de Linux encriptado através da encriptação de Linux Unified chave configuração (LUKS).
* Não recomendamos que o backup das VMs que contêm a configuração de Volumes de partilhado de Cluster (CSV) ou o servidor de ficheiros de escalamento horizontal. Se feito, espera-se a falha de escritores CSV. Eles exigem envolvendo todas as VMs incluídas na configuração do cluster durante uma tarefa de instantâneo. O Azure Backup não suporta a consistência de várias VMS.
* Dados de cópia de segurança não incluem unidades de rede montado anexadas a uma VM.
* A substituição de uma máquina virtual existente durante o restauro não é suportada. Se tentar restaurar a VM quando a VM existe, a operação de restauro falhará.
* Entre regiões criar cópias de segurança e restauro não são suportadas.
* Ao configurar o back cópia de segurança, certifique-se de que o **Firewalls e redes virtuais** as definições de conta de armazenamento permitirem o acesso de todas as redes.
* Para redes selecionadas, depois de configurar a firewall e as definições de rede virtual para a sua conta de armazenamento, selecione **permitir confiável a serviços da Microsoft para aceder a esta conta de armazenamento** como uma exceção para ativar o serviço de cópia de segurança do Azure Aceda à conta de armazenamento de restrição de rede. Recuperação ao nível do item não é suportada para contas de armazenamento de restrição de rede.
* Pode fazer uma cópia de segurança de máquinas virtuais em todas as regiões públicas do Azure. (Consulte a [lista de verificação](https://azure.microsoft.com/regions/#services) de regiões suportadas.) Se a região na qual está procurando não é suportada atualmente, não será apresentada na lista pendente durante a criação do cofre.
* Restaurar um controlador de domínio (DC) VM que faça parte de uma configuração de multi-DC é suportada apenas através do PowerShell. Para obter mais informações, consulte [restaurar um controlador de domínio do controlador de domínio com várias](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* O instantâneo do disco de acelerador de escrita ativados não é suportado. Esta restrição bloqueia a capacidade do serviço de cópia de segurança do Azure para efetuar um instantâneo consistente da aplicação de todos os discos da máquina virtual.
* Restaurar máquinas virtuais que têm as seguintes configurações de rede especiais é suportada apenas através do PowerShell. As VMs criadas no fluxo de trabalho de restauro na interface do Usuário não terão estas configurações de rede depois de concluída a operação de restauro. Para obter mais informações, consulte [restaurar VMs com configurações de rede especiais](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Máquinas virtuais em configuração de Balanceador de carga (interna e externa)
  * Máquinas virtuais com vários endereços IP reservados
  * Máquinas virtuais com vários adaptadores de rede

  > [!NOTE]
  > O Azure Backup suporta [Standard SSD Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/), um novo tipo de armazenamento durável para máquinas de virtuais do Microsoft Azure. É suportado para discos geridos num [pilha de cópia de segurança do Azure VM V2](backup-upgrade-to-vm-backup-stack-v2.md).

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Criar um cofre dos serviços de recuperação para uma VM
Um cofre dos serviços de recuperação é uma entidade que armazena as cópias de segurança e os pontos de recuperação que foram criados ao longo do tempo. O Cofre dos serviços de recuperação também contém as políticas de cópia de segurança que estão associadas a máquinas virtuais protegidas.

Para criar um cofre dos Serviços de Recuperação:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
1. Sobre o **Hub** menu, selecione **procurar**e, em seguida, escreva **serviços de recuperação**. Quando começar a escrever, sua entrada filtra a lista de recursos. Selecione **cofres dos serviços de recuperação**.

    ![Escrever na caixa e selecionando "Cofres de serviços de recuperação" nos resultados](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    É apresentada a lista dos cofres dos Serviços de Recuperação.
1. Sobre o **cofres dos serviços de recuperação** menu, selecione **Add**.

    ![Passo 2 da Criação do Cofre dos Serviços de Recuperação](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    O **cofres dos serviços de recuperação** painel abre-se. Pede-lhe para fornecer informações de **Name**, **subscrição**, **grupo de recursos**, e **localização**.

    ![Painel "Cofres dos serviços de recuperação"](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
1. Para o **Nome**, introduza um nome amigável para identificar o cofre. O nome tem de ser exclusivo para a subscrição do Azure. Escreva um nome que contenha 2 e 50 carateres. Ele tem de começar com uma letra e pode conter apenas letras, números e hífenes.
1. Selecione **subscrição** para ver a lista de subscrições disponíveis. Se não tiver a certeza de que subscrição utilizar, utilize a predefinição (ou sugerida) subscrição. Existem várias opções apenas se for seu trabalho ou a conta da instituição de ensino está associada a várias subscrições do Azure.
1. Selecione **grupo de recursos** para ver a lista de grupos de recursos disponíveis ou selecione **New** para criar um novo grupo de recursos. Para obter informações completas sobre grupos de recursos, consulte [descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
1. Selecione **localização** para selecionar a região geográfica do cofre. O cofre *tem de* estar na mesma região que as máquinas virtuais que pretende proteger.

   > [!IMPORTANT]
   > Se tiver dúvidas sobre a localização em que se encontra a VM, feche a caixa de diálogo de criação do cofre e aceda à lista de máquinas virtuais no portal. Se tiver máquinas virtuais em várias regiões, terá de criar um cofre dos serviços de recuperação em cada região. Crie o cofre na primeira localização antes de ir para a localização seguinte. Não é necessário especificar contas de armazenamento para armazenar os dados de cópia de segurança. O Cofre dos serviços de recuperação e o serviço de cópia de segurança do Azure processam que automaticamente.
   >
   >

1. Selecione **Criar**. Pode demorar algum tempo até que o cofre dos Serviços de Recuperação seja criado. Monitorize as notificações de estado na área de canto superior direito do portal. Depois do cofre for criado, aparece na lista de cofres dos serviços de recuperação. Se não vir o cofre, selecione **atualizar**.

    ![Lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Agora que criou o cofre, saiba como configurar a replicação de armazenamento.

## <a name="set-storage-replication"></a>Replicação de armazenamento do conjunto
A opção de replicação de armazenamento permite-lhe escolher entre o armazenamento com redundância geográfica e armazenamento localmente redundante. Por predefinição, o seu cofre tem um armazenamento georredundante. Deixe o valor de opção como armazenamento com redundância geográfica para a cópia de segurança primária. Se pretender que uma opção mais barata que não é tão durável, escolha o armazenamento localmente redundante.

Para editar a definição de replicação de armazenamento:

1. Sobre o **cofres dos serviços de recuperação** painel, selecione o cofre.
    Quando seleciona o cofre, o **definições** painel (que tem o nome do cofre na parte superior) e o painel de detalhes do cofre, aberto.

   ![Escolha o Cofre na lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

1. Na **definições** painel, utilize o controlo de deslize vertical de se deslocar para baixo até o **gerir** secção e selecione **infraestrutura de cópia de segurança**. Na **gerais** secção, selecione **configuração de cópia de segurança**. Sobre o **configuração de cópia de segurança** painel, escolha a opção de replicação de armazenamento para o cofre. Por predefinição, o seu cofre tem um armazenamento georredundante.

   ![Lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Se estiver a utilizar o Azure como um ponto de final de armazenamento de cópia de segurança primário, continue a utilizar o armazenamento georredundante. Se estiver a utilizar o Azure como um ponto de final de armazenamento de cópias de segurança não primário, escolha o armazenamento localmente redundante. Saiba mais sobre as opções de armazenamento na [descrição geral da replicação de armazenamento do Azure](../storage/common/storage-redundancy.md).

1. Se tiver alterado o tipo de replicação de armazenamento, selecione **guardar**.

Depois de escolher a opção de armazenamento para o cofre, está pronto para associar a VM com o cofre. Para começar a associação, detete e registe as máquinas virtuais do Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Selecionar um objetivo de cópia de segurança, configure a política e definir os itens para proteger
Antes de registar uma máquina virtual com um cofre dos serviços de recuperação, execute o processo de deteção para identificar as novas máquinas de virtuais incluídas na assinatura. O processo de deteção de consulta do Azure para obter a lista de máquinas virtuais na subscrição. Se forem encontradas novas máquinas virtuais, o portal apresenta o nome do serviço cloud e a região associada. No portal do Azure, o *cenário* é o que pretende introduzir no cofre dos serviços de recuperação. *Política* é a agenda da frequência e quando os pontos de recuperação são obtidos. A política também inclui o período de retenção para os pontos de recuperação.

1. Se já tiver um cofre dos Serviços de Recuperação aberto, avance para o passo 2. Se não tiver um cofre de serviços de recuperação aberto, abra a [portal do Azure](https://portal.azure.com/). Sobre o **Hub** menu, selecione **mais serviços**.

   a. Na lista de recursos, escreva **Serviços de Recuperação**. Quando começar a escrever, sua entrada filtra a lista. Quando vir **cofres dos serviços de recuperação**, selecioná-lo.

      ![Escrever na caixa e selecionando "Cofres de serviços de recuperação" nos resultados](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      É apresentada a lista dos cofres dos Serviços de Recuperação. Se não existirem não existem cofres na sua subscrição, esta lista está vazia.

      ![Vista da lista dos cofres dos Serviços de Recuperação](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. Na lista dos cofres dos Serviços de Recuperação, selecione um cofre.

      O **definições** painel e o dashboard do cofre para o escolhido cofre aberto.

      ![Dashboard de painel e o Cofre de definições](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
1. No menu do dashboard do cofre, selecione **cópia de segurança**.

   ![Botão de cópia de segurança](./media/backup-azure-arm-vms-prepare/backup-button.png)

   O **cópia de segurança** e **objetivo de cópia de segurança** painéis abrir.

1. Na **objetivo de cópia de segurança** painel, defina **em que a sua carga de trabalho é executado?** como **Azure** e **o que fazer quiser a cópia de segurança?** como  **Máquina virtual**. Em seguida, selecione **OK**.

   ![Painéis de cópia de segurança e objetivo de cópia de segurança](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Este passo registra a extensão de VM com o cofre. O **objetivo de cópia de segurança** fecha o painel e o **política de cópia de segurança** painel abre-se.

   !["Cópia de segurança" e "Política de cópia de segurança" painéis](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
1. Sobre o **política de cópia de segurança** painel, selecione a política de cópia de segurança que pretende aplicar ao cofre.

   ![Selecionar política de cópia de segurança](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   Os detalhes da política predefinida estão listados no menu pendente. Se pretende criar uma nova política, selecione **Criar Novo** no menu pendente. Para obter instruções sobre como definir uma política de cópia de segurança, consulte o artigo [Definir uma política de cópia de segurança](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Selecione **OK** para associar a política de cópia de segurança no cofre.

   O **política de cópia de segurança** fecha o painel e o **selecionar máquinas virtuais** painel abre-se.
1. Sobre o **selecionar máquinas virtuais** painel, escolha as máquinas virtuais a associar à política especificada e selecione **OK**.

   ![Painel "Selecionar máquinas virtuais"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   A máquina virtual selecionada é validada. Se não vir as máquinas virtuais esperadas, verifique que as máquinas virtuais estão na mesma região do Azure que o Cofre de serviços de recuperação. Se não vir as máquinas virtuais, verifique que eles já não estejam protegidos com outro cofre. Dashboard do cofre mostra a região onde existe o Cofre dos serviços de recuperação.

1. Agora que definiu todas as definições do cofre, o **cópia de segurança** painel, selecione **ativar cópia de segurança**. Este passo implementa a política no cofre e as VMs. Este passo não cria o ponto de recuperação inicial da máquina virtual.

   ![Botão "Ativar cópia de segurança"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Depois de ativar a cópia de segurança com êxito, a política de cópia de segurança será executada numa agenda. Se quiser gerar uma tarefa de cópia de segurança a pedido para efetuar cópias de segurança as máquinas virtuais agora, ver [acionar a tarefa de cópia de segurança](./backup-azure-vms-first-look-arm.md#initial-backup).

Se tiver problemas a registar a máquina virtual, consulte as seguintes informações sobre como instalar o agente da VM e na conectividade de rede. Provavelmente não precisará as seguintes informações se estiver a proteger máquinas virtuais criadas no Azure. Mas se tiver migrado máquinas virtuais para o Azure, certifique-se de que instalou corretamente o agente da VM e que sua máquina virtual podem comunicar com a rede virtual.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Instalar o agente da VM na máquina virtual
Para a extensão de cópia de segurança trabalhar, do Azure [agente da VM](../virtual-machines/extensions/agent-windows.md) tem de estar instalado na máquina virtual do Azure. Se a VM foi criada no Azure Marketplace, o agente da VM já se encontra presente na máquina virtual.

As seguintes informações são fornecidas para situações onde está *não* através de uma VM criada no Azure Marketplace. **Por exemplo, migrou uma VM de um datacenter no local. Nesse caso, o agente da VM tem de ser instalado para poder proteger a máquina virtual.**

**Tenha em atenção**: depois de instalar o agente da VM, também tem de utilizar Azure PowerShell para atualizar a propriedade de ProvisionGuestAgent, portanto, significa que o Azure a VM tem o agente instalado.

Se tiver problemas, fazer backup da VM do Azure, utilize a tabela seguinte para verificar que o agente de VM do Azure está corretamente instalado na máquina virtual. A tabela fornece informações adicionais sobre o agente da VM para Windows e VMs do Linux.

| **Operação** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalar o Agente da VM |Transfira e instale o [MSI do agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Necessita de privilégios de Administrador para concluir o processo de instalação. |<li> Instalar a versão mais recente [agente Linux](../virtual-machines/extensions/agent-linux.md). Necessita de privilégios de Administrador para concluir o processo de instalação. Recomendamos que instale o agente do seu repositório de distribuição. Estamos **não é recomendável** instalar agente de VM com Linux diretamente a partir do github.  |
| Atualizar o Agente da VM |A atualização do Agente da VM é tão simples como reinstalar os [binários do Agente da VM](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Certifique-se de que nenhuma operação de cópia de segurança está em execução enquanto o agente da VM está a ser atualizado. |Siga as instruções para [atualizar o Agente da VM do Linux ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Recomendamos que Atualize o agente a partir do seu repositório de distribuição. Estamos **não é recomendável** atualizar agente de VM com Linux diretamente a partir do github.<br>Certifique-se de que nenhuma operação de cópia de segurança está em execução enquanto o Agente da VM está a ser atualizado. |
| A validar a instalação do Agente da VM |<li>Navegue até à pasta *C:\WindowsAzure\Packages* na VM do Azure. <li>Deve encontrar o ficheiro WaAppAgent.exe presente.<li> Clique com o botão direito do rato no ficheiro, aceda a **Propriedades** e, em seguida, selecione o separador **Detalhes**. O campo da Versão do Produto deve ser 2.6.1198.718 ou superior. |N/A |

### <a name="backup-extension"></a>Extensão da cópia de segurança
Depois do agente da VM está instalado na máquina virtual, o serviço de cópia de segurança do Azure instala a extensão de cópia de segurança para o agente da VM. O serviço de cópia de segurança é atualizado de forma totalmente integrada e patches a extensão de cópia de segurança.

O serviço de cópia de segurança instala a extensão de cópia de segurança, independentemente da VM está em execução. Uma VM em execução fornece a maior possibilidade de obter um ponto de recuperação consistente com aplicações. No entanto, o serviço de cópia de segurança continua a fazer cópias de segurança da VM mesmo se ele é desativado e não foi possível instalar a extensão. Isso é conhecido como *offline VM*. Neste caso, o ponto de recuperação será *consistente com a falha*.

## <a name="establish-network-connectivity"></a>Estabelecer conectividade de rede
Para gerir os instantâneos VM, a extensão de cópia de segurança precisa de conectividade para os endereços IP públicos do Azure. Sem conectividade de internet adequada, tempo limite de pedidos de HTTP da máquina virtual e a operação de cópia de segurança falhará. Se a sua implementação tem restrições de acesso no local – através de um grupo de segurança de rede (NSG), por exemplo – escolha uma destas opções para fornecer um caminho claro para o tráfego de cópia de segurança:

* [Intervalos de IP do datacenter do Azure de lista branca](https://www.microsoft.com/download/details.aspx?id=41653).
* Implemente um servidor de proxy HTTP para o encaminhamento de tráfego.

Quando estiver a decidir qual a opção para utilizar, são os compromissos entre a capacidade de gerenciamento, um controle granular e custo.

| Opção | Vantagens | Desvantagens |
| --- | --- | --- |
| Intervalos de IP da lista branca |Sem custos adicionais.<br><br>Para abrir o acesso num NSG, utilize o **Set-AzureNetworkSecurityRule** cmdlet. |Complexo de gerir o afetado como intervalos de IP mudam ao longo do tempo.<br><br>Fornece acesso a todo do Azure e não apenas armazenamento. |
| Utilizar um proxy de HTTP |Um controle granular no proxy sobre o armazenamento é permitida a URLs.<br><br>Único ponto de acesso à internet para VMs.<br><br>Não sujeitas a alterações ao endereço IP do Azure. |Custos adicionais para executar uma VM com o software de proxy. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Intervalos de IP da lista de permissões do datacenter do Azure
A lista de permissões os intervalos IP do datacenter do Azure, consulte a [Web site do Azure](https://www.microsoft.com/download/details.aspx?id=41653) para obter detalhes sobre os intervalos de IP e as instruções.

Pode permitir ligações para o armazenamento da região específica, utilizando [etiquetas de serviço](../virtual-network/security-overview.md#service-tags). Certifique-se de que a regra que permite o acesso à conta de armazenamento tem uma prioridade mais alta do que a regra que bloqueia o acesso à internet.

![NSG com marcas de armazenamento para uma região](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

O vídeo seguinte orienta-o procedimento passo a passo para configurar as etiquetas de serviço:

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!NOTE]
> Para obter uma lista de etiquetas de serviço de armazenamento e regiões, consulte [etiquetas de serviço para o armazenamento](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>Utilizar um proxy HTTP para cópias de segurança VM
Quando estiver fazendo uma VM, a extensão de cópia de segurança na VM envia os comandos de gestão do instantâneo ao armazenamento do Azure com uma API de HTTPS. Encaminhe o tráfego de extensão de cópia de segurança através do proxy HTTP, porque é o único componente configurado para acesso à Internet pública.

> [!NOTE]
> Não recomendamos que o software de proxy específico que deve usar. Certifique-se de que escolhe um proxy que é compatível com os passos de configuração que se seguem.
>
>

A imagem de exemplo seguinte mostra os passos de configuração de três necessárias para utilizar um proxy de HTTP:

* As rotas VM de aplicação associado de todo o tráfego HTTP para a internet pública através do proxy de VM.
* O proxy de VM permite o tráfego de entrada de VMs na rede virtual.
* O grupo de segurança de rede com o nome NSF bloqueio tem uma regra de segurança que permite o tráfego de internet de saída do proxy VM.

Para utilizar um proxy HTTP para comunicar com a internet pública, conclua os passos seguintes.

> [!NOTE]
> Estes passos utilizam valores e nomes específicos para esse exemplo. Quando estiver inserindo (ou colar) detalhes em seu código, utilize os nomes e valores para a sua implementação.

#### <a name="step-1-configure-outgoing-network-connections"></a>Passo 1: Configurar ligações de rede de saída
###### <a name="for-windows-machines"></a>Para máquinas Windows
Este procedimento configura a configuração do servidor proxy para a conta sistema local.

1. Baixe [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
1. Abra o Internet Explorer, executando o seguinte comando a partir de uma linha de comandos elevada:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

1. No Internet Explorer, aceda a **ferramentas** > **opções da Internet** > **ligações** > **definições de LAN**.
1. Verifique as definições de proxy para a conta de sistema. Defina o IP de proxy e a porta.
1. Feche o Internet Explorer.

O script a seguir configura uma configuração de proxy de todo o computador e utiliza-a qualquer tráfego de saída HTTP ou HTTPS. Se tiver configurado um servidor de proxy numa conta de utilizador atual (e não uma conta do sistema local), utilize este script para aplicá-las para SYSTEMACCOUNT.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Se observar "(407) autenticação do Proxy necessária" no registo de servidor de proxy, verifique que a autenticação está corretamente configurada.
>
>

###### <a name="for-linux-machines"></a>Para máquinas do Linux
Adicione a seguinte linha para o ```/etc/environment``` ficheiro:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Adicione as seguintes linhas para o ```/etc/waagent.conf``` ficheiro:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Passo 2: Permitir ligações de entrada no servidor de proxy
1. No servidor de proxy, abra a Firewall do Windows. A forma mais fácil de acessar o firewall consiste em Procurar **Firewall do Windows com segurança avançada**.
1. Na **Firewall do Windows com segurança avançada** caixa de diálogo, clique com botão direito **regras de entrada** e selecione **nova regra**.
1. No Assistente de nova entrada regra, sobre o **tipo de regra** página, selecione o **personalizada** opção e selecione **seguinte**.
1. Sobre o **programa** página, selecione **todos os programas** e selecione **seguinte**.
1. Sobre o **protocolo e portas** página, introduza as seguintes informações e selecione **próxima**:
   * Para **tipo de protocolo**, selecione **TCP**.
   * Para **Porta Local**, selecione **portas específicas**. Na caixa seguinte, especifique o número da porta de proxy que foi configurado.
   * Para **porta remota**, selecione **todas as portas**.

Para o restante do assistente, aceite as predefinições, até chegar ao fim. Em seguida, atribua esta regra de um nome.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Passo 3: Adicionar uma regra de exceção para o NSG
O comando seguinte adiciona uma exceção ao NSG. Essa exceção permite tráfego de TCP de qualquer porta no 10.0.0.5 para qualquer endereço internet na porta 80 (HTTP) ou 443 (HTTPS). Se necessitar de uma porta específica na internet pública, certifique-se de que adicionar essa porta para ```-DestinationPortRange```.

Numa linha de comandos do Azure PowerShell, introduza o seguinte comando:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Tem dúvidas?
Se tiver dúvidas ou se houver alguma funcionalidade que pretende ver incluída, [envie-nos comentários](https://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Passos Seguintes
Agora que a preparar o ambiente para fazer backup de sua VM, a próxima etapa lógica é criar uma cópia de segurança. O planeamento artigo fornece informações mais detalhadas sobre o backup das VMs.

* [Fazer uma cópia de segurança das máquinas virtuais](backup-azure-arm-vms.md)
* [Planear a sua infraestrutura de cópia de segurança de VM](backup-azure-vms-introduction.md)
* [Gerir cópias de segurança da máquina virtual](backup-azure-manage-vms.md)
