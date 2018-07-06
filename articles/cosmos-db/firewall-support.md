---
title: Controlo de acesso do Azure Cosmos DB suporte de firewall & IP | Documentos da Microsoft
description: Saiba como utilizar políticas de controlo de acesso IP para o suporte de firewall em contas de base de dados do Azure Cosmos DB.
keywords: Controlo de acesso IP, o suporte de firewall
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: sngun
ms.openlocfilehash: e67fc5e00a638f116a69dbb36c60cf183cbde808
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857835"
---
# <a name="azure-cosmos-db-firewall-support"></a>Suporte de firewall do Azure Cosmos DB
Para proteger os dados armazenados numa conta de base de dados do Azure Cosmos DB, o Azure Cosmos DB forneceu suporte para um segredo com base [modelo de autorização](https://msdn.microsoft.com/library/azure/dn783368.aspx) que utiliza um código de autenticação forte de mensagens baseada em Hash (HMAC). Agora, além do modelo de autorização com base secreta, Azure Cosmos DB suporta a política de controlado por controlos de acesso baseado em IP para o suporte de firewall de entrada. Esse modelo é semelhante para as regras de firewall de um sistema de base de dados tradicionais e fornece um nível adicional de segurança para a conta de base de dados do Azure Cosmos DB. Com esse modelo, agora pode configurar uma conta de base de dados do Azure Cosmos DB para ser acessível apenas a partir de um conjunto aprovado de computadores e/ou serviços em nuvem. Acesso a recursos do Azure Cosmos DB a partir esses aprovados conjuntos de máquinas e serviços ainda exigem o autor da chamada apresentar um token de autorização válida.

> [!NOTE]
> Atualmente o suporte de firewall está disponível para contas de API de SQL do Azure Cosmos DB e a API do Mongo. Capacidade para configurar firewalls para outras APIs e clouds soberanas, como o Azure Alemanha ou do Azure Government estará disponível em breve. Se estiver a planear configurar o ponto final de serviço ACL para a sua conta do Azure Cosmos DB, que tem uma firewall do IP existente configurada, tenha em atenção a configuração de firewall, remover o firewall do IP e, em seguida, configure o ponto de final de serviço ACL. Depois de configurar o ponto final de serviço, pode voltar a ativar a firewall do IP se for necessário.

## <a name="ip-access-control-overview"></a>Descrição geral de controlo de acesso IP
Por predefinição, uma conta de base de dados do Azure Cosmos DB está acessível a partir da Internet pública, desde que o pedido seja acompanhado por um token de autorização válido. Para configurar o controlo de acesso baseado em política de IP, o utilizador tem de fornecer o conjunto de endereços IP ou intervalos de endereços IP no formulário CIDR para serem incluídos como a lista de IPs de cliente permitidos para uma determinada conta de base de dados. Quando esta configuração for aplicada, todos os pedidos provenientes de máquinas fora desta lista de permissões são bloqueados pelo servidor.  A ligação ao processamento de fluxo para o controlo de acesso baseado em IP é descrita no diagrama seguinte:

![Diagrama que mostra o processo de ligação para o controlo de acesso baseado em IP](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> Configurar a política de controlo de acesso IP
A política de controlo de acesso IP pode ser definida no portal do Azure, ou por meio de programação através de [CLI do Azure](cli-samples.md), [Azure Powershell](powershell-samples.md), ou o [REST API](/rest/api/cosmos-db/) atualizando o **ipRangeFilter** propriedade. 

Para definir a política de controlo de acesso IP no portal do Azure, navegue para a página de conta do Azure Cosmos DB, clique em **firewalls e redes virtuais** no menu de navegação, em seguida, altere a **autorizar o acesso** valor para **redes selecionadas**e, em seguida, clique em **guardar**. 

![Captura de ecrã que mostra como abrir a página de Firewall no portal do Azure](./media/firewall-support/azure-portal-firewall.png)

Assim que o controlo de acesso IP estiver no, o portal fornece a capacidade de especificar endereços IP e intervalos, bem como opções para ativar o acesso a outros serviços do Azure e o portal do Azure. São fornecidas informações adicionais sobre esses comutadores nas seções a seguir.

> [!NOTE]
> Ao ativar uma política de controlo de acesso IP para a sua conta de base de dados do Azure Cosmos DB, todo o acesso à sua conta de base de dados do Azure Cosmos DB a partir de máquinas fora configurada permitido a lista de intervalos de endereços IP são bloqueados. Por esse modelo, a operação de plano de dados no portal de navegação também será bloqueada para assegurar a integridade de controlo de acesso.

## <a name="connections-from-the-azure-portal"></a>Ligações a partir do portal do Azure 

Quando ativa a uma política de controlo de acesso IP por meio de programação, precisa adicionar o endereço IP para o portal do Azure para o **ipRangeFilter** propriedade para manter o acesso. Os endereços IP portais são:

|Região|Endereço IP|
|------|----------|
|Todas as regiões, exceto as especificado abaixo|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Alemanha|51.4.229.218|
|China|139.217.8.252|
|Governo dos EUA|52.244.48.71|

Acesso ao portal do Azure está ativado por predefinição, quando altera a definição de Firewall para **redes selecionadas** no portal do Azure. 

![Captura de ecrã que mostra como ativar o acesso ao portal do Azure](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-global-azure-datacenters-or-azure-paas-services"></a>Ligações a partir de centros de dados do Global Azure ou serviços PaaS do Azure
No Azure, os serviços de PaaS, como o do Azure Stream analytics, as funções do Azure e serviço de aplicações do Azure são utilizados em conjunto com o Azure Cosmos DB. Para ativar o acesso ao Azure Cosmos DB conta de base de dados a partir destes serviços cujos endereços IP não estão prontamente disponíveis, adicione o endereço IP 0.0.0.0 à lista de permissões de endereços IP associados a sua conta de base de dados do Azure Cosmos DB através de programação. 

Acesso para as ligações dos datacenters do Azure global está ativado por predefinição, quando altera a definição de Firewall para **redes selecionadas** no portal do Azure. 

![Captura de ecrã que mostra como abrir a página de Firewall no portal do Azure](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Ligações a partir de seu IP atual

Para simplificar o desenvolvimento, o portal do Azure ajuda-o a identificar e adicione o IP do seu computador cliente à lista de permissões, para que as aplicações em execução a máquina podem acessar a conta do Azure Cosmos DB. O endereço IP do cliente é detetado como visto pelo portal. Pode ser o endereço IP de cliente da sua máquina, mas também pode ser o endereço IP do gateway da rede. Não se esqueça de remover antes de entrar em produção.

Para ativar o seu IP atual, selecione **adicionar o meu IP atual**, que adiciona o seu IP atual para a lista de IPs e, em seguida, clique em **guardar**.

![Captura de ecrã que mostra um como configurar as definições de firewall para o IP atual](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Ligações a partir de serviços cloud
No Azure, serviços cloud são uma maneira comum para hospedar a lógica de serviço de camada média com o Azure Cosmos DB. Para ativar o acesso a uma conta de base de dados do Azure Cosmos DB a partir de um serviço em nuvem, o endereço IP público do serviço cloud tem de ser adicionado à lista de permissões de endereços IP associados à sua conta de base de dados do Azure Cosmos DB por [configurar o acesso IP diretiva de controle](#configure-ip-policy). Isto garante que todas as instâncias de função dos serviços cloud têm acesso à sua conta de base de dados do Azure Cosmos DB. Pode recuperar endereços IP para os seus serviços cloud no portal do Azure, conforme mostrado na captura de ecrã seguinte:

![Captura de ecrã que mostra o endereço IP público para um serviço cloud apresentado no portal do Azure](./media/firewall-support/public-ip-addresses.png)

Ao aumentar horizontalmente o seu serviço cloud adicionando instâncias de função adicionais, essas novas instâncias têm automaticamente acesso à conta de base de dados do Azure Cosmos DB, uma vez que fazem parte do mesmo serviço cloud.

## <a name="connections-from-virtual-machines"></a>Ligações a partir de máquinas virtuais
[Máquinas virtuais](https://azure.microsoft.com/services/virtual-machines/) ou [conjuntos de dimensionamento de máquinas virtuais](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) também podem ser utilizadas para alojar os serviços de camada média com o Azure Cosmos DB.  Para configurar a conta de base de dados do Azure Cosmos DB para permitir o acesso a partir de máquinas virtuais, endereços IP públicos de máquina virtual e/ou de máquina virtual, conjunto de dimensionamento deve ser configurado, como um dos endereços IP permitidos para a sua conta de base de dados do Azure Cosmos DB, pela [configurar a política de controlo de acesso IP](#configure-ip-policy). Pode recuperar endereços IP para máquinas virtuais no portal do Azure, conforme mostrado na captura de ecrã seguinte.

![Captura de ecrã que mostra um endereço IP público para uma máquina virtual apresentado no portal do Azure](./media/firewall-support/public-ip-addresses-dns.png)

Ao adicionar instâncias de máquina virtual adicionais ao grupo, eles são automaticamente fornecidos a acesso à sua conta de base de dados do Azure Cosmos DB.

## <a name="connections-from-the-internet"></a>Ligações da internet
Quando acessa uma conta de base de dados do Azure Cosmos DB a partir de um computador na internet, o endereço IP do cliente ou um intervalo de endereços IP da máquina tem de ser adicionado à lista de permitidos do endereço IP para a conta de base de dados do Azure Cosmos DB. 

## <a name="using-azure-resource-manager-template-to-set-up-the-ip-access-control"></a>Utilizar o modelo do Azure Resource Manager para configurar o controlo de acesso IP

Adicione o seguinte JSON ao modelo para configurar o controlo de acesso IP. Modelo do Resource Manager para uma conta terão o atributo de ipRangeFilter que é a lista de intervalos IP, o que deve ser na lista de permissões.

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
     "ipRangeFilter":"10.0.0.1,10.0.0.2,183.240.196.255"
   }
   }
```

## <a name="troubleshooting-the-ip-access-control-policy"></a>A política de controlo de acesso IP de resolução de problemas
### <a name="portal-operations"></a>Operações do portal
Ao ativar uma política de controlo de acesso IP para a sua conta de base de dados do Azure Cosmos DB, todo o acesso à sua conta de base de dados do Azure Cosmos DB a partir de máquinas fora configurada permitido a lista de intervalos de endereços IP são bloqueados. Portanto se pretender ativar as operações de plano de dados de portal, como navegar na contentores e consultar documentos, precisa permitir explicitamente o acesso ao portal do Azure utilizando o **Firewall** página no portal. 

### <a name="sdk--rest-api"></a>SDK e Rest API
Para questões de segurança, acesso via SDK ou REST API de máquinas não na lista de permitidos irão devolver uma resposta não foi encontrado genérico 404 com sem detalhes adicionais. Verifique se o IP permitido lista configurada para a conta de base de dados do Azure Cosmos DB para garantir que a configuração da política correta é aplicada à sua conta de base de dados do Azure Cosmos DB.

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre sugestões de desempenho relacionados com a rede, consulte [sugestões de desempenho](performance-tips.md).

