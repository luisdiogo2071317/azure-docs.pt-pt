---
title: Associar o tempo de execução de integração de SSIS do Azure a uma rede virtual | Microsoft Docs
description: Saiba como associar o tempo de execução de integração do Azure-SSIS a uma rede virtual do Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: eae350f751788eb09271e70f71f79b12e27c4e16
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061406"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Aderir a um tempo de execução de integração do Azure-SSIS a uma rede virtual
Associe o seu tempo de execução de integração do Azure-SSIS (IR) a uma rede virtual do Azure nos seguintes cenários: 

- Pretende ligar a arquivos de dados no local a partir de pacotes de SSIS em execução no integration runtime do Azure-SSIS. 

- Estão a alojar o SQL Server Integration Services (SSIS) catálogo base de dados na base de dados do Azure SQL com pontos finais do serviço de rede virtual/gerido instância (pré-visualização). 

 O Azure Data Factory permite-lhe associar o seu tempo de execução de integração do Azure SSIS para uma rede virtual criadas através do modelo de implementação clássica ou o modelo de implementação Azure Resource Manager. 

## <a name="access-to-on-premises-data-stores"></a>Acesso aos arquivos de dados no local
Se acedem a pacotes SSIS arquivos de dados de nuvem pública apenas, não precisa de associar a resposta a incidentes SSIS do Azure a uma rede virtual. Se acedem a pacotes SSIS arquivos de dados no local, tem de associar a resposta a incidentes SSIS do Azure a uma rede virtual que está ligada à rede no local. 

Eis alguns pontos importantes a ter em atenção: 

- Se não houver nenhuma existente rede virtual ligada à sua rede no local, primeiro crie um [rede virtual do Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) ou um [rede virtual clássica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para a integração do Azure-SSIS tempo de execução para associar. Em seguida, configure um site para site [ligação de gateway VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) ou [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) ligação de rede virtual à sua rede no local. 

- Se existir um existente do Azure Resource Manager ou a rede virtual clássica ligados à sua rede no local na mesma localização que a sua resposta a incidentes SSIS do Azure, pode associar a IR nessa rede virtual. 

- Se existir uma rede virtual clássica existente ligada à sua rede no local numa localização diferente da sua resposta a incidentes SSIS do Azure, pode criar primeiro um [rede virtual clássica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para a sua resposta a incidentes SSIS do Azure associar. Em seguida, configure um [rede virtual clássica para clássica](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) ligação. Ou pode criar um [rede virtual do Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) para o tempo de execução de integração do Azure-SSIS associar. Em seguida, configure um [rede virtual do clássico para o Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) ligação. 
 
- Se existir um existente do Azure Resource Manager rede virtual ligada à sua rede no local numa localização diferente da sua resposta a incidentes SSIS do Azure, pode criar primeiro um [rede virtual do Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) para sua SSIS do Azure IR para associar. Em seguida, configure uma ligação de rede virtual do Azure Resource Manager para o Azure Resource Manager. Em alternativa, pode criar um [rede virtual clássica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para a sua resposta a incidentes SSIS do Azure associar. Em seguida, configure um [rede virtual do clássico para o Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) ligação. 

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance-preview"></a>A base de dados SSIS catálogo na base de dados do Azure SQL com pontos finais do serviço de rede virtual/gerido instância (pré-visualização)
Se o catálogo SSIS está alojado na SQL Database do Azure com pontos finais do serviço de rede virtual ou instância geridos (pré-visualização), pode associar o seu IR SSIS do Azure para: 

- A mesma rede virtual 
- Uma rede virtual diferente que tenha uma ligação de rede de rede com o que é utilizada para a SQL Database do Azure com os pontos finais do serviço de rede virtual/gerido instância (pré-visualização) 

Se se associar a sua resposta a incidentes SSIS do Azure para a mesma rede virtual que a instância gerido, certifique-se que a resposta a incidentes SSIS do Azure numa sub-rede diferente, a instância de geridos. Se associar a resposta a incidentes SSIS do Azure para uma rede virtual diferente a instância geridos, recomendamos que o peering de rede virtual (que é limitada à mesma região) ou uma rede virtual à ligação de rede virtual. Consulte [ligar a aplicação à base de dados geridos instância do SQL Azure](../sql-database/sql-database-managed-instance-connect-app.md).

A rede virtual pode ser implementada através do modelo de implementação clássico ou modelo de implementação Azure Resource Manager.

As secções seguintes fornecem mais detalhes. 

## <a name="requirements-for-virtual-network-configuration"></a>Requisitos para a configuração de rede virtual
-   Certifique-se de que `Microsoft.Batch` é um fornecedor registado sob a subscrição da sua sub-rede da rede virtual que aloja o IR. SSIS do Azure Se estiver a utilizar rede virtual clássica, também associar `MicrosoftAzureBatch` para a função de contribuinte de Máquina Virtual clássico para essa rede virtual. 

-   Selecione a sub-rede adequada para alojar o IR. SSIS do Azure Consulte [seleciona a sub-rede](#subnet). 

-   Se estiver a utilizar o seu próprio servidor de serviços de nomes de domínio (DNS) na rede virtual, consulte o artigo [servidor dos serviços de nome de domínio](#dns_server). 

-   Se estiver a utilizar um grupo de segurança de rede (NSG) na sub-rede, consulte o artigo [grupo de segurança de rede](#nsg) 

-   Se estiver a utilizar o Azure Express Route ou configurar rotas definidas pelo utilizador (UDR), consulte [utilize Azure ExpressRoute ou de utilizador definidas rota](#route). 

-   Certifique-se de que o grupo de recursos da rede virtual pode criar e eliminar certos recursos de rede do Azure. Consulte [requisitos para o grupo de recursos](#resource-group). 

### <a name="subnet"></a> Selecione a sub-rede
-   Não selecione GatewaySubnet para implementar um tempo de execução para a integração de SSIS do Azure, porque esta se encontra dedicada para gateways de rede virtual. 

-   Certifique-se de que a sub-rede que selecionar tem suficiente espaço de endereços disponíveis para IR de SSIS do Azure a utilizar. Mantenha, pelo menos, 2 * número de nós de resposta a incidentes em endereços IP disponíveis. Azure reserva-se alguns endereços IP dentro de cada sub-rede e estes endereços não podem ser utilizados. Os endereços IP primeiro e últimos das sub-redes estão reservados para compatibilidade com o protocolo, juntamente com três endereços mais utilizados para serviços do Azure. Para obter mais informações, consulte [existem restrições sobre como utilizar estas sub-redes de endereços IP?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets). 

-   Não utilize uma sub-rede que exclusivamente está ocupada por outros serviços do Azure (por exemplo, instância SQL Server da base de dados geridos (pré-visualização), serviço de aplicações, etc.). 

### <a name="dns_server"></a> Servidor de serviços de nome de domínio 
Se precisar de utilizar o seu próprio servidor de serviços de nomes de domínio (DNS) numa rede virtual associada ao seu tempo de execução de integração de SSIS do Azure, certifique-se de que consegue resolver os nomes de anfitrião do Azure público (por exemplo, um nome de blob Storage do Azure, `<your storage account>.blob.core.windows.net`). 

Os seguintes passos são recomendados: 

-   Configure DNS personalizado para reencaminhar pedidos ao DNS do Azure. Pode reencaminhar os registos DNS não resolvidos para o endereço IP de resoluções de recursiva do Azure (168.63.129.16) no seu próprio servidor DNS. 

-   Configure o DNS personalizado como principal e o DNS do Azure como secundária para a rede virtual. Registe o endereço IP de resoluções de recursiva do Azure (168.63.129.16) como um servidor DNS secundário caso o próprio servidor DNS não está disponível. 

Para obter mais informações, consulte [resolução que utiliza o seu próprio servidor DNS de nomes](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a> Grupo de segurança de rede
Se precisar de implementar um grupo de segurança de rede (NSG) numa rede virtual associada ao seu tempo de execução de integração do Azure SSIS, permitir o tráfego de entrada/saída através das seguintes portas: 

| Direção | Protocolo de transporte | Origem | Intervalo de portas de origem | Destino | Intervalo de portas de destino | Comentários |
|---|---|---|---|---|---|---|
| Entrada | TCP | Internet | * | VirtualNetwork | 29876, 29877 (se a associação a IR para uma rede virtual do Azure Resource Manager) <br/><br/>10100, 20100, 30100 (se a associação a IR para uma rede virtual clássica)| O serviço fábrica de dados utiliza estas portas para comunicar com os nós do seu tempo de execução de integração do Azure-SSIS na rede virtual. <br/><br/> Se especificar um NSG ou não, o Data Factory sempre configura um NSG a nível de placas de interface de rede (NICs) anexados às máquinas virtuais que alojam o IR. SSIS do Azure Apenas o tráfego de entrada de endereços IP de fábrica de dados é permitido. Mesmo se abrir estas portas para o tráfego de Internet, o tráfego de endereços IP que não sejam endereços IP de fábrica de dados está bloqueado ao nível do NIC. |
| Saída | TCP | VirtualNetwork | * | Internet | 443 | Os nós do seu tempo de execução de integração do Azure-SSIS na rede virtual utilizam esta porta para aceder aos serviços do Azure, como o Storage do Azure e Event Hubs do Azure. |
| Saída | TCP | VirtualNetwork | * | Internet ou SQL Server | 1433, 11000 11999, 14000 14999 | Os nós do seu Azure SSIS integração tempo de execução a utilização de rede virtual, estas portas para aceder à SSISDB alojadas pelo servidor da SQL Database do Azure - esta finalidade não é aplicável a SSISDB alojada pela instância geridos (pré-visualização). |
||||||||

### <a name="route"></a> Utilizar o ExpressRoute do Azure ou a rota definida pelo utilizador
Pode ligar um [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuito à sua infraestrutura de rede virtual para expandir a sua rede no local para o Azure. 

Uma configuração comum consiste em utilizar a imposição do túnel (anunciar uma rota BGP, 0.0.0.0/0 para a rede virtual) que força o tráfego de Internet de saída do fluxo de rede virtual ao dispositivo de rede no local para inspeção e registo. Este fluxo de tráfego de quebras de conectividade entre o Azure SSIS IR na rede virtual com os serviços do Azure Data Factory dependentes. A solução é definir um (ou mais) [rotas definidas pelo utilizador (UDRs)](../virtual-network/virtual-networks-udr-overview.md) na sub-rede que contém o IR. SSIS do Azure Um UDR define rotas de sub-rede específica, que são cumpridas em vez da rota BGP. 

Em alternativa, pode definir as rotas definidas pelo utilizador (UDRs) para forçar o tráfego de Internet de saída da sub-rede que aloja a resposta a incidentes SSIS do Azure para outra sub-rede que aloja uma aplicação de rede Virtual como uma firewall ou um anfitrião de rede de Perímetro para inspeção e registo. 

Em ambos os casos, aplicar uma rota 0.0.0.0/0 com o tipo de próximo salto como **Internet** na sub-rede que aloja a resposta a incidentes SSIS do Azure, para que a comunicação entre o serviço fábrica de dados e o Azure SSIS é IR pode ter êxito. 

![Adicione uma rota](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Se estiver preocupados com a perder a capacidade de inspecionar o tráfego de Internet de saída dessa sub-rede, também pode adicionar uma regra NSG na sub-rede para restringir os destinos de saída para [endereços IP do Centro de dados do Azure](https://www.microsoft.com/download/details.aspx?id=41653). 

Consulte [este script do PowerShell](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) para obter um exemplo. Tem de executar o script semanalmente a par da lista de endereços IP de centro de dados do Azure. 

### <a name="resource-group"></a> Requisitos para o grupo de recursos
-   A resposta a incidentes SSIS do Azure tem de criar certos recursos de rede no mesmo grupo de recursos de rede virtual. Estes recursos incluem o seguinte:
    -   Um balanceador de carga do Azure, com o nome  *<Guid>- azurebatch cloudserviceloadbalancer*.
    -   Um Azure endereço IP público, com o nome  *<Guid>- azurebatch cloudservicepublicip*.
    -   Um grupo de segurança de trabalho de rede, com o nome  *<Guid>- azurebatch cloudservicenetworksecuritygroup*. 

-   Certifique-se de que não tem qualquer bloqueio de recursos no grupo de recursos ou subscrição à qual pertence a rede virtual. Se configurar um bloqueio de só de leitura ou de um bloqueio de eliminação, iniciar e parar a resposta a incidentes podem falhar ou bloquear. 

-   Certifique-se de que não tem uma política do Azure que impede que os seguintes recursos a ser criado sob o grupo de recursos ou subscrição à qual pertence a rede virtual: 
    -   Microsoft.Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft.Network/PublicIPAddresses 

## <a name="azure-portal-data-factory-ui"></a>Portal do Azure (IU da fábrica de dados)
Esta secção mostra como associar um tempo de execução do Azure-SSIS existente a uma rede virtual (clássico ou do Azure Resource Manager) utilizando o portal do Azure e a IU da fábrica de dados. Em primeiro lugar, terá de configurar a rede virtual corretamente antes de a associar a sua resposta a incidentes SSIS do Azure ao mesmo. Passar por uma das duas secções seguintes consoante o tipo da sua rede virtual (clássico ou do Azure Resource Manager). Em seguida, prossiga com a secção terceira para associar a sua resposta a incidentes SSIS do Azure para a rede virtual. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Utilizar o portal para configurar uma rede virtual do Azure Resource Manager
Terá de configurar uma rede virtual antes de pode associar uma resposta a incidentes SSIS do Azure ao mesmo. 

1. Inicie o Microsoft Edge ou o Google Chrome. Atualmente, a IU da fábrica de dados só é suportada nesses browsers da web. 

2. Inicie sessão no [portal do Azure](https://portal.azure.com). 

3. Selecione **mais serviços**. Filtrar e selecione **redes virtuais**. 

4. Filtrar e selecione a rede virtual na lista. 

5. No **rede Virtual** página, selecione **propriedades**. 

6. Selecione o botão Copiar para **ID de recurso** para copiar o ID de recurso para a rede virtual para a área de transferência. Guarde o ID da área de transferência no OneNote ou um ficheiro. 

7. Selecione **sub-redes** no menu da esquerda. Certifique-se de que o número de **endereços disponíveis** é maior do que os nós no seu tempo de execução de integração de SSIS do Azure. 

8. Certifique-se de que o fornecedor do Azure Batch está registado na subscrição do Azure com a rede virtual. Em alternativa, registe o fornecedor do Azure Batch. Se já tiver uma conta do Azure Batch na sua subscrição, em seguida, a sua subscrição está registada para o Azure Batch. (Se criar a resposta a incidentes SSIS do Azure no portal do Data Factory, o fornecedor do Azure Batch é automaticamente registado.) 

   a. No portal do Azure, selecione **subscrições** no menu da esquerda. 

   b. Selecione a sua subscrição. 

   c. Selecione **fornecedores de recursos** à esquerda e confirme que **Microsoft.Batch** é um fornecedor registado. 

   ![Confirmação de estado de "Registada"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Se não vir **Microsoft.Batch** na lista, para registá-lo, [criar uma conta do Azure Batch vazia](../batch/batch-account-create-portal.md) na sua subscrição. Pode eliminá-lo mais tarde. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Utilizar o portal para configurar uma rede virtual clássica
Terá de configurar uma rede virtual antes de pode associar uma resposta a incidentes SSIS do Azure ao mesmo. 

1. Inicie o Microsoft Edge ou o Google Chrome. Atualmente, a IU da fábrica de dados é suportada apenas em browsers estes web. 

2. Inicie sessão no [portal do Azure](https://portal.azure.com). 

3. Selecione **mais serviços**. Filtrar e selecione **redes virtuais (clássicas)**. 

4. Filtrar e selecione a rede virtual na lista. 

5. No **rede Virtual (clássica)** página, selecione **propriedades**. 

   ![ID de recurso de rede virtual clássica](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

6. Selecione o botão Copiar para **ID de recurso** para copiar o ID de recurso para a rede clássico para a área de transferência. Guarde o ID da área de transferência no OneNote ou um ficheiro. 

7. Selecione **sub-redes** no menu da esquerda. Certifique-se de que o número de **endereços disponíveis** é maior do que os nós no seu tempo de execução de integração de SSIS do Azure. 

   ![Número de endereços disponíveis na rede virtual](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

8. Associar **MicrosoftAzureBatch** para o **clássico contribuinte de Máquina Virtual** função para a rede virtual. 

    a. Selecione **(IAM) do controlo de acesso** no menu à esquerda e selecione **adicionar** na barra de ferramentas. 

    !["Controlo de acesso" e "Adicionar" botões](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. No **adicionar permissões** página, selecione **clássico contribuinte de Máquina Virtual** para **função**. Colar **ddbf3205-c6bd-46ae-8127-60eb93363864** no **selecione** caixa e, em seguida, selecione **do Microsoft Azure Batch** da lista de resultados da pesquisa. 

    ![Resultados da procura na página "Adicionar permissões"](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    c. Selecione **guardar** para guardar as definições e fechar a página. 

    ![Guardar as definições de acesso](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. Confirme que vê **do Microsoft Azure Batch** na lista de contribuintes. 

    ![Confirmar o acesso do Azure Batch](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

9. Certifique-se de que o fornecedor do Azure Batch está registado na subscrição do Azure com a rede virtual. Em alternativa, registe o fornecedor do Azure Batch. Se já tiver uma conta do Azure Batch na sua subscrição, em seguida, a sua subscrição está registada para o Azure Batch. (Se criar a resposta a incidentes SSIS do Azure no portal do Data Factory, o fornecedor do Azure Batch é automaticamente registado.) 

   a. No portal do Azure, selecione **subscrições** no menu da esquerda. 

   b. Selecione a sua subscrição. 

   c. Selecione **fornecedores de recursos** à esquerda e confirme que **Microsoft.Batch** é um fornecedor registado. 

   ![Confirmação de estado de "Registada"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Se não vir **Microsoft.Batch** na lista, para registá-lo, [criar uma conta do Azure Batch vazia](../batch/batch-account-create-portal.md) na sua subscrição. Pode eliminá-lo mais tarde. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Associar a resposta a incidentes SSIS do Azure a uma rede virtual
1. Inicie o Microsoft Edge ou o Google Chrome. Atualmente, a IU da fábrica de dados só é suportada nesses browsers da web. 

2. No [portal do Azure](https://portal.azure.com), selecione **fábricas de dados** no menu da esquerda. Se não vir **fábricas de dados** no menu, selecione **mais serviços**e selecione **fábricas de dados** no **INTELLIGENCE + análise**secção. 

   ![Lista de fábricas de dados](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

3. Selecione a fábrica de dados com o tempo de execução de integração do Azure-SSIS na lista. Verá a home page da fábrica de dados. Selecione o **autor & implementar** mosaico. Pode ver a IU da fábrica de dados noutro separador. 

   ![Home page da fábrica de dados](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

4. Na IU de fábrica de dados, mude para o **editar** separador, selecione **ligações**e mude para o **tempos de execução de integração** separador. 

   ![Separador de "Integração tempos de execução"](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

5. Se a sua resposta a incidentes SSIS do Azure está em execução, na lista de tempo de execução de integração, selecione o **parar** clique no botão no **ações** coluna para a sua IR. SSIS do Azure Não é possível editar uma resposta a incidentes enquanto não o interrompê-lo. 

   ![Parar a resposta a incidentes](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

6. Na lista de tempo de execução de integração, selecione o **editar** clique no botão no **ações** coluna para a sua IR. SSIS do Azure 

   ![Editar o tempo de execução de integração](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

7. No **definições gerais** página do **a configuração de tempo de execução de integração** janela, selecione **seguinte**. 

   ![Definições gerais para a configuração de resposta a incidentes](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)

8. No **as definições de SQL** página, introduza a palavra-passe de administrador e selecione **seguinte**. 

   ![Definições do SQL Server para a configuração de resposta a incidentes](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)

9. No **definições avançadas** página, efetue as seguintes ações: 

   a. Selecione a caixa de verificação **selecionar uma VNet para o tempo de execução de integração do Azure-SSIS associar e permitir que os serviços do Azure para configurar permissões/definições da VNet**. 

   b. Para **tipo**, selecione se a rede virtual está numa rede virtual clássica ou uma rede virtual do Azure Resource Manager. 

   c. Para **nome da VNet**, selecione a rede virtual. 

   d. Para **nome de sub-rede**, selecione a sub-rede na rede virtual. 

   e. Clique em **VNet validação** e se tiver êxito, clique em **atualização**. 

   ![Definições avançadas para a configuração de resposta a incidentes](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

10. Agora, pode começar a resposta a incidentes utilizando o **iniciar** clique no botão no **ações** coluna para a sua IR. SSIS do Azure Demora cerca de 20 a 30 minutos para iniciar um IR. SSIS do Azure 

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>Configurar uma rede virtual
Terá de configurar uma rede virtual antes de associar o seu IR SSIS do Azure ao mesmo. Para configurar automaticamente as permissões/definições de rede virtual para o tempo de execução de integração do Azure-SSIS associar a rede virtual, adicione o seguinte script:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Criar uma resposta a incidentes SSIS do Azure e associar a uma rede virtual
Pode criar uma resposta a incidentes SSIS do Azure e associar a uma rede virtual ao mesmo tempo. Para o script completa e instruções, consulte [criar um tempo de execução de integração do Azure-SSIS](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Associar uma resposta a incidentes SSIS do Azure existente a uma rede virtual
O script no [criar um tempo de execução de integração do Azure-SSIS](create-azure-ssis-integration-runtime.md) artigo mostra-lhe como criar uma resposta a incidentes SSIS do Azure e associar a uma rede virtual no mesmo script. Se tiver uma resposta a incidentes SSIS do Azure existente, execute os seguintes passos para associar à rede virtual: 
1. Parar o IR. SSIS do Azure 
2. Configure o Azure SSIS IR para associar a rede virtual. 
3. Iniciar o IR. SSIS do Azure 

### <a name="define-the-variables"></a>Definir as variáveis
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Parar a resposta a incidentes SSIS do Azure
Interrompa o tempo de execução de integração do Azure-SSIS antes de a poder associar a uma rede virtual. Este comando disponibiliza todos os respetivos nós e deixa de faturação:

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Configurar definições de rede virtual para o Azure SSIS IR associar
```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Configurar a resposta a incidentes SSIS do Azure
Para configurar o tempo de execução de integração do Azure-SSIS para associar a rede virtual, execute o `Set-AzureRmDataFactoryV2IntegrationRuntime` comando: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Iniciar a resposta a incidentes SSIS do Azure
Para iniciar o tempo de execução de integração de SSIS do Azure, execute o seguinte comando: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Este comando assume 20 a 30 minutos a concluir.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre o tempo de execução de SSIS do Azure, consulte os tópicos seguintes: 
- [Tempo de execução do Azure-SSIS integração](concepts-integration-runtime.md#azure-ssis-integration-runtime). Este artigo fornece informações concetuais sobre tempos de execução de integração em geral, incluindo o IR. SSIS do Azure 
- [Tutorial: implementar pacotes do SSIS no Azure](tutorial-create-azure-ssis-runtime-portal.md). Este artigo fornece instruções passo a passo para criar um IR. SSIS do Azure Utiliza a SQL Database do Azure para alojar o catálogo SSIS. 
- [Create an Azure-SSIS integration runtime](create-azure-ssis-integration-runtime.md) (Criar um runtime de integração Azure-SSIS). Este artigo expande o tutorial e fornece instruções sobre como utilizar a SQL Database do Azure com pontos finais do serviço de rede virtual/gerido instância (pré-visualização) para alojar o catálogo SSIS e associar a IR para uma rede virtual. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Monitorizar um IR Azure-SSIS) Este artigo mostra como obter informações sobre um IR Azure-SSIS e descrições dos estados nas informações devolvidas. 
- [Manage an Azure-SSIS IR](manage-azure-ssis-integration-runtime.md) (Gerir um IR Azure-SSIS). Este artigo mostra-lhe como parar, iniciar ou remover um IR Azure-SSIS. Mostra também como aumentar horizontalmente a resposta a incidentes SSIS do Azure ao adicionar nós. 
