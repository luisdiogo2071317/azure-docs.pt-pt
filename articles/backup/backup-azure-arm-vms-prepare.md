---
title: Prepare-se para fazer uma cópia de segurança de VMs do Azure com o Azure Backup
description: Descreve como preparar as VMs do Azure para cópia de segurança com o serviço de cópia de segurança do Azure
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: raynew
ms.openlocfilehash: a7a2d8729e1abdafa89eff912faf84d8f247b442
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54215444"
---
# <a name="prepare-to-back-up-azure-vms"></a>Preparar a cópia de segurança de VMs do Azure

Este artigo descreve como preparar para fazer backup de uma VM do Azure utilizando uma [Azure Backup](backup-introduction-to-azure-backup.md) cofre dos serviços de recuperação. A preparar a cópia de segurança inclui:


> [!div class="checklist"]
> * Antes de começar, reveja os cenários suportados e limitações.
> * Verifique os pré-requisitos, incluindo requisitos de VM do Azure e a conectividade de rede.
> * Crie um cofre.
> * Selecione a forma como o armazenamento replica.
> * Detetar VMs, configurar definições de cópia de segurança e a política.
> * Ativar cópia de segurança para VMs selecionadas


> [!NOTE]
   > Este artigo descreve como fazer cópias de segurança de VMs do Azure ao configurar um cofre e selecionar VMs para criar cópias de segurança. É útil se quiser fazer uma cópia de segurança de várias VMs. Também pode fazer backup de uma VM do Azure diretamente a partir de suas configurações de VM. [Saiba mais](backup-azure-vms-first-look-arm.md)

## <a name="before-you-start"></a>Antes de começar

1. [Obtenha uma visão geral](backup-azure-vms-introduction.md) da cópia de segurança do Azure para VMs do Azure.
2. Rever detalhes de suporte e as limitações abaixo.

   **/ Limitação de suporte** | **Detalhes**
   --- | ---
   **SO do Windows** | Windows Server 2008 R2 de 64 bits ou posterior.<br/><br/> Cliente do Windows 7 de 64 bits ou posterior.
   **SO Linux** | Pode fazer backup de distribuições do Linux de 64 bits [suportado pelo Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), com exceção do CoreOS Linux.<br/><br/> Reveja os sistemas operativos Linux que [suportar o restauro de ficheiro](backup-azure-restore-files-from-vm.md#for-linux-os).<br/><br/> Outras distribuições do Linux poderão funcionar, desde que o agente da VM está disponível na VM e haja suporte para Python. No entanto, estes distribuições não são suportadas.
   **Região** | Pode fazer backup de VMs do Azure em todos os [regiões suportadas](https://azure.microsoft.com/regions/#services). Se uma região não é suportada, não poderá selecioná-lo quando cria o cofre.<br/><br/> Não é possível criar cópias de segurança e restauro entre regiões do Azure. Apenas numa única região.
   **Limite de disco de dados** | Não é possível fazer uma cópia de segurança de VMs com mais de 16 discos de dados.
   **Armazenamento partilhado** | Não recomendamos backup das VMs utilizar CSV ou servidor de ficheiros de escalamento horizontal. Gravadores CSV são provável que falhem.
   **Encriptação do Linux** | Backup das VMs de Linux encriptados com o Linux Unified chave de configuração (LUKS) não é suportada.
   **Consistência VM** | O Azure Backup não suporta a consistência de várias VMS.
   **Redes** | Cópia de segurança dados não incluem unidades de rede montado anexadas a uma VM.<br/><br/>
   **Instantâneos** | Não é suportada a obtenção de instantâneos num disco de ativado o acelerador de escrita. Bloqueia a cópia de segurança do Azure de tirar um instantâneo consistente com a aplicação de todos os discos VM.
   **PowerShell** | Há uma série de ações que só estão disponíveis com o PowerShell:<br/><br/> -Restaurar VMs geridas por balanceadores de carga interna/externa ou com vários endereços IP reservados ou adaptadores. [Saiba mais](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations)<br/><br/> -Restaurar um controlador de domínio VM numa configuração de controlador de domínio de várias. [Saiba mais](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
   **Hora do sistema** | Cópia de segurança do Azure não suporta o ajuste automático de relógio de alterações de economia de hora de Verão para cópias de segurança de VM do Azure. Modificar as políticas de cópia de segurança manualmente conforme necessário.
   **Contas de armazenamento** | Se estiver a utilizar uma conta de armazenamento de restrição de rede, certifique-se de que ativa **permitir confiável a serviços da Microsoft para aceder a esta conta de armazenamento** para que o serviço Azure Backup pode aceder à conta. Recuperação ao nível do item não é suportada para contas de armazenamento de restrição de rede.<br/><br/> Numa conta de armazenamento, certifique-se de que **Firewalls e redes virtuais** configurações permitem o acesso a partir de **todas as redes**.


## <a name="prerequisites"></a>Pré-requisitos

- Tem de criar o Cofre na mesma região que as VMs do Azure que pretende criar cópias de segurança.
- Antes de começar, verifique as regiões de VM do Azure.
    - Se tiver VMs em várias regiões, crie um cofre em cada região.
    - Não precisa de especificar contas de armazenamento para armazenar os dados de cópia de segurança. O Cofre e o serviço Azure Backup processam que automaticamente.
- Certifique-se de que o agente da VM está instalado em VMs do Azure que pretende criar cópias de segurança.



### <a name="install-the-vm-agent"></a>Instalar o agente da VM

Para ativar a cópia de segurança, cópia de segurança do Azure instala uma extensão de cópia de segurança (instantâneo de VM ou do Linux de instantâneo de VM) para o agente da VM que é executado na VM do Azure.
    -  O agente da VM do Azure está instalado por predefinição em qualquer VM do Windows implementadas a partir de uma imagem do Azure Marketplace. Quando implementa uma imagem de Azure Marketplace a partir do portal, PowerShell, CLI ou um modelo Azure Resource Manager, também é instalado o agente da VM do Azure.
    - Se tiver migrado uma VM no local, o agente não está instalado e tem de instalá-lo antes de poder ativar cópia de segurança para a VM.

Se for necessário, instale o agente da seguinte forma.

**VM** | **Detalhes**
--- | ---
**VMs do Windows** | [Transferir e instalar](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) o agente com permissões de administrador na máquina.<br/><br/> Para verificar a instalação, na *C:\WindowsAzure\Packages* na VM, com o botão direito do WaAppAgent.exe > **propriedades**, > **detalhes** separador. **Versão do produto** deve ser 2.6.1198.718 ou superior.
**VMs do Linux** | A instalação a utilizar um RPM ou um pacote DEB no repositório de pacotes de sua distribuição é o método preferencial de instalação e atualização do agente Linux do Azure. Todas as a [apoiadas pelo fornecedores de distribuição](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integrar o pacote do agente Linux do Azure em suas imagens e repositórios. O agente está disponível no [GitHub](https://github.com/Azure/WALinuxAgent), mas não é recomendada a instalação a partir daí.
Se tiver problemas, fazer backup da VM do Azure, utilize a tabela seguinte para verificar que o agente de VM do Azure está corretamente instalado na máquina virtual. A tabela fornece informações adicionais sobre o agente da VM para Windows e VMs do Linux.

### <a name="establish-network-connectivity"></a>Estabelecer conectividade de rede

A extensão de cópia de segurança em execução na VM tem de ter acesso de saída para os endereços IP públicos do Azure. Para permitir o acesso, pode:


- **As regras do NSG**: Permitir a [intervalos IP do datacenter do Azure](https://www.microsoft.com/download/details.aspx?id=41653). Pode adicionar uma regra que permite o acesso para o serviço de cópia de segurança do Azure com um [etiqueta de serviço](../virtual-network/security-overview.md#service-tags), em vez de individualmente permitindo que cada intervalo de endereços e gerenciá-los ao longo do tempo.
- **Proxy**: Implemente um servidor de proxy HTTP para o encaminhamento de tráfego.
- **Firewall do Azure**: Permitir o tráfego através da Firewall do Azure na VM, com uma etiqueta do FQDN para o serviço de cópia de segurança do Azure.

Ao decidir entre opções, considere as compensações.

**Opção** | **Vantagens** | **Desvantagens**
--- | --- | ---
**NSG** | Sem custos adicionais. Simples de gerenciar com etiquetas de serviço | Fornece acesso a todo do Azure e não apenas armazenamento. |
**Proxy HTTP** | É permitido um controle granular sobre os URLs de armazenamento.<br/><br/> Único ponto de acesso à internet para VMs.<br/><br/> Custos adicionais para o proxy.
**Etiquetas FQDN** | Fácil de utilizar se tiver o Firewall do Azure numa sub-rede de VNet | Não é possível criar suas próprias etiquetas FQDN ou modificar os FQDNs numa marca.



Se utilizar Managed Disks do Azure, poderá ter de abertura de portas adicionais (porta 8443) nas firewalls.



### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Configurar uma regra NSG para permitir o acesso de saída para o Azure

Se a sua VM do Azure tem acesso gerido por um NSG, permita o acesso de saída para o armazenamento de cópia de segurança para as portas e intervalos necessários.



1. Na VM > **Networking**, clique em **Adicionar regra de saída através da porta**.
- Se tiver uma regra negar o acesso, o novo permite a regra tem de ser superior. Por exemplo, se tem um **Deny_All** conjunto com prioridade 1000, sua nova regra de regras devem ser definida para menos de 1000.
2. Na **Adicionar regra de segurança de saída**, clique em **avançadas**.
3. Na origem, selecione **VirtualNetwork**.
4. Na **intervalos de portas de origem**, escreva um asterisco (*) para permitir o acesso de saída de qualquer porta.
5. Na **destino**, selecione **etiqueta de serviço**. Na lista, selecione o armazenamento. <region>. A região é a região em que o Cofre e as VMs que pretende criar cópias de segurança, estão localizadas.
6. Na **intervalos de portas de destino**, selecione a porta.

    - VM com discos não geridos e conta de armazenamento sem encriptação: 80
    - VM com discos não geridos e conta de armazenamento encriptado: 443 (predefinição)
    - VM gerida: 8443.
1. Na **protocolo**, selecione **TCP**.
2. Na **prioridade**, conceda-lhe um valor de prioridade inferior a superior quaisquer negar regras.
3. Forneça um nome e descrição para a regra e clique em **OK**.

Pode aplicar a regra NSG para várias VMs para permitir o acesso de saída para o Azure para o Azure Backup.

Este vídeo o orienta pelo processo.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]



### <a name="route-backup-traffic-through-a-proxy"></a>Encaminhar o tráfego de cópia de segurança através de um proxy

Pode encaminhar o tráfego de cópia de segurança através de um proxy e, em seguida, dar o acesso de proxy para os intervalos do Azure necessários.

Deve configurar o proxy VM para permitir que o seguinte:

- A VM do Azure deve encaminhar todo o tráfego HTTP vinculado para a internet pública através do proxy.
- O proxy deve permitir tráfego de entrada de VMs na rede virtual (VNet) aplicável.
- O NSG **NSF bloqueio** necessita de uma regra que permita o tráfego de internet de saída do proxy VM.

Aqui está como precisa configurar o proxy. Podemos utilizar valores de exemplo. Deverá substituí-los com os seus próprios.

#### <a name="set-up-a-system-account-proxy"></a>Configurar um proxy de conta de sistema
Se não tiver um proxy de conta de sistema, configure um da seguinte forma:

1. Baixe [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Execute **PsExec.exe -i -s cmd.exe** para executar a linha de comandos com uma conta do sistema.
3. Execute o navegador no contexto do sistema. Por exemplo: **ProgramFiles%\Internet Explorer\iexplore.exe** para o Internet Explorer.  
4. Defina as definições de proxy.
    - Nas máquinas do Linux:
        - Adicionar esta linha para o **/etc/ambiente** ficheiro:
            - **http_proxy =http://proxy porta de proxy: endereço IP**
        - Adicionar estas linhas para o **/etc/waagent.Conf.** ficheiro:
            - **Endereço IP HttpProxy.Host=proxy**
            - **Porta de HttpProxy.Port=proxy**
    - Nas máquinas do Windows, nas configurações do navegador, especifica que um proxy a utilizar. Se estiver atualmente a utilizar um proxy numa conta de utilizador, pode utilizar este script para aplicar a definição ao nível da conta do sistema.
        ```
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

        ```

#### <a name="allow-incoming-connections-on-the-proxy"></a>Permitir ligações de entrada no proxy

1. Permita ligações de entrada nas definições de proxy.
2. Por exemplo, abra **Firewall do Windows com segurança avançada**.
    - Com o botão direito **regras de entrada** > **nova regra**.
    - Na **tipo de regra** selecionar **personalizada** > **seguinte**.
    - Na **programa**, selecione **todos os programas** > **seguinte**.
    - Na **protocolos e portas** definir o tipo como **TCP**, **portas Local** para **portas específicas**, e **porta remota**para **todas as portas**.
    - Concluir o assistente e especificar um nome para a regra.

#### <a name="add-an-exception-rule-to-the-nsg"></a>Adicionar uma regra de exceção para o NSG

No NSG **NSF bloqueio**, permitir tráfego a partir de qualquer porta no 10.0.0.5 para qualquer endereço internet na porta 80 (HTTP) ou 443 (HTTPS).

- O seguinte script do PowerShell fornece um exemplo para permitir o tráfego.
- Em vez de permitir saída para todos os endereços de internet pública, pode especificar um intervalo de endereços IP (-DestinationPortRange), ou usar a marca de serviço storage.region.   

    ```
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```
### <a name="allow-firewall-access-with-fqdn-tag"></a>Permitir o acesso de firewall com a etiqueta do FQDN

Pode configurar a Firewall do Azure para permitir o acesso de saída para o tráfego de rede para o Azure Backup.

- [Saiba mais sobre](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) implantar o Firewall do Azure.
- [Leia sobre](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN etiquetas.


## <a name="create-a-vault"></a>Criar um cofre

Um cofre de serviços de recuperação para cópia de segurança armazena cópias de segurança e pontos de recuperação criados ao longo do tempo e armazena as diretivas de cópia de segurança associadas a máquinas de cópia de segurança. Crie um cofre da seguinte forma:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Sobre o **Hub** menu, selecione **procurar**e o tipo **serviços de recuperação**. Quando começar a escrever, sua entrada filtra a lista de recursos. Selecione **cofres dos serviços de recuperação**.

    ![Escrever na caixa e selecionando "Cofres de serviços de recuperação" nos resultados](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    É apresentada a lista dos cofres dos Serviços de Recuperação.
3. Sobre o **cofres dos serviços de recuperação** menu, selecione **Add**.

    ![Passo 2 da Criação do Cofre dos Serviços de Recuperação](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    O **cofres dos serviços de recuperação** painel abre-se. Pede-lhe para fornecer informações de **Name**, **subscrição**, **grupo de recursos**, e **localização**.

    ![Painel "Cofres dos serviços de recuperação"](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Em **Nome**, introduza um nome amigável para identificar o cofre.
    - O nome tem de ser exclusivo para a subscrição do Azure.
    - Pode conter 2 e 50 carateres.
    - Ele tem de começar com uma letra e pode conter apenas letras, números e hífenes.
5. Selecione **subscrição** para ver a lista de subscrições disponíveis. Se não tiver a certeza de que subscrição utilizar, utilize a predefinição (ou sugerida) subscrição. Existem várias opções apenas se for seu trabalho ou a conta da instituição de ensino está associada a várias subscrições do Azure.
6. Selecione **grupo de recursos** para ver a lista de grupos de recursos disponíveis ou selecione **New** para criar um novo grupo de recursos. [Saiba mais](../azure-resource-manager/resource-group-overview.md) sobre grupos de recursos.
7. Selecione **localização** para selecionar a região geográfica do cofre. O Cofre *tem* estar na mesma região que as VMs que pretende criar cópias de segurança.
8. Selecione **Criar**.
    - Pode demorar algum tempo para o cofre a ser criada.
    - Monitorize as notificações de estado na área de canto superior direito do portal.
    ![Lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Depois do cofre for criado, aparece na lista de cofres dos serviços de recuperação. Se não vir o cofre, selecione **atualizar**.

## <a name="set-up-storage-replication"></a>Configurar a replicação de armazenamento

Por predefinição, o seu Cofre tem [armazenamento georredundante (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). Recomendamos GRS para a cópia de segurança primária, mas pode usar[armazenamento localmente redundante](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para uma opção mais barata. 

Modificar os replicação de armazenamento da seguinte forma:

1. No cofre > **infraestrutura de cópia de segurança**, clique em **configuração de cópia de segurança**

   ![Lista de cofres de cópia de segurança](./media/backup-azure-arm-vms-prepare/full-blade.png)

2. Na **configuração de cópia de segurança**, modifique o método de redundância de armazenamento como necessário e selecione **guardar**.


## <a name="configure-backup"></a>Configurar a cópia de segurança

Detetar VMs na subscrição e configurar a cópia de segurança.

1. No cofre > **descrição geral**, clique em **+ cópia de segurança**

   ![Botão de cópia de segurança](./media/backup-azure-arm-vms-prepare/backup-button.png)

   O **cópia de segurança** e **objetivo de cópia de segurança** painéis abrir.

2. Na **objetivo de cópia de segurança**> **em que a sua carga de trabalho é executado?**, selecione **Azure**. Na **o que fazer quiser a cópia de segurança?**, selecione **Máquina Virtual** >  **OK**. A extensão de VM é registada no cofre.

   ![Painéis de cópia de segurança e objetivo de cópia de segurança](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Este passo registra a extensão de VM com o cofre. O **objetivo de cópia de segurança** fecha o painel e o **política de cópia de segurança** painel abre-se.

3. Na **política de cópia de segurança**, selecione a política que pretende associar ao cofre. Em seguida, clique em **OK**.
    - Os detalhes da política predefinida estão listados no menu pendente.
    - Clique em **criar novo** para criar uma política. [Saiba mais](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) sobre como definir uma política.

    !["Cópia de segurança" e "Política de cópia de segurança" painéis](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. Na **selecionar máquinas virtuais** painel, selecione as VMs que irão utilizar a política de cópia de segurança especificada > **OK**.

    - A VM selecionada é validada.
    - Pode selecionar apenas as VMs na mesma região que o cofre. As VMs só podem ser copiadas num único cofre.

   ![Painel "Selecionar máquinas virtuais"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. Na **cópia de segurança**, selecione **ativar cópia de segurança**.

   - Isto implementa a política para o Cofre e para as VMs e instala a extensão de cópia de segurança no agente VM em execução na VM do Azure.
   - Este passo não cria o ponto de recuperação inicial para a VM.

   ![Botão "Ativar cópia de segurança"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Depois de ativar a cópia de segurança:

- A política de cópia de segurança é executada de acordo com sua agenda de cópia de segurança.
- O serviço de cópia de segurança instala a extensão de cópia de segurança, independentemente da VM está em execução.
    - Uma VM em execução fornece a maior possibilidade de obter um ponto de recuperação consistente com aplicações.
    -  No entanto, a VM é uma cópia de segurança, mesmo que ele é desativado e não é possível instalar a extensão. Isso é conhecido como *offline VM*. Neste caso, o ponto de recuperação será *consistente com a falha*.
- Se quiser gerar imediatamente, uma cópia de segurança a pedido para a VM na **itens de cópia de segurança**, clique nas reticências (...) junto a VM > **cópia de segurança agora**.


## <a name="next-steps"></a>Passos Seguintes

- Resolver quaisquer problemas que ocorrem com o [agentes de VM do Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) ou [cópia de segurança de VM do Azure](backup-azure-vms-troubleshoot.md).
- [Fazer cópia de segurança de VMs do Azure](backup-azure-vms-first-look-arm.md)
