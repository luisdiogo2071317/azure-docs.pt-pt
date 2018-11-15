---
title: Como configurar a firewall do IP para a sua conta do Cosmos do Azure
description: Saiba como configurar políticas de controlo de acesso IP para o suporte de firewall em contas de base de dados do Azure Cosmos DB.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: d9a00bccb83fc60c96594ffacc5abde98c0f8470
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632588"
---
# <a name="how-to-configure-ip-firewall-for-your-azure-cosmos-account"></a>Como configurar a firewall do IP para a sua conta do Cosmos do Azure

Pode proteger os dados armazenados na sua conta do Cosmos do Azure através de firewalls IP. O Azure Cosmos DB suporta controlos de acesso baseado em IP para o suporte de firewall de entrada. Pode definir uma firewall do IP da conta do Cosmos do Azure ao utilizar uma das seguintes formas:

1. No portal do Azure
2. Forma declarativa utilizando um modelo Azure Resource Manager
3. Por meio de programação através da CLI do Azure ou o Azure Powershell, atualizando a **ipRangeFilter** propriedade.

## <a id="configure-ip-policy"></a> Configurar a firewall do IP através do portal do Azure

Para definir a política de controlo de acesso IP no portal do Azure, navegue para a página de conta do Azure Cosmos DB, clique em **firewalls e redes virtuais** no menu de navegação, em seguida, altere a **autorizar o acesso** valor para **redes selecionadas**e, em seguida, clique em **guardar**. 

![Captura de ecrã que mostra como abrir a página de Firewall no portal do Azure](./media/how-to-configure-firewall/azure-portal-firewall.png)

Assim que o controlo de acesso IP estiver ativado, o portal do Azure fornece a capacidade de especificar endereços IP, intervalos de endereços IP, bem como opções para ativar o acesso a outros serviços do Azure e o portal do Azure. Os detalhes sobre esses comutadores são fornecidos nas secções seguintes.

> [!NOTE]
> Depois de ativar a política de controlo de acesso IP para a sua conta do Cosmos do Azure, todos os pedidos à sua conta do Cosmos do Azure a partir de máquinas fora da lista de intervalos de endereços IP permitidos são rejeitados. Os recursos do Azure Cosmos DB no portal de navegação também estão bloqueadas para assegurar a integridade de controlo de acesso.

### <a name="allow-requests-from-the-azure-portal"></a>Permitir pedidos a partir do portal do Azure 

Quando ativa a uma política de controlo de acesso IP por meio de programação, precisa adicionar o endereço IP para o portal do Azure para o **ipRangeFilter** propriedade para manter o acesso. Os endereços IP portais são:

|Região|Endereço IP|
|------|----------|
|Alemanha|51.4.229.218|
|China|139.217.8.252|
|Governo dos EUA|52.244.48.71|
|Todas as regiões, exceto os três acima|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Pode ativar o acesso ao portal do Azure ao escolher o **permitir o acesso a partir do portal do Azure** opção conforme mostrado na captura de ecrã seguinte: 

![Captura de ecrã que mostra como ativar o acesso ao portal do Azure](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Permitir pedidos a partir de datacenters globais do Azure ou de outras origens no Azure

Se aceder a conta do Cosmos do Azure dos serviços que não fornecem um IP estático, análises de Stream como o Azure, etc. de funções do Azure, ainda pode usar o firewall do IP para limitar o acesso. Para permitir o acesso à conta do Cosmos do Azure de tais serviços, uma definição de firewall tem de estar ativada. Para ativar esta definição de firewall, adicione o endereço IP-0.0.0.0 à lista de endereços IP permitidos. 0.0.0.0 restringe pedidos à sua conta do Cosmos do Azure de intervalo IP do datacenter do Azure. Esta definição não permite o acesso para quaisquer outros intervalos IP para a sua conta do Cosmos do Azure.

> [!NOTE]
> Esta opção configura a firewall para permitir que todos os pedidos do Azure pedidos incluindo de subscrições de outros clientes implementados no Azure. A lista de IPs permitidos por esta opção for grande, então, limita a eficácia da política de firewall. Esta opção deve ser utilizada apenas se os pedidos não provêm de IPs estáticos ou de sub-redes em VNETs. Escolher automaticamente esta opção permite o acesso do portal do Azure, uma vez que o portal do Azure é implementado no Azure.

Pode ativar o acesso ao portal do Azure ao escolher o **aceitar ligações dos datacenters do Azure públicos** opção conforme mostrado na captura de ecrã seguinte: 

![Captura de ecrã que mostra como abrir a página de Firewall no portal do Azure](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Pedidos de seu IP atual

Para simplificar o desenvolvimento, o portal do Azure ajuda-o a identificar e adicione o IP do seu computador cliente à lista de permissões, para que as aplicações em execução de seu computador podem aceder à sua conta de Cosmos do Azure. O endereço IP do cliente é detetado automaticamente pelo portal. Pode ser o endereço IP de cliente da sua máquina ou o endereço IP do gateway da rede. Certifique-se remover este endereço IP antes de fazer as suas cargas de trabalho de produção. 

Para ativar o seu IP atual, selecione **adicionar o meu IP atual**, que adiciona o seu IP atual para a lista de IPs e, em seguida, clique em **guardar**.

![Captura de ecrã que mostra um como configurar as definições de firewall para o IP atual](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Pedidos de serviços cloud

No Azure, serviços cloud são uma maneira comum para hospedar a lógica de serviço de camada média com o Azure Cosmos DB. Para ativar o acesso à sua conta do Cosmos do Azure a partir de um serviço em nuvem, o endereço IP público do serviço cloud tem de ser adicionado à lista de permissões de endereços IP associados à sua conta do Cosmos do Azure por [configurar a política de controlo de acesso IP](#configure-ip-policy). Isto garante que todas as instâncias de função dos serviços cloud têm acesso à sua conta do Cosmos do Azure. Pode recuperar endereços IP para os seus serviços cloud no portal do Azure, conforme mostrado na captura de ecrã seguinte:

![Captura de ecrã que mostra o endereço IP público para um serviço cloud apresentado no portal do Azure](./media/how-to-configure-firewall/public-ip-addresses.png)

Ao aumentar horizontalmente o seu serviço cloud adicionando instâncias de função adicionais, essas novas instâncias têm automaticamente acesso à conta do Cosmos do Azure, uma vez que fazem parte do mesmo serviço cloud.

### <a name="requests-from-virtual-machines"></a>Pedidos provenientes de máquinas virtuais

[Máquinas virtuais](https://azure.microsoft.com/services/virtual-machines/) ou [conjuntos de dimensionamento de máquinas virtuais](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) também podem ser utilizadas para alojar os serviços de camada média com o Azure Cosmos DB. Para configurar a sua conta do Cosmos para permitir o acesso a partir de máquinas virtuais, os endereços IP públicos de máquina virtual e/ou conjunto de dimensionamento de máquina virtual tem de ser configurados como um dos endereços IP permitidos para a sua conta do Cosmos do Azure por [configuração de IP política de controlo de acesso](#configure-ip-policy). Pode recuperar endereços IP para máquinas virtuais no portal do Azure, conforme mostrado na captura de ecrã seguinte:

![Captura de ecrã que mostra um endereço IP público para uma máquina virtual apresentado no portal do Azure](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Ao adicionar instâncias de máquina virtual adicionais ao grupo, eles são automaticamente fornecidos a acesso à sua conta do Cosmos do Azure.

### <a name="requests-from-the-internet"></a>Pedidos a partir da internet

Quando acessa o sua conta do Cosmos do Azure a partir de um computador na internet, o endereço IP do cliente ou um intervalo de endereços IP da máquina tem de ser adicionado à lista de permitidos do endereço IP para a sua conta.

## <a id="configure-ip-firewall-arm"></a>Configurar a firewall do IP utilizando o modelo do Resource Manager

Para configurar o controlo de acesso à sua conta do Cosmos do Azure, deve especificar o modelo do Resource Manager a **ipRangeFilter** atributo com uma lista de intervalos IP, o que deve estar na lista de permissões. Por exemplo, adicione o seguinte código JSON ao seu modelo:

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
     "databaseAccountOfferType": "Standard",
     "name": "[parameters('databaseAccountName')]",
     "ipRangeFilter":"183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
   }
```

## <a id="configure-ip-firewall-cli"></a>Configurar a política de controlo de acesso IP com a CLI do Azure

O comando seguinte mostra como criar uma conta do Cosmos do Azure com o controlo de acesso IP: 

```azurecli-interactive

name="<Azure Cosmos account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

Para atualizar as definições de firewall para uma conta existente, execute o seguinte comando:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="troubleshoot-ip-firewall"></a>Como resolver problemas de políticas de controlo de acesso IP

Pode resolver problemas de políticas de controlo de acesso IP utilizando as seguintes opções: 

### <a name="azure-portal"></a>Portal do Azure 
Ao ativar uma política de controlo de acesso IP para a sua conta do Cosmos do Azure, todos os pedidos à sua conta de máquinas fora da lista de intervalos de endereços IP permitidos são bloqueados. Portanto, se pretender ativar as operações de plano de dados de portal, como navegar na contentores e consultar documentos, precisa permitir explicitamente o acesso ao portal do Azure utilizando o **Firewall** painel no portal.

### <a name="sdks"></a>SDKs 
Ao aceder aos recursos do Azure Cosmos através de SDKs de máquinas que não estejam na lista de permitidos **uma resposta não foi encontrado de 404 genérica é devolvido com sem detalhes adicionais**. Verifique se a lista IP permitida para a sua conta e certifique-se de que a configuração da política correta é aplicada à sua conta do Cosmos. 

### <a name="check-source-ips-in-blocked-requests"></a>Verifique os IPs de origem em pedidos bloqueados
Ativar o diagnóstico de registo na sua conta do Cosmos do Azure, estes registos mostrar cada solicitação e resposta. **As mensagens relacionadas com a firewall internamente são registadas com um código de retorno 403**. Ao filtrar essas mensagens, pode ver os IPs de origem para os pedidos bloqueados. Ver [registo de diagnósticos do Azure Cosmos DB](logging.md).

### <a name="requests-from-subnet-with-service-endpoint-for-azure-cosmos-database-enabled"></a>Pedidos de sub-rede com o ponto final de serviço da base de dados do Cosmos do Azure ativado
Pedidos a partir de uma sub-rede na VNET com o ponto final de serviço para o Azure Cosmos DB ativada a identidade VNET e sub-rede envia para contas do Cosmos do Azure. Estes pedidos não tem o IP público da origem para que eles são rejeitados pelo filtros de IP. Para permitir o acesso a partir de sub-redes específicas em VNETs, adicionar a lista de controle de acesso VNET descrita [como configurar a rede virtual e o acesso com base na sub-rede para a sua conta do Azure Cosmos](how-to-configure-vnet-service-endpoint.md). Pode demorar até 15 minutos para as regras de firewall aplicar.


## <a name="next-steps"></a>Passos Seguintes

Para configurar o ponto final de serviço de rede virtual para a sua conta do Azure Cosmos DB, veja os artigos seguintes:

* [Controlo de acesso VNET e sub-rede para a sua conta do Cosmos do Azure](vnet-service-endpoint.md)
* [Como configurar a rede virtual e acesso de uma sub-rede com base para a sua conta do Cosmos do Azure](how-to-configure-vnet-service-endpoint.md)


