---
title: "Cópia de segurança do Azure: Preparar a cópia de segurança de máquinas virtuais | Microsoft Docs"
description: "Certifique-se de que o seu ambiente está preparado para fazer uma cópia de segurança de máquinas virtuais no Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "cópias de segurança; cópia de segurança;"
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/21/2017
ms.author: markgal;trinadhk;sogup;
ms.openlocfilehash: 568509eba47facfc5966d06dff5a1b32dce1008f
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Preparar o seu ambiente para fazer cópias de segurança de máquinas virtuais implementadas com o Resource Manager

Este artigo fornece os passos para preparar o ambiente para fazer uma cópia de segurança de uma máquina de virtual (VM) implementado o Azure Resource Manager. Os passos apresentados nos procedimentos utilizam o portal do Azure.  

O serviço de cópia de segurança do Azure tem dois tipos de cofres para proteger as suas VMs: cofres de cópia de segurança e os cofres dos serviços de recuperação. Um cofre de cópia de segurança ajuda a proteger as VMs implementadas através do modelo de implementação clássica. Um cofre dos serviços de recuperação ajuda a proteger *VMs implementadas clássico tanto implementadas no Resource Manager*. Tem de utilizar um cofre dos serviços de recuperação se pretender proteger uma VM implementadas no Resource Manager.

> [!NOTE]
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e clássico](../azure-resource-manager/resource-manager-deployment-model.md).

Antes de poder proteger ou fazer uma cópia de segurança de uma máquina de virtual implementadas no Resource Manager, certifique-se de que estes pré-requisitos existem:

* Criar um cofre dos serviços de recuperação (ou identificar um cofre dos serviços de recuperação existente) *na mesma localização que a VM*.
* Selecionar um cenário, definir a política de cópia de segurança e definir os itens para proteger.
* Verifique a instalação de um agente de VM na máquina virtual.
* Verifique a conectividade de rede.
* Para VMs com Linux, se pretender personalizar o seu ambiente de cópia de segurança para cópias de segurança consistentes com aplicações, siga o [passos para configurar scripts anterior ao instantâneo e posterior ao instantâneo](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Se estas condições já existem no seu ambiente, avance para o [cópia de segurança as suas VMs](backup-azure-arm-vms.md) artigo. Se precisar de configurar ou qualquer um destes pré-requisitos verificar, este artigo orienta-o através dos passos.

## <a name="supported-operating-systems-for-backup"></a>Sistemas operativos suportados para cópia de segurança
 * **Linux**: cópia de segurança do Azure suporta [uma lista das distribuições Azure patrocina](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), exceto CoreOS Linux. 
 
    > [!NOTE] 
    > Outras bring-your-proprietário-as distribuições do Linux podem funcionar, desde que o agente VM está disponível na máquina virtual e suporte para o Python existe. No entanto, não podemos apoia esses distribuições para cópia de segurança.
 * **Windows Server**: não são suportadas as versões mais antigas do que o Windows Server 2008 R2.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Limitações ao fazer cópias de segurança e restauro de uma VM
Antes de preparar o seu ambiente, é necessário compreender estas limitações:

* Não é suportada a cópia de segurança de máquinas virtuais com mais de 16 discos de dados.
* Não é suportada a cópia de segurança das máquinas virtuais com dados tamanhos de disco superiores 1,023 GB.

  > [!NOTE]
  > Temos uma versão de pré-visualização privada para suportar cópias de segurança para VMs com discos > 1TB. Para obter detalhes, consulte [pré-visualização privada para o suporte de cópia de segurança de VM de disco grande](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a).
  >

* Não é suportada a cópia de segurança de máquinas virtuais com um endereço IP reservado e nenhum ponto final de definidos.
* Não é suportada a cópia de segurança de VMs encriptadas através de apenas uma BitLocker chave de encriptação (BEK). Não é suportada a cópia de segurança de VMs com Linux encriptados através de encriptação de Linux Unified chave configuração (LUKS).
* Não recomendamos a cópia de segurança de VMs que contém a configuração de Volumes Partilhados de Cluster (CSV) ou servidor de ficheiros de escalamento horizontal. Necessitam que envolvem todas as VMs incluídas na configuração do cluster durante a uma tarefa de instantâneo. A consistência multi VM não suporta a cópia de segurança do Azure. 
* Dados de cópia de segurança não incluem unidades de rede montado ligadas a uma VM.
* A substituição de uma máquina virtual existente durante o restauro não é suportada. Se tentar restaurar a VM quando existe a VM, a operação de restauro irá falhar.
* Não são suportados por várias regiões cópia de segurança e restauro.
* Cópia de segurança e restauro de máquinas virtuais utilizando discos não geridos em contas de armazenamento com regras de rede aplicadas atualmente não é suportada. Ao configurar a cópia de segurança, certifique-se de que as definições de "Firewalls e redes virtuais" para a conta de armazenamento permitem o acesso a partir de "Todas as redes".
* Pode criar cópias de segurança máquinas virtuais em todas as regiões públicas do Azure. (Consulte o [lista de verificação](https://azure.microsoft.com/regions/#services) de regiões suportadas.) Se a região que procura não é suportada atualmente, não serão apresentados na lista pendente durante a criação do cofre.
* Restaurar um controlador de domínio (DC) VM que faz parte de uma configuração de várias DC é suportada apenas através do PowerShell. Para obter mais informações, consulte [restaurar um controlador de domínio do DC várias](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Máquinas virtuais que têm as seguintes configurações de rede especiais o restauro é suportado apenas através do PowerShell. VMs criadas através do fluxo de trabalho de restauro na IU não terá estas configurações de rede após a conclusão da operação de restauro. Para obter mais informações, consulte [restaurar VMs com configurações de rede especiais](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Máquinas virtuais em configuração de Balanceador de carga (interna e externa)
  * Máquinas virtuais com vários endereços IP reservados
  * Máquinas virtuais com vários adaptadores de rede

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Criar um cofre dos serviços de recuperação para uma VM
Um cofre dos serviços de recuperação é uma entidade que armazena as cópias de segurança e os pontos de recuperação que foram criados ao longo do tempo. O Cofre de serviços de recuperação também contém as políticas de cópia de segurança que estão associadas a máquinas virtuais protegidas.

Para criar um cofre dos Serviços de Recuperação:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. No **Hub** menu, selecione **procurar**e, em seguida, escreva **dos serviços de recuperação**. À medida que começa a escrever, a sua entrada filtra a lista de recursos. Selecione **cofres dos serviços de recuperação**.

    ![Escrever na caixa e selecionando "Cofres dos serviços de recuperação" nos resultados](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    É apresentada a lista dos cofres dos Serviços de Recuperação.
3. No **cofres dos serviços de recuperação** menu, selecione **adicionar**.

    ![Passo 2 da Criação do Cofre dos Serviços de Recuperação](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    O **cofres dos serviços de recuperação** abre o painel. Pede-lhe fornecer informações para **nome**, **subscrição**, **grupo de recursos**, e **localização**.

    ![Painel "Cofres dos serviços de recuperação"](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Para o **Nome**, introduza um nome amigável para identificar o cofre. O nome tem de ser exclusivo para a subscrição do Azure. Escreva um nome que contenha 2 a 50 carateres. Tem de começar com uma letra e pode conter apenas letras, números e hífenes.
5. Selecione **subscrição** para ver a lista de subscrições disponíveis. Se não tiver a certeza de que subscrição utilizar, utilize a predefinição (ou sugerida) subscrição. Existem várias opções apenas se for seu trabalho ou a conta da escola associada com várias subscrições do Azure.
6. Selecione **grupo de recursos** para ver a lista de grupos de recursos disponíveis ou selecione **novo** para criar um novo grupo de recursos. Para obter informações completas sobre os grupos de recursos, consulte [descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Selecione **localização** para selecionar a região geográfica do cofre. O cofre *tem de* estar na mesma região que as máquinas virtuais que pretende proteger.

   > [!IMPORTANT]
   > Se não tiver a certeza de que a localização na qual existe a VM, feche a caixa de diálogo de criação do cofre e aceda à lista de máquinas virtuais no portal. Se tiver máquinas virtuais em várias regiões, terá de criar um cofre dos serviços de recuperação em cada região. Crie o cofre na primeira localização antes de ir para a localização seguinte. Não é necessário especificar contas de armazenamento para armazenar os dados de cópia de segurança. O Cofre dos serviços de recuperação e o serviço de cópia de segurança do Azure processam que automaticamente.
   >
   >

8. Selecione **Criar**. Pode demorar algum tempo até que o cofre dos Serviços de Recuperação seja criado. Monitorize as notificações de estado na área superior direita do portal. Depois do cofre for criado, aparecerá na lista de cofres dos serviços de recuperação. Se não vir o cofre, selecione **atualizar**.

    ![Lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Agora que criou o cofre, saiba como configurar a replicação de armazenamento.

## <a name="set-storage-replication"></a>Replicação de armazenamento do conjunto
A opção de replicação de armazenamento permite-lhe escolher entre o armazenamento georredundante e localmente redundante. Por predefinição, o seu cofre tem um armazenamento georredundante. Deixe a opção definida para armazenamento georredundante se for a sua cópia de segurança primária. Escolha o armazenamento localmente redundante se pretender uma opção mais barata e com menos duração.

Para editar a definição de replicação de armazenamento:

1. No **cofres dos serviços de recuperação** painel, selecione o cofre.
    Quando seleciona o cofre, a **definições** painel (que tem o nome do Cofre de na parte superior) e o painel de detalhes de cofre aberto.

   ![Escolha o seu Cofre da lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. No **definições** painel, utilize o controlo de deslize vertical para desloque para baixo até o **gerir** secção e selecione **infraestrutura de cópia de segurança**. No **geral** secção, selecione **configuração de cópia de segurança**. No **configuração de cópia de segurança** painel, escolha a opção de replicação de armazenamento para o cofre. Por predefinição, o seu cofre tem um armazenamento georredundante.

   ![Lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Se estiver a utilizar o Azure como um ponto final de armazenamento de cópia de segurança primário, continue a utilizar o armazenamento georredundante. Se estiver a utilizar o Azure como um ponto final de armazenamento de cópia de segurança não primário, escolha armazenamento localmente redundante. Saiba mais sobre as opções de armazenamento na [descrição geral da replicação de armazenamento do Azure](../storage/common/storage-redundancy.md).

3. Se tiver alterado o tipo de replicação de armazenamento, selecione **guardar**.
    
Após escolher a opção de armazenamento para o cofre, está pronto para associar a VM com o cofre. Para começar a associação, detete e registe as máquinas virtuais do Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Selecione um objetivo de cópia de segurança, definir uma política e definir os itens para proteger
Antes de registar uma VM com um cofre, execute o processo de deteção para se certificar de que as novas máquinas virtuais que foram adicionadas à subscrição são identificadas. O processo de consulta do Azure para obter a lista de máquinas virtuais na subscrição, juntamente com informações como o nome do serviço de nuvem e a região. 

No portal do Azure, *cenário* refere-se ao irá colocar no cofre dos serviços de recuperação. *Política* é a agenda da frequência e quando são tirados pontos de recuperação. A política também inclui o período de retenção para os pontos de recuperação.

1. Se já tiver um cofre dos Serviços de Recuperação aberto, avance para o passo 2. Se não tiver um cofre dos serviços de recuperação, abrir, abra o [portal do Azure](https://portal.azure.com/). No **Hub** menu, selecione **mais serviços**.

   a. Na lista de recursos, escreva **Serviços de Recuperação**. À medida que começa a escrever, a sua entrada filtra a lista. Quando vir **cofres dos serviços de recuperação**, selecione-o.

      ![Escrever na caixa e selecionando "Cofres dos serviços de recuperação" nos resultados](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      É apresentada a lista dos cofres dos Serviços de Recuperação. Se não houver nenhuma cofres na sua subscrição, esta lista está vazia.

      ![Vista da lista dos cofres dos Serviços de Recuperação](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. Na lista dos cofres dos Serviços de Recuperação, selecione um cofre.

      O **definições** painel e o dashboard do cofre para a escolhida cofre aberta.

      ![Dashboard de painel e Cofre de definições](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. No menu do dashboard do cofre, selecione **cópia de segurança**.

   ![Botão de cópia de segurança](./media/backup-azure-arm-vms-prepare/backup-button.png)

   O **cópia de segurança** e **objetivo de cópia de segurança** painéis abrir.

3. No **objetivo de cópia de segurança** painel, defina **onde está a carga de trabalho em execução?** para **Azure** e **que itens pretende cópia de segurança?** para  **Máquina virtual**. Em seguida, selecione **OK**.

   ![Painéis de cópia de segurança e o objetivo de cópia de segurança](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Este passo regista a extensão da VM com o cofre. O **objetivo de cópia de segurança** painel fecha-se e o **política de cópia de segurança** abre o painel.

   !["Cópia de segurança" e "Política de cópia de segurança" painéis](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. No **política de cópia de segurança** painel, selecione a política de cópia de segurança que pretende aplicar ao cofre.

   ![Selecionar política de cópia de segurança](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   Os detalhes da política predefinida estão listados no menu pendente. Se pretende criar uma nova política, selecione **Criar Novo** no menu pendente. Para obter instruções sobre como definir uma política de cópia de segurança, consulte o artigo [Definir uma política de cópia de segurança](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Selecione **OK** para associar a política de cópia de segurança com o cofre.

   O **política de cópia de segurança** painel fecha-se e o **selecionar máquinas virtuais** abre o painel.
5. No **selecionar máquinas virtuais** painel, escolha as máquinas virtuais para associar à política especificada e selecione **OK**.

   ![Painel "Selecionar máquinas virtuais"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   A máquina virtual selecionada é validada. Se não vir as máquinas virtuais que pretende ver, verifique se existe na mesma localização que o Cofre dos serviços de recuperação do Azure e já não estão protegidos no Cofre de outro. O dashboard do cofre mostra a localização do cofre dos serviços de recuperação.

6. Agora que definiu todas as definições para o cofre, no **cópia de segurança** painel, selecione **ativar cópia de segurança**. Este passo implementa a política para o Cofre e as VMs. Este passo não cria o ponto de recuperação inicial da máquina virtual.

   ![Botão "Ativar a cópia de segurança"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Depois de ativar a cópia de segurança com êxito, a política de cópia de segurança será executado numa agenda. Se pretende gerar uma tarefa de cópia de segurança a pedido para cópia de segurança de máquinas virtuais, ver [acionar a tarefa de cópia de segurança](./backup-azure-arm-vms.md#triggering-the-backup-job).

Se tiver problemas ao registar a máquina virtual, consulte as seguintes informações sobre como instalar o agente da VM e conectividade de rede. Provavelmente, não terá as seguintes informações se estiver a proteger máquinas virtuais criadas no Azure. Mas, se tiver migrado máquinas virtuais no Azure, não se esqueça de que instalou corretamente o agente da VM e que a máquina virtual pode comunicar com a rede virtual.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Instalar o agente da VM na máquina virtual
Para a extensão de cópia de segurança funcione, o Azure [agente da VM](../virtual-machines/windows/agent-user-guide.md) tem de estar instalado na máquina virtual do Azure. Se a VM foi criada no Azure Marketplace, o agente da VM já se encontra presente na máquina virtual. 

As seguintes informações são fornecidas para situações em que são *não* através de uma VM criada no Azure Marketplace. Por exemplo, migrou uma VM a partir de um datacenter no local. Nesse caso, o agente da VM tem de ser instalado para poder proteger a máquina virtual.

Se tiver problemas a cópia de segurança a VM do Azure, utilize a tabela seguinte para verificar que o agente da VM do Azure está corretamente instalado na máquina virtual. A tabela fornece informações adicionais sobre o agente da VM para o Windows e VMs com Linux.

| **Operação** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalar o agente da VM |Transfira e instale o [MSI do agente](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Necessita de privilégios de administrador para concluir a instalação. |Instalar a versão mais recente [agente Linux](../virtual-machines/linux/agent-user-guide.md). Necessita de privilégios de administrador para concluir a instalação. Recomendamos que instale o agente do seu repositório de distribuição. Iremos *não é recomendada a* instalar o agente de VM com Linux diretamente a partir do GitHub.  |
| Atualizar o agente da VM |Atualizar o agente da VM é tão simple como reinstalar o [binários do agente VM](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Certifique-se de que nenhuma operação de cópia de segurança está em execução enquanto o agente da VM está a ser atualizado. |Siga as instruções para [atualizar o agente da VM com Linux](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Recomendamos a atualizar o agente do seu repositório de distribuição. Iremos *não é recomendada a* atualizar o agente da VM com Linux diretamente a partir do GitHub.<br><br>Certifique-se de que nenhuma operação de cópia de segurança está em execução enquanto o agente da VM está a ser atualizado. |
| Validar a instalação do agente VM |1. Procure a pasta de C:\WindowsAzure\Packages na VM do Azure. <br><br>2. Encontrar o ficheiro WaAppAgent.exe. <br><br>3. Clique com o botão direito do rato no ficheiro, aceda a **Propriedades** e, em seguida, selecione o separador **Detalhes**. O **versão do produto** campo deve ser 2.6.1198.718 ou superior. |N/A |

### <a name="backup-extension"></a>Extensão da cópia de segurança
Depois do agente VM está instalado na máquina virtual, o serviço de cópia de segurança do Azure instala a extensão de cópia de segurança para o agente da VM. O serviço de cópia de segurança atualiza de forma totalmente integrada e patches a extensão de cópia de segurança.

O serviço de cópia de segurança instala a extensão de cópia de segurança se pretende ou não a VM está em execução. Uma VM em execução fornece a maior possibilidade de obter um ponto de recuperação consistente com aplicações. No entanto, o serviço de cópia de segurança continua a fazer cópias de segurança da VM mesmo se está desativada e não foi possível instalar a extensão. Isto é conhecido como *offline VM*. Neste caso, o ponto de recuperação será *consistente com a falha*.

## <a name="establish-network-connectivity"></a>Estabelecer conectividade de rede
Para gerir os instantâneos VM, a extensão de cópia de segurança necessita de conectividade para os endereços IP públicos do Azure. Sem o acesso correto à internet, o tempo limite de pedidos de HTTP da máquina virtual e a operação de cópia de segurança falhará. Se a sua implementação tem restrições de acesso no local - através de um grupo de segurança de rede (NSG), por exemplo – escolher uma destas opções para fornecer um caminho de simples para o tráfego de cópia de segurança:

* [Lista branca de IP do datacenter do Azure intervalos](http://www.microsoft.com/en-us/download/details.aspx?id=41653).
* Implemente um servidor de proxy HTTP para o encaminhamento de tráfego.

Quando estiver a decidir qual a opção a utilizar, são os compromissos entre capacidade de gestão, o controlo granular e o custo.

| Opção | Vantagens | Desvantagens |
| --- | --- | --- |
| Intervalos de IP da lista branca |Sem custos adicionais.<br><br>Para abrir o acesso num NSG, utilize o **conjunto AzureNetworkSecurityRule** cmdlet. |Complexo de gerir o afetado como alteram os intervalos de IP ao longo do tempo.<br><br>Fornece acesso a totalidade do Azure e não apenas armazenamento. |
| Utilizar um proxy HTTP |Controlo granular no proxy sobre o armazenamento URLs é permitida.<br><br>Único ponto de acesso à internet às VMs.<br><br>Não sujeitos a alterações do endereço IP do Azure. |Custos adicionais para executar uma VM com o software de proxy. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Intervalos de IP da lista branca de datacenter do Azure
A lista de permissões os intervalos IP do datacenter do Azure, consulte o [Web site Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653) para obter detalhes sobre os intervalos de IP e as instruções.

Pode permitir que as ligações para o armazenamento da região específica utilizando [etiquetas de serviço](../virtual-network/security-overview.md#service-tags). Certifique-se de que a regra que permite o acesso à conta de armazenamento tem prioridade mais alta do que a regra que bloqueia o acesso à internet. 

![NSG com etiquetas de armazenamento para uma região](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> As etiquetas de serviço de armazenamento estão disponíveis apenas em regiões específicas e estão em pré-visualização. Para obter uma lista de regiões, consulte [etiquetas de serviço para o armazenamento](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>Utilizar um proxy HTTP para cópias de segurança VM
Quando estiver a cópia de segurança de uma VM, a extensão de cópia de segurança na VM envia os comandos de gestão do instantâneo ao Storage do Azure através da utilização de uma API de HTTPS. Encaminhar o tráfego de extensão de cópia de segurança através do proxy HTTP, porque é o único componente configurado para acesso à internet pública.

> [!NOTE]
> Não recomendamos que o software de proxy específicas que deve utilizar. Certifique-se de que escolha um proxy que é compatível com os passos de configuração que se seguem.
>
>

A imagem de exemplo seguinte mostra os passos de três configuração necessárias para utilizar um proxy HTTP:

* As rotas VM de aplicação vinculado todo o tráfego HTTP para a internet pública através do proxy VM.
* O proxy VM permite tráfego de entrada de VMs na rede virtual.
* O grupo de segurança de rede com o nome de bloqueio NSF necessita de uma regra de segurança que permita o tráfego de internet de saída do proxy VM.

Para utilizar um proxy HTTP para comunicar com a internet pública, conclua os passos seguintes.

> [!NOTE]
> Estes passos utilizam valores e nomes específicos para este exemplo. Quando estiver a introduzir (ou colar) detalhes para o seu código, utilize os nomes e valores para a sua implementação.

#### <a name="step-1-configure-outgoing-network-connections"></a>Passo 1: Configurar ligações de rede de saída
###### <a name="for-windows-machines"></a>Para máquinas do Windows
Este procedimento configura a configuração do servidor proxy para a conta de sistema local.

1. Transferir [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Abra o Internet Explorer, executando o seguinte comando a partir de uma linha de comandos elevada:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

3. No Internet Explorer, aceda a **ferramentas** > **opções da Internet** > **ligações** > **definições de LAN**.
4. Verifique as definições de proxy para a conta de sistema. Defina o IP de proxy e a porta.
5. Feche o Internet Explorer.

O script seguinte configura uma configuração de proxy a nível de máquina e utiliza-o para qualquer tráfego de saída HTTP ou HTTPS. Se configurar um servidor proxy numa conta de utilizador atual (não uma conta de sistema local), utilize este script para aplicá-las para SYSTEMACCOUNT.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Se observar "(407) a autenticação de Proxy necessário" no registo de servidor de proxy, verifique se a autenticação está corretamente configurado.
>
>

###### <a name="for-linux-machines"></a>Para computadores Linux
Adicione a seguinte linha para o ```/etc/environment``` ficheiro:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Adicione as seguintes linhas à ```/etc/waagent.conf``` ficheiro:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Passo 2: Permitir ligações de entrada no servidor de proxy
1. No servidor de proxy, abra a Firewall do Windows. A forma mais fácil de aceder a firewall está a procurar **Firewall do Windows com segurança avançada**.
2. No **Firewall do Windows com segurança avançada** caixa de diálogo, clique com botão direito **regras de entrada** e selecione **nova regra**.
3. No Assistente de nova entrada regra, no **tipo de regra** página, selecione o **personalizada** opção e selecione **seguinte**.
4. No **programa** página, selecione **todos os programas** e selecione **seguinte**.
5. No **protocolo e portas** página, introduza as seguintes informações e selecione **seguinte**:
   * Para **tipo de protocolo**, selecione **TCP**.
   * Para **Porta Local**, selecione **portas específicas**. Na caixa seguinte, especifique o número da porta proxy que tenha sido configurada.
   * Para **portas remotas**, selecione **todas as portas**.

Para o resto do assistente, aceite as predefinições até ao final. Em seguida, atribua esta regra de um nome. 

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Passo 3: Adicionar uma regra de exceção para o NSG
O comando seguinte adiciona uma exceção para o NSG. Esta exceção permite tráfego TCP a partir de qualquer porta no 10.0.0.5 para qualquer endereço internet na porta 80 (HTTP) ou 443 (HTTPS). Se necessitar de uma porta específica na internet pública, lembre-se de que adicione essa porta para ```-DestinationPortRange```.

Numa linha de comandos do Azure PowerShell, introduza o seguinte comando:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Tem dúvidas?
Se tiver dúvidas ou se houver alguma funcionalidade que pretende ver incluída, [envie-nos comentários](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Passos Seguintes
Agora que tem de preparar o ambiente para fazer cópias de segurança a VM, o próximo passo lógico é criar uma cópia de segurança. O planeamento artigo fornece informações mais detalhadas sobre a cópia de segurança de VMs.

* [Fazer uma cópia de segurança de máquinas virtuais](backup-azure-arm-vms.md)
* [Planear a infraestrutura de cópia de segurança de VM](backup-azure-vms-introduction.md)
* [Gerir cópias de segurança da máquina virtual](backup-azure-manage-vms.md)
