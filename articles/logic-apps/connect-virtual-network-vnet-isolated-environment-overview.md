---
title: Acesso a redes virtuais do Azure a partir do Azure Logic Apps com ambientes de serviço de integração (ISEs)
description: Esta descrição geral descreve como ambientes de serviço de integração (ISEs) ajudam as aplicações lógicas, aceder a redes virtuais do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: f21af23cf0b7b121441b1433f382db60ef7f13fe
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408664"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acesso a recursos de rede Virtual do Azure a partir do Azure Logic Apps, utilizando ambientes de serviço de integração (ISEs)

> [!NOTE]
> Esta capacidade está em *pré-visualização privada*. Para pedir acesso [criar o pedido de associação aqui](https://aka.ms/iseprivatepreview).

Às vezes, o logic apps e as contas de integração precisam de acesso a recursos protegidos, como máquinas virtuais (VMs) e outros sistemas ou serviços no interior de uma [rede virtual do Azure](../virtual-network/virtual-networks-overview.md). Para configurar este acesso, pode [crie uma *ambiente de serviço de integração* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) que utilizar como a localização para o logic apps e as contas de integração. 

![Selecione o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Criar um ISE implementa uma instância de Logic Apps privada e isolada na rede virtual do Azure. A instância privada utiliza recursos dedicados, como o armazenamento e é executado separadamente do serviço de aplicações lógicas "global" público. Essa separação também ajuda a reduzir o impacto que outros inquilinos do Azure podem ter sobre o desempenho das suas aplicações, ou o [efeito de "vizinhos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). 

Esta descrição geral descreve como criar um ISE ajuda logic apps e contas de integração aceder diretamente a recursos no interior da rede virtual do Azure e compara as diferenças entre uma ISE e o serviço de aplicações lógicas global.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Versus global em ambiente isolado

Quando cria um ambiente de serviço integrado (ISE) no Azure, pode selecionar uma rede virtual do Azure como um *ponto a ponto* para o seu ambiente. Azure implementa uma instância privada do serviço de aplicações lógicas em sua rede virtual, resultando num ambiente isolado, onde pode criar e executar as aplicações lógicas em recursos dedicados. Quando cria uma aplicação lógica, pode selecionar esse ambiente como localização da sua aplicação, que também oferece o acesso direto da aplicação de lógica aos recursos na sua rede virtual.  

Aplicações lógicas num ISE fornecem as mesmas experiências de usuário e recursos semelhantes que o serviço de aplicações lógicas global. Não só pode utilizar o mesmo built-ins e conectores fornecidas pelo serviço de aplicações lógicas global, mas pode escolher entre os conectores que oferecem versões ISE. Por exemplo, eis alguns conectores padrão que oferecem versões que são executados num ISE:
 
* Armazenamento de Blobs do Azure, armazenamento de ficheiros e armazenamento de tabelas
* Filas do Azure
* Service Bus do Azure
* FTP
* SSH FTP (SFTP)
* SQL Server
* AS2, X12 e EDIFACT

A diferença entre os conectores ISE e não ISE é nos locais onde executar os acionadores e ações:

* Se usar incorporados acionadores e ações, como HTTP no seu ISE, estes acionadores e ações sempre executam no ISE do mesmo que a sua aplicação lógica. 

* Para os conectores que oferecem duas versões: uma versão é executado num ISE, enquanto a outra versão é executada no serviço de aplicações lógicas global.  

  Os conectores que tenham o **ISE** sempre rotular execução no ISE do mesmo que a sua aplicação lógica. Conectores sem o **ISE** etiqueta em execução no serviço de aplicações lógicas global. 

  ![Selecionar conectores do ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Os conectores que configurar no seu ISE também estão disponíveis para utilização num serviço de aplicações lógicas global. 

> [!IMPORTANT]
> Logic apps, ações incorporadas e conectores que executar no seu ISE utilizam um plano de preços diferente, não o consumo baseado no plano de preços. Para obter mais informações, consulte [preços de aplicações lógicas](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Permissões para acesso à rede virtual

Quando cria um ambiente de serviço de integração (ISE), pode selecionar uma rede virtual do Azure como um *ponto a ponto* para o seu ambiente. No entanto, pode *apenas* criar esta relação, ou *peering*, quando cria seu ISE. Esta relação fornece o acesso ISE aos recursos na sua rede virtual, o que permite que aplicações lógicas em que o ISE ligar diretamente aos recursos na sua rede virtual. Para sistemas no local numa rede virtual que está ligada a um ISE, aplicações lógicas podem acessar diretamente desses sistemas, utilizando qualquer um destes itens: 

* Conector do ISE para esse sistema, por exemplo, o SQL Server

* Ação de HTTP 

* Conector personalizado

Para sistemas no local que não estão numa rede virtual ou não tem conectores ISE, pode ligar depois [configurar e utilizar o gateway de dados no local](../logic-apps/logic-apps-gateway-install.md).

Pode selecionar uma rede virtual do Azure como um elemento de rede para o seu ambiente, tem de configurar permissões de controlo de acesso baseado em funções (RBAC) na sua rede virtual para o serviço do Azure Logic Apps. Esta tarefa requer que atribua os **contribuinte de rede** e **contribuinte clássico** funções para o serviço do Azure Logic Apps. Para obter mais informações sobre as permissões de função necessários para o peering, veja a [permissões secção criação, alteração, ou eliminar um peering de rede virtual](../virtual-network/virtual-network-manage-peering.md#permissions).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Contas de integração com o ISE

Pode utilizar contas de integração com logic apps, que são executados num ambiente de serviço de integração (ISE), mas essas contas de integração também tem de utilizar o *ISE mesmo* como as aplicações de lógica ligada. Aplicações lógicas num ISE podem referenciar apenas as contas de integração que estão no ISE do mesmo. Quando cria uma conta de integração, pode selecionar o ISE como a localização para a sua conta de integração.

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">fórum do Azure Logic Apps</a>.
* Para submeter ou votar em ideias para funcionalidades, visite o <a href="http://aka.ms/logicapps-wish" target="_blank">site de comentários dos utilizadores do Logic Apps</a>.

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [ligar às redes virtuais do Azure a partir de aplicações lógicas isoladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Saiba mais sobre [rede Virtual do Azure](../virtual-network/virtual-networks-overview.md)
* Saiba mais sobre [integração da rede virtual para serviços do Azure](../virtual-network/virtual-network-for-azure-services.md)
