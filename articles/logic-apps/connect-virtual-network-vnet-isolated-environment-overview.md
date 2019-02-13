---
title: Acesso a redes virtuais do Azure a partir do Azure Logic Apps com ambientes de serviço de integração (ISEs)
description: Esta descrição geral descreve como ambientes de serviço de integração (ISEs) ajudam as aplicações lógicas, redes virtuais do Azure (VNETs) de acesso
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 02/12/2019
ms.openlocfilehash: 204138e7b8b3846e2d50607b3c5ec0836abefe24
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162378"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acesso a recursos de rede Virtual do Azure a partir do Azure Logic Apps, utilizando ambientes de serviço de integração (ISEs)

> [!NOTE]
> Esta capacidade está em *pré-visualização privada*. Para aderir à pré-visualização privada, [criar o pedido de aqui](https://aka.ms/iseprivatepreview).

Às vezes, o logic apps e as contas de integração precisam de acesso a recursos protegidos, como máquinas virtuais (VMs) e outros sistemas ou serviços, num [rede virtual do Azure](../virtual-network/virtual-networks-overview.md). Para configurar este acesso, pode [crie uma *ambiente de serviço de integração* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) para executar as suas aplicações lógicas e as contas de integração.

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Criar um ISE implementa uma instância de Logic Apps privada e isolada na rede virtual do Azure. Esta instância privada utiliza recursos dedicados, como o armazenamento e é executado separadamente do serviço de aplicações lógicas "global" público. Essa separação também ajuda a reduzir o impacto que outros inquilinos do Azure podem ter sobre o desempenho das suas aplicações, ou o [efeito de "vizinhos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). 

Esta descrição geral descreve como um ISE fornece as logic apps e contas de integração acesso direto à rede virtual do Azure e compara as diferenças entre uma ISE e o serviço de aplicações lógicas global.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Versus global em ambiente isolado

Quando cria um ambiente de serviço integrado (ISE) no Azure, selecione a rede virtual do Azure onde pretende *injetar* seu ISE. Azure implementa uma instância privada do serviço de aplicações lógicas na sua rede virtual. Esta ação cria um ambiente isolado, onde pode criar e executar as aplicações lógicas em recursos dedicados. Quando criar uma aplicação lógica, selecione esse ambiente como a localização da sua aplicação, o que lhe dá o seu acesso direto da aplicação de lógica aos recursos na sua rede virtual.

Aplicações lógicas num ISE fornecem as mesmas experiências de usuário e recursos semelhantes que o serviço de aplicações lógicas global. Não só pode usar as mesmas ações incorporadas e conectores no serviço de aplicações lógicas global, mas também pode utilizar conectores específicos do ISE. Por exemplo, eis alguns conectores padrão que oferecem versões que são executados num ISE:

* Armazenamento de Blobs do Azure, armazenamento de ficheiros e armazenamento de tabelas
* As filas do Azure, o Service bus do Azure, o Hubs de eventos do Azure e o IBM MQ
* FTP e SFTP-SSH
* SQL Server, o SQL Data Warehouse, Azure Cosmos DB
* AS2, X12 e EDIFACT

A diferença entre os conectores ISE e não ISE é nos locais onde executar os acionadores e ações:

* No seu ISE, incorporados acionadores e ações, como HTTP sempre executam no ISE do mesmo que a sua aplicação lógica.

* Para os conectores que oferecem duas versões, executa uma versão no ISE do, enquanto a outra versão é executada no serviço de aplicações lógicas global.  

  Os conectores que tenham o **ISE** sempre rotular execução no ISE do mesmo que a sua aplicação lógica. Conectores sem o **ISE** etiqueta em execução no serviço de aplicações lógicas global.

  ![Selecionar conectores do ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Os conectores que são executados num ISE também estão disponíveis no serviço de aplicações lógicas global.

> [!IMPORTANT]
> Logic apps, ações incorporadas e conectores que executar no seu ISE utilizam um plano de preços diferente, não o consumo baseado no plano de preços. Para obter mais informações, consulte [preços de aplicações lógicas](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Permissões para acesso à rede virtual

Quando cria um ambiente de serviço de integração (ISE), selecione uma rede virtual do Azure para onde *injetar* seu ambiente. Injeção implementa uma instância privada do serviço de aplicações lógicas na sua rede virtual. Esta ação resulta num ambiente isolado, onde pode criar e executar as aplicações lógicas em recursos dedicados. Quando cria as aplicações lógicas selecionar seu ISE como localização das suas aplicações. Estas aplicações de lógica podem, em seguida, aceder à sua rede virtual diretamente e ligar a recursos na rede.

Para sistemas que estão ligados a uma rede virtual, pode injetar um ISE nessa rede virtual para que as aplicações lógicas podem acessar diretamente desses sistemas, utilizando qualquer um desses itens:

* Conector do ISE para esse sistema, por exemplo, o SQL Server

* Ação de HTTP

* Conector personalizado

Para sistemas no local que não estão ligados a uma rede virtual ou não tem conectores ISE, pode ligar a esses sistemas por [configurar e utilizar o gateway de dados no local](../logic-apps/logic-apps-gateway-install.md).

Pode selecionar uma rede virtual do Azure para injetar seu ambiente, tem de configurar permissões de controlo de acesso baseado em funções (RBAC) na sua rede virtual para o serviço do Azure Logic Apps. Esta tarefa requer que atribua os **contribuinte de rede** e **contribuinte clássico** funções para o serviço do Azure Logic Apps.
Para configurar estas permissões, consulte [ligar a redes virtuais do Azure a partir de aplicações lógicas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#vnet-access)

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Contas de integração com o ISE

Pode utilizar contas de integração com logic apps dentro de um ambiente de serviço de integração (ISE). No entanto, essas contas de integração tem de utilizar o *ISE mesmo* como as aplicações de lógica ligada. Aplicações lógicas num ISE podem referenciar apenas as contas de integração que estão no ISE do mesmo. Quando cria uma conta de integração, pode selecionar o ISE como a localização para a sua conta de integração.

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">fórum do Azure Logic Apps</a>.
* Para submeter ou votar em ideias para funcionalidades, visite o <a href="https://aka.ms/logicapps-wish" target="_blank">site de comentários dos utilizadores do Logic Apps</a>.

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [ligar às redes virtuais do Azure a partir de aplicações lógicas isoladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Saiba mais sobre [rede Virtual do Azure](../virtual-network/virtual-networks-overview.md)
* Saiba mais sobre [integração da rede virtual para serviços do Azure](../virtual-network/virtual-network-for-azure-services.md)
