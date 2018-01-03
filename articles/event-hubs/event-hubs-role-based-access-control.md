---
title: "Pré-visualização do controlo de acesso de Event Hubs Role-Based (RBAC) do Azure | Microsoft Docs"
description: "Controlo de acesso baseado em funções de Hubs de eventos do Azure"
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
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 0d3a779eb2cccf242bcd42d82c1a90048b3512ab
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="active-directory-role-based-access-control-preview"></a>Controlo de acesso do Active Directory Directory Role-Based (pré-visualização)

Microsoft Azure fornece gestão de controlo de acesso integrada para recursos e aplicações baseadas no Azure Active Directory (Azure AD). Com o Azure AD, pode optar por gerir contas de utilizador e aplicações especificamente para o Azure com aplicações, ou podem federar a infraestrutura existente do Active Directory com o Azure AD para toda a empresa single-sign-on que abranja também os recursos do Azure e as aplicações do Azure hospedadas. Em seguida, pode atribuir as identidades de utilizador e a aplicação do Azure AD para as funções globais e específicos do serviço para conceder acesso a recursos do Azure.

Para os Event Hubs do Azure, a gestão de espaços de nomes e relacionados todos os recursos através do portal do Azure e a API de gestão de recursos do Azure já estão protegidas utilizando o *controlo de acesso baseado em funções* modelo (RBAC). RBAC para operações de runtime é uma funcionalidade agora em pré-visualização pública. 

Não precisa de uma aplicação que utiliza o RBAC do Azure AD processar regras SAS e chaves ou quaisquer outros tokens de acesso específicas para os Event Hubs. A aplicação cliente interage com o Azure AD para estabelecer um contexto de autenticação e adquire um token de acesso para os Event Hubs. Com contas de utilizador de domínio que necessitam de início de sessão interativo, a aplicação nunca processa as credenciais diretamente.

## <a name="event-hubs-roles-and-permissions"></a>Permissões e funções de Hubs de eventos

Para a pré-visualização pública inicial, só é possível adicionar contas do Azure AD e principais de serviço para as funções de "Proprietário" ou "Contribuinte" de um espaço de nomes de Event Hubs. Esta operação concede o controlo total da identidade através de todas as entidades no espaço de nomes. Operações de gestão que alteram a topologia de espaço de nomes inicialmente são apenas suportados gestão de recursos apesar do Azure e não através da interface de gestão de REST de Hubs de eventos nativa. Este suporte também significa que o cliente do .NET Framework [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) objeto não pode ser utilizado com uma conta do Azure AD.  

## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>Utilizar os Hubs de eventos com uma conta de utilizador de domínio do Azure AD

A secção seguinte descreve os passos necessários para criar e executar uma aplicação de exemplo que solicita um Azure interativo utilizador do AD para iniciar sessão, como conceder acesso de Event Hubs a essa conta de utilizador e como utilizar essa identidade para aceder aos Hubs de eventos. 

Esta introdução descreve uma aplicação de consola simples, o [código para o qual é no Github](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/)

### <a name="create-an-active-directory-user-account"></a>Criar uma conta de utilizador do Active Directory

Neste primeiro passo é opcional. Cada subscrição do Azure automaticamente se encontra emparelhada com um inquilino do Azure Active Directory e se tem acesso a uma subscrição do Azure, a sua conta de utilizador já está registada. Isto significa que apenas pode utilizar a sua conta. 

Se pretender continuar a criar uma conta específica para este cenário, [siga estes passos](../automation/automation-create-aduser-account.md). Tem de ter permissão para criar contas no inquilino do Azure Active Directory, que pode não ser o caso de cenários de empresa maior.

### <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes de Event Hubs

Em seguida, [criar um espaço de nomes de Event Hubs](event-hubs-create.md) de uma das regiões do Azure que tenham suporte de pré-visualização de Event Hubs para RBAC: **dos EUA Leste**, **dos EUA Leste 2**, ou **Europa Ocidental** . 

Quando o espaço de nomes for criado, navegue até à respetiva **controlo de acesso (IAM)** página no portal e, em seguida, clique em **adicionar** para adicionar a conta de utilizador do Azure AD para a função de proprietário. Se utilizar a sua própria conta de utilizador e criar o espaço de nomes, já estão encontra na função de proprietário. Para adicionar uma conta diferente para a função, procure o nome da aplicação web a **adicionar permissões** painel **selecione** campo e, em seguida, clique na entrada. Em seguida, clique em **Guardar**.
 
![](./media/event-hubs-role-based-access-control/rbac1.PNG)

A conta de utilizador tem agora acesso para o espaço de nomes de Event Hubs e para o hub de eventos que criou anteriormente.
 
### <a name="register-the-application"></a>Registar a aplicação

Antes de poder executar a aplicação de exemplo, registá-lo no Azure AD e aprovar o pedido de consentimento que permite que a aplicação para aceder ao Event Hubs no seu nome. 

Porque a aplicação de exemplo é uma aplicação de consola, tem de registar uma aplicação nativa e adicione permissões de API para **Microsoft.EventHub** ao conjunto de "as permissões necessárias". Também precisam de aplicações nativas um **URI de redirecionamento** no Azure AD que funciona como um identificador; o URI não têm de ser um destino de rede. Utilize `http://eventhubs.microsoft.com` para este exemplo, porque o exemplo de código já utiliza esse URI.

Os passos detalhados de registo são explicados em [neste tutorial](../active-directory/develop/active-directory-integrating-applications.md). Siga os passos para registar um **nativo** aplicação e, em seguida, siga as instruções de atualização para adicionar o **Microsoft.EventHub** API para as permissões necessárias. Como seguir os passos, anote o **TenantId** e o **ApplicationId**, uma vez que irá necessitar destes valores para executar a aplicação.

### <a name="run-the-app"></a>Executar a aplicação

Antes de poder executar o exemplo, edite o ficheiro App. config e, dependendo do seu cenário, defina os seguintes valores:

- `tenantId`: Definido como **TenantId** valor.
- `clientId`: Definido como **ApplicationId** valor. 
- `clientSecret`: Se pretender iniciar sessão com o segredo do cliente, crie-o no Azure AD. Além disso, utilize uma aplicação web ou a API em vez de uma aplicação nativa. Além disso, adicionar a aplicação em **controlo de acesso (IAM)** no espaço de nomes que criou anteriormente.
- `eventHubNamespaceFQDN`: Definido como o nome DNS completo do seu espaço de nomes de Event Hubs recém-criado; Por exemplo, `example.servicebus.windows.net`.
- `eventHubName`: Definida para o nome do hub de eventos que criou.
- O URI de redirecionamento especificado na sua aplicação nos passos anteriores.
 
Quando executar a aplicação de consola, lhe for pedido para selecionar um cenário; Clique em **início de sessão de utilizador interativa** , escrevendo o respetivo número e prima ENTER. A aplicação apresenta uma janela de início de sessão, pede-lhe o consentimento aceder aos Hubs de eventos e, em seguida, utiliza o serviço para percorrer o cenário de envio/receção de mensagens em fila utilizando a identidade de início de sessão.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre os Hubs de Eventos, visite as seguintes ligações:

* Introdução a um [Tutorial dos Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)
* [Detalhes de preços de Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Aplicações de exemplo que utilizam Hubs de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples)