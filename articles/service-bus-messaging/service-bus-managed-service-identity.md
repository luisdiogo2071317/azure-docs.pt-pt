---
title: "Identidade do serviço gerido pré-visualização do Service Bus do Azure | Microsoft Docs"
description: "Utilizar as identidades de serviço geridas com o Service Bus do Azure"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 6965e80cf10b732d4d0a8fb78447f188c133979d
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="managed-service-identity-preview"></a>Identidade de serviço geridas (pré-visualização)

Uma identidade de serviço geridas (MSI) é uma funcionalidade de cross-Azure que permite-lhe criar uma identidade segura associada à implementação em que é executado o código da aplicação. Pode, em seguida, associar essa identidade com funções de controlo de acesso que conceda permissões personalizadas para aceder a recursos do Azure específicos que necessita da sua aplicação.

Com MSI, a plataforma do Azure gere esta identidade de tempo de execução. Não é necessário armazenar e proteger chaves de acesso no seu código da aplicação ou a configuração, para a identidade do próprio ou para os recursos que necessita para aceder. Não precisa de uma aplicação de cliente do Service Bus com no interior de uma aplicação do App Service do Azure ou numa máquina virtual com o suporte MSI ativado processar regras SAS e chaves ou quaisquer outros tokens de acesso. A aplicação de cliente apenas tem o endereço de ponto final do espaço de nomes de mensagens do Service Bus. Quando se liga a aplicação, o Service Bus vincula o contexto do MSI para o cliente numa operação que é apresentado um exemplo neste artigo. 

Assim que estiver associado a uma identidade de serviço geridas, um cliente do Service Bus pode efetuar operações de todos os autorizados. Autorização é concedida ao associar um MSI com as funções do Service Bus. 

## <a name="service-bus-roles-and-permissions"></a>As funções do Service Bus e permissões

Para a versão de pré-visualização pública inicial, só é possível adicionar uma identidade de serviço geridas para as funções de "Proprietário" ou "Contribuinte" de um espaço de nomes do Service Bus, que concede o controlo total da identidade em todas as entidades no espaço de nomes. No entanto, a gestão de operações que alteram a topologia de espaço de nomes são inicialmente apenas apesar suportada do Azure Resource Manager e não através da interface de gestão de REST do Service Bus nativa. Este suporte também significa que não é possível utilizar o cliente do .NET Framework [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) objeto dentro de uma identidade de serviço geridas.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Utilizar o Service Bus com uma identidade de serviço geridas

A secção seguinte descreve os passos necessários para criar e implementar uma aplicação de exemplo que é executado sob uma identidade de serviço geridas, como de conceder à identidade acesso a um espaço de nomes de mensagens do Service Bus e como a aplicação interage com o Service Bus entidades utilizando essa identidade.

Esta introdução descreve uma aplicação web alojada no [App Service do Azure](https://azure.microsoft.com/services/app-service/). Os passos necessários para uma aplicação alojada em VM são semelhantes.

### <a name="create-an-app-service-web-application"></a>Criar uma aplicação do serviço de aplicações web

O primeiro passo é criar uma aplicação ASP.NET do serviço de aplicações. Se não estiver familiarizado com como fazê-lo no Azure, siga [neste guia de procedimentos](../app-service/app-service-web-get-started-dotnet-framework.md). No entanto, em vez de criar uma aplicação MVC, conforme mostrado no tutorial, crie uma aplicação de formulários Web.

### <a name="set-up-the-managed-service-identity"></a>Configurar a identidade de serviço geridas

Depois de criar a aplicação, navegue para a aplicação web recentemente criada no portal do Azure (também apresentado o procedimentos), em seguida, navegue para o **identidade de serviço geridas** página e ativar a funcionalidade: 

![](./media/service-bus-managed-service-identity/msi1.png)

Assim que tiver ativada a funcionalidade, uma nova identidade de serviço é criada no Azure Active Directory e configurada para o anfitrião do serviço de aplicações.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Criar um novo espaço de nomes mensagens do Service Bus

Em seguida, [criar um espaço de nomes de mensagens do Service Bus](service-bus-create-namespace-portal.md) de uma das regiões do Azure que tenham suporte de pré-visualização para RBAC: **dos EUA Leste**, **dos EUA Leste 2**, ou **Europa Ocidental** . 

Navegue para o espaço de nomes **controlo de acesso (IAM)** página no portal e, em seguida, clique em **adicionar** para adicionar a identidade de serviço geridas para o **proprietário** função. Para tal, procure o nome da aplicação web a **adicionar permissões** painel **selecione** campo e, em seguida, clique na entrada. Em seguida, clique em **Guardar**.

![](./media/service-bus-managed-service-identity/msi2.png)
 
Identidade de serviço gerida da aplicação web tem agora acesso para o espaço de nomes do Service Bus e para a fila que criou anteriormente. 

### <a name="run-the-app"></a>Executar a aplicação

Agora, modifique a página predefinida da aplicação ASP.NET que criou. Também pode utilizar o código de aplicação web de [este repositório do GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/ManagedServiceIdentity). 

A página de Default.aspx é a página de destino. O código pode ser encontrado no ficheiro default.aspx. O resultado é uma aplicação web mínima com alguns campos de entrada e com **enviar** e **receber** botões ligar ao Service Bus para enviar ou receber mensagens.

Tenha em atenção o [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objecto foi inicializado. Em vez de utilizar o fornecedor de tokens de acesso partilhado Token (SAS), o código cria um fornecedor de tokens para a identidade de serviço geridas com o `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` chamada. Como tal, não existem nenhum segredos manter e utilizar. O fluxo do contexto de identidade de serviço geridas ao Service Bus e o handshake de autorização são processadas automaticamente pelo fornecedor de token, o que é um modelo mais simples que através da SAS.

Depois de efetuar estas alterações, publicar e executar a aplicação. É uma forma fácil de obter os dados de publicação corretos transferir e, em seguida, importar um perfil de publicação no Visual Studio:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Para enviar ou receber mensagens, introduza o nome do espaço de nomes e o nome da entidade que criou, em seguida, clique em **enviar** ou **receber**. 
 
Tenha em atenção que a identidade de serviço geridas só funciona no ambiente do Azure e apenas na implementação do serviço de aplicações em que esteja configurado. Note também que identidades de serviço geridas não funcionam com as ranhuras de implementação do serviço de aplicações neste momento.

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre mensagens do Service Bus, consulte os seguintes tópicos.

* [Noções básicas sobre o Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)