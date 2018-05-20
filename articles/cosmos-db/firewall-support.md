---
title: Controlo de acesso de suporte de firewall da BD do Cosmos & IP Azure | Microsoft Docs
description: Saiba como utilizar políticas de controlo de acesso IP para o suporte de firewall em contas de base de dados de base de dados do Azure Cosmos.
keywords: Controlo de acesso IP, o suporte de firewall
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-resource-manager
documentationcenter: ''
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: sngun
ms.openlocfilehash: 21274a71042c5acf38711d29a5062e9f68b6a6a0
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="azure-cosmos-db-firewall-support"></a>Suporte de firewall do Cosmos BD do Azure
Para proteger os dados armazenados numa conta de base de dados do Azure Cosmos DB, base de dados do Azure Cosmos tem suporte fornecido para um segredo com base [modelo de autorização](https://msdn.microsoft.com/library/azure/dn783368.aspx) que utiliza um código de autenticação de mensagem com base em Hash seguro (HMAC). Agora, para além do modelo de autorização baseada em segredo BD do Cosmos do Azure suporta a política orientadas por controlos de acesso baseado em IP para o suporte de firewall de entrada. Este modelo é semelhante para as regras de firewall de um sistema de bases de dados tradicionais e fornece um nível adicional de segurança para a conta de base de dados de base de dados do Azure Cosmos. Com este modelo, pode agora configurar uma conta de base de dados de base de dados do Azure Cosmos para ser acessível apenas a partir de um conjunto de máquinas aprovado e/ou serviços em nuvem. Acesso a recursos de base de dados do Azure Cosmos destes conjuntos de serviços e máquinas aprovados ainda requerem o autor da chamada apresentar um token de autorização válida.

> [!NOTE]
> Suporte de firewall está atualmente disponível para contas de API do Azure Cosmos BD SQL e Mongo API. Capacidade de configurar as firewalls para outras APIs e nuvens sovereign como Datacenters do Azure ou o Azure Government estará disponível brevemente. Se estiver a planear configurar o ponto final de serviço ACL para a sua conta de base de dados do Azure Cosmos que tem uma firewall existente do IP configurada, tenha em atenção a configuração da firewall, remova a firewall IP e, em seguida, configurar o ponto final de serviço ACL. Depois de configurar o ponto final de serviço, pode voltar a ativar a firewall do IP se necessário.

## <a name="ip-access-control-overview"></a>Descrição geral de controlo de acesso de IP
Por predefinição, uma conta de base de dados do Azure Cosmos DB está acessível a partir da Internet pública, desde que o pedido seja acompanhado por um token de autorização válido. Para configurar o controlo de acesso baseado em política de IP, o utilizador tem de fornecer o conjunto de endereços IP ou intervalos de endereços IP no formulário CIDR para serem incluídos como a lista de IPs de cliente permitidos para uma determinada conta de base de dados. Quando esta configuração for aplicada, todos os pedidos provenientes de máquinas fora desta lista de permissões são bloqueados pelo servidor.  A ligação ao processamento de fluxo para o controlo de acesso baseado em IP é descrita no diagrama seguinte:

![Diagrama que mostra o processo de ligação para o controlo de acesso baseado em IP](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> Configurar a política de controlo de acesso IP
A política de controlo de acesso IP pode ser definida no portal do Azure ou através de programação através de [CLI do Azure](cli-samples.md), [Azure Powershell](powershell-samples.md), ou o [REST API](/rest/api/cosmos-db/) atualizando o **ipRangeFilter** propriedade. 

Para definir a política de controlo de acesso IP no portal do Azure, navegue para a página de conta de base de dados do Azure Cosmos, clique em **Firewall** no menu de navegação, em seguida, altere o **autorizar o acesso** valor  **Selecionado redes**e, em seguida, clique em **guardar**. 

![Captura de ecrã que mostra como abrir a página de Firewall no portal do Azure](./media/firewall-support/azure-portal-firewall.png)

Depois de controlo de acesso IP está ativado, o portal fornece a capacidade de especificar endereços IP e intervalos, bem como os comutadores, para ativar o acesso a outros serviços do Azure e o portal do Azure. Informações adicionais sobre os comutadores são fornecidas nas secções seguintes.

> [!NOTE]
> Ao ativar uma política de controlo de acesso IP para a sua conta de base de dados de base de dados do Azure Cosmos, todo o acesso à sua conta de base de dados de base de dados do Azure Cosmos máquinas fora configurada permitido lista de intervalos de endereços IP são bloqueados. Em virtude neste modelo, a operação de plane de dados do portal de navegação será também bloqueado para assegurar a integridade do controlo de acesso.

## <a name="connections-from-the-azure-portal"></a>Ligações a partir do portal do Azure 

Quando ativar uma política de controlo de acesso IP através de programação, terá de adicionar o endereço IP do portal do Azure para o **ipRangeFilter** propriedade para manter o acesso. Os endereços IP portais são:

|Região|Endereço IP|
|------|----------|
|Todas as regiões exceto às especificadas abaixo|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Alemanha|51.4.229.218|
|China|139.217.8.252|
|Governo dos EUA|52.244.48.71|

Acesso ao portal do Azure está ativado por predefinição, quando alterar a definição de Firewall para **selecionado redes** no portal do Azure. 

![Captura de ecrã que mostra como ativar o acesso ao portal do Azure](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-other-azure-paas-services"></a>Ligações a partir de outros serviços do Azure PaaS 
No Azure, PaaS serviços do Azure Stream analytics, as funções do Azure e App Service do Azure são utilizados em conjunto com a base de dados do Azure Cosmos. Para ativar o acesso à base de dados do Azure Cosmos conta de base de dados a partir destes serviços cujos endereços IP não estejam prontamente disponíveis, adicione o endereço IP do 0.0.0.0 à lista permitida de endereços IP associados a sua conta de base de dados de base de dados do Azure Cosmos através de programação. 

Acesso a outros serviços do Azure está ativado por predefinição, quando alterar a definição de Firewall para **selecionado redes** no portal do Azure. 

![Captura de ecrã que mostra como abrir a página de Firewall no portal do Azure](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Ligações do seu atual IP

Para simplificar o desenvolvimento, o portal do Azure ajuda-o a identificar e adicione o IP do seu computador cliente à lista de permitidos, para que as aplicações com o seu computador possam aceder a conta de base de dados do Azure Cosmos. O endereço IP do cliente é detetado como visto pelo portal. Poderá ser o endereço IP do cliente do seu computador, mas também poderia ser o endereço IP do gateway de rede. Não se esqueça de removê-lo antes de ir para produção.

Para ativar o IP atual, selecione **adicionar meu IP atual**, que adiciona o seu atual IP à lista de IPs e, em seguida, clique em **guardar**.

![Captura de ecrã que mostra um como para configurar as definições da firewall para o IP atual](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Ligações a partir de serviços cloud
No Azure, serviços em nuvem são uma forma comum para o alojamento de lógica de serviço de camada média utilizando a BD do Cosmos do Azure. Para ativar o acesso a uma conta de base de dados de base de dados do Azure Cosmos de um serviço em nuvem, o endereço IP público do serviço de nuvem tem de ser adicionado à lista permitida de endereços IP associados à sua conta de base de dados de base de dados do Azure Cosmos por [configurar a política de controlo de acesso IP](#configure-ip-policy). Isto garante que todas as instâncias de função dos serviços em nuvem tem acesso à sua conta de base de dados de base de dados do Azure Cosmos. Pode obter endereços IP para os seus serviços em nuvem no portal do Azure, conforme mostrado na captura de ecrã seguinte:

![Captura de ecrã que mostra o endereço IP público para um serviço em nuvem apresentado no portal do Azure](./media/firewall-support/public-ip-addresses.png)

Quando ampliar o seu serviço em nuvem, adicionando as instâncias de função adicionais, as novas instâncias terão automaticamente acesso à conta de base de dados do Azure Cosmos DB, uma vez que fazem parte do serviço em nuvem do mesmo.

## <a name="connections-from-virtual-machines"></a>Ligações a partir de máquinas virtuais
[Máquinas virtuais](https://azure.microsoft.com/services/virtual-machines/) ou [conjuntos de dimensionamento de máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) também podem ser utilizados para alojar os serviços de camada média utilizando a BD do Cosmos do Azure.  Para configurar a conta de base de dados de base de dados do Azure Cosmos para permitir o acesso a partir de máquinas virtuais, os endereços IP públicos de máquina virtual e/ou conjunto de dimensionamento da máquina virtual tem de ser configurados como um dos endereços IP permitidos para a sua conta de base de dados de base de dados do Azure Cosmos por [configurar a política de controlo de acesso IP](#configure-ip-policy). Pode obter endereços IP para máquinas virtuais no portal do Azure, conforme mostrado na captura de ecrã seguinte.

![Captura de ecrã com um endereço IP público para uma máquina virtual apresentado no portal do Azure](./media/firewall-support/public-ip-addresses-dns.png)

Quando adicionar as instâncias de máquina virtual adicional para o grupo, são automaticamente fornecidos acesso à sua conta de base de dados de base de dados do Azure Cosmos.

## <a name="connections-from-the-internet"></a>Ligações a partir da internet
Ao aceder a uma conta de base de dados de base de dados do Azure Cosmos de um computador com a internet, o endereço IP do cliente ou um intervalo de endereços IP da máquina tem de ser adicionado à lista de permitidos do endereço IP para a conta de base de dados de base de dados do Azure Cosmos. 

## <a name="troubleshooting-the-ip-access-control-policy"></a>A política de controlo de acesso IP de resolução de problemas
### <a name="portal-operations"></a>Operações de portais
Ao ativar uma política de controlo de acesso IP para a sua conta de base de dados de base de dados do Azure Cosmos, todo o acesso à sua conta de base de dados de base de dados do Azure Cosmos máquinas fora configurada permitido lista de intervalos de endereços IP são bloqueados. Por conseguinte, se pretender permitir operações de plane dados portal como navegação coleções e consultar documentos, terá de permitir explicitamente o acesso ao portal do Azure utilizando o **Firewall** página no portal. 

![Captura de ecrã que mostra um como para permitir o acesso ao portal do Azure](./media/firewall-support/azure-portal-firewall.png)

### <a name="sdk--rest-api"></a>SDK & Rest API
Para motivos de segurança, o acesso através do SDK ou da REST API do máquinas não na lista de permitidos irão devolver uma resposta não foi encontrado genérico 404 com sem detalhes adicionais. Certifique-se o IP permitido lista configurada para a conta de base de dados de base de dados do Azure Cosmos para garantir que a configuração da política correto é aplicada à sua conta de base de dados de base de dados do Azure Cosmos.

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre as sugestões de desempenho relacionados com a rede, consulte [sugestões de desempenho](performance-tips.md).

