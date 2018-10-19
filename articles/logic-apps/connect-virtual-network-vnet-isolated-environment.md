---
title: Ligar a redes virtuais do Azure a partir do Azure Logic Apps, por meio de um ambiente de serviço de integração (ISE)
description: Criar um ambiente de serviço de integração (ISE) para que o logic apps e contas de integração podem aceder a redes virtuais do Azure, mantendo-se isolada do Azure "global" ou público e privadas
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/25/2018
ms.openlocfilehash: b4e4e801c3c54b635f2f13b319257018ea544c03
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404125"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-through-an-integration-service-environment-ise"></a>Ligar a redes virtuais do Azure a partir do Azure Logic Apps, por meio de um ambiente de serviço de integração (ISE)

> [!NOTE]
> Esta capacidade está em *pré-visualização privada*. Para pedir acesso [criar o pedido de associação aqui](https://aka.ms/iseprivatepreview).

Para cenários de integração em que o logic apps e as contas de integração precisam de acesso a uma [rede virtual do Azure](../virtual-network/virtual-networks-overview.md), criar um [ *ambiente de serviço de integração* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), que é um ambiente isolado e privado que utiliza armazenamento dedicado e outros recursos mantidos separados do público ou *global* serviço de aplicações lógicas. Essa separação também reduz a qualquer impacto que outros inquilinos do Azure podem ter sobre o desempenho das suas aplicações. Pode ligar este ISE à sua rede virtual do Azure, em seguida, implementa o serviço de aplicações lógicas na sua rede virtual. Quando cria uma conta de integração ou da aplicação lógica, selecione esta ISE como a respetiva localização. Sua conta de integração ou da aplicação lógica, em seguida, pode aceder diretamente a recursos, como máquinas virtuais (VMs), servidores, sistemas e serviços, na sua rede virtual. 

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Este artigo mostra como realizar estas tarefas:

* Configure as permissões na sua rede virtual do Azure, para que a instância privada do Logic Apps possa acessar a rede virtual.

* Crie o seu ambiente de serviço de integração (ISE). 

* Crie uma aplicação lógica que pode ser executados no seu ISE. 

* Crie uma conta de integração para as aplicações lógicas no seu ISE.

Para obter mais informações sobre os ambientes de serviço de integração, consulte [acesso aos recursos de rede Virtual do Azure do Azure Logic Apps isolado](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. 

* Se não tiver uma rede virtual do Azure, saiba como [criar uma Azure virtual network](../virtual-network/quick-create-portal.md). 

  > [!IMPORTANT]
  > Embora não precisa de uma rede virtual do Azure para criar o seu ambiente, pode *apenas* selecionar uma rede virtual como ponto a ponto do seu ambiente ao criar nesse ambiente. 

* Para proporcionar acesso direto à rede virtual do Azure, a suas aplicações lógicas [configurar permissões de controlo de acesso baseado em funções (RBAC)](#vnet-access) para que o serviço de aplicações lógicas tem as permissões para aceder à sua rede virtual. 

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Definir permissões de rede virtual

Ao criar o ambiente de serviço de integração, pode selecionar uma rede virtual do Azure como um *ponto a ponto* para o seu ambiente. No entanto, só pode efetuar este passo, ou *peering*, ao criar o seu ambiente. Esta relação permite que o serviço de aplicações lógicas ligar diretamente a recursos na rede virtual e concede o acesso de ambiente a esses recursos. 

Pode selecionar a rede virtual, tem de configurar permissões de controlo de acesso baseado em funções (RBAC) na sua rede virtual. Para concluir essa tarefa, tem de atribuir funções específicas para o serviço do Azure Logic Apps.

1. Na [portal do Azure](https://portal.azure.com), localize e selecione a rede virtual. 

1. No menu da sua rede virtual, selecione **controlo de acesso (IAM)**. 

1. Sob **controlo de acesso**, selecione **atribuição de função** se não estiver selecionado. Sobre o **atribuição de função** barra de ferramentas, escolha **Add**. 

   ![Adicionar as atribuições de funções](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Sobre o **adicionar permissões** painel, configurar cada função nesta tabela para o serviço do Azure Logic Apps. Certifique-se de que escolher **guardar** depois de concluir a cada função:

   | Função | Atribuir acesso a | Selecione | 
   |------|------------------|--------|
   | **Contribuidor de Rede** | **Utilizador, grupo ou aplicação AD do Azure** | Introduza **do Azure Logic Apps**. Depois de aparece a lista de membros, selecione o mesmo valor. <p>**Sugestão**: Se não conseguir encontrar este serviço, introduza o ID da aplicação do serviço de aplicações lógicas: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   | **Contribuinte de clássica** | **Utilizador, grupo ou aplicação AD do Azure** | Introduza **do Azure Logic Apps**. Depois de aparece a lista de membros, selecione o mesmo valor. <p>**Sugestão**: Se não conseguir encontrar este serviço, introduza o ID da aplicação do serviço de aplicações lógicas: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   |||| 

   Por exemplo:

   ![Adicionar permissões](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

   Para obter mais informações sobre as permissões de função necessários para o peering, veja a [permissões secção criação, alteração, ou eliminar um peering de rede virtual](../virtual-network/virtual-network-manage-peering.md#permissions). 

Se a rede virtual estiver ligada através do ExpressRoute do Azure, Azure ponto-para-Site VPN ou VPN de Site a Site do Azure, avance para a secção seguinte para que pode adicionar a sub-rede do gateway necessário. Caso contrário, continue com [criar um ambiente](#create-environment).

<a name="add-gateway-subnet"></a>

## <a name="add-gateway-subnet-for-virtual-networks-with-expressroute-or-vpns"></a>Adicionar sub-rede do gateway para redes virtuais com o ExpressRoute ou de VPNs

Depois de concluir os passos anteriores, para dar o ambiente de serviço de integração (ISE) aceder a uma rede virtual do Azure que tenha ligado através de [Azure ExpressRoute](../expressroute/expressroute-introduction.md), [Azure ponto-para-Site VPN](../vpn-gateway/point-to-site-about.md), ou [VPN de Site a Site do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), também tem de adicionar uma [ *sub-rede do gateway* ](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsub) à sua rede virtual:

1. Na [portal do Azure](https://portal.azure.com), localize e selecione a rede virtual. No menu da sua rede virtual, selecione **sub-redes**e, em seguida, escolha **sub-rede do Gateway** > **OK**.

   ![Adicionar sub-rede do gateway](./media/connect-virtual-network-vnet-isolated-environment/add-gateway-subnet.png)

1. Agora, crie uma [ *tabela de rotas*](../virtual-network/manage-route-table.md), que irá ser associado a sub-rede do gateway que criou anteriormente.

   1. No menu principal do Azure, selecione **criar um recurso** > 
    **rede** > **tabela de rotas**.

      ![Criar tabela de rotas](./media/connect-virtual-network-vnet-isolated-environment/create-route-table.png)

   1. Fornece informações sobre a tabela de rotas, como o nome, a subscrição do Azure para utilizar, grupo de recursos do Azure e localização. Certifique-se de que o **propagação de rotas BGP** estiver definida como **ativado**e, em seguida, escolha **criar**.

      ![Forneça os detalhes da tabela de rota](./media/connect-virtual-network-vnet-isolated-environment/enter-route-table-information.png)

   1. No menu de tabela da rota, selecione **sub-redes**e, em seguida, escolha **associar**. 

      ![Ligue-se a tabela de rotas à sub-rede](./media/connect-virtual-network-vnet-isolated-environment/associate-route-table.png)

   1. Selecione **rede Virtual**e, em seguida, selecione a rede virtual.
   
   1. Selecione **sub-rede**e, em seguida, selecione a sub-rede do gateway criada anteriormente.

   1. Quando tiver terminado, escolha **OK**.

1. Se tiver uma VPN ponto a site, muito a concluir estes passos:

   1. No Azure, localize e selecione o recurso de gateway de rede virtual.

   1. No menu do gateway, selecione **configuraçãopontoasiteda**. 
   e, em seguida, escolha **transferir cliente VPN** para que tenha a configuração mais recente do cliente VPN.

      ![Transferir o cliente VPN mais recente](./media/connect-virtual-network-vnet-isolated-environment/download-vpn-client.png)

Está agora concluído com a configuração de uma sub-rede de gateway para redes virtuais que utilizam o ExpressRoute, VPNs ponto a site ou VPNs de site a site. Para continuar a criar o seu ambiente de serviço de integração, siga os passos seguintes.

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Criar seu ISE

Para criar o ambiente de serviço de integração (ISE), siga estes passos:

1. Na [portal do Azure](https://portal.azure.com), no menu principal do Azure, selecione **criar um recurso**.

   ![Criar novo recurso](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Na caixa de pesquisa, introduza "ambiente de serviço de integração" como o filtro.
Na lista de resultados, selecione **o ambiente de serviço de integração (pré-visualização)** e, em seguida, escolha **criar**.

   ![Selecione "Ambiente de serviço de integração"](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Escolha "Criar"](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Forneça estes detalhes para o seu ambiente:

   ![Fornecer detalhes do ambiente](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propriedade | Necessário | Valor | Descrição |
   |----------|----------|-------|-------------|
   | **Nome** | Sim | <*nome do ambiente*> | O nome para dar o seu ambiente | 
   | **Subscrição** | Sim | <*Azure-subscription-name*> | A subscrição do Azure a utilizar para o seu ambiente | 
   | **Grupo de recursos** | Sim | <*Azure-resource--nome do grupo*> | O grupo de recursos do Azure onde pretende criar o seu ambiente |
   | **Localização** | Sim | <*Região do datacenter do Azure*> | A região do datacenter do Azure onde pretende armazenar as informações sobre o ambiente |
   | **VNET com peering** | Não | <*Nome da VNET do Azure*> | A rede virtual do Azure para associar o seu ambiente como um *ponto a ponto* para aplicações lógicas nesse ambiente podem aceder a sua rede virtual. Antes de poder criar esta relação, certifique-se de que já [configurar o controlo de acesso baseado em funções na sua rede virtual para o Azure Logic Apps](#vnet-access). <p>**Importante**: Embora uma rede virtual não é necessária, pode selecionar uma rede virtual *apenas* ao criar o seu ambiente. | 
   | **Nome de peering** | Sim, com uma rede virtual selecionada | <*nome de peering*> | O nome para dar a relação de ponto a ponto | 
   | **Intervalo de IP de VNET** | Sim, com uma rede virtual selecionada | <*Intervalo de endereços IP*> | O intervalo de endereços IP a utilizar para criar recursos no seu ambiente. Este intervalo tem de utilizar o [formato de encaminhamento de entre domínios Classless (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing), por exemplo, 10.0.0.1/16 e necessita de um espaço de endereços de classe B. O intervalo não pode existir dentro do espaço de endereço para a rede virtual selecionada na **VNET de ponto a ponto** propriedade, nem em quaisquer outros endereços IP privados em que o elemento da rede está ligado, através do peering ou gateways. <p><p>**Importante**: *não é possível alterar* este intervalo de endereços, depois de criar o seu ambiente. |
   |||||
   
1. Quando tiver terminado, escolha **Create** (Criar). 

   Azure inicia a implantação de seu ambiente, mas este processo poderá demorar *até duas horas* antes de terminar. 
   Para verificar o estado de implementação, na sua barra de ferramentas do Azure, selecione o ícone de notificações, que abre o painel de notificações.

   ![Verificar o estado de implementação](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Após a conclusão com êxito da implementação, o Azure mostra esta notificação:

   ![Implementação efetuada com êxito](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

1. Para ver o seu ambiente, escolha **Ir para recurso** se o Azure não passar automaticamente para o seu ambiente após a conclusão da implementação.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Criar aplicação lógica - ISE

Para criar aplicações lógicas que utilizem o ambiente de serviço de integração (ISE), siga os passos habituais [como criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) , mas com estas diferenças e considerações: 

* Quando criar a sua aplicação lógica, o **localização** sua ISEs em listas de propriedades **ambientes de serviço de integração** juntamente com as regiões disponíveis. Selecione seu ISE, em vez de uma região, por exemplo:

  ![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Pode utilizar os mesmo built-ins, como o acionador HTTP ou a ação, que são executadas no ISE do mesmo que a aplicação de lógica principal. Conectores com o **ISE** etiqueta também são executados no ISE do mesmo que a aplicação de lógica principal. Conectores sem o **ISE** etiqueta em execução no serviço de aplicações lógicas global.

  ![Selecionar conectores do ISE](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Se configurou anteriormente o ISE com uma rede virtual do Azure como uma ponto a ponto, os logic apps no seu ISE podem acessar diretamente recursos nessa rede virtual. Para sistemas no local numa rede virtual que está ligada a um ISE, aplicações lógicas podem acessar diretamente desses sistemas, utilizando qualquer um destes itens: 

  * Conector do ISE para esse sistema, por exemplo, o SQL Server

  * Ação de HTTP 

  * Conector personalizado

  Para sistemas no local que não estão numa rede virtual ou não tem conectores ISE, pode ligar depois [configurar e utilizar o gateway de dados no local](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Criar conta de integração - ISE

Para utilizar uma conta de integração com logic apps num ambiente de serviço de integração (ISE), essa conta de integração tem de utilizar o *mesmo ambiente* como as aplicações lógicas. Aplicações lógicas num ISE podem referenciar apenas as contas de integração no ISE do mesmo. 

Para criar uma conta de integração que utiliza um ISE, siga os passos habituais [como criar contas de integração](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) exceto para o **localização** propriedade, que agora apresenta uma lista de sua ISEs em  **Ambientes de serviço de integração** juntamente com as regiões disponíveis. Selecione seu ISE, em vez de uma região, por exemplo:

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">fórum do Azure Logic Apps</a>.
* Para submeter ou votar em ideias para funcionalidades, visite o <a href="http://aka.ms/logicapps-wish" target="_blank">site de comentários dos utilizadores do Logic Apps</a>.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [rede Virtual do Azure](../virtual-network/virtual-networks-overview.md)
* Saiba mais sobre [integração da rede virtual para serviços do Azure](../virtual-network/virtual-network-for-azure-services.md)
