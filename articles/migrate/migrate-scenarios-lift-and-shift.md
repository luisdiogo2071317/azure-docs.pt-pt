---
title: Migrar cargas de trabalho no local para o Azure com o Azure DMS e recuperação de Site | Microsoft Docs
description: Saiba como preparar o Azure para a migração de máquinas no local utilizando o serviço de migração de base de dados do Azure e o serviço do Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/21/2018
ms.author: raynew
ms.openlocfilehash: d6fc1af3c5a587ab62b86dcd4189165d96e6e3bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="scenario-2-lift-and-shift-migration-to-azure"></a>Cenário 2: Comparação de precisão e shift migração para Azure

Empresa Contoso estiver a considerar migração para o Azure. Para experimentar este limite, pretende avaliar e migrar uma aplicação de levar pequeno no local para o Azure. É uma aplicação de levar de duas camadas, com uma aplicação web que executem uma VM e uma base de dados do SQL Server na VM do segundo. A aplicação é implementada no VMware e o ambiente é gerido por um vCenter Server. 

No [cenário 1: avaliar a migração para o Azure](migrate-scenarios-assessment.md), utilizaram o Assistente de dados de migração (DMA) para avaliar a base de dados do SQL Server para a aplicação. Além disso, o serviço Azure migrar estes utilizados para avaliar a aplicação VMs. Agora, neste cenário, depois de concluir com êxito a avaliação, que pretendem migrar a base de dados para uma instância do Azure SQL geridos com o serviço de migração de base de dados do Azure (DMS) e as máquinas no local para VMs do Azure utilizando o serviço do Azure Site Recovery.

Se pretender experimentar [cenário 1](migrate-scenarios-assessment.md), e, em seguida, este cenário através desta aplicação levar ilustrativa, poderá transferi-lo de [github](https://github.com/Microsoft/SmartHotel360).



**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Serviço de gestão de base de dados](https://docs.microsoft.com/azure/dms/dms-overview) | O DMS permite migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure, com o período de indisponibilidade mínimo. | O serviço está atualmente em pré-visualização pública (Abril de 2018) e pode ser utilizado livre de encargos durante a pré-visualização.<br/><br/> Tenha em atenção que para a pré-visualização pública DMS está limitado a um [número das regiões](https://docs.microsoft.com/azure/dms/dms-overview).
[Instância de SQL gerida do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Instância gerida é um serviço de base de dados gerida que representa uma instância de servidor de SQL completamente gerido na nuvem do Azure. -Partilha o mesmo código com a versão mais recente do motor de base de dados do SQL Server e tem as funcionalidades mais recentes, melhoramentos de desempenho e patches de segurança. | Utilizar base de dados geridos instâncias do SQL do Azure em execução no Azure implicar custos com base na capacidade. [Saiba mais](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | O serviço orquestra e gere a migração e recuperação após desastre para as VMs do Azure e no local VMs e servidores físicos.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  As VMs do Azure são criadas e implicar custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre os encargos e preços.

Neste cenário, iremos configurar uma VPN de site a site para que o DMS possam ligar à base de dados no local, criar uma instância do Azure SQL geridas no Azure e migrar a base de dados. Para a recuperação de Site, iremos irá preparar o ambiente de VMware no local, configurar a replicação e migrar as VMs para o Azure.  


> [!IMPORTANT]
> Tem de estar inscritos no SQL Server geridos instância limitado pré-visualização pública. Necessita de uma subscrição do Azure para [inscrever](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Inscrição pode demorar de alguns dias para concluir, pelo que certifique-se de fazê-lo antes de começar a implementar este cenário.

## <a name="architecture"></a>Arquitetura

### <a name="site-recovery"></a>Site Recovery

![Site Recovery](media/migrate-scenarios-lift-and-shift/asr-architecture.png)  

### <a name="dms"></a>DMS
![DMS](media/migrate-scenarios-lift-and-shift/dms-architecture.png)  

Neste cenário:

- Contoso é um nome fictious que representa uma organização empresarial típica. A Contoso pretende avaliar e migrar as respetivas aplicações de levar de duas camadas no local.
- Contoso tem um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- A aplicação de levar interno está em camadas através de duas VMs, WEBVM e SQLVM e localizada num anfitrião do VMware ESXi (**contosohost1.contoso.com**).
- O ambiente do VMware é gerido pelo vCenter Server (**vcenter.contoso.com**) em execução numa VM.

## <a name="prerequisites"></a>Pré-requisitos

Se pretender executar neste cenário, eis o que deve ter.

Requisitos | Detalhes
--- | ---
**Subscrição do Azure** | Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não tiver o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de mais granulares permissões, consulte [neste artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Recuperação de sites (no local)** | Um servidor do vCenter no local a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Um anfitrião ESXi a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Uma ou mais VMs de VMware no anfitrião ESXi a executar.<br/><br/> As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Suportado [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuração.
**DMS** | Para que o DMS tem um [compatível no dispositivo VPN local](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Tem de ser capaz de configurar o dispositivo VPN no local. Tem de ter um endereço IPv4 público com acesso exterior, e o endereço não pode estar localizado atrás de um dispositivo NAT.<br/><br/> Certifique-se de que tem acesso à base de dados do SQL Server no local.<br/><br/> A Firewall do Windows deverá conseguir aceder ao motor de base de dados de origem. [Saiba mais](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Se existir uma firewall à frente do seu computador da base de dados, adicione regras para permitir o acesso à base de dados e aos ficheiros através de porta SMB 445.<br/><br/> As credenciais utilizadas para ligar à origem de SQL Server e instância geridos de destino têm de ser membros da função de servidor sysadmin.<br/><br/> Precisa de uma rede de partilha na base de dados no local que o DMS podem utilizar para criar cópias de segurança da base de dados de origem.<br/><br/> Certifique-se de que a conta de serviço que executa a instância do SQL Server de origem tem privilégios de escrita na partilha de rede.<br/><br/> Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede. O serviço de migração de base de dados do Azure representa tem estas credenciais de utilizador para carregar ficheiros de cópia de segurança para o contentor de armazenamento do Azure.<br/><br/> O processo de instalação do SQL Server Express define o protocolo de TCP/IP para **desativado** por predefinição. Certifique-se de que está ativada.


# <a name="scenario-overview"></a>Descrição geral do cenário

Eis o que vamos fazer:

> [!div class="checklist"]
> * **Passo 1: Configurar uma ligação de VPN de site para site**: DMS estabelece ligação à base de dados no local através de uma ligação do VPN site a site. Iremos criar uma rede virtual para a ligação VPN e mais tarde, iremos reutilizar esta rede para a recuperação de Site. A rede que cria tem de estar localizada na região na qual criar um cofre dos serviços de recuperação.
> * **Passo 2: Configurar uma instância do SQL do Azure gerida**: precisa de uma instância de gerido previamente criada para o qual irá migrar a base de dados do SQL Server no local.
> * **Passo 3: Configurar o URI de SA para DMS**: uma assinatura de acesso partilhado (SAS) Uniform Resource Identifier (URI) concede acesso delegado a recursos na sua conta de armazenamento, para que pode conceder permissões limitadas para objetos de armazenamento. Configure um URI de SAS para que o DMS pode aceder ao contentor de conta de armazenamento ao qual o serviço de carregamentos de ficheiros de cópia de segurança do SQL Server.
> * **Passo 4: Preparar DMS**: registar o fornecedor de migração de base de dados, crie uma instância e, em seguida, criar um projeto DMS.
> * **Passo 5: Preparar do Azure Site Recovery**: criar uma conta de armazenamento do Azure para armazenar dados replicados.
> * **Passo 6: Preparar VMware no local para a recuperação de Site**: preparar contas para a VM deteção e instalação do agente e preparar a ligação para VMs do Azure após a ativação pós-falha. 
> * **Passo 7: Replicar VMs**: configurar o ambiente de origem e destino de recuperação de sites, configurar uma política de replicação e iniciar a replicar VMs de armazenamento do Azure.
> * **Passo 8: Migrar a base de dados com o DMS**: executar uma migração de base de dados.
> * **Passo 9: Migrar as VMs com a recuperação de Site**: executar uma ativação pós-falha para migrar as VMs no Azure.


## <a name="step-1-set-up-site-to-site-vpn-connection"></a>Passo 1: Configurar a ligação de VPN de site a site

Para preparar a ligação do VPN site a site, configurar uma rede virtual e sub-redes, criar um gateway VPN para a rede virtual e configurar um gateway de rede local, de modo que Azure sabe como ligar à sua VPN no local. Em seguida, crie e verificar a ligação de site para site.

### <a name="set-up-a-virtual-network"></a>Configurar uma rede virtual

Crie uma rede virtual do Azure para fornecer DMS conetividade site a site para o SQL Server no local.


1. No [portal do Azure](https://portal.azure.com), clique em **crie um recurso**. Selecione **redes** > **rede Virtual**.
2. No **rede Virtual** > **selecionar um modelo de implementação**, selecione **Resource Manager** > **criar**
3. No **criar rede virtual** > **nome**, introduza um nome de rede exclusivos. Para o nosso cenário **ContosoVNET**.
4. No **espaço de endereços**, adicione o intervalo de endereços IP. O intervalo não pode sobrepor-se com o seu espaço de endereços no local.
5. No **grupo de recursos**, crie um novo grupo ou selecione um existente. Neste cenário estamos a utilizar **ContosoRG**.
6. No **localização**, selecione a região na qual pretende criar a rede virtual. Estamos a utilizar **EUA Leste 2**.
7. No **sub-rede**, adicionar o primeiro nome de sub-rede e o intervalo de endereços. Criámos o **aplicações** sub-rede.
8. Desative pontos finais de serviço.

    ![Criar uma rede virtual](media/migrate-scenarios-lift-and-shift/vnet-create-network.png)

9. Selecione **afixar ao dashboard** para que possa localizar facilmente a sua rede no futuro e, em seguida, clique em **criar**.
10. A rede virtual demora alguns segundos a ser criada. Depois de criado, certifique-se-lo no dashboard do portal do Azure.
11. Certifique-se de que estas portas de comunicação são permitidas na sua rede virtual: 443, 53, 9354, 445, 12000.


### <a name="set-up-subnets"></a>Configurar sub-redes

Temos quatro sub-redes na nossa rede do Azure:

![Lista de sub-rede](media/migrate-scenarios-lift-and-shift/vnet-subnet-list.png)

Configurar a primeira sub-rede (aplicações) ao criar a rede. Agora, crie o resto das sub-redes. A sub-rede do gateway é utilizada pela ligação de gateway VPN, para o Azure enviar o tráfego através de uma ligação VPN para o seu site no local.

1. Na rede virtual, em **definições**, clique em **sub-rede** > **+ sub-rede**.
2. Configurar uma sub-rede **Datamigration**, com este intervalo de endereços e, em seguida, clique em **OK**.

    ![Sub-rede de dados](media/migrate-scenarios-lift-and-shift/vnet-subnet-data.png)  


3. No **+ sub-rede**, configurar uma sub-rede **identidade**, com este intervalo de endereços e, em seguida, clique em **OK**.
    ![Sub-rede de identidade](media/migrate-scenarios-lift-and-shift/vnet-subnet-identity.png)

4. No **sub-rede**, clique em **sub-rede do Gateway**e, em seguida, clique em **OK**.
    - Esta sub-rede contém os endereços IP que irá utilizar o gateway de VPN para ligações VPN.
    - O nome desta sub-rede está definido automaticamente, de modo que Azure pode reconhecê-lo.
    - Ajuste o intervalo de endereços de preenchimento automático para corresponder a sub-rede no local. 

    ![Sub-rede de gateway](media/migrate-scenarios-lift-and-shift/vnet-subnet-gateway.png)
    
### <a name="set-up-a-virtual-network-gateway"></a>Configurar um gateway de rede virtual

1. No lado esquerdo da página do portal, clique em **+** e escreva "Gateway de Rede Virtual" na pesquisa. No **resultados**, clique em **gateway de rede Virtual**.
2. Na parte inferior da página “Gateway de rede virtual”, clique em **Criar**.
3. No **criar gateway de rede virtual** >  **nome**, especifique um nome para o objeto do gateway.
4. No **tipo de Gateway**, selecione **VPN**.
5. No **tipo de VPN**, selecione **baseado na rota**.
6. No **SKU**, selecione o SKU de gateway na lista pendente. Os SKUs listados na lista pendente dependem do tipo de VPN que selecionar. Não ative o modo ativo-ativo. [Saiba mais](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku) sobre SKUs.
7. No **rede Virtual** > **escolha uma rede virtual**, adicione a rede que criou. O gateway será adicionado a esta rede.
8. No **configuração de IP primeiro**, especifique um endereço IP público.  Um gateway de VPN tem de ter um endereço IP público. Este endereço é dinamicamente atribuído ao recurso quando é criado o gateway VPN. Apenas endereços dinâmicos são atualmente suportados, mas não irá alterar o endereço IP depois está atribuído.
    - Clique em **configuração de IP do gateway criar**. No **escolher endereço IP público**, clique em **+ criar nova**.
    - No **Criar endereço IP público**, especifique um nome para o seu endereço IP público (Gateway de Contoso). Deixe o SKU como **básico**e clique em **OK** para guardar as alterações.
        
    ![Gateway de rede virtual](media/migrate-scenarios-lift-and-shift/vnet-create-vpn-gateway2.png) 

9. Clique em **Criar** para começar a criar o gateway de VPN. As definições são validadas e "Deploying a gateway de rede virtual" aparece no dashboard.

    ![Validar o gateway de rede virtual](media/migrate-scenarios-lift-and-shift/vnet-vpn-gateway-validate.png)
    
A criação de um gateway pode demorar até 45 minutos. Poderá ter de atualizar a página do portal para ver o estado concluído.

### <a name="set-up-a-local-network-gateway"></a>Configurar um gateway de rede local

Configure um gateway de rede local no Azure, para representar o seu site no local.

1. No portal, clique em **Criar um recurso**.
2. Na caixa de pesquisa, escreva **Gateway de rede Local** e, em seguida, prima **Enter** para procurar. Nos resultados, clique em **gateway de rede Local** > **criar**.
3. No **criar gateway de rede local** > **nome**, especifique um nome que utiliza o Azure para o objeto do gateway de rede local.
4. **Endereço IP**, especifique o endereço IP público do dispositivo VPN no local para o qual se ligará Azure. O endereço IP não deve ser NAT'ed e o Azure tem de ser capaz de alcançá-lo.
5. No **espaço de endereços**, escreva os intervalos de endereços de rede local que será encaminhado através do gateway VPN para o dispositivo no local. Certifique-se de que não se sobreponham com intervalos na rede virtual do Azure.
6. **Configurar definições de BGP** não é relevante para este cenário.
7. No **subscrição**, certifique-se de que a subscrição está correta.
8. No **grupo de recursos**: selecione o grupo de recursos que utilizou para criar a rede (ContosoRG).
9. No **localização**, selecione a região onde criou a rede virtual.

    ![Local gateway](media/migrate-scenarios-lift-and-shift/vnet-create-local-gateway.png)

4. Clique em **criar** para criar o gateway local.

### <a name="configure-your-on-premises-vpn-device"></a>Configurar o dispositivo VPN no local

Eis o que precisa de configurar no dispositivo VPN no local:

- O endereço IPv4 público do gateway de rede virtual do Azure que acabou de criar.
- A chave pré-partilhada (a chave que utilizou ao criar a ligação do VPN site a site).  

Para o ajudar a configurar o dispositivo VPN no local:

- Consoante o dispositivo VPN no local que tem, poderá conseguir transferir um script de configuração do dispositivo VPN. [Saiba mais](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-download-vpndevicescript).
- Reveja [recursos mais úteis](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#VPNDevice).

### <a name="create-the-vpn-connection"></a>Criar a ligação VPN

Agora, crie a ligação de VPN de site a site entre o gateway de rede virtual e o dispositivo VPN no local.

1. Abra a página de rede virtual no **redes virtuais** > **descrição geral** > **dispositivos ligados**. 
2. Clique em **ligações** > **+ adicionar**.
3. No **adicionar ligação** > **nome**, especifique um nome para a ligação.
4. No **tipo de ligação**, selecione **Site a site (IPSec)**.
5. O **gateway de rede Virtual** valor é fixo porque está a ligar deste gateway.
6. No **gateway de rede Local**, especifique o gateway de rede local que criou.
7. No **chave partilhada**, especifique a chave que corresponde ao valor de que está a utilizar para o dispositivo VPN local no local. O exemplo utiliza "abc123", mas pode e (deve) utilizar algo mais complexo. O importante é que o valor que especificar aqui seja igual ao valor que especificou quando configurou o seu dispositivo VPN.
8. Os valores para **subscrição**, **grupo de recursos**, e **localização** sejam corrigidos.

    ![Ligação VPN](media/migrate-scenarios-lift-and-shift/vpn-connection.png) 

4. Clique em **OK** para criar a ligação. Verá "A criar ligação" flash no ecrã.
5. Depois de criar a ligação, vê-la no **ligações** página do gateway de rede virtual. Serão enviados para o estado de desconhecido > ligação > com êxito.

### <a name="verify-the-vpn-connection"></a>Verificar a ligação VPN

No portal do Azure, pode ver o estado da ligação de um Gateway de VPN do Resource Manager ao navegar para essa ligação. 

1. Clique em **todos os recursos**e navegue para o gateway de rede virtual.
2. Na página de gateway de rede virtual, clique em **ligações**. Pode ver o estado de cada ligação.
3. Clique no nome da ligação e ver mais informações no **Essentials**. O estado é 'Com êxito' e 'Ligado' depois de efetuar uma ligação com êxito.

Agora, verifique se consegue ligar à VM do SQL Server através de VPN. Utilize uma ligação de ambiente de trabalho remoto e ligar para o endereço IP da VM.



## <a name="step-2-prepare-an-azure-sql-managed-instance"></a>Passo 2: Preparar uma instância de gerido de SQL do Azure

### <a name="set-up-a-virtual-network"></a>Configurar uma rede virtual

Para este cenário, terá de criar outra rede virtual que está dedicado à instância de gerida do Azure SQL.  Tenha em atenção os seguintes requisitos:

- Ao criar a instância gerida precisa de uma sub-rede dedicada.
- A sub-rede tem de estar vazio e não contém qualquer outro serviço de nuvem e não pode ser uma sub-rede de gateway. Depois da instância gerida é criada, não pode adicionar recursos à sub-rede.
- A sub-rede não pode ter um NSG associado.
- A sub-rede tem de ter uma tabela de rota de utilizador (UDR) com Internet de salto seguinte 0.0.0.0/0 como a rota apenas atribuída. 
- DNS personalizado opcional: Se não for especificado DNS personalizado na VNet, endereço IP do Azure recursiva resoluções (por exemplo, 168.63.129.16) tem de ser adicionado à lista. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- A sub-rede não pode ter um ponto final de serviço (armazenamento ou SQL) associado ao mesmo. Pontos finais de serviço devem ser desativados na rede virtual.
- A sub-rede tem de ter pelo menos 16 endereços IP. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) sobre o dimensionamento a sub-rede de instância gerido.

Configure a rede virtual da seguinte forma:

1.  No portal do Azure, clique em **Criar um recurso**. 
2. Selecione **redes** > **rede Virtual**.
3. No **rede Virtual** > **selecionar um modelo de implementação**, selecione **Resource Manager** > **criar**.
4. No **criar rede virtual** > **nome**, introduza um nome de rede exclusivos. Para o nosso cenário **ContosoVNETSQLMI**.
5. No **espaço de endereços**, adicione o intervalo de endereços IP mostrado abaixo. O intervalo não pode sobrepor-se com os seus no local e o espaço de endereços ContosoVNET.
6. No **grupo de recursos**, crie um novo grupo ou selecione um existente. Neste cenário estamos a utilizar **ContosoRG**.
7. No **localização**, selecione a região na qual pretende criar a rede virtual. Estamos a utilizar **EUA Leste 2**.
8. No **sub-rede**, adicionar o primeiro nome de sub-rede e o intervalo de endereços. Criámos o **ManagedInstances** sub-rede.
9. Desative pontos finais de serviço.

    ![Rede de instância geridos](media/migrate-scenarios-lift-and-shift/network-mi.png)

10. Depois da rede é implementada, terá de alterar as definições de DNS. Neste cenário, o DNS é primeiro aponta para um controlador de domínio na sub-rede de identidade com um endereço IP privado estático (172.16.3.4) e, em seguida, a resolução de DNS do Azure 168.63.129.16.

    ![Rede de instância geridos](media/migrate-scenarios-lift-and-shift/network-mi2.png)


### <a name="set-up-routing"></a>Configurar o encaminhamento

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure. Clique em **tabela de rotas** > **criar na tabela de rotas** página.
2. No **criar tabela de rotas** > **nome**, especifique um nome para a tabela.
3. Selecione a sua subscrição, o grupo de recursos e localização. Deixe o BGP desativado e clique em **criar**.
    ![Tabela de rotas](media/migrate-scenarios-lift-and-shift/route-table.png)

4. Depois de criar a tabela de rotas, abra-o e clique em **rotas** > **+ adicionar**.
5. No **adicionar rota** > **nome**, especifique um nome de rota.
6. No **prefixo de endereço**, especifique 0.0.0.0/0.
7. No **próximo salto tipo**, selecione **Internet**. Em seguida, clique em **OK**.

     ![Tabela de rotas](media/migrate-scenarios-lift-and-shift/route-table2.png)


### <a name="apply-the-route-to-the-managed-instance-subnet"></a>Aplicar a rota para a sub-rede de instância gerido

1. Abra a rede virtual que criou. Clique em **sub-redes** > nome da sub-rede.
2. Clique em **tabela de rotas** > nome da tabela. Em seguida, clique em **Guardar**.

     ![Tabela de rotas](media/migrate-scenarios-lift-and-shift/route-table3.png)

### <a name="create-a-managed-instance"></a>Criar uma instância gerida

1. Clique em **crie um recurso**, localize **geridos instância**e selecione **Azure instância SQL Server da base de dados geridos (pré-visualização)**.
2. Clique em **Criar**.
3. No **instância do SQL geridos**, selecione a sua subscrição e certifique-se de que **termos de pré-visualização** mostrar **aceites**.
4. No **nome da instância geridos**, especifique um nome. 
5. Especifique um nome de utilizador e palavra-passe como administrador para a instância.
6. Selecione o grupo de recursos, localização e a rede virtual para a instância.
7. Clique em **escalão de preço** para armazenamento e computação de tamanho. Por predefinição, a instância obtém 32 GB de espaço de armazenamento livre de encargos (Abril de 2018).
8. Especifique o armazenamento e o número de núcleos virtuais.
9. Clique em **aplicar** para guardar as definições e **criar** para implementar a instância gerida. Para ver o estado da implementação, clique em **notificações**, e **implementação em curso**.

    ![Instância gerida](media/migrate-scenarios-lift-and-shift/mi-1.png)

Depois da instância geridos conclusão da implementação, dois novos recursos aparecem no grupo de recursos de ContosoRG: Cluster Virtual para as instâncias geridas e a instância do SQL Server geridos. Ambos estão na localização EUA Leste 2.

![Instância gerida](media/migrate-scenarios-lift-and-shift/managed-instance2.png)


## <a name="step-3-get-a-sas-uri-for-dms"></a>Passo 3: Obter um URI de SAS para o DMS

Obter um URI de SAS para que o DMS podem aceder ao contentor de conta de armazenamento, para carregar os ficheiros de cópia de segurança que são utilizados para migrar as bases de dados para a base de dados geridas por instância de SQL do Azure. 

1. Abrir o Explorador de Armazenamento (Pré-visualização).
2. No painel esquerdo, expanda a conta de armazenamento que contém o contentor do blob para o qual pretende obter uma SAS. Expanda a conta de armazenamento **contentores de BLOBs**.
3. O contentor com o botão direito e selecione **obter assinatura de acesso partilhado**.

     ![SAS](media/migrate-scenarios-lift-and-shift/sas-1.png)

4. No **assinatura de acesso partilhado**, especifique a política, as datas de início e de expiração, fuso horário e níveis de que pretende para o recurso de acesso. Em seguida, clique em **Criar**.

    ![SAS](media/migrate-scenarios-lift-and-shift/sas-2.png)

5. Uma segunda caixa de diálogo apresenta o contentor de blob, juntamente com o URL e QueryStrings pode utilizar para aceder ao recurso de armazenamento. Selecione **cópia** para copiar os valores. Mantê-las num local seguro. Necessário quando configurou o DMS.

      ![SAS](media/migrate-scenarios-lift-and-shift/sas-3.png)  

## <a name="step-4-prepare-dms"></a>Passo 4: Preparar DMS

Registar o fornecedor de migração de base de dados e criar uma instância.

### <a name="register-the-microsoftdatamigration-resource-provider"></a>Registar o fornecedor de recursos Microsoft.DataMigration

1. Na sua subscrição, selecione **fornecedores de recursos**. 
2. Procure "migração" e, em seguida, para a direita da Microsoft.DataMigration, selecione **registar**. 


### <a name="create-an-instance"></a>Criar uma instância

1. Selecione **+ criar um recurso**, procure o 'Serviço de migração de base de dados do Azure' e, em seguida, selecione **serviço de migração de base de dados do Azure** na lista pendente.
2. No ecrã de serviço de migração de base de dados do Azure (pré-visualização), selecione **criar**.
3. Especifique um nome para o serviço, a subscrição, grupo de recursos, rede virtual e o escalão de preço.
4. Selecione **criar** ao criar o serviço.

    ![DMS](media/migrate-scenarios-lift-and-shift/dms-instance.png)  




## <a name="step-5-prepare-azure-for-the-site-recovery-service"></a>Passo 5: Preparar do Azure para o serviço de recuperação de sites

### <a name="set-up-a-virtual-network"></a>Configurar uma rede virtual

Precisará de uma rede Azure no qual as VMs do Azure estarão localizadas quando são criados após a ativação pós-falha e uma conta de armazenamento do Azure na qual os dados replicados são guardados.

- Para efeitos deste cenário, iremos utilizar a rede do Azure que criámos anteriormente, DMS.
- Tenha em atenção que a rede que utiliza tem de estar na mesma região que o Cofre de recuperação de sites.


### <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure

Recuperação de site replica os dados VM para o armazenamento do Azure. As VMs do Azure são criadas a partir do armazenamento quando fizer a ativação pós-falha do local para o Azure.

1. No menu [portal do Azure](https://portal.azure.com), selecione **Novo** > **Armazenamento** > **Conta de armazenamento**.
2. Em **Criar conta de armazenamento**, introduza um nome para a conta. Nestes tutoriais, utilize o nome **contosovmsacct1910171607**. O nome tem de ser exclusivo no Azure e ter entre 3 e 24 carateres compostos apenas por números e letras minúsculas.
3. Em **Modelo de implementação**, selecione **Resource Manager**.
4. Em **Tipo de conta**, selecione **Fins gerais**. Em **Desempenho**, selecione **Padrão**. Não selecione o armazenamento de blobs.
5. Em **Replicação**, selecione o **Armazenamento georredundante com acesso de leitura** predefinido para redundância de armazenamento.
6. Em **Subscrição**, selecione a subscrição na qual pretende criar a nova conta de armazenamento.
7. Em **Grupo de recursos**, introduza um novo grupo de recursos. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Nestes tutoriais, utilize o nome **ContosoRG**.
8. Em **Localização**, selecione a localização geográfica para a conta de armazenamento. A conta de armazenamento tem de estar na mesma região que o cofre dos Serviços de Recuperação. Para este cenário, estamos a utilizar o **EUA Leste 2** região.

   ![Create a storage account](media/migrate-scenarios-lift-and-shift/create-storageacct.png)

9. Selecione **Criar** para criar a conta de armazenamento.

### <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

1. No portal do Azure, selecione **Criar um recurso** > **Monitorização + Gestão** > **Backup e Site Recovery**.
2. Em **Nome**, introduza um nome amigável para identificar o cofre. Neste tutorial, utilize **ContosoVMVault**.
3. Em **Grupo de recursos**, selecione o grupo de recursos existente com o nome **contosoRG**.
4. No **localização**, introduza a região do Azure **EUA Leste 2**.
5. Para aceder rapidamente ao cofre a partir do dashboard, selecione **Afixar ao dashboard** > **Criar**.
O novo cofre é apresentado em **Dashboard** > **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.

## <a name="step-6-prepare-on-premises-vmware-for-site-recovery"></a>Passo 6: Preparar VMware no local para a recuperação de Site

Para preparar a recuperação de Site, aqui VMware que precisa de fazer:

- Preparar uma conta no servidor vCenter ou no anfitrião vSphere ESXi para automatizar a deteção de VMs
- Preparar uma conta para a instalação automática do serviço de Mobilidade em VMs de VMware
- Se pretende ligar-se para VMs do Azure após a ativação pós-falha, prepare VMs no local.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detete automaticamente as VMs. É necessária, pelo menos, uma conta só de leitura.
- Orquestrar a replicação, a ativação pós-falha e a reativação pós-falha. Precisa de uma conta que pode executar operações, tais como criar e a remoção de discos e a ativar as VMs.

Crie a conta da seguinte forma:

1. Para utilizar uma conta dedicada, crie uma função ao nível do vCenter. Dê um nome à função, como **Azure_Site_Recovery**.
2. Atribua à função as permissões resumidas na tabela abaixo.
3. Crie um utilizador no servidor vCenter ou no anfitrião vSphere. Atribua a função ao utilizador.

**Tarefa** | **Função/Permissões** | **Detalhes**
--- | --- | ---
**Deteção de VMs** | Pelo menos um utilizador só de leitura<br/><br/> Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua a função **Sem acesso** com **Propagar ao objeto subordinado** aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).
**Replicação, ativação pós-falha e reativação pós-falha completas** |  Criar uma função (Azure_Site_Recovery) com as permissões necessárias e, em seguida, atribuir a função a um grupo ou utilizador de VMware<br/><br/> Objeto Data Center –> Propagar ao Objeto Subordinado, função=Azure_Site_Recovery<br/><br/> Arquivo de Dados -> Alocar espaço, navegar no arquivo de dados, operações de ficheiro de baixo nível, remover ficheiros, atualizar ficheiros de máquinas virtuais<br/><br/> Rede -> Atribuição de rede<br/><br/> Recursos -> Atribuir VM a agrupamento de recursos, migrar VMs desligadas, migrar VMs ligadas<br/><br/> Tarefas -> Criar tarefa, atualizar tarefa<br/><br/> Máquina virtual -> Configuração<br/><br/> Máquina virtual -> Interagir -> responder a perguntas, ligação de dispositivos, configurar suportes de dados em CD, configurar suportes de dados em disquete, desligar, ligar, instalação de ferramentas de VMware<br/><br/> Máquina virtual -> Inventário -> Criar, registar, anular o registo<br/><br/> Máquina virtual -> Aprovisionamento -> Permitir transferência de máquinas virtuais, permitir carregamento de ficheiros de máquinas virtuais<br/><br/> Máquina virtual -> Instantâneos -> Remover instantâneos | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua a função **Sem acesso** com **Propagar ao objeto subordinado** aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de Mobilidade tem de ser instalado na VM que quer replicar.

- Recuperação de sites pode efetuar uma instalação push automática deste componente ao ativar a replicação para a VM.
- Para a instalação push automática, terá de preparar uma conta que a recuperação de sites irá utilizar para aceder a VM.
- Especificar esta conta ao configurar a replicação na consola do Azure.
- Precisa de um domínio ou conta local com permissões para instalar na VM.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha para o Azure poderá ligar à sua replicadas VMs no Azure da sua rede no local.

Para ligar às VMs do Windows via RDP após a ativação pós-falha, faça o seguinte:

1. Para aceder através da Internet, ative o protocolo RDP na VM local antes da ativação pós-falha. Certifique-se de que as regras TCP e UDP estão adicionadas para o **pública** perfil e que o RDP é permitido na **Firewall do Windows** > **aplicações permitidas** para todos os perfis.
2. Para aceder através da rede de VPNs, ative o protocolo RDP na máquina local. O protocolo RDP deve estar permitido em **Firewall do Windows** -> **Aplicações e funcionalidades permitidas** para redes de **Domínio e Privadas**.
3. Verifique se a política de SAN do sistema operativo está definida como **OnlineAll**. [Saiba mais](https://support.microsoft.com/kb/3031135).
4. Não devem haver atualizações do Windows pendentes na VM quando aciona uma ativação pós-falha. Se houverem, só poderá iniciar sessão na máquina virtual depois de a atualização estar concluída.
5. Na VM do Azure do Windows após a ativação pós-falha, consulte o **Diagnóstico de arranque**  para ver uma captura de ecrã da VM. Se não conseguir ligar-se, verifique se a VM está em execução e reveja estas [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).



## <a name="step-7-replicate-vms-with-site-recovery"></a>Passo 7: Replicar as VMs com a recuperação de Site

### <a name="select-a-replication-goal"></a>Selecionar um objetivo de replicação

1. Em **Cofres dos Serviços de Recuperação**, selecione o nome do cofre, **ContosoVMVault**.
2. Em **Introdução**, selecione Site Recovery. Em seguida, selecione **Preparar Infraestrutura**.
3. Em **Objetivo de proteção** > **Onde estão localizadas as máquinas virtuais**, selecione **No local**.
4. Em **Para onde pretende replicar as máquinas**, selecione **Para o Azure**.
5. Em **As suas máquinas estão virtualizadas**, selecione **Sim, com o VMware vSphere Hypervisor**. Em seguida, selecione **OK**.

    ![Modelo OVF](./media/migrate-scenarios-lift-and-shift/replication-goal.png)


### <a name="confirm-deployment-planning"></a>Confirme o planeamento da implementação

No **planeamento da implementação**, clique em **Sim, ter efetuado esta**, na lista pendente.

### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem


Para configurar o ambiente de origem, precisará de uma única máquina de elevada disponibilidade no local para alojar componentes do Site Recovery no local. Componentes incluem o servidor de configuração e o servidor de processos.

- O servidor de configuração coordena as comunicações entre o local e o Azure, e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.


Para configurar o servidor de configuração como uma VM de VMware altamente disponível:

- Transferir um modelo preparado de abrir virtualização formato OVF ().
- Importe o modelo para VMware para criar a VM.
- Configurar o servidor de configuração e registá-lo no cofre. 

Após o registo, o Site Recovery deteta as VMs VMware no local.



#### <a name="download-the-vm-template"></a>Transferir o modelo de VM

1. No cofre, vá para **Preparar Infraestrutura** > **Origem**.
2. Em **Preparar origem**, selecione **+Servidor de configuração**.

    ![Transferir modelo](./media/migrate-scenarios-lift-and-shift/new-cs.png)

4. Em **Adicionar Servidor**, verifique se o **Servidor de configuração para VMware** é apresentado no **Tipo de servidor**.
2. Transfira o modelo OVF para o servidor de configuração.

    ![Transferir OVF](./media/migrate-scenarios-lift-and-shift/add-cs.png)

  > [!TIP]
  Pode transferir a versão mais recente do modelo do servidor de configuração diretamente do [Centro de Transferências da Microsoft](https://aka.ms/asrconfigurationserver).

#### <a name="import-the-template-in-vmware"></a>Importar o modelo no VMware

1. Inicie sessão no VMware vCenter server com o VMWare vSphere Client.
2. No menu **Ficheiro**, selecione **Implementar o Modelo OVF**, para iniciar o assistente para Implementar o Modelo OVF. 

     ![Modelo OVF](./media/migrate-scenarios-lift-and-shift/vcenter-wizard.png)

3. Em **Selecionar origem**, introduza a localização do OVF transferido.
4. Em **Rever detalhes**, selecione **Seguinte**.
5. Em **Selecionar nome e pasta** e **Selecionar configuração**, aceite as predefinições.
6. Em **Selecionar armazenamento**, para obter o melhor desempenho, selecione **Thick Provision Eager Zeroed** em **Selecionar formato de disco virtual**.
7. Nas restantes páginas do assistente, aceite as predefinições.
8. Em **Pronto para concluir**:

    * Para configurar a VM com as predefinições, selecione **Ligar após a implementação** > **Concluir**.

    * Se pretender adicionar outra interface de rede, desmarque **Ligar após a implementação**. Em seguida, selecione **Concluir**. Por predefinição, o modelo do servidor de configuração é implementado com um único NIC. Pode adicionar mais NICs após a implementação.



#### <a name="register-the-configuration-server"></a>Registar o servidor de configuração 

1. A partir da consola do VMWare vSphere Client, ligue a VM.
2. A VM arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
3. Após a conclusão da instalação, inicie sessão na VM como administrador.
4. Da primeira vez que iniciar sessão, a Ferramenta de Configuração do Azure Site Recovery é iniciada.
5. Introduza um nome que será utilizado para registar o servidor de configuração no Site Recovery. Em seguida, selecione **Seguinte**.

    ![Modelo OVF](./media/migrate-scenarios-lift-and-shift/config-server-register1.png)

6. A ferramenta verifica se a VM pode ligar ao Azure. Depois de a ligação estar estabelecida, selecione **Iniciar sessão** para iniciar sessão na sua subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.

    ![Modelo OVF](./media/migrate-scenarios-lift-and-shift/config-server-register2.png)

7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Inicie sessão na máquina novamente. O assistente de gestão do servidor de configuração inicia automaticamente.

#### <a name="configure-settings-and-add-the-vmware-server"></a>Configurar as definições e adicione o servidor VMware

1. No assistente de gestão do servidor de configuração, selecione **Configurar a conectividade** e, em seguida, selecione o NIC para receber o tráfego de replicação. Em seguida, selecione **Guardar**. Não é possível alterar esta definição depois de estar configurada.
2. Em **Selecionar cofre dos Serviços de Recuperação**, selecione a sua subscrição do Azure e o grupo de recursos e cofre relevantes.

    ![Cofre](./media/migrate-scenarios-lift-and-shift/cswiz1.png) 

3. Em **Instalar software de terceiros**, aceite o contrato de licença. Selecione **Transferir e Instalar** para instalar o Servidor MySQL.
4. Selecione **Instalar VMware PowerCLI**. Certifique-se de que todas as janelas do browser estão fechadas antes de efetuar este procedimento. Em seguida, selecione **Continuar**.
5. Em **Validar configuração da aplicação**, os pré-requisitos são verificados antes de continuar.
6. Em **Configurar servidor vCenter Server/vSphere ESXi**, introduza o FQDN ou endereço IP do servidor vCenter ou anfitrião vSphere onde estão localizadas as VMs que pretende replicar. Introduza a porta em que o servidor está a escutar. Introduza um nome amigável a utilizar para o servidor VMware no cofre.
7. Introduza as credenciais que o servidor de configuração irá utilizar para ligar ao servidor VMware. O Site Recovery utiliza estas credenciais para detetar automaticamente as VMs VMware que estão disponíveis para replicação. Selecione **Adicionar** e, em seguida, selecione **Continuar**.

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

8. Em **Configurar as credenciais da máquina virtual**, introduza o nome de utilizador e a palavra-passe a utilizar para instalar automaticamente o Serviço de Mobilidade nas máquinas, quando a replicação estiver ativada. Para computadores Windows, a conta precisa de privilégios de administrador local nos computadores que pretende replicar. 

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

7. Selecione **Finalizar configuração** para concluir o registo. 
8. Após a conclusão do registo, no portal do Azure, certifique-se de que o servidor de configuração e o servidor VMware estão listados na página **Origem** do cofre. Em seguida, selecione **OK** para configurar as definições de destino.
9. O Site Recovery liga-se aos servidores VMware com as definições especificadas e deteta as VMs.

> [!NOTE]
> Pode demorar 15 minutos ou mais para o nome da conta aparecer no portal. Para atualizar imediatamente, selecione **Servidores de Configuração** > ***nome do servidor*** > **Atualizar Servidor**.

### <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e verifique os recursos de destino.

1. No **preparar a infraestrutura** > **destino**, selecione a subscrição do Azure que pretende utilizar.
2. Para o modelo de implementação de destino, selecione **do Azure Resource Manager**.

3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.


### <a name="create-a-replication-policy"></a>Criar uma política de replicação

1. No **preparar a infraestrutura** > **as definições de replicação** > **política de replicação**, clique em **crie e associe** .
1. Em **Criar política de replicação**, introduza o nome de política **VMwareRepPolicy**.
2. Em **Limiar de RPO**, utilize a predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
3. Em **Retenção do ponto de recuperação**, utilize a predefinição de 24 horas como a duração da janela de retenção para cada ponto de recuperação. Para este tutorial, utilize 72 horas. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
4. Em **Frequência de instantâneos consistentes com a aplicação**, utilize a predefinição de 60 minutos para a frequência com que os instantâneos consistentes com a aplicação são criados. Selecione **OK** para criar a política.

    ![Criar política de replicação](./media/migrate-scenarios-lift-and-shift/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/migrate-scenarios-lift-and-shift/replication-policy2.png)



### <a name="enable-replication"></a>Ativar a replicação

Agora inicie replicar as VMs. Recuperação de site instala o serviço de mobilidade em cada VM ao ativar a replicação para o mesmo. 


1. Selecione **Replicar aplicação** > **Origem**.
2. Em **Origem**, selecione o servidor de configuração.
3. Em **Tipo de máquina**, selecione **Máquinas Virtuais**.
4. Em **vCenter/vSphere Hypervisor**, selecione o servidor vCenter que gere o anfitrião vSphere ou selecione o anfitrião.
5. Selecione o servidor de processos (servidor de configuração). Em seguida, selecione **OK**.

    ![Ativar a replicação](./media/migrate-scenarios-lift-and-shift/enable-replication1.png)

1. Em **Destino**, selecione a subscrição e o grupo de recursos no qual pretende criar as VMs de ativação pós-falha. Selecione o modelo de implementação que pretende utilizar no Azure (clássico ou Resource Manager) para as VMs de ativação pós-falha.
2. Selecione a conta de armazenamento do Azure que pretende utilizar para replicar os dados.
3. Selecione a rede e a sub-rede do Azure às quais as VMs do Azure se ligam quando forem criadas após a ativação pós-falha.
4. Selecione **Configurar agora para as máquinas selecionadas** para aplicar a definição de rede para todas as máquinas selecionadas para proteção. Selecione **Configurar mais tarde** para selecionar a rede do Azure por máquina.
5. Selecione a sub-rede na rede virtual. Em seguida, selecione **OK**.

    ![Ativar a replicação](./media/migrate-scenarios-lift-and-shift/enable-replication2.png)

6. No **máquinas virtuais** > **selecionar máquinas virtuais**, selecione a VM (WEBVM) que pretende. Só pode selecionar máquinas para as quais a replicação pode ser ativada. 

    ![Ativar a replicação](./media/migrate-scenarios-lift-and-shift/enable-replication3.png)

7. Para monitorizar as VMs que adiciona, verifique a última hora de deteção de VMs em **Servidores de Configuração** > **Último Contacto Em**. Para adicionar VMs sem aguardar a deteção agendada, realce o servidor de configuração (não o selecione) e selecione **Atualizar**. Pode demorar 15 minutos ou mais tempo para as alterações produzirem efeitos e aparecerem no portal.
8. Em **Propriedades** > **Configurar propriedades**, selecione a conta que o servidor de processos irá utilizar para instalar automaticamente o Serviço de Mobilidade na máquina. 

    ![Ativar a replicação](./media/migrate-scenarios-lift-and-shift/enable-replication4.png)

9. Em **Definições de replicação** > **Configurar as definições de replicação**, certifique-se de que a política de replicação correta está selecionada.
10. Selecione **Ativar Replicação**.

    ![Ativar a replicação](./media/migrate-scenarios-lift-and-shift/enable-replication5.png)

11. Controlar o progresso do **ativar proteção** da tarefa no **definições** > **tarefas** > **tarefas de recuperação de Site**. 
12. Depois do **finalizar proteção** tarefa é executada, a máquina está preparada para ativação pós-falha e é apresentado na **descrição geral** > **Essentials**.

    ![Essentials](./media/migrate-scenarios-lift-and-shift/essentials.png)


## <a name="step-8-migrate-the-database-with-dms"></a>Passo 8: Migrar a base de dados com o DMS

Criar um projeto DMS e executar a migração.


### <a name="create-a-database-migration-project"></a>Criar um projeto de migração de base de dados

1. Localize o recurso DMS no portal do Azure e abri-lo.
2. Selecione **+ novo projeto de migração**.
3. No **novo projeto de migração**, especifique um nome para o projeto.
4. Em **Source server type** (Tipo de servidor de origem), selecione **SQL Server**. No **tipo de servidor de destino**, selecione **da base de dados geridas por instância de SQL do Azure**.
5. Clique em **criar** para criar o projeto.
6. No **origem detalhes**, especifique os detalhes de ligação para o local de origem do SQL Server. Clique em **Guardar**.
7. No **detalhes de destino**, especifique os detalhes de ligação para o destino, o que é pré-aprovisionada Azure instância do SQL da base de dados geridos para que a base de dados no local será migrada. Em seguida, clique em **Guardar**.
8. No **resumo de projeto**, reveja e verifique os detalhes associados ao projeto de migração.

    ![Projeto DMS](./media/migrate-scenarios-lift-and-shift/dms-project.png)

### <a name="migrate-the-database"></a>Migrar a base de dados

1. Depois de criar o projeto, é apresentado o Assistente de migração.
2. Especifique as credenciais para os servidores de origem e de destino e clique em **guardar**. O assistente tenta iniciar sessão no SQL Server no local.

    ![Assistente DMS](./media/migrate-scenarios-lift-and-shift/dms-wiz1.png)

3. No **mapa para bases de dados de destino**, selecione a base de dados de origem que pretende migrar. Em seguida, clique em **Guardar**.

    ![Assistente DMS](./media/migrate-scenarios-lift-and-shift/dms-wiz2.png)

4. No **detalhes de destino**, selecione **sei a minha detalhes de destino**. Forneça o nome DNS do seu geridos as instâncias SQL Server, juntamente com as credenciais. Em seguida, clique em **Guardar**.

    ![Assistente DMS](./media/migrate-scenarios-lift-and-shift/dms-wiz3.png)

5. No projeto guardado, selecione **+ nova atividade**. Em seguida, selecione **executar migração**.
6. Quando lhe for pedido, introduza as credenciais para os servidores de origem e de destino. Em seguida, clique em **Guardar**.
7. No **mapa para bases de dados de destino**, selecione a base de dados de origem que pretende migrar. Em seguida, clique em **guardar**
8. No **configurar definições de migração** > **localização de cópia de segurança do servidor**, especifique a partilha de rede que criou na máquina no local. O DMS leva cópias de segurança de origem para esta partilha.  Lembre-se de que a conta de serviço que executa a instância do SQL Server de origem têm de ter privilégios de escrita nesta partilha.
9. Especifique o nome de utilizador e palavra-passe que podem representar DMS para carregar os ficheiros de cópia de segurança para o contentor de armazenamento do Azure, para restauro.
10. Especifique o URI de SAS que fornece o DMS com acesso ao contentor de conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança, e que é utilizada para a migração para a base de dados geridas por instância de SQL do Azure.  Em seguida, clique em **Guardar**.
11. No **resumo de migração**, especifique um nome para a atividade de migração > **executar a migração**.
12. No projeto > **descrição geral**, monitorizar o estado de migração. Após a conclusão da migração, certifique-se de que as bases de dados de destino existem na instância de gerido.


## <a name="step-9-migrate-vms-with-site-recovery"></a>Passo 9: Migrar VMs com a recuperação de Site

Sugerimos que execute uma ativação pós-falha de teste para se certificar de que tudo está a funcionar conforme esperado, antes de executar uma migração completa. Quando executa uma ativação pós-falha de teste, ocorre o seguinte:

1. Verificação de um pré-requisitos é executado para se certificar de que todas as condições necessárias para a migração estão em vigor.
2. A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
3. É criada uma VM do Azure com base nos dados processados no passo anterior.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

1. Em **Itens Protegidos**, clique em **Itens Replicados** > VM.
2. Nas propriedades da VM, clique em **+ ativação pós-falha de teste**.

    ![Ativação pós-falha de teste](./media/migrate-scenarios-lift-and-shift/test-failover.png)

3. Selecione **mais recentes processados**, para efetuar a ativação pós-falha da VM para o ponto mais recente no tempo. O carimbo de data/hora é apresentado. Com esta opção, não é despendido tempo a processar os dados, pelo que oferece um RTO (objetivo de tempo de recuperação) baixo.
2. Em **Ativação Pós-Falha de Teste**, selecione a rede do Azure de destino para as VMs do Azure que serão ligadas após a ocorrência da ativação pós-falha.
3. Clique em **OK** para iniciar a ativação pós-falha. Pode controlar bin progresso cofre > **definições** > **tarefas** > **ativação pós-falha de teste**.
4. Após a conclusão da ativação pós-falha, a VM do Azure de réplica é apresentada no portal do Azure > **Máquinas Virtuais**. Verifique que a VM tem o tamanho adequado, está ligada à rede certa e está em execução. Deverá conseguir ligar-se à VM replicada no Azure agora.
5. Para eliminar as VMs do Azure criadas durante a ativação pós-falha de teste, clique em **Limpar ativação pós-falha de teste** na VM. Em **Notas**, registe e guarde todas as observações associadas à ativação pós-falha de teste.

### <a name="migrate-the-machines"></a>Migrar as máquinas

Depois de ter verificado que a ativação pós-falha de teste funciona conforme esperado, crie um plano de recuperação para migrar para o Azure.

### <a name="create-a-recovery-plan"></a>Criar um plano de recuperação

1. No cofre, selecione **planos de recuperação (recuperação de sites)** > **+ plano de recuperação**.
2. No **criar plano de recuperação**, especifique um nome para o plano.
3. Escolha o servidor de configuração de origem e o Azure como destino. Selecione **Resource Manager** do modelo de implementação. A localização de origem tem de ter máquinas que estão ativadas para ativação pós-falha e recuperação. 
4. No **selecionar itens**, adicionar as máquinas (WEBVM) para o plano. Em seguida, clique em **OK**.
5. Clique em **OK** para criar o plano.

    ![Plano de recuperação](./media/migrate-scenarios-lift-and-shift/recovery-plan1.png)

### <a name="run-a-failover"></a>Executar uma ativação pós-falha

1. No **planos de recuperação**, clique o plano > **ativação pós-falha**.
2. No **ativação pós-falha** > **ponto de recuperação**, selecione o ponto de recuperação mais recente.
3. A definição da chave de encriptação não é relevante para este cenário.
4. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha**. O Site Recovery tentará fazer um encerramento das máquinas virtuais de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Tarefas**.

    ![Ativação pós-falha](./media/migrate-scenarios-lift-and-shift/failover1.png)

5. Certifique-se de que a VM do Azure é apresentada no Azure conforme esperado.

    ![Ativação pós-falha](./media/migrate-scenarios-lift-and-shift/failover2.png)

6. Em **Itens replicados**, clique com o botão direito do rato na VM > **Concluir a Migração**. Desta forma, o processo de migração é concluído, a replicação para a VM é parada e a faturação do Site Recovery para a VM também é parada.

    ![Ativação pós-falha](./media/migrate-scenarios-lift-and-shift/failover3.png)

### <a name="update-the-connection-string"></a>Atualizar a cadeia de ligação

É o último passo no processo de migração ao atualizar a cadeia de ligação da aplicação para apontar para a base de dados migrado em execução no seu MI do SQL Server.

1. Localizar a nova cadeia de ligação no portal do Azure, clicando em **definições** > **cadeias de ligação**.

    ![Ativação pós-falha](./media/migrate-scenarios-lift-and-shift/failover4.png)  

2. Tem de atualizar esta cadeia com o nome de utilizador e palavra-passe para o MI do SQL Server.
3. Depois da cadeia é configurada, é necessário substituir a cadeia de ligação atual no ficheiro Web. config da aplicação.
4. Depois de atualizar o ficheiro e guardá-lo, reinicie o IIS no WEBVM. Isto pode ser feito utilizando o IISRESET /RESTART de uma linha de comandos.
5. Após o reinício do IIS, a aplicação irá agora utilizar a base de dados em execução no seu MI do SQL Server.
6. Nesta fase pode ser encerrado a SQLVM máquina no local e a migração estar concluída.



## <a name="conclusion"></a>Conclusão

Neste cenário:

> [!div class="checklist"]
> * Migrar a nossa base de dados no local para uma instância de gerida do SQL do Azure com o DMS.
> * Migrar VMs nosso no local para VMs do Azure, com o serviço do Azure Site Recovery.
