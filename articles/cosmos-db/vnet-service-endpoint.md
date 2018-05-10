---
title: Proteger o acesso a uma conta de base de dados do Azure Cosmos utilizando o ponto final do serviço de rede Virtual do Azure | Microsoft Docs
description: Este documento descreve os passos necessários para o ponto final do serviço de rede virtual de base de dados do Azure Cosmos do programa de configuração.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 05/07/2018
ms.author: govindk
ms.openlocfilehash: b07a159e69a11656555a8550b807cce0b2c9ef6c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="secure-access-to-an-azure-cosmos-db-account-by-using-azure-virtual-network-service-endpoint"></a>Proteger o acesso a uma conta de base de dados do Azure Cosmos utilizando o ponto final do serviço de rede Virtual do Azure

As contas de CosmosDB do Azure podem ser configuradas para permitir o acesso apenas a partir de uma sub-rede específica da rede Virtual do Azure. Ao ativar uma [ponto final de serviço](../virtual-network/virtual-network-service-endpoints-overview.md) para Azure CosmosDB de uma rede Virtual e a respetiva sub-rede, o tráfego está a certificar-se uma rota segura e ideal para a BD do Cosmos do Azure.  

Azure Cosmos DB é um serviço de base de dados com múltiplos modelos distribuído globalmente. Pode replicar os dados presentes na conta de base de dados do Azure Cosmos para várias regiões. Quando a BD do Cosmos Azure está configurado com um ponto final de serviço de rede virtual, cada rede virtual permite o acesso IPs que pertence à sub-rede specicifc. A imagem seguinte mostra uma ilustração de uma base de dados de Cosmos do Azure com o ponto final de serviço de rede virtual ativado:

![arquitetura de ponto final do serviço de rede virtual](./media/vnet-service-endpoint/vnet-service-endpoint-architecture.png)

Depois de ter uma conta de base de dados do Azure Cosmos configurada com um ponto final de serviço de rede virtual, pode ser acedido apenas da sub-rede especificada, todo o acesso público/internet é removido. Para saber em detalhadas sobre os pontos finais de serviço, consulte o Azure [descrição geral de pontos finais de serviço de rede Virtual](../virtual-network/virtual-network-service-endpoints-overview.md) artigo.

> [!NOTE]
> Atualmente, pontos finais do serviço de rede Virtual podem ser configurados para a API do Azure Cosmos BD do SQL Server ou contas de API do Mongo. Capacidade de configurar pontos finais de serviço para outros APIs e nuvens sovereign como Datacenters do Azure ou o Azure Government estará disponível brevemente. Se tiver uma firewall IP existente configurada para a sua conta de base de dados do Azure Cosmos, tenha em atenção a configuração da firewall, remova a firewall IP e, em seguida, configurar o ponto final de serviço ACL. Depois de configurar o ponto final de serviço, pode voltar a ativar a firewall do IP se necessário.

## <a name="configure-service-endpoint-by-using-azure-portal"></a>Configurar o ponto final de serviço utilizando o portal do Azure
### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Configurar o ponto final de serviço para uma rede virtual do Azure existente e sub-rede

1. De **todos os recursos** painel, localizar a rede virtual pretende configurar o ponto final de serviço para a base de dados do Azure Cosmos. Navegue para o **pontos finais de serviço** painel e certifique-se de que a sub-rede da rede virtual foi ativada para o ponto final de serviço de "Azure.CosmosDB".  

   ![Confirme o ponto final de serviço ativado](./media/vnet-service-endpoint/confirm-service-endpoint-enabled.png)

2. De **todos os recursos** painel, localizar a base de dados do Azure Cosmos conta pretende proteger.  

3. Antes de ativar o ponto final do serviço de rede virtual, copie as informações de firewall IP associadas com a sua conta de base de dados do Azure Cosmos para utilização futura. Pode reativar o IP firewall depois de configurar o ponto final de serviço.  

4. Selecione **Firewalls e redes virtuais** a partir do menu de definições e escolha autorizar o acesso **selecionado redes**.  

3. Para conceder acesso à sub-rede da rede virtual existente, em redes virtuais, selecione **adicionar rede virtual do Azure existente**.  

4. Selecione o **subscrição** partir da qual pretende adicionar a rede virtual do Azure. Selecione o Azure **redes virtuais** e **sub-redes** que pretende fornecer acesso à sua conta de base de dados do Azure Cosmos. Em seguida selecione **ativar** para ativar a redes selecionadas com pontos finais de serviço para "Microsoft.AzureCosmosDB". Quando estiver concluída, selecione **adicionar**.  

   ![Selecione a rede virtual e sub-rede](./media/vnet-service-endpoint/choose-subnet-and-vnet.png)

   > [!NOTE]
   > Se o ponto final de serviço para a base de dados do Azure Cosmos anteriormente não está configurado para as redes virtuais do Azure selecionadas e sub-redes, pode ser configurado como parte desta operação. Ativar o acesso irá demorar até 15 minutos a concluir. 

   ![rede virtual e sub-rede configurada com êxito](./media/vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

Agora a conta de base de dados do Azure Cosmos só irá permitir tráfego deste escolhido sub-rede. Se tinha anteriormente ativado firewall IP,. reativá-los utilizando as informações anteriores.

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Configurar o ponto final de serviço para uma nova rede virtual do Azure e a sub-rede

1. De **todos os recursos** painel, localizar a base de dados do Azure Cosmos conta pretende proteger.  

2. Antes de ativar o ponto final do serviço de rede virtual, copie as informações de firewall IP associadas com a sua conta de base de dados do Azure Cosmos para utilização futura. Pode reativar o IP firewall depois de configurar o ponto final de serviço.  

3. Selecione **Firewalls e redes virtuais do Azure** a partir do menu de definições e escolha autorizar o acesso **selecionado redes**.  

4. Para conceder acesso a uma nova rede virtual do Azure, em redes virtuais, selecione **Adicionar nova rede virtual**.  

5. Forneça os detalhes necessários para criar uma nova rede virtual e, em seguida, selecione a criar. A sub-rede será criada com um ponto final de serviço para "Microsoft.AzureCosmosDB" ativada.

   ![Selecione a rede virtual e sub-rede para a nova rede virtual](./media/vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

## <a name="allow-access-from-azure-portal"></a>Permitir o acesso a partir do portal do Azure

Depois de pontos finais do serviço de rede Virtual do Azure estão ativados para a sua conta de base de dados de base de dados do Azure Cosmos, acesso a partir do portais ou outros serviços do Azure está desativado por predefinição. Acesso à sua conta de base de dados de base de dados do Azure Cosmos máquinas fora da sub-rede configurada estão bloqueadas, incluindo acesso a partir do portal.

![Permitir o acesso a partir do portal](./media/vnet-service-endpoint/allow-access-from-portal.png)

Se a conta de base de dados do Azure Cosmos é utilizada por outros serviços do Azure, como pesquisa do Azure ou acedida a partir do Stream analytics ou o Power BI, permitir o acesso ao verificar **permitir o acesso aos serviços do Azure**.

Para garantir que o utilizador tem acesso à base de dados do Azure Cosmos métricas do portal, tem de ativar **permitir o acesso ao portal do Azure** opções. Para saber mais sobre estas opções, consulte [ligações a partir do portal do Azure](firewall-support.md#connections-from-the-azure-portal) e [ligações a partir de serviços do Azure PaaS](firewall-support.md#connections-from-other-azure-paas-services) secções. Depois de selecionar o acesso, selecione **guardar** para guardar as definições.

## <a name="remove-a-virtual-network-or-subnet"></a>Remover uma rede virtual ou a sub-rede 

1. De **todos os recursos** painel, a conta de localizar a base de dados do Azure do Cosmos aos quais é atribuído pontos finais de serviço.  

2. Selecione **Firewalls e redes virtuais** a partir do menu de definições.  

3. Para remover uma rede virtual ou a regra de sub-rede, selecione "..." ao lado de rede virtual ou a sub-rede e selecione **remover**.

   ![Remover uma rede virtual](./media/vnet-service-endpoint/remove-a-vnet.png)

4.  Clique em **guardar** para aplicar as suas alterações.

## <a name="configure-service-endpoint-by-using-azure-powershell"></a>Configurar o ponto final de serviço com o Azure PowerShell 

Utilize os seguintes passos para configurar o ponto final de serviço para uma conta de base de dados do Azure Cosmos utilizando o Azure PowerShell:  

1. Instalar a versão mais recente [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) e [início de sessão](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  Certifique-se de que tenha em atenção as definições de firewall IP e eliminar a firewall do IP completamente antes de ativar o ponto final de serviço para a conta.

2. Antes de ativar o ponto final do serviço de rede virtual, copie as informações de firewall IP associadas com a sua conta de base de dados do Azure Cosmos para utilização futura. Será novamente ativar a firewall do IP após configurar o ponto final de serviço.  

3. Ative o ponto final de serviço para uma sub-rede existente de uma rede virtual.  

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

4. Prepare para a ativação da ACL na conta CosmosDB, certificando-se de que a rede virtual e a sub-rede têm ponto final de serviço ativado para a base de dados do Azure Cosmos.

   ```powershell
   $subnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName  | Get-AzureRmVirtualNetworkSubnetConfig -Name $sname
   $vnProp = Get-AzureRmVirtualNetwork `-Name $vnName  -ResourceGroupName $rgName
   ```

5. Obter propriedades de conta de base de dados do Azure Cosmos executando o seguinte cmdlet:  

   ```powershell
   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

6. Inicializar as variáveis para utilização posterior. Configurar todas as variáveis da definição de conta existente, se tiver várias localizações, terá de adicioná-los como parte da matriz. Neste passo, configure também ponto final do serviço de rede virtual ao definir a variável "accountVNETFilterEnabled" para "True". Mais tarde, este valor é atribuído ao parâmetro "isVirtualNetworkFilterEnabled".  

   ```powershell
   $locations = @(@{})
   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName
   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy

   $accountVNETFilterEnabled = $True
   $subnetID = $vnProp.Id+"/subnets/" + $subnetName  
   $virtualNetworkRules = @(@{"id"=$subnetID})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

7. Atualize propriedades da conta de base de dados do Azure Cosmos com a nova configuração executando os seguintes cmdlets: 

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource ``
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName -Properties $CosmosDBProperties
   ```

8. Execute o seguinte comando para verificar que a sua conta de base de dados do Azure Cosmos foi atualizada com o ponto de final de serviço de rede virtual que configurou no passo anterior:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a name="add-virtual-network-service-endpoint-for-an-azure-cosmos-db-account-that-has-ip-firewall-enabled"></a>Adicionar o ponto final de serviço de rede virtual para uma conta de base de dados do Azure Cosmos que tenha de Firewall de IP ativada

1. Primeiro, desative o acesso de firewall IP para a conta de base de dados do Azure Cosmos.  

2. Ative o ponto final da rede virtual para a conta de base de dados do Azure Cosmos utilizando um dos métodos descritos nas secções anteriores.  

3. Volte a ativar o acesso de firewall IP. 

## <a name="test-the-access"></a>Testar o acesso

Para verificar se os pontos finais de serviço para a base de dados do Azure Cosmos estão configurados como esperado, utilize os seguintes passos:

* Configure duas sub-redes numa rede virtual como de front-end e back-end, ativar o ponto final de serviço de base de dados do Cosmos para a sub-rede de back-end.  

* Ative o acesso na conta de base de dados do Cosmos para a sub-rede de back-end.  

* Crie duas máquinas virtuais-na sub-rede de back-end e outra na sub-rede do front-end.  

* Tente aceder a conta de base de dados do Azure Cosmos de ambas as máquinas virtuais. Deverá conseguir ligar da máquina virtual criada na sub-rede de back-end, mas não a partir de um criado na sub-rede do front-end. O pedido será com 404 erro quando tenta ligar a partir da sub-rede do front-end que confirma que o acesso à base de dados do Azure Cosmos está protegido utilizando o ponto final de serviço de rede virtual. A máquina na sub-rede de back-end poderá ajustar a trabalhar.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="what-happens-when-you-access-an-azure-cosmos-db-account-that-has-virtual-network-access-control-list-acl-enabled"></a>O que acontece ao aceder a uma conta de base de dados do Azure Cosmos que tenha ativado a lista de controlo de acesso (ACL) de rede virtual?  

HTTP 404 é devolvido o erro.  

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-cosmos-db-account-in-another-region-for-example-if-azure-cosmos-db-account-is-in-west-us-or-east-us-and-virtual-networks-are-in-multiple-regions-can-the-virtual-network-access-azure-cosmos-db"></a>São sub-redes de uma rede virtual criada em regiões diferentes permitidas para aceder a uma conta de base de dados do Azure Cosmos noutra região? Por exemplo, se a conta de base de dados do Azure Cosmos está nos EUA Oeste ou EUA leste e da rede virtual são em várias regiões, pode a rede virtual aceder à base de dados do Azure Cosmos?  

Sim, podem aceder a redes virtuais criadas em regiões diferentes para a capacidade de novo. 

### <a name="can-an-azure-cosmos-db-account-have-both-virtual-network-service-endpoint-and-a-firewall"></a>Uma conta de base de dados do Azure Cosmos pode ter o ponto final do serviço de rede virtual e uma firewall?  

Sim, ponto final do serviço de rede Virtual e uma firewall podem coexistir. Em geral, deve garantir que o acesso ao portal é sempre ativado antes de configurar um ponto final de serviço de rede virtual que lhe permite ver as métricas associadas o contentor.

### <a name="can-i-allow-access-to-other-azure-services-from-a-given-azure-region-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Pode posso "permitir o acesso a outros serviços do Azure a partir de uma determinada região do Azure" quando o acesso ao ponto final de serviço está ativado para a base de dados do Azure Cosmos?  

Isto é necessário apenas quando quiser que a sua conta de base de dados do Azure Cosmos para ser acedido por outra entidade primeiro do Azure para serviços como o Azure Data factory, pesquisa do Azure ou em qualquer serviço que é implementada no fornecido região do Azure.

### <a name="how-many-virtual-network-service-endpoints-are-allowed-for-azure-cosmos-db"></a>Pontos finais de serviço de rede virtual quantos são permitidos para a base de dados do Azure Cosmos?  

pontos finais de serviço de rede virtual 64 são permitidos para uma conta de base de dados do Azure Cosmos.

### <a name="what-is-the-relationship-of-service-endpoint-with-respect-to-network-security-group-nsg-rules"></a>O que é a relação de ponto final de serviço no que respeita à regras do grupo de segurança de rede (NSG)?  

Regra de base de dados do Azure Cosmos do NSG permite acesso de restric apenas para o intervalo de endereços IP de BD do Cosmos do Azure.
  
### <a name="what-is-relationship-between-an-ip-firewall-and-virtual-network-service-endpoint-capability"></a>O que é a relação entre um firewall IP e a capacidade de ponto final do serviço de rede Virtual?  

Estas duas funcionalidades complementam entre si para assegure o isolamento de recursos de base de dados do Azure Cosmos e Proteja-os. Utilizar IP firewall garante que IPs estáticos podem aceder a conta de base de dados do Azure Cosmos.  

### <a name="can-an-on-premise-devices-ip-address-that-is-connected-through-azure-virtual-network-gatewayvpn-or-express-route-gateway-access-azure-cosmos-db-account"></a>Endereço IP de um dispositivo local que está ligado através de gateway(VPN) de rede Virtual do Azure ou o gateway de rota Express pode aceder a BD do Azure do Cosmos conta?  

Endereço IP ou intervalo de endereços IP do dispositivo no local deve ser adicionado à lista de IPs estáticos para aceder à conta de base de dados do Azure Cosmos.  

### <a name="what-happens-if-you-delete-a-virtual-network-that-has-service-endpoint-setup-for-azure-cosmos-db"></a>O que acontece se eliminar uma rede virtual que tenha o programa de configuração de ponto final de serviço para a base de dados do Azure Cosmos?  

Quando uma rede virtual é eliminada, as informações de ACL associados com que a base de dados do Azure Cosmos é eliminado e remove a interação entre a rede virtual e a conta de base de dados do Azure Cosmos. 

### <a name="what-happens-if-an-azure-cosmos-db-account-that-has-virtual-network-service-endpoint-enabled-is-deleted"></a>O que acontece se uma conta de base de dados do Azure Cosmos que tenha o ponto final de serviço de rede virtual ativada é eliminado?

Os metadados associados à conta de base de dados do Azure Cosmos específica é eliminado da sub-rede. Sendo responsabilidade do utilizador final a eliminar a sub-rede e a rede virtual utilizada.

### <a name="can-i-use-a-peered-virtual-network-to-create-service-endpoint-for-azure-cosmos-db"></a>Pode utilizar uma rede virtual em modo de peering para criar o ponto final de serviço para a BD do Cosmos Azure?  

Não, apenas direta rede virtual e as respetivas sub-redes, podem criar pontos finais do serviço de base de dados do Azure Cosmos.

### <a name="what-happens-to-the-source-ip-address-of-resource-like-virtual-machine-in-the-subnet-that-has-azure-cosmos-db-service-endpoint-enabled"></a>O que acontece para o endereço IP de origem do recurso como máquina virtual na sub-rede que tem o ponto final de serviço de base de dados do Azure Cosmos ativado?

Quando são ativados pontos finais do serviço de rede virtual, os endereços IP de origem de recursos na sub-rede da rede virtual passará da utilização de endereços IPV4 públicos para endereços privados da rede Virtual do Azure para o tráfego de BD do Cosmos do Azure.

### <a name="does-azure-cosmos-db-reside-in-the-azure-virtual-network--provided-by-the-customer"></a>Base de dados do Azure Cosmos reside na rede virtual do Azure fornecida ao cliente?  

BD do Cosmos do Azure é um serviço de multi-inquilino com um endereço IP público. Quando restringir o acesso a uma sub-rede de uma rede Virtual do Azure utilizando a funcionalidade de ponto final de serviço, o acesso é limitado para a sua conta de base de dados do Azure Cosmos através da rede Virtual do Azure fornecida e respetiva sub-rede.  Conta de base de dados do Cosmos do Azure não reside na rede Virtual do Azure. 

### <a name="what-if-anything-will-be-logged-in-log-analyticsoms-if-it-is-enabled"></a>E se nada será registado no registo de análise/OMS se estiver ativado?  

BD do Azure do Cosmos emitirá registos com o endereço IP (sem o último octeto) com o estado 403 para o pedido está bloqueado por ACL.  

## <a name="next-steps"></a>Passos Seguintes
Para configurar uma firewall de base de dados do Azure Cosmos consulte [suporte de firewall](firewall-support.md) artigo.

