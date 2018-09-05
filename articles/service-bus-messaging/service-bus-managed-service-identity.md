---
title: Identidade do serviço gerido com a pré-visualização do Azure Service Bus | Documentos da Microsoft
description: Utilizar identidades de serviço gerido com o Azure Service Bus
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2018
ms.author: spelluru
ms.openlocfilehash: e886089bbceff5fc1963044c4c1f2f7a012ea2a2
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697735"
---
# <a name="managed-service-identity-preview"></a>Identidade de Serviço Gerida (pré-visualização)

Uma identidade de serviço gerida (MSI) é uma funcionalidade de entre o Azure permite-lhe criar uma identidade segura associada à implementação em que o código da aplicação é executado. Em seguida, pode associar essa identidade com funções de controlo de acesso que concedem permissões personalizadas para aceder a recursos específicos do Azure que a aplicação precisa.

Com o MSI, a plataforma do Azure gere esta identidade de tempo de execução. Não é necessário armazenar e proteger chaves de acesso no seu código da aplicação ou a configuração, para a identidade propriamente dita, ou para os recursos que precisa acessar. Uma aplicação de cliente do Service Bus em execução dentro de um aplicativo de serviço de aplicações do Azure ou numa máquina virtual com o suporte MSI ativado não precisa lidar com regras SAS e as chaves ou quaisquer outros tokens de acesso. A aplicação cliente precisa apenas o endereço de ponto final do espaço de nomes de mensagens do Service Bus. Quando se liga a aplicação, o Service Bus vincula o contexto do MSI para o cliente de uma operação que é mostrado um exemplo deste artigo. 

Assim que estiver associado a uma identidade de serviço gerida, um cliente do Service Bus pode efetuar operações de todos os autorizados. Autorização é concedida ao associar um MSI com funções do Service Bus. 

## <a name="service-bus-roles-and-permissions"></a>Permissões e funções do Service Bus

Para a versão de pré-visualização pública inicial, só pode adicionar uma identidade de serviço gerido para as funções de "Proprietário" ou "Contribuinte" de um espaço de nomes do Service Bus, que concede o controlo total de identidade em todas as entidades no espaço de nomes. No entanto, a gestão de operações que alteram a topologia de espaço de nomes são inicialmente suportada apenas através do Azure Resource Manager e não através da interface de gestão do REST do Service Bus nativa. Esse suporte também significa que não é possível utilizar o cliente do .NET Framework [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) objeto dentro de uma identidade de serviço gerida.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Utilizar o Service Bus com uma identidade de serviço gerido

A secção seguinte descreve os passos necessários para criar e implementar uma aplicação de exemplo que é executado sob uma identidade de serviço gerido, como pode conceder esse acesso de identidade para um espaço de nomes de mensagens do Service Bus e como o aplicativo interage com o Service Bus entidades usando essa identidade.

Esta introdução descreve uma aplicação web alojada na [App Service do Azure](https://azure.microsoft.com/services/app-service/). Os passos necessários para um aplicativo hospedado por VM são semelhantes.

### <a name="create-an-app-service-web-application"></a>Criar uma aplicação web do serviço de aplicações

A primeira etapa é criar um aplicativo ASP.NET de serviço de aplicações. Se não estiver familiarizado com a forma de fazê-lo no Azure, siga [este guia de procedimentos](../app-service/app-service-web-get-started-dotnet-framework.md). No entanto, em vez de criar uma aplicação de MVC, conforme mostrado no tutorial, crie uma aplicação de Web Forms.

### <a name="set-up-the-managed-service-identity"></a>Configurar a identidade de serviço gerido

Depois de criar a aplicação, navegue para a aplicação web recentemente criada no portal do Azure (também mostrada os procedimentos), em seguida, navegue para o **identidade do serviço gerido** de páginas e ativar a funcionalidade: 

![](./media/service-bus-managed-service-identity/msi1.png)

Assim que tive ativado a funcionalidade, uma nova identidade de serviço é criada no Azure Active Directory e configurada o Host de serviço de aplicações.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Criar um novo namespace de mensagens do Service Bus

Em seguida, [criar um espaço de nomes de mensagens do Service Bus](service-bus-create-namespace-portal.md) de uma das regiões do Azure com suporte de pré-visualização para RBAC: **E.U. a leste**, **E.U. a leste 2**, ou **Europa Ocidental** . 

Navegue para o espaço de nomes **controlo de acesso (IAM)** página no portal e, em seguida, clique em **Add** para adicionar a identidade de serviço gerido para o **proprietário** função. Para tal, procure o nome da aplicação web no **adicionar permissões** painel **selecione** campo e, em seguida, clique na entrada. Em seguida, clique em **Guardar**.

![](./media/service-bus-managed-service-identity/msi2.png)
 
Identidade do serviço gerido do aplicativo web tem agora acesso ao espaço de nomes do Service Bus e para a fila que criou anteriormente. 

### <a name="run-the-app"></a>Executar a aplicação

Agora, modifique a página predefinida do aplicativo ASP.NET que criou. Pode usar o código de aplicativo da web [este repositório de GitHub](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet).  

A página default. aspx é sua página de destino. O código pode ser encontrado no arquivo Default.aspx.cs. O resultado é um aplicativo web mínima com alguns campos de entrada e com **enviar** e **receber** botões que ligar ao Service Bus para enviar ou receber mensagens.

Observe como o [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) objeto é inicializado. Em vez de utilizar o fornecedor do token de acesso partilhado Token (SAS), o código cria um fornecedor do token para a identidade de serviço gerido com o `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)` chamar. Como tal, não há nenhum segredos para manter e utilizar. O fluxo do contexto de identidade do serviço gerido para o Service Bus e o handshake de autorização são manipulados automaticamente pelo fornecedor do token, que é um modelo mais simples do que através da SAS.

Depois de efetuar estas alterações, publicar e executar a aplicação. Uma forma fácil de obter os dados de publicação corretos é baixar e, em seguida, importar um perfil de publicação no Visual Studio:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Para enviar ou receber mensagens, introduza o nome do espaço de nomes e o nome da entidade que criou, em seguida, clique em **enviar** ou **receber**.


> [!NOTE]
> - A identidade de serviço gerido funciona apenas dentro do ambiente do Azure, nos serviços de aplicações, as VMs do Azure, e os conjuntos de dimensionamento. Para aplicativos .NET, a biblioteca de Microsoft.Azure.Services.AppAuthentication, que é utilizada pelo pacote NuGet do Service Bus, fornece uma abstração sobre este protocolo e oferece suporte a uma experiência de desenvolvimento local. Esta biblioteca também permite-lhe testar o seu código localmente no computador de desenvolvimento, com a sua conta de utilizador do Visual Studio, a CLI 2.0 do Azure ou a autenticação integrada do Active Directory. Para obter mais informações sobre as opções de desenvolvimento local com esta biblioteca, consulte [autenticação de serviço a serviço para o Azure Key Vault com o .NET](../key-vault/service-to-service-authentication.md).  
> 
> - Atualmente, identidades de serviço geridas não funcionam com blocos de implementação do serviço de aplicações.

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre mensagens do Service Bus, consulte os seguintes tópicos.

* [Noções básicas sobre o Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)