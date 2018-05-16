---
title: Azure Active Directory Graph API | Microsoft Docs
description: Um guia de descrição geral e o início rápido para AD Graph API do Azure que permite o acesso programático para o Azure AD através de pontos finais da REST API.
services: active-directory
documentationcenter: ''
author: mtillman
manager: mtillman
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: 4b4f698042f6688e3db484f7d96ccfb06c5cdd4f
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="azure-active-directory-graph-api"></a>Graph API do Azure Active Directory
> [!IMPORTANT]
> Recomendamos vivamente que utilize o [Microsoft Graph](https://graph.microsoft.io/) em vez da Graph API do Azure AD para aceder aos recursos do Azure Active Directory. Os nossos esforços de desenvolvimento concentram-se agora no Microsoft Graph e não estão previstos mais melhoramentos para a Graph API do Azure AD. Existe um número muito limitado de cenários nos quais a Graph API do Azure AD ainda poderá ser adequada. Para obter mais informações, veja a mensagem do blogue [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) (Microsoft Graph ou Graph do Azure AD), no Office Dev Center.
> 
> 

O Active Directory Graph API do Azure fornece acesso programático para o Azure AD através de pontos finais da REST API. As aplicações podem utilizar AD Graph API do Azure para efetuar a criar, ler, atualizar e eliminar operações (CRUD) em dados de diretório e objetos. Por exemplo, AD Graph API do Azure suporta as seguintes operações comuns para um objeto de utilizador:

* Criar um novo utilizador num diretório
* Obter propriedades de detalhado de um utilizador, tais como os respetivos grupos
* Atualizar propriedades de um utilizador, tais como a respetiva localização e o número de telefone, ou altere a palavra-passe
* Verifique a associação a grupos do utilizador para o acesso baseado em funções
* Desativar a conta de utilizador ou eliminá-lo completamente

Além disso, pode efetuar operações similares noutros objetos, tais como grupos e aplicações. Para chamar AD Graph API do Azure num diretório, a aplicação tem de ser registada com o Azure AD. A aplicação deve ser também concedida acesso ao Azure AD Graph API. Este acesso é normalmente conseguido através de um fluxo de consentimento do utilizador ou administrador.

Para começar a utilizar o Active Directory Graph API do Azure, consulte o [Azure guia de introdução ao AD Graph API](active-directory-graph-api-quickstart.md), ou ver o [documentação de referência interativa do Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Funcionalidades
Azure AD Graph API fornece as seguintes funcionalidades:

* **Pontos finais da API REST**: AD Graph API do Azure é um serviço RESTful composta por pontos finais que são acedidos através de pedidos de HTTP padrão. Azure AD Graph API suporta tipos de conteúdo XML ou Javascript Object Notation (JSON) para os pedidos e respostas. Para obter mais informações, consulte [referência da API do REST do Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Autenticação com o Azure AD**: todos os pedidos para o Azure AD Graph API tem de ser autenticado, acrescentando um Token Web JSON (JWT) no cabeçalho de autorização do pedido. Este token é adquirida ao efetuar um pedido para ponto final de token do Azure AD e fornecer credenciais válidas. Pode utilizar o fluxo de credenciais de cliente OAuth 2.0 ou o código de autorização conceder fluxo adquirir um token para chamar o gráfico. Para obter mais informações, [OAuth 2.0 no Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Autorização baseada em funções (RBAC)**: grupos de segurança são utilizados para efetuar o RBAC no AD Graph API do Azure. Por exemplo, se pretende determinar se um utilizador tem acesso a um recurso específico, a aplicação pode chamar o [Verifique a associação de grupo (transitiva)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) operação, que devolve true ou false.
* **Consulta diferencial**: consulta diferencial permite-lhe controlar as alterações de diretório entre dois períodos de tempo sem ter de efetuar consultas frequentes AD Graph API do Azure. Este tipo de pedido irá devolver apenas as alterações efetuadas entre o pedido de consulta diferencial anterior e o pedido atual. Para obter mais informações, consulte [do Azure AD Graph API diferencial consulta](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Extensões de diretórios**: pode adicionar propriedades personalizadas para objetos de diretório sem necessidade de um arquivo de dados externas. Por exemplo, se a sua aplicação precisar de uma propriedade de ID de Skype para cada utilizador, pode registar a nova propriedade no diretório e estarão disponível para utilização em todos os objetos de utilizador. Para obter mais informações, consulte [extensões do Azure AD Graph API do esquema](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Protegidos por âmbitos de permissões**: AD Graph API do Azure expõe âmbitos de permissões que permitem acesso seguro a dados do Azure AD com OAuth 2.0. Suporta uma variedade de tipos de aplicação de cliente, incluindo:
  
  * interfaces de utilizador que recebem acesso delegado aos dados através de autorização do utilizador com sessão iniciada (delegado)
  * aplicações de serviço/daemon que funcionam em segundo plano, sem um utilizador com sessão iniciada estar presente e utilizam o controlo de acesso definidas pela aplicação de baseada em funções
    
    Ambos delegados e permissões de aplicações representam um privilégio exposto pelo Azure AD Graph API e podem ser pedidas por aplicações de cliente através de funcionalidades de permissões de registo de aplicação no [portal do Azure](https://portal.azure.com). [Azure AD Graph API permissão âmbitos](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) fornece informações sobre o que está disponível para utilização pela sua aplicação de cliente.

## <a name="scenarios"></a>Cenários
Azure AD Graph API permite vários cenários de aplicação. Os cenários seguintes são as mais comuns:

* **Linha de negócio (único inquilino) aplicação**: neste cenário, um programador de enterprise funciona de uma organização que tem uma subscrição do Office 365. O programador está a criar uma aplicação web que interage com o Azure AD para efetuar tarefas de tais atribuir uma licença a um utilizador. Esta tarefa requer acesso ao AD Graph API do Azure, para que o programador da aplicação de inquilino único se regista no Azure AD e configura de leitura e escrita permissões para o Azure AD Graph API. Em seguida, a aplicação está configurada para utilizar as suas próprias credenciais ou de utilizador atualmente início de sessão para adquirir um token para chamar AD Graph API do Azure.
* **Software como uma aplicação de serviço (multi-inquilino)**: neste cenário, um fabricante independente de software (ISV) está a desenvolver aplicações web do multi-inquilino alojado que fornece funcionalidades de gestão de utilizadores de outras organizações que utilizam o Azure AD. Estas funcionalidades requerem acesso a objetos de diretório e, por isso, a aplicação tem de chamar AD Graph API do Azure. O programador regista a aplicação no Azure AD, configura-o para exigir a leitura e escrita permissões para AD Graph API do Azure e, em seguida, ativa o acesso externo para que as outras organizações podem autorizar a utilizar a aplicação no seu diretório. Quando um utilizador na outra organização efetua a autenticação para a aplicação pela primeira vez, é apresentada uma caixa de diálogo de consentimento com as permissões que a aplicação está a solicitar. Os conceder consentimento, em seguida, irá dar-a aplicação pediu permissões para o Azure AD Graph API no diretório do utilizador. Para obter mais informações sobre a arquitetura de consentimento, consulte [descrição geral do Framework consentimento](active-directory-integrating-applications.md).

## <a name="see-also"></a>Consultar Também
[Guia de introdução ao Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Documentação de AD Graph REST do Azure](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Guia para programadores do Azure Active Directory](active-directory-developers-guide.md)

