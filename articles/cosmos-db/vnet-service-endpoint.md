---
title: Proteger o acesso a uma conta do Azure Cosmos DB utilizando o ponto final de serviço de rede Virtual do Azure | Documentos da Microsoft
description: Este documento descreve os passos necessários para o ponto final de serviço do programa de configuração do Azure Cosmos DB rede virtual.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: govindk
ms.openlocfilehash: 6d9d05a56376c07013fdea1c94b0a3262d2397c2
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026301"
---
# <a name="secure-access-to-an-azure-cosmos-db-account-by-using-azure-virtual-network-service-endpoint"></a>Proteger o acesso a uma conta do Azure Cosmos DB utilizando o ponto final de serviço de rede Virtual do Azure

As contas do Azure cosmos DB podem ser configuradas para permitir o acesso apenas a partir de uma sub-rede específica da rede Virtual do Azure. Ativando um [ponto final de serviço](../virtual-network/virtual-network-service-endpoints-overview.md) para o Azure cosmos DB de uma rede Virtual e a sua sub-rede, o tráfego é certificar-se de uma rota ideal e segura para o Azure Cosmos DB.  

Azure Cosmos DB é um serviço de base de dados com múltiplos modelos distribuído globalmente. Pode replicar os dados presentes na conta do Azure Cosmos DB em várias regiões. Quando o Azure Cosmos DB está configurado com um ponto de extremidade do serviço de rede virtual, cada rede virtual permite o acesso de IPs que pertencem a specicifc sub-rede. A imagem seguinte mostra uma ilustração de um Azure Cosmos DB com o ponto final de serviço de rede virtual ativado:

![arquitetura de ponto final de serviço de rede virtual](./media/vnet-service-endpoint/vnet-service-endpoint-architecture.png)

Depois de uma conta do Azure Cosmos DB está configurada com um ponto de extremidade do serviço de rede virtual, pode ser acedido apenas a partir de sub-rede especificada, todo o acesso público/internet é removido. Para saber mais no detalhadas sobre a pontos finais de serviço, consulte o do Azure [descrição geral de pontos finais de serviço de rede Virtual](../virtual-network/virtual-network-service-endpoints-overview.md) artigo.

## <a name="configure-service-endpoint-by-using-azure-portal"></a>Configurar o ponto final de serviço utilizando o portal do Azure
### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Configurar o ponto final de serviço para uma rede virtual do Azure existente e a sub-rede

1. Partir **todos os recursos** painel, localização de rede virtual pretende configurar o ponto final de serviço para o Azure Cosmos DB. Navegue para o **pontos finais de serviço** painel e certifique-se de que a sub-rede da rede virtual foi ativada para o ponto de final de serviço "Azure.CosmosDB".  

   ![Confirmar o ponto final de serviço ativado](./media/vnet-service-endpoint/confirm-service-endpoint-enabled.png)

2. Partir **todos os recursos** painel, localize a conta do Azure Cosmos DB que pretende proteger.  

3. Antes de ativar o ponto final de serviço de rede virtual, copie as informações de firewall do IP associadas com a sua conta do Azure Cosmos DB para utilização futura. Pode reativar o firewall do IP depois de configurar o ponto final de serviço.  

4. Selecione **Firewalls e redes virtuais** no menu de definições e selecione permitir o acesso a partir de **redes selecionadas**.  

3. Para conceder acesso à sub-rede da rede virtual existente, em redes virtuais, selecione **adicionar rede virtual do Azure existente**.  

4. Selecione o **subscrição** partir da qual pretende adicionar a rede virtual do Azure. Selecione o Azure **redes virtuais** e **sub-redes** que deseja fornecer acesso à sua conta do Azure Cosmos DB. Em seguida selecione **ativar** para ativar redes selecionadas com pontos finais de serviço para "Microsoft.AzureCosmosDB". Quando terminar, selecione **adicionar**.  

   ![Selecione a rede virtual e sub-rede](./media/vnet-service-endpoint/choose-subnet-and-vnet.png)

   > [!NOTE]
   > Se o ponto final de serviço para o Azure Cosmos DB anteriormente não está configurado para o selecionadas redes virtuais do Azure e sub-redes, ele pode ser configurado como parte desta operação. Ativar o acesso irá demorar até 15 minutos a concluir. É muito importante desativar a firewall do IP após anota o conteúdo do firewall ACL para renabling-los mais tarde. 

   ![rede virtual e sub-rede configurada com êxito](./media/vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

Agora a sua conta do Azure Cosmos DB será a permitir apenas tráfego nesta escolhido sub-rede. Se anteriormente tiver habilitado a firewall do IP,. Volte a ativá-los usando as informações anteriores.

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Configurar o ponto final de serviço para uma nova rede virtual do Azure e uma sub-rede

1. Partir **todos os recursos** painel, localize a conta do Azure Cosmos DB que pretende proteger.  

   > [!NOTE]
   > Se tiver uma firewall do IP existente configurado para a sua conta do Azure Cosmos DB, tenha em atenção a configuração de firewall, remover o firewall do IP e, em seguida, ativar o ponto de final de serviço. Se ativar o ponto final de serviço sem disbling o firewall, o tráfego a partir desse intervalo de ip perderá a identidade IP virtual e é removido com uma mensagem de erro de filtro IP. Portanto, para evitar este erro deve sempre de desativar as regras de firewall, copiá-los, ative o ponto final de serviço da sub-rede e, finalmente, a ACL a sub-rede do Cosmos DB. Depois de configurar o ponto final de serviço e adicionar a ACL pode reativar o firewall do IP novamente se necessário.

2. Antes de ativar o ponto final de serviço de rede virtual, copie as informações de firewall do IP associadas com a sua conta do Azure Cosmos DB para utilização futura. Pode reativar o firewall do IP depois de configurar o ponto final de serviço.  

3. Selecione **Firewalls e redes virtuais do Azure** no menu de definições e selecione permitir o acesso a partir de **redes selecionadas**.  

4. Para conceder acesso a uma nova rede virtual do Azure, em redes virtuais, selecione **Adicionar nova rede virtual**.  

5. Indique os detalhes necessários para criar uma nova rede virtual e, em seguida, selecione criar. A sub-rede será criada com um ponto de extremidade de serviço para "Microsoft.AzureCosmosDB" ativada.

   ![Selecione a rede virtual e sub-rede para a nova rede virtual](./media/vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

## <a name="allow-access-from-azure-portal"></a>Permitir o acesso a partir do portal do Azure

Depois de pontos finais de serviço de rede Virtual do Azure estão ativados para a sua conta de base de dados do Azure Cosmos DB, acesso a partir do portais ou outros serviços do Azure está desativado por predefinição. Acesso à sua conta de base de dados do Azure Cosmos DB a partir de máquinas fora da sub-rede configurada são bloqueados incluindo o acesso a partir do portal.

![Permitir o acesso a partir do portal](./media/vnet-service-endpoint/allow-access-from-portal.png)

Se a sua conta do Azure Cosmos DB é utilizada por outros serviços do Azure, como o Azure Search ou acessada a partir de análise de Stream ou Power BI, permitir o acesso, verificando **permitir acesso aos serviços do Azure**.

Para garantir que tem acesso a métricas do Azure Cosmos DB a partir do portal, tem de ativar **permitir o acesso ao portal do Azure** opções. Para saber mais sobre estas opções, veja [ligações a partir do portal do Azure](firewall-support.md#connections-from-the-azure-portal) e [ligações de serviços de PaaS do Azure](firewall-support.md#connections-from-global-azure-datacenters-or-azure-paas-services) secções. Depois de selecionar o acesso, selecione **guardar** para guardar as definições.

## <a name="remove-a-virtual-network-or-subnet"></a>Remover uma rede virtual ou sub-rede 

1. Partir **todos os recursos** painel, a conta de localizar o Azure Cosmos DB para o qual é atribuída a pontos finais de serviço.  

2. Selecione **Firewalls e redes virtuais** no menu de definições.  

3. Para remover uma rede virtual ou a regra de sub-rede, selecione "..." ao lado de rede virtual ou sub-rede e selecione **remover**.

   ![Remover uma rede virtual](./media/vnet-service-endpoint/remove-a-vnet.png)

4.  Clique em **guardar** para aplicar as alterações.

## <a name="configure-service-endpoint-by-using-azure-powershell"></a>Configurar o ponto final de serviço com o Azure PowerShell 

Utilize os seguintes passos para configurar o ponto final de serviço a uma conta do Azure Cosmos DB com o Azure PowerShell:  

1. Instalar a versão mais recente [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) e [início de sessão](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  Certifique-se de que tenha em atenção as definições da firewall IP e eliminar a firewall do IP completamente antes de ativar o ponto final de serviço para a conta.

  > [!NOTE]
  > Se tiver uma firewall do IP existente configurado para a sua conta do Azure Cosmos DB, tenha em atenção a configuração de firewall, remover o firewall do IP e, em seguida, ativar o ponto de final de serviço. Se ativar o ponto final de serviço sem disbling o firewall, o tráfego a partir desse intervalo de ip perderá a identidade IP virtual e é removido com uma mensagem de erro de filtro IP. Portanto, para evitar este erro deve sempre de desativar as regras de firewall, copiá-los, ative o ponto final de serviço da sub-rede e, finalmente, a ACL a sub-rede do Cosmos DB. Depois de configurar o ponto final de serviço e adicionar a ACL pode reativar o firewall do IP novamente se necessário.

2. Antes de ativar o ponto final de serviço de rede virtual, copie as informações de firewall do IP associadas com a sua conta do Azure Cosmos DB para utilização futura. Será de reativar o firewall do IP depois de configurar o ponto final de serviço.  

3. Ative o ponto de final de serviço para uma sub-rede existente de uma rede virtual.  

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

4. Prepare-se para a ativação da ACL na conta do cosmos DB, tornando-se de que a rede virtual e a sub-rede tem o ponto final de serviço ativado para o Azure Cosmos DB.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

5. Obter propriedades de conta do Azure Cosmos DB, execute o seguinte cmdlet:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

6. Inicialize as variáveis para utilização posterior. Configurar a todas as variáveis da definição de conta existente, se tiver várias localizações, terá de adicioná-los como parte da matriz. Neste passo, também configurar ponto final de serviço de rede virtual ao definir a variável "accountVNETFilterEnabled" para "True". Mais tarde, este valor é atribuído para o parâmetro de "isVirtualNetworkFilterEnabled".  

   ```powershell
   $locations = @(@{})

   <# If you have read regions in addition to a write region, use the following code to set the $locations variable instead.

   $locations = @(@{"locationName"="<Write location>"; 
                 "failoverPriority"=0}, 
               @{"locationName"="<Read location>"; 
                  "failoverPriority"=1}) #>

   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName

   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy

   $accountVNETFilterEnabled = $True
   $subnetID = $vnProp.Id+"/subnets/" + $sname  
   $virtualNetworkRules = @(@{"id"=$subnetID})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

7. Atualize propriedades da conta do Azure Cosmos DB com a nova configuração ao executar os seguintes cmdlets: 

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName -Properties $CosmosDBProperties
   ```

8. Execute o seguinte comando para verificar que a sua conta do Azure Cosmos DB é atualizada com o ponto de final de serviço de rede virtual que configurou no passo anterior:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a name="add-virtual-network-service-endpoint-for-an-azure-cosmos-db-account-that-has-ip-firewall-enabled"></a>Adicionar ponto final de serviço de rede virtual para uma conta do Azure Cosmos DB com a Firewall do IP ativada

1. Primeiro de desativar o acesso da firewall IP para a conta do Azure Cosmos DB.  

2. Ative o ponto final de rede virtual para a conta do Azure Cosmos DB através de um dos métodos descritos nas secções anteriores.  

3. Voltar a ativar o acesso de firewall do IP. 

## <a name="test-the-access"></a>Testar o acesso

Para verificar se os pontos finais de serviço para o Azure Cosmos DB estão configurados como esperado, utilize os seguintes passos:

* Configure duas sub-redes numa rede virtual, como o front-end e back-end, ativar o ponto final de serviço do Cosmos DB para a sub-rede de back-end.  

* Ative o acesso na conta do Cosmos DB para a sub-rede de back-end.  

* Crie duas máquinas virtuais: uma na sub-rede de back-end e outra na sub-rede de front-end.  

* Tente aceder à conta do Azure Cosmos DB de ambas as máquinas virtuais. Deverá conseguir ligar a partir da máquina virtual criado na sub-rede de back-end, mas não a partir de um criado na sub-rede de front-end. O pedido irá apresentar um erro com 404 ao tentar ligar a partir da sub-rede de front-end que confirma que o acesso ao Azure Cosmos DB é protegido com o ponto de extremidade do serviço de rede virtual. A máquina na sub-rede de back-end poderá funcionar bem.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="what-happens-when-you-access-an-azure-cosmos-db-account-that-has-virtual-network-access-control-list-acl-enabled"></a>O que acontece quando acessar uma conta do Azure Cosmos DB que tenha ativado a lista de controlo de acesso (ACL) de rede virtual?  

HTTP 404 erro é retornado.  

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-cosmos-db-account-in-another-region-for-example-if-azure-cosmos-db-account-is-in-west-us-or-east-us-and-virtual-networks-are-in-multiple-regions-can-the-virtual-network-access-azure-cosmos-db"></a>São as sub-redes de uma rede virtual criados em regiões diferentes, permitidas para aceder a uma conta do Azure Cosmos DB noutra região? Por exemplo, se a conta do Azure Cosmos DB está em E.U.A. oeste ou E.U.A. leste e da rede virtual estão em várias regiões, pode a rede virtual de acesso do Azure Cosmos DB?  

Sim, as redes virtuais criadas em regiões diferentes podem aceder a nova capacidade. 

### <a name="can-an-azure-cosmos-db-account-have-both-virtual-network-service-endpoint-and-a-firewall"></a>Uma conta do Azure Cosmos DB pode ter o ponto final de serviço de rede virtual e uma firewall?  

Sim, ponto final de serviço de rede Virtual e uma firewall podem coexistir. Em geral, deve garantir que o acesso ao portal está sempre ativado antes de configurar um ponto de extremidade do serviço de rede virtual para que possa ver as métricas associadas ao contentor.

### <a name="can-i-accept-connections-from-within-public-azure-datacenters-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Pode me "Aceitar ligações dos datacenters do Azure público" quando o acesso ao ponto final de serviço está ativado para o Azure Cosmos DB?  

Isto é necessário apenas se pretender que a sua conta do Azure Cosmos DB para ser acedida por outra parte do Azure de primeira dos serviços, como a fábrica de dados do Azure, Azure Search ou qualquer serviço que é implementado numa determinada região do Azure.

### <a name="how-many-virtual-network-service-endpoints-are-allowed-for-azure-cosmos-db"></a>Quantos pontos de extremidade de serviço de rede virtual são permitidos para o Azure Cosmos DB?  

64 pontos de extremidade de serviço de rede virtual são permitidos para uma conta do Azure Cosmos DB.

### <a name="what-is-the-relationship-between-service-endpoint-and-network-security-group-nsg-rules"></a>Qual é a relação entre as regras de ponto final de serviço e o grupo de segurança de rede (NSG)?  

As regras do NSG no Azure Cosmos DB permitem-lhe restringir o acesso ao intervalo de endereços IP do Azure Cosmos DB específico. Se pretender permitir o acesso a uma instância do Azure Cosmos DB, que está presente numa determinada [região](https://azure.microsoft.com/global-infrastructure/regions/), pode especificar a região no seguinte formato: 

    AzureCosmosDB.<region name>

Para saber mais sobre o NSG com as etiquetas, consulte [etiquetas de serviço de rede virtual](../virtual-network/security-overview.md#service-tags) artigo. 
  
### <a name="what-is-relationship-between-an-ip-firewall-and-virtual-network-service-endpoint-capability"></a>O que é a relação entre um firewall IP e a capacidade de ponto final de serviço de rede Virtual?  

Esses dois recursos complementam entre si para garantir o isolamento de recursos do Azure Cosmos DB e protegê-los. Utilizar um IP firewall assegura que IPs estáticos podem aceder a conta do Azure Cosmos DB.  

### <a name="can-an-on-premises-devices-ip-address-that-is-connected-through-azure-virtual-network-gatewayvpn-or-express-route-gateway-access-azure-cosmos-db-account"></a>Endereço IP de um dispositivo no local que está conectado por meio de gateway(VPN) de rede Virtual do Azure ou o gateway do Express route pode acessar a conta do Azure Cosmos DB?  

Endereço IP ou intervalo de endereços IP do dispositivo no local deve ser adicionado à lista de IPs estáticos para aceder à conta do Azure Cosmos DB.  

### <a name="what-happens-if-you-delete-a-virtual-network-that-has-service-endpoint-setup-for-azure-cosmos-db"></a>O que acontece se eliminar uma rede virtual que tenha a configuração do ponto final de serviço para o Azure Cosmos DB?  

Quando uma rede virtual é eliminada, as informações de ACL associadas com que o Azure Cosmos DB é eliminado e remove a interação entre a rede virtual e a conta do Azure Cosmos DB. 

### <a name="what-happens-if-an-azure-cosmos-db-account-that-has-virtual-network-service-endpoint-enabled-is-deleted"></a>O que acontece se uma conta do Azure Cosmos DB com o ponto final de serviço de rede virtual ativado é eliminado?

Os metadados associados a conta do Azure Cosmos DB específica é eliminado da sub-rede. E é responsabilidade do utilizador final para eliminar a sub-rede e rede virtual utilizada.

### <a name="can-i-use-a-peered-virtual-network-to-create-service-endpoint-for-azure-cosmos-db"></a>Pode utilizar uma rede virtual em modo de peering para criar o ponto final de serviço para o Azure Cosmos DB?  

Não, apenas a rede virtual direta e suas sub-redes, podem criar pontos finais de serviço do Azure Cosmos DB.

### <a name="what-happens-to-the-source-ip-address-of-resource-like-virtual-machine-in-the-subnet-that-has-azure-cosmos-db-service-endpoint-enabled"></a>O que acontece com o endereço IP de origem do recurso como máquina virtual na sub-rede que tem o ponto final de serviço do Azure Cosmos DB ativado?

Quando estão ativados pontos finais de serviço de rede virtual, os endereços IP de origem dos recursos na sub-rede da rede virtual muda de utilizar endereços IPV4 públicos para os endereços privados da rede Virtual do Azure para o tráfego para o Azure Cosmos DB.

### <a name="does-azure-cosmos-db-reside-in-the-azure-virtual-network--provided-by-the-customer"></a>Azure Cosmos DB residem na rede virtual do Azure fornecida pelo cliente?  

O Azure Cosmos DB é um serviço de multi-inquilino com um endereço IP público. Quando restringir o acesso a uma sub-rede de uma rede Virtual do Azure ao utilizar a funcionalidade de ponto final de serviço, o acesso é restrito para a sua conta do Azure Cosmos DB através de determinada rede Virtual do Azure e a respetiva sub-rede.  Conta do Azure Cosmos DB não reside na rede Virtual do Azure. 

### <a name="what-if-anything-will-be-logged-in-log-analytics-if-it-is-enabled"></a>E se nada será registado no Log Analytics se estiver ativado?  

O Azure Cosmos DB irá enviar os registos com o endereço IP (sem o último octeto) com o estado 403 para o pedido bloqueado por ACL.  

## <a name="next-steps"></a>Passos Seguintes
Configurar uma firewall para Azure Cosmos DB, veja [suporte de firewall](firewall-support.md) artigo.

