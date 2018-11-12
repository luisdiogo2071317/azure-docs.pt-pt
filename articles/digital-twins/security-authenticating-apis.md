---
title: Compreender a autenticação de API de duplos Digital do Azure | Documentos da Microsoft
description: Utilizar duplos Digital do Azure para ligar e autenticar para APIs
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: f85ab05e785ea559962490b43e75b196d1602159
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016221"
---
# <a name="connect-and-authenticate-to-apis"></a>Ligar e autenticar para APIs

Duplos Digital do Azure utiliza o Azure Active Directory (Azure AD) para autenticar os utilizadores e proteger as aplicações. O Azure AD suporta a autenticação para uma variedade de modernas arquiteturas. Todos eles são baseiam-se os protocolos de norma da indústria OAuth 2.0 ou OpenID Connect. Além disso, os programadores podem utilizar o Azure AD para a criação de inquilino único e linha de negócio (LOB). Os desenvolvedores também podem utilizar o Azure AD para desenvolver aplicações multi-inquilino.

Para obter uma descrição geral do Azure AD, visite o [página de conceitos básicos](https://docs.microsoft.com/azure/active-directory/fundamentals/index) para guias passo a passo, conceitos e guias de introdução.

Para integrar uma aplicação ou serviço com o Azure AD, primeiro, o programador deve registar a aplicação no Azure AD. Para obter instruções detalhadas e capturas de ecrã, consulte [este guia de introdução](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

[Cinco cenários de aplicação principal](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) são suportadas pelo Azure AD:

* Aplicação de página única (SPA): o utilizador precisa de iniciar sessão a uma aplicação de página única que está protegida pelo Azure AD.
* Navegador da Web para a aplicação web: o utilizador precisa de iniciar sessão a uma aplicação web que está protegida pelo Azure AD.
* Aplicação nativa a web API: um aplicativo nativo que é executado num telefone, tablet ou PC tem de autenticar um usuário para obter recursos de uma API web que está protegido pelo Azure AD.
* Aplicação Web API Web: uma aplicação web tem de obter recursos de uma API web protegida pelo Azure AD.
* O daemon ou servidor de aplicativo web API: uma aplicação de daemon ou um aplicativo de servidor com nenhuma IU da web tem de obter recursos de uma API web protegida pelo Azure AD.

A biblioteca de autenticação do Windows Azure oferece várias formas de adquirir os tokens de Active Directory. Para obter detalhes sobre os exemplos de código e de biblioteca, consulte [este artigo](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Chamar duplos Digital a partir de uma API da web de camada intermediária

Quando os desenvolvedores arquitetar soluções digitais duplos, eles normalmente, criaram uma aplicação de camada intermediária ou a API. A aplicação ou a API em seguida, chama a API de duplos Digital downstream. Os usuários primeiro autenticar para o aplicativo de camada média e, em seguida, um fluxo em-nome-de token é utilizado para chamar a jusante. Para obter instruções sobre como orquestrar o fluxo em-nome-de, veja [esta página](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Também pode ver exemplos de código na [esta página](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).


## <a name="test-with-the-postman-client"></a>Testar com o cliente do Postman

Para começar a trabalhar com as APIs de duplos Digital, pode utilizar um cliente, como o Postman como ambiente de API. Postman ajuda-o a criar pedidos HTTP complexos rapidamente. Os passos seguintes mostram como obter um token do Azure AD que é necessário chamar duplos digitais dentro da interface do Usuário do Postman.


1. Aceda a https://www.getpostman.com/ para transferir a aplicação.
1. Siga os passos em [este guia de introdução](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) para criar uma aplicação do Azure AD. Ou pode reutilizar um registo existente. 
1. Sob **permissões obrigatórias**, introduza "Duplos Digital do Azure" e selecione **permissões delegadas**. Em seguida, selecione **conceder permissões**.
1. Abra o manifesto do aplicativo e defina **oauth2AllowImplicitFlow** como true.
1. Configurar um URL de resposta para [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
1. Selecione o **autorização** separador, selecione **OAuth 2.0**e, em seguida, selecione **obter novo Token de acesso**.

    |**Campo**  |**Valor** |
    |---------|---------|
    | Tipo de concessão | Implícito |
    | Url de chamada de retorno | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Auth URL | https://login.microsoftonline.com/<Your Azure AD Tenant e.g. Contoso>.onmicrosoft.com/oauth2/Authorize?Resource=0b07f429-9F4B-4714-9392-cc5e8e80c8b0 |
    | ID de Cliente | Utilize o ID da aplicação para a aplicação do Azure AD que tenha sido criada ou redirecionado para outros objetivos do passo 2. |
    | Âmbito | Deixe em branco. |
    | Estado | Deixe em branco. |
    | Autenticação de cliente | Envie como um cabeçalho de autenticação básica. |

1. Selecione **pedir o Token**.

    >[!NOTE]
    >Se receber a mensagem de erro "Não foi possível concluir o OAuth 2", experimente o seguinte:
    > * Feche o Postman e abri-lo novamente e tente novamente.
   
1. Role para baixo e selecione **utilização Token**.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre segurança de duplos Digital do Azure, leia [criar e gerir atribuições de funções](./security-create-manage-role-assignments.md).
