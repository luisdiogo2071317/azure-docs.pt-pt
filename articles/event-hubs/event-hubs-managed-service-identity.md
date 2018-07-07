---
title: Identidade do serviço gerido com a pré-visualização de Event Hubs do Azure | Documentos da Microsoft
description: Utilizar identidades de serviço gerido com os Hubs de eventos do Azure
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: sethm
ms.openlocfilehash: abff3f715a1fccba172147f02b83f7209f87cf9e
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902521"
---
# <a name="managed-service-identity-preview"></a>Identidade de Serviço Gerida (pré-visualização)

Uma identidade de serviço gerida (MSI) é uma funcionalidade de entre o Azure permite-lhe criar uma identidade segura associada à implementação em que o código da aplicação é executado. Em seguida, pode associar essa identidade com funções de controlo de acesso que concedem permissões personalizadas para aceder a recursos específicos do Azure que a aplicação precisa. 

Com o MSI, a plataforma do Azure gere esta identidade de tempo de execução. Não é necessário armazenar e proteger chaves de acesso no seu código da aplicação ou a configuração, para a identidade propriamente dita, ou para os recursos que precisa acessar. Uma aplicação de cliente dos Hubs de eventos em execução dentro de um aplicativo de serviço de aplicações do Azure ou numa máquina virtual com o suporte MSI ativado não precisa lidar com regras SAS e as chaves ou quaisquer outros tokens de acesso. A aplicação cliente precisa apenas o endereço de ponto final do espaço de nomes dos Hubs de eventos. Quando se liga a aplicação, os Hubs de eventos vincula o contexto do MSI para o cliente de uma operação que é mostrado um exemplo deste artigo.

Assim que estiver associado a uma identidade de serviço gerida, um cliente dos Hubs de eventos pode efetuar operações de todos os autorizados. Autorização é concedida ao associar um MSI com funções de Hubs de eventos. 

## <a name="event-hubs-roles-and-permissions"></a>Permissões e funções de Hubs de eventos

Para a versão de pré-visualização pública inicial, só pode adicionar uma identidade de serviço gerido para as funções de "Proprietário" ou "Contribuinte" de um espaço de nomes de Hubs de eventos, que concede o controlo total de identidade em todas as entidades no espaço de nomes. No entanto, a gestão de operações que alteram a topologia de espaço de nomes são inicialmente suportada apenas através do Azure Resource Manager e não através da interface de gestão do REST dos Hubs de eventos nativa. Esse suporte também significa que não é possível utilizar o cliente do .NET Framework [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) objeto dentro de uma identidade de serviço gerida. 
 
## <a name="use-event-hubs-with-a-managed-service-identity"></a>Utilizar os Hubs de eventos com uma identidade de serviço gerido

A secção seguinte descreve os passos necessários para criar e implementar uma aplicação de exemplo que é executado sob uma identidade de serviço gerido, como pode conceder esse acesso de identidade para um espaço de nomes de Hubs de eventos e como o aplicativo interage com os hubs de eventos com que identidade.

Esta introdução descreve uma aplicação web alojada na [App Service do Azure](https://azure.microsoft.com/services/app-service/). Os passos necessários para um aplicativo hospedado por VM são semelhantes.

### <a name="create-an-app-service-web-application"></a>Criar uma aplicação web do serviço de aplicações

A primeira etapa é criar um aplicativo ASP.NET de serviço de aplicações. Se não estiver familiarizado com a forma de fazê-lo no Azure, siga [este guia de procedimentos](../app-service/app-service-web-get-started-dotnet-framework.md). No entanto, em vez de criar uma aplicação de MVC, conforme mostrado no tutorial, crie uma aplicação de Web Forms.

### <a name="set-up-the-managed-service-identity"></a>Configurar a identidade de serviço gerido

Depois de criar a aplicação, navegue para a aplicação web recentemente criada no portal do Azure (também mostrada os procedimentos), em seguida, navegue para o **identidade do serviço gerido** de páginas e ativar a funcionalidade: 

![](./media/event-hubs-managed-service-identity/msi1.png)
 
Assim que tive ativado a funcionalidade, uma nova identidade de serviço é criada no Azure Active Directory e configurada o Host de serviço de aplicações.

### <a name="create-a-new-event-hubs-namespace"></a>Criar um novo espaço de nomes de Hubs de eventos

Em seguida, [criar um espaço de nomes de Hubs de eventos](event-hubs-create.md) de uma das regiões do Azure que tem suporte de pré-visualização para MSI: **E.U. a leste**, **E.U. a leste 2**, ou **Europa Ocidental**. 

Navegue para o espaço de nomes **controlo de acesso (IAM)** página no portal e, em seguida, clique em **Add** para adicionar a identidade de serviço gerido para o **proprietário** função. Para tal, procure o nome da aplicação web no **adicionar permissões** painel **selecione** campo e, em seguida, clique na entrada. Em seguida, clique em **Guardar**.

![](./media/event-hubs-managed-service-identity/msi2.png)
 
A identidade de serviço gerido para a aplicação web agora tem acesso ao espaço de nomes dos Hubs de eventos e para o hub de eventos que criou anteriormente. 

### <a name="run-the-app"></a>Executar a aplicação

Agora, modifique a página predefinida do aplicativo ASP.NET que criou. Também pode utilizar o código de aplicativo da web [este repositório de GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

>[!NOTE] 
> Embora a funcionalidade MSI está em pré-visualização, não se esqueça de utilizar o [versão de pré-visualização da biblioteca do Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/4.2.2-preview) para acessar as novas APIs. 

Depois de iniciar a aplicação, o browser aponte para EventHubsMSIDemo.aspx. Em alternativa, defina-o como a página inicial. O código pode ser encontrado no arquivo EventHubsMSIDemo.aspx.cs. O resultado é um aplicativo web mínima com alguns campos de entrada e com **enviar** e **receber** botões que se ligam aos Hubs de eventos para enviar ou receber eventos. 

Observe como o [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objeto é inicializado. Em vez de utilizar o fornecedor do token de acesso partilhado Token (SAS), o código cria um fornecedor do token para a identidade de serviço gerido com o `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` chamar. Como tal, não há nenhum segredos para manter e utilizar. O fluxo do contexto de identidade do serviço gerido para os Hubs de eventos e o handshake de autorização são manipulados automaticamente pelo fornecedor do token, que é um modelo mais simples do que através da SAS.

Depois de efetuar estas alterações, publicar e executar a aplicação. Uma forma fácil de obter os dados de publicação corretos é baixar e, em seguida, importar um perfil de publicação no Visual Studio:

![](./media/event-hubs-managed-service-identity/msi3.png)
 
Para enviar ou receber mensagens, introduza o nome do espaço de nomes e o nome da entidade que criou, em seguida, clique em **enviar** ou **receber**. 
 
Tenha em atenção que a identidade de serviço gerido só funciona dentro do ambiente do Azure e apenas no âmbito do implementação de serviço de aplicações em que tiver configurado. Observe também que identidades de serviço geridas não funcionam com blocos de implementação do serviço de aplicações neste momento.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre os Hubs de Eventos, visite as seguintes ligações:

* Introdução a um [Tutorial dos Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)
* [Os detalhes dos preços dos Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Aplicações de exemplo que utilizam Hubs de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples)