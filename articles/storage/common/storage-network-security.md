---
title: Configurar o armazenamento do Azure Firewalls e redes virtuais | Documentos da Microsoft
description: Configure a segurança de rede em camadas para a sua conta de armazenamento.
services: storage
author: cbrooksmsft
ms.service: storage
ms.topic: article
ms.date: 10/25/2017
ms.author: cbrooks
ms.component: common
ms.openlocfilehash: 56c464b9fbc9fe5f2d9acd1a403333fd1c729113
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023397"
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks"></a>Configurar o armazenamento do Azure Firewalls e redes virtuais
O armazenamento do Azure fornece um modelo de segurança em camadas, permitindo-lhe proteger as contas de armazenamento para um conjunto específico de redes permitidas.  Quando as regras de rede estiverem configuradas, apenas as aplicações de redes permitidas podem aceder a uma conta de armazenamento.  Ao chamar a partir de uma rede permitida, o aplicações continuam a exigir a autorização adequada (uma chave de acesso válido ou um token SAS) para aceder à conta de armazenamento.

> [!IMPORTANT]
> Ativar as regras de Firewall para a sua conta de armazenamento irá bloquear o acesso a pedidos recebidos para os dados, incluindo a partir de outros serviços do Azure.  Isto inclui a utilizar o Portal, escrever os registos, etc.  Para os serviços de participantes pode reativar o funcionalidade por meio da [exceções](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) secção abaixo.  Para aceder ao Portal terá de fazê-lo a partir de uma máquina dentro do limite confiável (IP ou VNet) que configurou.
>

## <a name="scenarios"></a>Cenários
Contas de armazenamento podem ser configuradas para negar o acesso ao tráfego de todas as redes (incluindo o tráfego de internet), por predefinição.  Acesso pode ser concedido para tráfego de redes virtuais do Azure específicas, permitindo que crie um limite de rede segura para as suas aplicações.  Também é possível conceder acesso à internet pública intervalos de endereços IP, habilitar conexões de clientes de internet ou no local específicos.

Regras de rede são impostas em todos os protocolos de rede ao armazenamento do Azure, incluindo o SMB e REST.  Acesso aos seus dados a partir das ferramentas como o portal do Azure, o Explorador de armazenamento, e AZCopy requerem regras de rede explícita conceder acesso quando as regras de rede estão em vigor.

Regras de rede podem ser aplicadas a contas de armazenamento existentes ou podem ser aplicadas durante a criação de novas contas de armazenamento.

Assim que as regras de rede são aplicadas, elas são impostas para todos os pedidos.  Tokens SAS que concedem acesso a um serviço específico do endereço IP servem para **limite** o acesso do titular da token, mas que não conceder novo acesso além das regras de rede configuradas. 

O tráfego de disco da máquina virtual (incluindo montar e desmontar as operações e e/s de disco) é **não** afetadas pelas regras de rede.  Acesso REST aos blobs de página está protegido por regras de rede.

Contas de armazenamento clássicas **não** suportar Firewalls e redes virtuais.

Cópia de segurança e restauro das máquinas virtuais com discos não geridos nas contas de armazenamento com regras de rede aplicadas é suportado por meio de criar uma exceção, conforme documentado no [exceções](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) seção deste artigo.  Exceções de firewall não são aplicáveis com os Managed Disks, como já são geridos pelo Azure.

## <a name="change-the-default-network-access-rule"></a>Alterar a regra de acesso de rede predefinida
Por predefinição, as contas de armazenamento aceitam ligações de clientes em qualquer rede.  Para limitar o acesso a redes selecionadas, primeiro tem de alterar a ação predefinida.

> [!WARNING]
> Fazer alterações às regras de rede pode afetar a capacidade de seus aplicativos para se ligar ao armazenamento do Azure.  Definir a regra de rede predefinido para **negar** todos os blocos de acesso aos dados, a menos que as regras de rede específicas *conceder* acesso também são aplicadas.  Certifique-se de que conceder acesso a quaisquer redes permitidas utilizar regras de rede antes de alterar a regra predefinida para negar o acesso.
>

#### <a name="azure-portal"></a>Portal do Azure
1. Navegue para a conta de armazenamento que pretende proteger.  

2. Clique no menu de definições denominado **Firewalls e redes virtuais**.
3. Para negar o acesso por predefinição, optar por permitir acesso a partir de "Redes selecionadas".  Para permitir tráfego de todas as redes, optar por permitir acesso de "Todas as redes".
4. Clique em *guardar* para aplicar as alterações.

#### <a name="powershell"></a>PowerShell
1. Instalar a versão mais recente [do Azure PowerShell](/powershell/azure/install-azurerm-ps) e [início de sessão](/powershell/azure/authenticate-azureps).

2. Apresenta o estado da regra predefinida para a conta de armazenamento.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet  -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
``` 

3. Defina a regra predefinida para negar o acesso à rede por predefinição.  
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
```    

4. Defina a regra predefinida para permitir o acesso à rede por predefinição.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
```    

#### <a name="cliv2"></a>CLIv2
1. [Instalar a CLI do Azure 2.0](/cli/azure/install-azure-cli) e [início de sessão](/cli/azure/authenticate-azure-cli).
2. Apresenta o estado da regra predefinida para a conta de armazenamento.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
```

3. Defina a regra predefinida para negar o acesso à rede por predefinição.  
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Deny
```

4. Defina a regra predefinida para permitir o acesso à rede por predefinição.
```azurecli
az storage account update --name "mystorageaccount" --resource-group "myresourcegroup" --default-action Allow
```

## <a name="grant-access-from-a-virtual-network"></a>Conceder acesso a partir de uma rede virtual
Contas de armazenamento podem ser configuradas para permitir o acesso apenas a partir de redes virtuais do Azure específicas. 

Ativando um [ponto final de serviço](/azure/virtual-network/virtual-network-service-endpoints-overview) armazenamento do Azure dentro da rede Virtual, o tráfego é certificar-se de uma rota ideal para o serviço de armazenamento do Azure. As identidades de rede virtual e sub-rede também são transmitidas com cada solicitação.  Os administradores podem, em seguida, configurar regras de rede para a conta de armazenamento que permitem que pedidos a serem recebidos de sub-redes específicas na rede Virtual.  Os clientes concedido o acesso através destas regras de rede têm de continuar para cumprir os requisitos de autorização da conta de armazenamento para acessar os dados.

Cada conta de armazenamento pode suportar até 100 regras de rede Virtual que podem ser combinadas com [regras de rede IP](#grant-access-from-an-internet-ip-range).

### <a name="available-virtual-network-regions"></a>Regiões disponíveis da rede Virtual
Em geral, os pontos finais de serviço funcionam entre redes virtuais e instâncias do serviço na mesma região do Azure.  Quando os pontos finais de serviço são utilizados com o armazenamento do Azure, este âmbito é expandido para incluir o [região emparelhada](/azure/best-practices-availability-paired-regions).  Isso permite continuidade durante uma ativação pós-falha regional, bem como acesso totalmente integrado para instâncias só de leitura (RA-GRS) de armazenamento georredundante.  Regras de rede que concedam acesso a partir de uma rede virtual para uma conta de armazenamento também concedem acesso a qualquer instância de RA-GRS.

Quando planear a recuperação após desastre durante uma falha regional, deve aprovisionar as redes virtuais na região emparelhada com antecedência. Pontos finais de serviço do armazenamento do Azure deve estar ativados e conceder acesso a partir destas redes virtuais alternativo de regras de rede devem ser aplicadas às suas contas de armazenamento georredundante.

> [!NOTE]
> Pontos finais de serviço não se aplicam ao tráfego fora da região de rede Virtual e o par de regiões designado.  Regras de rede, conceder acesso a partir de redes virtuais para contas de armazenamento só podem ser aplicadas para as redes virtuais na região primária de uma conta de armazenamento ou na região emparelhada designado.
>

### <a name="required-permissions"></a>Permissões obrigatórias
Para aplicar uma regra de rede Virtual a uma conta de armazenamento, o utilizador tem de ter permissão para *aderir ao serviço a uma sub-rede* para as sub-redes que está a ser adicionadas.  Esta permissão está incluído nos *contribuinte de conta de armazenamento* função incorporada e podem ser adicionadas às definições de função personalizada.

Conta de armazenamento e as redes virtuais acesso concedido **poderá** ser em subscrições diferentes, mas essas subscrições têm de fazer parte do mesmo inquilino do Azure Active Directory.

### <a name="managing-virtual-network-rules"></a>Gerir regras de rede Virtual
Regras de rede virtual para contas de armazenamento podem ser geridas através do portal do Azure, PowerShell ou CLIv2.

#### <a name="azure-portal"></a>Portal do Azure
1. Navegue para a conta de armazenamento que pretende proteger.  
2. Clique no menu de definições denominado **Firewalls e redes virtuais**.
3. Certifique-se de que tiver optado por permitir acesso a partir de "Redes selecionadas".
4. Para conceder acesso a uma rede Virtual com uma nova regra de rede, em "Redes virtuais", clique em "Adicionar existente" para selecionar uma rede Virtual existente e sub-redes, em seguida, clique em *adicionar*.  Para criar uma nova rede Virtual e conceder acesso, clique em *adicionar novo*, forneça as informações necessárias para criar a nova rede Virtual e, em seguida, clique em *criar*.

> [!NOTE]
> Se não tiver sido configurado anteriormente um ponto de extremidade do serviço de armazenamento do Azure para a rede Virtual e sub-redes selecionadas, ele pode ser configurado como parte desta operação.
>

5. Para remover uma regra de rede ou sub-rede Virtual, clique em "..." para abrir o menu de contexto para a rede Virtual ou a sub-rede e clique em "Remover".
6. Clique em *guardar* para aplicar as alterações.

#### <a name="powershell"></a>PowerShell
1. Instalar a versão mais recente [do Azure PowerShell](/powershell/azure/install-azurerm-ps) e [início de sessão](/powershell/azure/authenticate-azureps).
2. Regras de rede Virtual da lista
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
```

3. Ativar o ponto final de serviço do armazenamento do Azure numa rede Virtual e sub-rede existente
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"  -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzureRmVirtualNetwork
```

4. Adicione uma regra de rede para uma rede Virtual e uma sub-rede.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

5. Remova uma regra de rede para uma rede Virtual e uma sub-rede.  
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Remove-AzureRmStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id   
```    

> [!IMPORTANT]
> Não se esqueça [definir a regra predefinida](#change-the-default-network-access-rule) para negar, ou as regras de rede não tem qualquer efeito.
>

#### <a name="cliv2"></a>CLIv2
1. [Instalar a CLI do Azure 2.0](/cli/azure/install-azure-cli) e [início de sessão](/cli/azure/authenticate-azure-cli).
2. Regras de rede Virtual da lista
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
```

2. Ativar o ponto final de serviço do armazenamento do Azure numa rede Virtual e sub-rede existente
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
```

3. Adicione uma regra de rede para uma rede Virtual e uma sub-rede.  
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

4. Remova uma regra de rede para uma rede Virtual e uma sub-rede. 
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
```

> [!IMPORTANT]
> Não se esqueça [definir a regra predefinida](#change-the-default-network-access-rule) para negar, ou as regras de rede não tem qualquer efeito.
>

## <a name="grant-access-from-an-internet-ip-range"></a>Conceder acesso a partir de um internet intervalo IP
Contas de armazenamento podem ser configuradas para permitir o acesso a partir da internet pública específica intervalos de endereços IP.  Esta configuração permite que os serviços específicos baseados na internet e redes para ter acesso enquanto o tráfego de internet geral é bloqueado no local.

Permitido de intervalos de endereços da internet podem ser fornecidos usando [notação CIDR](https://tools.ietf.org/html/rfc4632) sob a forma *16.17.18.0/24* ou como individual IP, como endereços *16.17.18.19* .

> [!NOTE]
> Intervalos de endereços pequenos com "/ 31" ou "/ 32" prefixo tamanhos não são suportados.  Estes intervalos devem ser configurados com regras de endereços IP individuais.
>

Regras de rede IP só são permitidas para **internet pública** endereços IP.  Intervalos de endereços IP reservados para redes privadas (conforme definido em RFC 1918) não são permitidos em regras de IP.  Redes privadas incluem endereços que comecem com *10.\** , *172.16.\** , e *192.168.\** .

Apenas endereços IPV4 são suportados neste momento.

Cada conta de armazenamento pode suportar até 100 regras de rede IP que podem ser combinadas com [regras de rede Virtual](#grant-access-from-a-virtual-network)

### <a name="configuring-access-from-on-premises-networks"></a>Configurar o acesso a partir de redes no local
Para conceder acesso a partir de suas redes no local para a sua conta de armazenamento com uma regra de rede IP, tem de identificar endereços IP utilizados pela sua rede de acesso à internet.  Contacte o administrador de rede para obter ajuda.

Se a sua rede está ligada ao uso de rede do Azure [ExpressRoute](/azure/expressroute/expressroute-introduction), cada circuito está configurado com dois endereços IP públicos no Microsoft Edge que são utilizados para ligar ao Microsoft Services, como o armazenamento do Azure com [Peering público do azure](/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains).  Para permitir a comunicação do seu circuito ao armazenamento do Azure, tem de criar regras de rede IP para os endereços IP públicos dos seus circuitos.  Para localizar os endereços IP públicos do seu circuito do ExpressRoute, [abra um pedido de suporte no ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) através do portal do Azure.


### <a name="managing-ip-network-rules"></a>Gerir regras de rede IP
Regras de rede IP para as contas de armazenamento podem ser geridas através do portal do Azure, PowerShell ou CLIv2.

#### <a name="azure-portal"></a>Portal do Azure
1. Navegue para a conta de armazenamento que pretende proteger.  
2. Clique no menu de definições denominado **Firewalls e redes virtuais**.
3. Certifique-se de que tiver optado por permitir acesso a partir de "Redes selecionadas".
4. Para conceder acesso a um internet intervalo de IP, introduza o endereço IP ou intervalo de endereços (no formato CIDR) na Firewall, intervalos de endereços.
5. Para remover uma regra de rede IP, clique em "..." para abrir o menu de contexto para a regra e clique em "Remover".
6. Clique em *guardar* para aplicar as alterações.

#### <a name="powershell"></a>PowerShell
1. Instalar a versão mais recente [do Azure PowerShell](/powershell/azure/install-azurerm-ps) e [início de sessão](/powershell/azure/authenticate-azureps).
2. Listar regras de rede IP.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
```

3. Adicione uma regra de rede para um endereço IP individual.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19" 
``` 

4. Adicione uma regra de rede para um intervalo de endereços IP.  
```PowerShell
Add-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24" 
```    

5. Remova uma regra de rede para um endereço IP individual. 
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"  
```

6. Remova uma regra de rede para um intervalo de endereços IP.  
```PowerShell
Remove-AzureRMStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"  
```    

> [!IMPORTANT]
> Não se esqueça [definir a regra predefinida](#change-the-default-network-access-rule) para negar, ou as regras de rede não tem qualquer efeito.
>

#### <a name="cliv2"></a>CLIv2
1. [Instalar a CLI do Azure 2.0](/cli/azure/install-azure-cli) e [início de sessão](/cli/azure/authenticate-azure-cli).
2. Regras de rede IP de lista
```azurecli
az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
```

3. Adicione uma regra de rede para um endereço IP individual.
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

4. Adicione uma regra de rede para um intervalo de endereços IP.  
```azurecli
az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

5. Remova uma regra de rede para um endereço IP individual.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
```

6. Remova uma regra de rede para um intervalo de endereços IP.  
```azurecli
az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
```

> [!IMPORTANT]
> Não se esqueça [definir a regra predefinida](#change-the-default-network-access-rule) para negar, ou as regras de rede não tem qualquer efeito.
>

## <a name="exceptions"></a>Exceções
Enquanto as regras de rede podem ativar uma configuração de rede segura para a maioria dos cenários, existem alguns casos em que as exceções devem ser concedidas para ativar a funcionalidade completa.  Contas de armazenamento podem ser configuradas com exceções para os serviços Microsoft fidedignos e para acesso a dados de análise de armazenamento.

### <a name="trusted-microsoft-services"></a>Serviços Microsoft fidedignos
Alguns serviços da Microsoft que interagem com contas de armazenamento operam de redes que não não possível conceder acesso por meio de regras de rede. 

Para permitir que este tipo de serviço a funcionar como pretendido, é possível permitir o conjunto de serviços Microsoft fidedignos para ignorar as regras de rede. Estes serviços, em seguida, irão utilizar a autenticação segura para aceder à conta de armazenamento.

Quando a exceção de "Serviços Microsoft fidedignos" está ativada, os seguintes serviços (quando é registado na sua subscrição) são concedidos acesso à conta de armazenamento:

|Serviço|Nome do fornecedor de recursos|Objetivo|
|:------|:---------------------|:------|
|Azure Backup|Microsoft.Backup|Efetue cópias de segurança e restauros de discos não geridos em máquinas de virtuais IAAS. (não necessário para discos geridos). [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup).|
|Azure DevTest Labs|Microsoft.DevTestLab|Instalação de Federação e de criação de imagem personalizada.  [Saiba mais](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-overview).|
|Azure Event Grid|Microsoft.EventGrid|Ative a publicação de eventos de armazenamento de Blobs.  [Saiba mais](https://docs.microsoft.com/azure/event-grid/overview).|
|Azure Event Hubs|Microsoft.EventHub|Arquivar dados com a captura de Hubs de eventos.  [Saiba mais](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview).|
|Redes do Azure|Microsoft.Networking|Store e analisar registos de tráfego de rede.  [Saiba mais](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview).|
||||

### <a name="storage-analytics-data-access"></a>Acesso de dados de análise de armazenamento
Em alguns casos, o acesso para ler os registos de diagnóstico e métricas é necessário a partir de fora do limite de rede.  Exceções às regras de rede podem ser concedidas para permitir o acesso de leitura para o armazenamento de ficheiros de registo de conta, as tabelas de métricas ou ambos. [Saiba mais sobre como trabalhar com a análise de armazenamento.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Gestão de exceções
Exceções de regra de rede podem ser geridas através do portal do Azure, o PowerShell ou a CLI do Azure v2.

#### <a name="azure-portal"></a>Portal do Azure
1. Navegue para a conta de armazenamento que pretende proteger.  
2. Clique no menu de definições denominado **Firewalls e redes virtuais**.
3. Certifique-se de que tiver optado por permitir acesso a partir de "Redes selecionadas".
4. Em exceções, selecione as exceções que pretende conceder.
5. Clique em *guardar* para aplicar as alterações.

#### <a name="powershell"></a>PowerShell
1. Instalar a versão mais recente [do Azure PowerShell](/powershell/azure/install-azurerm-ps) e [início de sessão](/powershell/azure/authenticate-azureps).
2. Apresente as exceções para as regras de rede da conta de armazenamento.
```PowerShell
(Get-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
```

3. Configure as exceções às regras de rede de conta de armazenamento.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass AzureServices,Metrics,Logging
```

4. Remova as exceções às regras de rede de conta de armazenamento.
```PowerShell
Update-AzureRmStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount"  -Bypass None
```

> [!IMPORTANT]
> Não se esqueça [definir a regra predefinida](#change-the-default-network-access-rule) para negar, ou remoção de exceções não tem qualquer efeito.
>

#### <a name="cliv2"></a>CLIv2
1. [Instalar a CLI do Azure 2.0](/cli/azure/install-azure-cli) e [início de sessão](/cli/azure/authenticate-azure-cli).
2. Apresente as exceções para as regras de rede da conta de armazenamento.
```azurecli
az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
```

3. Configure as exceções às regras de rede de conta de armazenamento.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
```

4. Remova as exceções às regras de rede de conta de armazenamento.
```azurecli
az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
```

> [!IMPORTANT]
> Não se esqueça [definir a regra predefinida](#change-the-default-network-access-rule) para negar, ou remoção de exceções não tem qualquer efeito.
>

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre pontos finais de serviço de rede do Azure no [pontos finais de serviço](/azure/virtual-network/virtual-network-service-endpoints-overview).

Aprofunde os seus conhecimentos sobre a segurança de armazenamento do Azure no [guia de segurança de armazenamento do Azure](storage-security-guide.md).
