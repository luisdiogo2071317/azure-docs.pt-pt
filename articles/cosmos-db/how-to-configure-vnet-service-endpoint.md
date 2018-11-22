---
title: Como configurar a rede virtual e o acesso com base na sub-rede para a sua conta do Cosmos do Azure
description: Este documento descreve os passos necessários para o ponto final de serviço do programa de configuração do Azure Cosmos DB rede virtual.
author: kanshiG
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: 16cd959a83850a3bc940803cd23e7542e34825c8
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52283217"
---
# <a name="how-to-access-azure-cosmos-db-resources-from-virtual-networks"></a>Como aceder a recursos do Azure Cosmos DB a partir de redes virtuais

As contas do Azure cosmos DB podem ser configuradas para permitir o acesso apenas a partir de uma sub-rede específica da rede Virtual do Azure. Existem dois passos necessários para limitar o acesso à conta do Cosmos do Azure com ligações a partir de uma sub-rede numa rede virtual (VNET).
 
1. Ative a sub-rede enviar a sub-rede e a identidade VNET para o Azure Cosmos DB. Pode conseguir isto ao ativar o ponto final de serviço para o Azure Cosmos DB na sub-rede específica.

1. Adicionar uma regra na conta do Azure Cosmos especificar a sub-rede como uma origem a partir do qual, a conta pode ser acessada.

> [!NOTE]
> Uma vez ponto final de serviço para o seu Cosmos do Azure conta está ativada numa sub-rede, a origem de tráfego do Azure Cosmos DB a atingir muda de IP público para a VNET e sub-rede. A mudança de tráfego aplica-se para qualquer conta do Cosmos do Azure que está a ser acessada a partir desta sub-rede. Se a sua conta do Cosmos do Azure (s) tem uma firewall com base no IP para permitir que esta sub-rede, pedidos de serviço ativado sub-rede já não corresponderia as regras de firewall do IP, e eles são rejeitados. Para obter mais informações, veja os passos descritos em [migrar de regra de firewall do IP à lista de controlo de acesso de VNET](#migrate-from-firewall-to-vnet) seção deste artigo. 

As secções seguintes descrevem como configurar o ponto final de serviço VNET para uma conta do Cosmos do Azure.

## <a id="configure-using-portal"></a>Configurar o ponto final de serviço utilizando o portal do Azure

### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Configurar o ponto final de serviço para uma rede virtual do Azure existente e a sub-rede

1. Partir **todos os recursos** painel, localize a conta do Cosmos do Azure que pretende proteger.

1. Selecione **Firewalls e redes virtuais** no menu de definições e selecione permitir o acesso a partir de **redes selecionadas**.

1. Para conceder acesso à sub-rede da rede virtual existente, em **redes virtuais**, selecione **adicionar rede virtual do Azure existente**.

1. Selecione o **subscrição** partir da qual pretende adicionar a rede virtual do Azure. Selecione o Azure **redes virtuais** e **sub-redes** que deseja fornecer acesso à sua conta do Cosmos do Azure. Em seguida selecione **ativar** para ativar redes selecionadas com pontos finais de serviço para "Microsoft.AzureCosmosDB". Quando terminar, selecione **adicionar**. 

   ![Selecione a rede virtual e sub-rede](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)


1. Depois da conta do Cosmos do Azure está ativada para aceder a partir de uma rede virtual, irá permitir apenas tráfego nesta escolhido sub-rede. A rede virtual e sub-rede adicionou devem aparecer como mostrado na captura de ecrã seguinte:

   ![rede virtual e sub-rede configurada com êxito](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Para ativar pontos finais de serviço de rede virtual, terá de ter as seguintes permissões de subscrição:
  * Subscrição com a VNET: Contribuidor de rede
  * Subscrição com a conta do Cosmos do Azure: contribuinte de conta do DocumentDB

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Configurar o ponto final de serviço para uma nova rede virtual do Azure e uma sub-rede

1. Partir **todos os recursos** painel, localize a conta do Cosmos do Azure que pretende proteger.

1. Selecione **Firewalls e redes virtuais do Azure** no menu de definições e selecione permitir o acesso a partir de **redes selecionadas**.  

1. Para conceder acesso a uma nova rede virtual do Azure, em redes virtuais, selecione **Adicionar nova rede virtual**.  

1. Indique os detalhes necessários para criar uma nova rede virtual e, em seguida, selecione criar. A sub-rede será criada com um ponto de extremidade de serviço para "Microsoft.AzureCosmosDB" ativada.

   ![Selecione a rede virtual e sub-rede para a nova rede virtual](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Se a sua conta do Cosmos do Azure é utilizada por outros serviços do Azure, como o Azure Search ou acessada a partir de análise de Stream ou Power BI, permitir o acesso, verificando **aceitar ligações dos datacenters do Azure público**.

Para garantir que tem acesso a métricas do Azure Cosmos DB a partir do portal, tem de ativar **permitir o acesso a partir do Portal do Azure** opções. Para saber mais sobre estas opções, consulte pedidos a partir do portal do Azure e o pedido de secções de serviços de PaaS do Azure da configurar [firewall do IP](how-to-configure-firewall.md) artigo. Depois de selecionar o acesso, selecione **guardar** para guardar as definições.

## <a id="remove-vnet-or-subnet"></a>Remover uma rede virtual ou sub-rede 

1. Partir **todos os recursos** painel, localizar a conta do Cosmos do Azure para o qual atribuídos pontos finais de serviço.  

2. Selecione **Firewalls e redes virtuais** no menu de definições.  

3. Para remover uma rede virtual ou a regra de sub-rede, selecione "..." ao lado de rede virtual ou sub-rede e selecione **remover**.

   ![Remover uma rede virtual](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

4.  Clique em **guardar** para aplicar as alterações.

## <a id="configure-using-powershell"></a>Configurar o ponto final de serviço com o Azure PowerShell

> [!NOTE]
> Ao utilizar o PowerShell ou a CLI, certifique-se de que especifica a lista completa de filtros IP e ACLs de VNET nos parâmetros, não apenas aqueles que precisam de ser adicionados.

Utilize os seguintes passos para configurar o ponto final de serviço a uma conta do Cosmos do Azure com o Azure PowerShell:  

1. Instalar a versão mais recente [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) e [início de sessão](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Ative o ponto de final de serviço para uma sub-rede existente de uma rede virtual.  

   ```powershell
   $rgname = "<Resource group name>"
   $vnName = "<Virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. Obtenha informações de VNET.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName `
     -ResourceGroupName $rgName
   ```

1. Obter propriedades de conta do Cosmos do Azure, execute o seguinte cmdlet:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Inicialize as variáveis para utilização posterior. Configure todas as variáveis da definição de conta existente.

   ```powershell
   $locations = @()

   foreach ($readLocation in $cosmosDBConfiguration.Properties.readLocations) {
      $locations += , @{
         locationName     = $readLocation.locationName;
         failoverPriority = $readLocation.failoverPriority;
      }
   }

   $virtualNetworkRules = @(@{
      id = "$($vnProp.Id)/subnets/$sname";
   })

   if ($cosmosDBConfiguration.Properties.isVirtualNetworkFilterEnabled) {
      $virtualNetworkRules = $cosmosDBConfiguration.Properties.virtualNetworkRules + $virtualNetworkRules
   }
   ```

1. Atualize propriedades de conta do Cosmos do Azure com a nova configuração ao executar os seguintes cmdlets: 

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName `
     -Properties $CosmosDBProperties
   ```

1. Execute o seguinte comando para verificar que a sua conta do Cosmos do Azure é atualizada com o ponto de final de serviço de rede virtual que configurou no passo anterior:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a id="configure-using-cli"></a>Configurar o ponto final de serviço ao utilizar a CLI do Azure 

1. Ative o ponto de final de serviço para uma sub-rede numa rede virtual.

1. Atualizar conta do Cosmos do Azure existente com as ACLs de sub-rede

   ```azurecli-interactive

   name="<Azure Cosmos account name>"
   resourceGroupName="<Resource group name>"

   az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/frontend"
   ```

1. Criar uma nova conta do Cosmos do Azure com ACLs de sub-rede

   ```azurecli-interactive
   az cosmosdb create \
    --name $name \
    --kind GlobalDocumentDB \
    --resource-group $resourceGroupName \
    --max-interval 10 \
    --max-staleness-prefix 200 \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/default"
   ```

## <a id="migrate-from-firewall-to-vnet"></a>Migrar de regra de firewall do IP para a ACL de VNET 

Só são necessários os seguintes passos para contas do Cosmos do Azure com regras de firewall IP existente, permitindo uma sub-rede e que pretende utilizar a VNET e ACLs baseadas em sub-rede em vez de regra de firewall do IP.

Assim que o ponto final de serviço para a conta do Cosmos do Azure está ativada para uma sub-rede, as solicitações são enviadas com origem que contém informações de VNET e sub-rede em vez de IP público. Por conseguinte, esses pedidos não coincidem um filtro IP. Este parâmetro de origem ocorre para todas as contas do Cosmos do Azure acedidas a partir da sub-rede com o ponto final de serviço ativado. Para impedir que o tempo de inatividade, utilize os seguintes passos:

1. Obter propriedades de conta do Cosmos do Azure, execute o seguinte cmdlet:

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Inicialize as variáveis para utilizá-los mais tarde. Configure todas as variáveis da definição de conta existente. Adicione a ACL de VNET para Cosmos do Azure todas as contas de que está sendo acessado a partir da sub-rede com `ignoreMissingVNetServiceEndpoint` sinalizador.

   ```powershell
   $locations = @()

   foreach ($readLocation in $cosmosDBConfiguration.Properties.readLocations) {
      $locations += , @{
         locationName     = $readLocation.locationName;
         failoverPriority = $readLocation.failoverPriority;
      }
   }

   $subnetID = "Subnet ARM URL" e.g "/subscriptions/f7ddba26-ab7b-4a36-a2fa-7d01778da30b/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/subnet1"

   $virtualNetworkRules = @(@{
      id = $subnetID;
      ignoreMissingVNetServiceEndpoint = "True";
   })

   if ($cosmosDBConfiguration.Properties.isVirtualNetworkFilterEnabled) {
      $virtualNetworkRules = $cosmosDBConfiguration.Properties.virtualNetworkRules + $virtualNetworkRules
   }
   ```

1. Atualize propriedades de conta do Cosmos do Azure com a nova configuração ao executar os seguintes cmdlets:

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzureRmResource `
      -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
      -ApiVersion $apiVersion `
      -ResourceGroupName $rgName `
      -Name $acctName `
      -Properties $CosmosDBProperties
   ```

1. Repita os passos 1 a 3 para todas as contas do Cosmos do Azure que aceder a partir da sub-rede.

1.  Aguarde 15 minutos e, em seguida, atualizar a sub-rede para ativar o ponto final de serviço.

1.  Ative o ponto de final de serviço para uma sub-rede existente de uma rede virtual.

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
      -ResourceGroupName $rgname `
      -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
      -Name $sname `
      -AddressPrefix $subnetPrefix `
      -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. Remova a regra de firewall do IP da sub-rede.

## <a name="next-steps"></a>Passos Seguintes

* Para configurar uma firewall para o Azure Cosmos DB, consulte [suporte de firewall](firewall-support.md) artigo.
