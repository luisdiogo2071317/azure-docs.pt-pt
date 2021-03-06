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
ms.date: 02/15/2019
ms.openlocfilehash: d67bc99a63242dd56d65d6bdac0448c7742a6b9d
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311907"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Ligar a redes virtuais do Azure do Azure Logic Apps com um ambiente de serviço de integração (ISE)

> [!NOTE]
> Esta capacidade está em *pré-visualização privada*. Para aderir à pré-visualização privada, [criar o pedido de aqui](https://aka.ms/iseprivatepreview).

Para cenários em que o logic apps e as contas de integração precisam de acesso a uma [rede virtual do Azure](../virtual-network/virtual-networks-overview.md), criar um [ *ambiente de serviço de integração* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Um ISE é um ambiente isolado e privado que utiliza armazenamento dedicado e outros recursos mantidos separados do serviço de aplicações lógicas "global" ou público. Essa separação também reduz a qualquer impacto que outros inquilinos do Azure podem ter sobre o desempenho das suas aplicações. O ISE encontra-se *injetado* para a sua rede virtual do Azure, que, em seguida, implementa o serviço de aplicações lógicas na sua rede virtual. Quando cria uma conta de integração ou da aplicação lógica, selecione esta ISE como a respetiva localização. Sua conta de integração ou da aplicação lógica, em seguida, pode aceder diretamente a recursos, como máquinas virtuais (VMs), servidores, sistemas e serviços, na sua rede virtual.

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Este artigo mostra como concluir estas tarefas:

* Defina as portas de rede virtual do Azure para que o tráfego pode ser transferidos através de seu ambiente de serviço de integração (ISE) em sub-redes na rede virtual.

* Configure as permissões na sua rede virtual do Azure, para que a instância privada do Logic Apps possa acessar a rede virtual.

* Crie o seu ambiente de serviço de integração (ISE).

* Crie uma aplicação lógica que pode ser executados no seu ISE.

* Crie uma conta de integração para as aplicações lógicas no seu ISE.

Para obter mais informações sobre os ambientes de serviço de integração, consulte [acesso a recursos de rede Virtual do Azure a partir do Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>.

  > [!IMPORTANT]
  > Logic apps, ações incorporadas e conectores que executar no seu ISE utilizam um plano de preços diferente, não o consumo baseado no plano de preços. Para obter mais informações, consulte [preços de aplicações lógicas](../logic-apps/logic-apps-pricing.md).

* Uma [rede virtual do Azure](../virtual-network/virtual-networks-overview.md). Se não tiver uma rede virtual, saiba como [criar uma Azure virtual network](../virtual-network/quick-create-portal.md). Também precisa de sub-redes na rede virtual para implementar o seu ISE. Pode criar estas sub-redes com antecedência ou aguarde até que criar seu ISE onde pode criar sub-redes, ao mesmo tempo. Além disso, [certificar-se de que a rede virtual disponibiliza estas portas](#ports) para que seu ISE funciona corretamente e permanece acessível.

* Para proporcionar acesso direto à rede virtual do Azure, a suas aplicações lógicas [configurar permissões de controlo de acesso baseado em funções (RBAC) de sua rede](#vnet-access) para que o serviço de aplicações lógicas tem as permissões para aceder à sua rede virtual.

* Para utilizar um ou mais servidores DNS personalizados para implementar a sua rede virtual do Azure, [configurar esses servidores de seguir estas orientações](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) antes de implementar seu ISE à sua rede virtual. Caso contrário, sempre que alterar o seu servidor DNS, também tem de reiniciar o ISE, que é um recurso que está disponível em pré-visualização pública do ISE.

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="set-up-network-ports"></a>Configurar portas de rede

Para funcionar corretamente e mantenha-se acessível, o ambiente de serviço de integração (ISE) tem de ter as portas específicas disponíveis na sua rede virtual. Caso contrário, se qualquer uma destas portas não estão disponíveis, poderá perder o acesso ao seu ISE, que poderá parar de funcionar. Quando utiliza um ISE numa rede virtual, um problema de configuração comum é ter um ou mais portas bloqueadas. Para as ligações entre seu ISE e o sistema de destino, o conector que utiliza também pode ter seus próprios requisitos de porta. Por exemplo, se comunicar com um sistema FTP utilizando o conector FTP, certifique-se a porta a que utilizar em que o sistema FTP, como a porta 21 para o envio de comandos, está disponível.

Para controlar o tráfego de entrada e saído entre sub-redes da rede virtual onde implementar seu ISE, pode configurar [grupos de segurança de rede](../virtual-network/security-overview.md) para essas sub-redes Learning [como filtrar o tráfego de rede em sub-redes](../virtual-network/tutorial-filter-network-traffic.md). Estas tabelas descrevem as portas na sua rede virtual que utiliza o ISE e onde se acostumar essas portas. O asterisco (*) representa as origens de todo e qualquer tráfego. O [etiqueta de serviço](../virtual-network/security-overview.md#service-tags) representa um grupo de prefixos de endereços IP que o ajudam a minimizar a complexidade ao criar regras de segurança.

| Objetivo | Direção | Porta de origem <br>Porta de destino | Etiqueta de serviço de origem <br>Etiqueta do serviço de destino |
|---------|-----------|---------------------------------|-----------------------------------------------|
| Comunicação com o Azure Logic Apps <br>Comunicação a partir do Azure Logic Apps | Entrada <br>Saída | * <br>80 & 443 | INTERNET <br>VIRTUAL_NETWORK |
| Azure Active Directory | Saída | * <br>80 & 443 | VIRTUAL_NETWORK <br>AzureActiveDirectory |
| Dependência de armazenamento do Azure | Saída | * <br>80 & 443 | VIRTUAL_NETWORK <br>Armazenamento |
| Histórico de execução da sua aplicação lógica | Entrada | * <br>443 | INTERNET <br>VIRTUAL_NETWORK |
| Gerenciamento de conexões | Saída | * <br>443 | VIRTUAL_NETWORK <br>INTERNET |
| Publicar os registos de diagnóstico e métricas | Saída | * <br>443 | VIRTUAL_NETWORK <br>AzureMonitor |
| Estruturador de aplicações lógicas - propriedades dinâmicas <br>Implementação do conector <br>Ponto final de Acionador de pedido | Entrada | * <br>454 | INTERNET <br>VIRTUAL_NETWORK |
| Dependência de aplicação do serviço de gestão | Entrada | * <br>454 & 455 | AppServiceManagement <br>VIRTUAL_NETWORK |
| Gestão de API - ponto final de gestão | Entrada | * <br>3443 | APIManagement <br>VIRTUAL_NETWORK |
| Dependência do registo para a política do Hub de eventos e o agente de monitorização | Saída | * <br>5672 | VIRTUAL_NETWORK <br>EventHub |
| Aceder a Cache do Azure para instâncias de Redis entre instâncias de função | Entrada <br>Saída | * <br>6381-6383 | VIRTUAL_NETWORK <br>VIRTUAL_NETWORK |
|||||

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Definir permissões de rede virtual

Quando cria um ambiente de serviço de integração (ISE), selecione uma rede virtual do Azure para onde *injetar* seu ambiente. No entanto, antes de poder selecionar uma rede virtual para injetar seu ambiente, tem de configurar permissões de controlo de acesso baseado em funções (RBAC) na sua rede virtual. Para configurar as permissões, atribua essas funções específicas para o serviço do Azure Logic Apps:

1. Na [portal do Azure](https://portal.azure.com), localize e selecione a rede virtual.

1. No menu da sua rede virtual, selecione **controlo de acesso (IAM)**.

1. Sob **controlo de acesso (IAM)**, escolha **adicionar atribuição de função**.

   ![Adicionar funções](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Sobre o **adicionar atribuição de função** painel, adicione a função necessária para o serviço do Azure Logic Apps, conforme descrito.

   1. Sob **função**, selecione **Contribuidor de rede**.

   1. Sob **atribuir acesso aos**, selecione **utilizador, grupo ou principal de serviço do Azure AD**.

   1. Sob **selecionar**, introduza **Azure Logic Apps**.

   1. Depois da lista de membros for apresentada, selecione **do Azure Logic Apps**.

      > [!TIP]
      > Se não conseguir encontrar este serviço, introduza o ID da aplicação do serviço de aplicações lógicas: `7cd684f4-8a78-49b0-91ec-6a35d38739ba`

   1. Quando tiver terminado, escolha **Save** (Guardar).

   Por exemplo:

   ![Adicionar atribuição de função](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

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

   | Propriedade | Necessário | Value | Descrição |
   |----------|----------|-------|-------------|
   | **Subscrição** | Sim | <*Azure-subscription-name*> | A subscrição do Azure a utilizar para o seu ambiente |
   | **Grupo de recursos** | Sim | <*Azure-resource-group-name*> | O grupo de recursos do Azure onde pretende criar o seu ambiente |
   | **Nome do ambiente de serviço de integração** | Sim | <*environment-name*> | O nome para dar o seu ambiente |
   | **Localização** | Sim | <*Azure-datacenter-region*> | A região do datacenter do Azure onde pretende implementar o seu ambiente |
   | **Capacidade adicional** | Sim | 0, 1, 2, 3 | O número de unidades de processamento para utilizar para este recurso do ISE |
   | **Rede virtual** | Sim | <*Azure-virtual-network-name*> | A rede virtual do Azure em que deseja injetar o seu ambiente para que aplicações lógicas nesse ambiente podem acessar a rede virtual. Se não tiver uma rede, pode criar uma aqui. <p>**Importante**: Pode *apenas* realizar este injeção quando cria seu ISE. No entanto, antes de poder criar esta relação, certifique-se de que já [configurar o controlo de acesso baseado em funções na sua rede virtual para o Azure Logic Apps](#vnet-access). |
   | **Sub-redes** | Sim | <*subnet-resource-list*> | Um ISE requer quatro *vazio* sub-redes para a criação de recursos no seu ambiente. Por isso, certifique-se de que estas sub-redes *não são delegados* a qualquer serviço. *Não é possível alterar* estes endereços de sub-rede depois de criar o seu ambiente. <p><p>Para criar cada sub-rede [siga os passos nesta tabela](#create-subnet). Cada sub-rede tem de cumprir estes critérios: <p>-Pode estar vazio. <br>-Utiliza um nome que não começa com um número ou um hífen. <br>-Consome os [formato de encaminhamento de entre domínios Classless (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) e um espaço de endereços de classe B. <br>-Inclui, pelo menos, um `/27` no espaço de endereços, de modo a sub-rede obtém, pelo menos, 32 endereços. Para saber mais sobre o cálculo do número de endereços, veja [blocos CIDR de IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks). Por exemplo: <p>- `10.0.0.0/24` tem de 256 endereços porque 2<sup>(32-24)</sup> é 2<sup>8</sup> ou 256. <br>- `10.0.0.0/27` tem 32 endereços porque 2<sup>(32-27)</sup> é 2<sup>5</sup> ou 32. <br>- `10.0.0.0/28` tem apenas 16 endereços porque 2<sup>(32-28)</sup> é 2<sup>4</sup> ou 16. |
   |||||

   <a name="create-subnet"></a>

   **Criar sub-rede**

   1. Sob o **sub-redes** lista, escolha **configuração de sub-rede de gerir**.

      ![Gerir a configuração de sub-rede](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. Sobre o **sub-redes** painel, escolha **sub-rede**.

      ![Adicionar sub-rede](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. Sobre o **adicionar sub-rede** painel, indique estas informações.

      * **Nome**: O nome para a sua sub-rede
      * **Intervalo de endereços (bloco CIDR)**: Intervalo da sua sub-rede na sua rede virtual e no formato CIDR

      ![Adicionar detalhes da sub-rede](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Quando tiver terminado, escolha **OK**.

   1. Repita estes passos para três sub-redes mais.

1. Depois do Azure com êxito valida suas informações de ISE, escolha **criar**, por exemplo:

   ![Após a validação com êxito, escolha "Criar"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure começa a implantação de seu ambiente, mas esse processo *poderá* demorar até duas horas antes de terminar. 
   Para verificar o estado de implementação, na sua barra de ferramentas do Azure, selecione o ícone de notificações, que abre o painel de notificações.

   ![Verificar o estado de implementação](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Se a conclusão da implementação, o Azure mostra esta notificação:

   ![Implementação efetuada com êxito](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   > [!NOTE]
   > Se falhar a implementação ou eliminar seu ISE, do Azure *poderá* demorar até uma hora antes de lançar as sub-redes. Por isso, poderá ter de aguardar antes de reutilizar essas sub-redes no ISE do outro.

1. Para ver o seu ambiente, escolha **Ir para recurso** se o Azure não passar automaticamente para o seu ambiente após a conclusão da implementação.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Criar aplicação lógica - ISE

Para criar aplicações lógicas que utilizem o ambiente de serviço de integração (ISE), siga os passos em [como criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) mas com estas diferenças: 

* Ao criar a aplicação lógica, no **localização** propriedade, selecione o ISE do **ambientes de serviço de integração** secção, por exemplo:

  ![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Pode utilizar o mesmo acionadores incorporados e ações, como HTTP, que são executadas no ISE do mesmo que a sua aplicação lógica. Conectores com o **ISE** etiqueta também são executados no ISE do mesmo que a sua aplicação lógica. Conectores sem o **ISE** etiqueta em execução no serviço de aplicações lógicas global.

  ![Selecionar conectores do ISE](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Depois de inserir sua ISE numa rede virtual do Azure, os logic apps no seu ISE podem acessar diretamente recursos nessa rede virtual. Para sistemas no local que estão ligados a uma rede virtual, injete um ISE nessa rede, para que as logic apps podem acessar diretamente desses sistemas, utilizando qualquer um destes itens: 

  * Conector do ISE para esse sistema, por exemplo, o SQL Server
  
  * Ação de HTTP 
  
  * Conector personalizado

  Para sistemas no local que não estão numa rede virtual ou não tem conectores ISE, primeiro [configurar o gateway de dados no local](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Criar conta de integração - ISE

Para utilizar uma conta de integração com logic apps num ambiente de serviço de integração (ISE), essa conta de integração tem de utilizar o *mesmo ambiente* como as aplicações lógicas. Aplicações lógicas num ISE podem referenciar apenas as contas de integração no ISE do mesmo. 

Para criar uma conta de integração que utiliza um ISE, siga os passos em [como criar contas de integração](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) exceto para o **localização** propriedade onde o **ambientes de serviço de integração**  secção é apresentado. Em vez disso, selecione seu ISE, em vez de uma região, por exemplo:

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">fórum do Azure Logic Apps</a>.
* Para submeter ou votar em ideias para funcionalidades, visite o <a href="https://aka.ms/logicapps-wish" target="_blank">site de comentários dos utilizadores do Logic Apps</a>.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [rede Virtual do Azure](../virtual-network/virtual-networks-overview.md)
* Saiba mais sobre [integração da rede virtual para serviços do Azure](../virtual-network/virtual-network-for-azure-services.md)
