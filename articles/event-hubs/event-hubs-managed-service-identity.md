---
title: "Identidade do serviço gerido pré-visualização do Event Hubs do Azure | Microsoft Docs"
description: "Utilizar as identidades de serviço geridas com o Event Hubs do Azure"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2017
ms.author: sethm
ms.openlocfilehash: dd50e4f6ebc5fdf5496a5127fde20bd052087b59
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="managed-service-identity-preview"></a>Identidade de serviço geridas (pré-visualização)

Uma identidade de serviço geridas (MSI) é uma funcionalidade de cross-Azure que permite-lhe criar uma identidade segura associada à implementação em que é executado o código da aplicação. Pode, em seguida, associar essa identidade com funções de controlo de acesso que conceda permissões personalizadas para aceder a recursos do Azure específicos que necessita da sua aplicação. 

Com MSI, a plataforma do Azure gere esta identidade de tempo de execução. Não é necessário armazenar e proteger chaves de acesso no seu código da aplicação ou a configuração, para a identidade do próprio ou para os recursos que necessita para aceder. Não precisa de uma aplicação de cliente dos Event Hubs com no interior de uma aplicação do App Service do Azure ou numa máquina virtual com o suporte MSI ativado processar regras SAS e chaves ou quaisquer outros tokens de acesso. A aplicação de cliente apenas tem o endereço de ponto final do espaço de nomes de Event Hubs. Quando se liga a aplicação, os Event Hubs vincula o contexto do MSI para o cliente numa operação que é apresentado um exemplo neste artigo.

Assim que estiver associado a uma identidade de serviço geridas, um cliente de Event Hubs possa realizar operações de todos os autorizados. Autorização é concedida ao associar um MSI com funções de Event Hubs. 

## <a name="event-hubs-roles-and-permissions"></a>Permissões e funções de Hubs de eventos

Para a versão de pré-visualização pública inicial, só é possível adicionar uma identidade de serviço geridas para as funções de "Proprietário" ou "Contribuinte" de um espaço de nomes de Event Hubs, que concede o controlo total da identidade em todas as entidades no espaço de nomes. No entanto, a gestão de operações que alteram a topologia de espaço de nomes são inicialmente apenas apesar suportada do Azure Resource Manager e não através da interface de gestão de REST de Hubs de eventos nativa. Este suporte também significa que não é possível utilizar o cliente do .NET Framework [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) objeto dentro de uma identidade de serviço geridas. 
 
## <a name="use-event-hubs-with-a-managed-service-identity"></a>Utilizar os Hubs de eventos com uma identidade de serviço geridas

A secção seguinte descreve os passos necessários para criar e implementar uma aplicação de exemplo que é executado sob uma identidade de serviço geridas, como de conceder à identidade acesso a um espaço de nomes de Event Hubs e como a aplicação interage com os event hubs utilizando que identidade.

Esta introdução descreve uma aplicação web alojada no [App Service do Azure](https://azure.microsoft.com/services/app-service/). Os passos necessários para uma aplicação alojada em VM são semelhantes.

### <a name="create-an-app-service-web-application"></a>Criar uma aplicação do serviço de aplicações web

O primeiro passo é criar uma aplicação ASP.NET do serviço de aplicações. Se não estiver familiarizado com como fazê-lo no Azure, siga [neste guia de procedimentos](../app-service/app-service-web-get-started-dotnet-framework.md). No entanto, em vez de criar uma aplicação MVC, conforme mostrado no tutorial, crie uma aplicação de formulários Web.

### <a name="set-up-the-managed-service-identity"></a>Configurar a identidade de serviço geridas

Depois de criar a aplicação, navegue para a aplicação web recentemente criada no portal do Azure (também apresentado o procedimentos), em seguida, navegue para o **identidade de serviço geridas** página e ativar a funcionalidade: 

![](./media/event-hubs-managed-service-identity/msi1.png)
 
Assim que tiver ativada a funcionalidade, uma nova identidade de serviço é criada no Azure Active Directory e configurada para o anfitrião do serviço de aplicações.

### <a name="create-a-new-event-hubs-namespace"></a>Criar um novo espaço de nomes de Event Hubs

Em seguida, [criar um espaço de nomes de Event Hubs](event-hubs-create.md) de uma das regiões do Azure que tenham suporte de pré-visualização para MSI: **dos EUA Leste**, **dos EUA Leste 2**, ou **Europa Ocidental**. 

Navegue para o espaço de nomes **controlo de acesso (IAM)** página no portal e, em seguida, clique em **adicionar** para adicionar a identidade de serviço geridas para o **proprietário** função. Para tal, procure o nome da aplicação web a **adicionar permissões** painel **selecione** campo e, em seguida, clique na entrada. Em seguida, clique em **Guardar**.

![](./media/event-hubs-managed-service-identity/msi2.png)
 
Identidade de serviço gerida da aplicação web tem agora acesso para o espaço de nomes de Event Hubs e para o hub de eventos que criou anteriormente. 

### <a name="run-the-app"></a>Executar a aplicação

Agora, modifique a página predefinida da aplicação ASP.NET que criou. Também pode utilizar o código de aplicação web de [este repositório do GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

Depois de iniciar a sua aplicação, o browser aponte para EventHubsMSIDemo.aspx. Em alternativa, defina-o como a página de início. O código pode ser encontrado no ficheiro EventHubsMSIDemo.aspx.cs. O resultado é uma aplicação web mínima com alguns campos de entrada e com **enviar** e **receber** botões que se ligam ao Event Hubs para enviar ou receber mensagens. 

Tenha em atenção o [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objecto foi inicializado. Em vez de utilizar o fornecedor de tokens de acesso partilhado Token (SAS), o código cria um fornecedor de tokens para a identidade de serviço geridas com o `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` chamada. Como tal, não existem nenhum segredos manter e utilizar. O fluxo do contexto de identidade de serviço geridas para os Event Hubs e o handshake de autorização são processadas automaticamente pelo fornecedor de token, o que é um modelo mais simples que através da SAS.

Depois de efetuar estas alterações, publicar e executar a aplicação. É uma forma fácil de obter os dados de publicação corretos transferir e, em seguida, importar um perfil de publicação no Visual Studio:

![](./media/event-hubs-managed-service-identity/msi3.png)
 
Para enviar ou receber mensagens, introduza o nome do espaço de nomes e o nome da entidade que criou, em seguida, clique em **enviar** ou **receber**. 
 
Tenha em atenção que a identidade de serviço geridas só funciona no ambiente do Azure e apenas na implementação do serviço de aplicações em que esteja configurado. Note também que identidades de serviço geridas não funcionam com as ranhuras de implementação do serviço de aplicações neste momento.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre os Hubs de Eventos, visite as seguintes ligações:

* Introdução a um [Tutorial dos Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)
* [Detalhes de preços de Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Aplicações de exemplo que utilizam Hubs de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples)