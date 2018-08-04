---
title: O Azure Active Directory Graph API | Documentos da Microsoft
description: Um guia de visão geral e de início rápido para o Azure AD Graph API que permite o acesso programático para o Azure AD através de pontos finais de REST API.
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
ms.openlocfilehash: 3ec6bbcb9250626f8d6ef75330991c5ebac053a7
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504722"
---
# <a name="azure-active-directory-graph-api"></a>Graph API do Azure Active Directory
> [!IMPORTANT]
> Recomendamos vivamente que utilize o [Microsoft Graph](https://graph.microsoft.io/) em vez da Graph API do Azure AD para aceder aos recursos do Azure Active Directory. Os nossos esforços de desenvolvimento concentram-se agora no Microsoft Graph e não estão previstos mais melhoramentos para a Graph API do Azure AD. Existe um número muito limitado de cenários nos quais a Graph API do Azure AD ainda poderá ser adequada. Para obter mais informações, veja a mensagem do blogue [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) (Microsoft Graph ou Graph do Azure AD), no Office Dev Center.
> 
> 

A API do Azure Active Directory Graph fornece acesso programático para o Azure AD através de pontos finais de REST API. Aplicações podem utilizar o Azure AD Graph API para efetuar a criar, ler, atualizar e eliminar operações de (CRUD) em objetos e dados do diretório. Por exemplo, o Azure AD Graph API suporta as seguintes operações comuns para um objeto de utilizador:

* Criar um novo utilizador num diretório
* Obter propriedades detalhadas de um utilizador, tais como os grupos
* Atualizar propriedades de um utilizador, como a respetiva localização e o número de telefone, ou alterar a palavra-passe
* Verifique a associação do grupo de um utilizador para acesso baseado em funções
* Desativar a conta de um utilizador ou eliminá-lo totalmente

Além disso, pode efetuar operações semelhantes em outros objetos, tais como grupos e aplicações. Para chamar o Azure AD Graph API num diretório, a sua aplicação tem de estar registada com o Azure AD. Seu aplicativo deve também ser concedido acesso ao Azure AD Graph API. Este acesso é feito normalmente através de um fluxo de consentimento do utilizador ou administrador.

Para começar a utilizar o Azure Active Directory Graph API, consulte a [do Azure AD Graph API do início rápido guia](active-directory-graph-api-quickstart.md), ou ver a [interativa documentação de referência do Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Funcionalidades
O Azure AD Graph API fornece as seguintes funcionalidades:

* **Pontos finais da API REST**: Azure AD Graph API é um serviço RESTful, composto por pontos de extremidade que são acedidos através de pedidos de HTTP padrão. O Azure AD Graph API suporta tipos de conteúdo XML ou Javascript Object Notation (JSON) para solicitações e respostas. Para obter mais informações, consulte [referência da API do REST do Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Autenticação com o Azure AD**: todos os pedidos para o Azure AD Graph API tem de ser autenticado, acrescentando um JSON Web Token (JWT) no cabeçalho de autorização do pedido. Este token é obtido ao efetuar um pedido ao ponto final de token do Azure AD e ao fornecer credenciais válidas. Pode utilizar o fluxo de credenciais de cliente OAuth 2.0 ou fluxo para adquirir um token para chamar o gráfico de concessão de código de autorização. Para obter mais informações, [OAuth 2.0 no Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Autorização baseada em funções (RBAC)**: grupos de segurança são utilizados para realizar o RBAC no Azure AD Graph API. Por exemplo, se quiser determinar se um utilizador tem acesso a um recurso específico, a aplicação pode chamar o [Verifique a associação de grupo (transitiva)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) operação, que retorna VERDADEIRO ou FALSO.
* **Consulta diferencial**: consulta diferencial permite-lhe controlar as alterações num diretório entre dois períodos de tempo sem ter de realizar consultas frequentes para o Azure AD Graph API. Este tipo de pedido irá devolver apenas as alterações feitas entre o pedido de consulta diferencial anterior e a solicitação atual. Para obter mais informações, consulte [do Azure AD Graph API diferencial consulta](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Extensões de diretório**: pode adicionar propriedades personalizadas com objetos de diretório sem a necessidade de um arquivo de dados externo. Por exemplo, se a sua aplicação requeira uma propriedade de ID do Skype para cada usuário, pode registrar a nova propriedade no diretório e estará disponível para uso em todos os objetos de utilizador. Para obter mais informações, consulte [do Azure AD Graph API Extensões de esquema do](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Protegido por âmbitos de permissão**: Azure AD Graph API expõe os âmbitos de permissão que permitem acesso seguro a dados do Azure AD com o OAuth 2.0. Ele suporta uma variedade de tipos de aplicações de cliente, incluindo:
  
  * interfaces do usuário que recebem acesso delegado a dados por meio de autorização do utilizador com sessão iniciada (delegado)
  * aplicativos de serviço/daemon que operam em segundo plano sem um utilizador com sessão iniciada que estejam presentes e utilizam o controlo de acesso com base na função definida pelo aplicativo
    
    Os dois delegados e permissões de aplicação representam um privilégio exposto pela API do Azure AD Graph e podem ser solicitadas por aplicações de cliente por meio de recursos de permissões de registo de aplicação no [portal do Azure](https://portal.azure.com). [Azure AD Graph API permissão âmbitos](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) fornece informações sobre o que está disponível para utilização pela sua aplicação de cliente.

## <a name="scenarios"></a>Cenários
O Azure AD Graph API permite que muitos cenários de aplicativos. Os cenários seguintes são as mais comuns:

* **Linha de aplicativo de negócios (único inquilino)**: neste cenário, um programador empresarial funciona para uma organização que tenha uma subscrição do Office 365. O desenvolvedor está criando um aplicativo web que interage com o Azure AD para executar tarefas, tais como atribuir uma licença a um utilizador. Esta tarefa requer acesso à API do Azure AD Graph, para que o desenvolvedor registra a aplicação de inquilino único no Azure AD e configura ler e escrever as permissões para o Azure AD Graph API. Em seguida, a aplicação está configurada para utilizar as suas próprias credenciais ou aqueles do utilizador atualmente início de sessão para adquirir um token para chamar a API do Azure AD Graph.
* **Software como um aplicativo de serviço (multi-inquilino)**: neste cenário, um fabricante de software independente (ISV) está a desenvolver uma aplicação web de multi-inquilino alojado que fornece recursos de gerenciamento de usuário para outras organizações que utilizam o Azure AD. Esses recursos exigem acesso a objetos de diretório, por isso, a aplicação tem de chamar a API do Azure AD Graph. O desenvolvedor registra a aplicação no Azure AD, configura-o para exigir a leitura e escrita permissões para o Azure AD Graph API e, em seguida, ativa o acesso externo para que outras organizações podem dar consentimento para utilizar a aplicação no respetivo diretório. Quando um utilizador na sua organização efetua a autenticação à aplicação pela primeira vez, eles são apresentados uma caixa de diálogo de consentimento com as permissões que a aplicação está a solicitar. Concessão de consentimento, em seguida, irá dar ao aplicativo aqueles solicitadas permissões para o Azure AD Graph API no diretório do utilizador. Para obter mais informações sobre a estrutura de consentimento, consulte [descrição geral da estrutura de consentimento](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="see-also"></a>Consultar Também
[Guia de início rápido do Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Documentação do Azure AD Graph REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Guia para programadores do Azure Active Directory](azure-ad-developers-guide.md)

