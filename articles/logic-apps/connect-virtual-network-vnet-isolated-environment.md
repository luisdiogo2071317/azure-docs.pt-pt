---
title: Ligar a redes virtuais do Azure a partir do Azure Logic Apps, por meio de um ambiente de serviço de integração (ISE)
description: Criar um ambiente de serviço de integração (ISE) para que o logic apps e contas de integração podem aceder a redes virtuais do Azure (VNETs), enquanto se mantém isolado do Azure "global" ou público e privadas
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 11/29/2018
ms.openlocfilehash: 798b50887bcfdf5b4298c37beb1b9eea8f9abdda
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682202"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-through-an-integration-service-environment-ise"></a>Ligar a redes virtuais do Azure a partir do Azure Logic Apps, por meio de um ambiente de serviço de integração (ISE)

> [!NOTE]
> Esta capacidade está em *pré-visualização privada*. Para pedir acesso [criar o pedido de associação aqui](https://aka.ms/iseprivatepreview).

Para cenários em que o logic apps e as contas de integração precisam de acesso a uma [rede virtual do Azure](../virtual-network/virtual-networks-overview.md), criar um [ *ambiente de serviço de integração* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Um ISE é um ambiente isolado e privado que utiliza armazenamento dedicado e outros recursos mantidos separam do público ou *global* serviço de aplicações lógicas. Essa separação também reduz a qualquer impacto que outros inquilinos do Azure podem ter sobre o desempenho das suas aplicações. O ISE encontra-se *injetado* para a sua rede virtual do Azure, que, em seguida, implementa o serviço de aplicações lógicas na sua rede virtual. Quando cria uma conta de integração ou da aplicação lógica, selecione esta ISE como a respetiva localização. Sua conta de integração ou da aplicação lógica, em seguida, pode aceder diretamente a recursos, como máquinas virtuais (VMs), servidores, sistemas e serviços, na sua rede virtual. 

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Este artigo mostra como concluir estas tarefas:

* Configure as permissões na sua rede virtual do Azure, para que a instância privada do Logic Apps possa acessar a rede virtual.

* Crie o seu ambiente de serviço de integração (ISE). 

* Crie uma aplicação lógica que pode ser executados no seu ISE. 

* Crie uma conta de integração para as aplicações lógicas no seu ISE.

Para obter mais informações sobre os ambientes de serviço de integração, consulte [acesso a recursos de rede Virtual do Azure a partir do Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. 

* Uma [rede virtual do Azure](../virtual-network/virtual-networks-overview.md). Se não tiver uma rede virtual, saiba como [criar uma Azure virtual network](../virtual-network/quick-create-portal.md). 

* Para proporcionar acesso direto à rede virtual do Azure, a suas aplicações lógicas [configurar permissões de controlo de acesso baseado em funções (RBAC)](#vnet-access) para que o serviço de aplicações lógicas tem as permissões para aceder à sua rede virtual. 

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Definir permissões de rede virtual

Quando cria um ambiente de serviço de integração (ISE), selecione uma rede virtual do Azure para onde *injetar* seu ambiente. No entanto, antes de poder selecionar uma rede virtual para injetar seu ambiente, tem de configurar permissões de controlo de acesso baseado em funções (RBAC) na sua rede virtual. Para configurar as permissões, atribua essas funções específicas para o serviço do Azure Logic Apps:

1. Na [portal do Azure](https://portal.azure.com), localize e selecione a rede virtual. 

1. No menu da sua rede virtual, selecione **controlo de acesso (IAM)**. 

1. Sob **controlo de acesso (IAM)**, escolha **Add**. 

   ![Adicionar funções](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Sobre o **adicionar atribuição de função** painel, configurar cada função para o serviço do Azure Logic Apps, conforme descrito pela tabela de neste passo. Certifique-se de que escolher **guardar** depois de concluir a cada função.

   ![Adicionar atribuição de função](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

   | Função | Atribuir acesso a | Selecione | 
   |------|------------------|--------|
   | **Contribuidor de Rede** | **Utilizador, grupo ou aplicação AD do Azure** | Introduza **do Azure Logic Apps**. Depois de aparece a lista de membros, selecione o mesmo valor. <p>**Sugestão**: Se não conseguir encontrar este serviço, introduza o ID da aplicação do serviço de aplicações lógicas: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   | **Contribuinte de clássica** | **Utilizador, grupo ou aplicação AD do Azure** | Introduza **do Azure Logic Apps**. Depois de aparece a lista de membros, selecione o mesmo valor. <p>**Sugestão**: Se não conseguir encontrar este serviço, introduza o ID da aplicação do serviço de aplicações lógicas: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   |||| 

Para obter mais informações, consulte [permissões para acesso à rede virtual](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Criar seu ISE

Para criar o ambiente de serviço de integração (ISE), siga estes passos:

1. Na [portal do Azure](https://portal.azure.com), no menu principal do Azure, selecione **criar um recurso**.

   ![Criar novo recurso](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Na caixa de pesquisa, introduza "ambiente de serviço de integração" como o filtro.
Na lista de resultados, selecione **o ambiente de serviço de integração (pré-visualização)** e, em seguida, escolha **criar**.

   ![Selecione "Ambiente de serviço de integração"](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Escolha "Criar"](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Forneça estes detalhes para o seu ambiente e, em seguida, escolha **rever + criar**, por exemplo:

   ![Fornecer detalhes do ambiente](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propriedade | Necessário | Valor | Descrição |
   |----------|----------|-------|-------------|
   | **Subscrição** | Sim | <*Azure-subscription-name*> | A subscrição do Azure a utilizar para o seu ambiente | 
   | **Grupo de recursos** | Sim | <*Azure-resource--nome do grupo*> | O grupo de recursos do Azure onde pretende criar o seu ambiente |
   | **Nome do ambiente de serviço de integração** | Sim | <*nome do ambiente*> | O nome para dar o seu ambiente | 
   | **Localização** | Sim | <*Região do datacenter do Azure*> | A região do datacenter do Azure onde pretende implementar o seu ambiente | 
   | **Capacidade** | Sim | 0, 1, 2, 3 | O número de unidades de processamento para utilizar para este recurso do ISE | 
   | **Rede virtual** | Sim | <*Azure---nome de rede virtual*> | A rede virtual do Azure em que deseja injetar o seu ambiente para que aplicações lógicas nesse ambiente podem acessar a rede virtual. Se não tiver uma rede, pode criar uma aqui. <p>**Importante**: pode *apenas* realizar este injeção quando cria seu ISE. No entanto, antes de poder criar esta relação, certifique-se de que já [configurar o controlo de acesso baseado em funções na sua rede virtual para o Azure Logic Apps](#vnet-access). | 
   | **Sub-redes** | Sim | <*Intervalo de endereços IP*> | Um ISE requer quatro *vazio* sub-redes, que não têm a delegação a qualquer serviço e são usadas para criar recursos no seu ambiente. Cada sub-rede tem de cumprir estes critérios: <p>-Consome os [formato de encaminhamento de entre domínios Classless (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). <br>-Requer um espaço de endereços de classe B. <br>-Tem um nome que não começa com um número ou um hífen. <br>-Inclui um `/27`, por exemplo, cada sub-rede aqui Especifica um intervalo de endereços de 32 bits: `10.0.0.0/27`, `10.0.0.32/27`, `10.0.0.64/27`, e `10.0.0.96/27`. <br>-Não pode existir no mesmo intervalo de endereços para a sua rede virtual selecionada nem quaisquer outros endereços IP privados em que a rede virtual está ligada. <br>-Pode estar vazio. <p><p>**Importante**: *não é possível alterar* estes intervalos IP depois de criar o seu ambiente. |
   |||||

1. Depois do Azure com êxito valida suas informações de ISE, escolha **criar**, por exemplo:

   ![Após a validação com êxito, escolha "Criar"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure inicia a implantação de seu ambiente, mas este processo poderá demorar *até duas horas* antes de terminar. 
   Para verificar o estado de implementação, na sua barra de ferramentas do Azure, selecione o ícone de notificações, que abre o painel de notificações.

   ![Verificar o estado de implementação](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Após a conclusão com êxito da implementação, o Azure mostra esta notificação:

   ![Implementação efetuada com êxito](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

1. Para ver o seu ambiente, escolha **Ir para recurso** se o Azure não passar automaticamente para o seu ambiente após a conclusão da implementação.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Criar aplicação lógica - ISE

Para criar aplicações lógicas que utilizem o ambiente de serviço de integração (ISE), siga os passos em [como criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) mas com estas diferenças: 

* Ao criar a sua aplicação lógica, selecionar seu ISE, em vez de uma região do Azure, o **localização** lista a partir da **ambientes de serviço de integração** secção, por exemplo:

  ![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Pode utilizar o mesmo acionadores incorporados e ações, como HTTP, que são executadas no ISE do mesmo que a sua aplicação lógica. Conectores com o **ISE** etiqueta também são executados no ISE do mesmo que a sua aplicação lógica. Conectores sem o **ISE** etiqueta em execução no serviço de aplicações lógicas global.

  ![Selecionar conectores do ISE](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Depois de inserir sua ISE numa rede virtual do Azure, os logic apps no seu ISE podem acessar diretamente recursos nessa rede virtual. Para sistemas no local numa rede virtual que está ligada a um ISE, aplicações lógicas podem acessar diretamente desses sistemas, utilizando qualquer um destes itens: 

  * Conector do ISE para esse sistema, por exemplo, o SQL Server
  * Ação de HTTP 
  * Conector personalizado

  Para sistemas no local que não estão numa rede virtual ou não tem conectores ISE, primeiro [configurar e utilizar o gateway de dados no local](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Criar conta de integração - ISE

Para utilizar uma conta de integração com logic apps num ambiente de serviço de integração (ISE), essa conta de integração tem de utilizar o *mesmo ambiente* como as aplicações lógicas. Aplicações lógicas num ISE podem referenciar apenas as contas de integração no ISE do mesmo. 

Para criar uma conta de integração que utiliza um ISE, siga os passos habituais [como criar contas de integração](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) exceto para o **localização** propriedade, que agora apresenta uma lista de sua ISEs em  **Ambientes de serviço de integração** juntamente com as regiões disponíveis. Selecione seu ISE, em vez de uma região, por exemplo:

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">fórum do Azure Logic Apps</a>.
* Para submeter ou votar em ideias para funcionalidades, visite o <a href="https://aka.ms/logicapps-wish" target="_blank">site de comentários dos utilizadores do Logic Apps</a>.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [rede Virtual do Azure](../virtual-network/virtual-networks-overview.md)
* Saiba mais sobre [integração da rede virtual para serviços do Azure](../virtual-network/virtual-network-for-azure-services.md)
