---
title: Autenticação de compreensão de API de duplos Digital do Azure | Documentos da Microsoft
description: Utilização duplos Digital do Azure para ligar e autenticar para APIs
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: dc5570b188bfdc0e1be78aa2bd5c5d92e884f377
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638023"
---
# <a name="connect-and-authenticate-to-apis"></a>Ligar e autenticar para APIs

Duplos Digital do Azure utiliza o Azure Active Directory (Azure AD) para autenticar os utilizadores e proteger as aplicações. O Azure AD suporta a autenticação para uma variedade de modernas arquiteturas, todos eles com base em protocolos de norma da indústria OAuth 2.0 ou OpenID Connect. Além disso, o Azure AD permite que os desenvolvedores que criam tanto inquilino único, linha de negócio (LOB) aplicativos, bem como os desenvolvedores que desejam para desenvolver aplicações multi-inquilino.

Para obter uma descrição geral de visita do Azure AD a [página de conceitos básicos](https://docs.microsoft.com/azure/active-directory/fundamentals/index) para guias passo a passo, conceitos e guias de introdução.

Para integrar uma aplicação ou serviço com o Azure AD, primeiro, o programador deve registar a aplicação no Azure AD. Para obter instruções detalhadas e capturas de tela ver as instruções [aqui](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)

Estes são os [cinco cenários de aplicação principal](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) suportado pelo Azure AD:

* Aplicação de página única (SPA): o utilizador precisa de iniciar sessão a uma aplicação de página única que está protegida pelo Azure AD.
* Navegador da Web para a aplicação web: o utilizador precisa de iniciar sessão a uma aplicação web que está protegida pelo Azure AD.
* Aplicação nativa a web API: um aplicativo nativo que é executado num telefone, tablet ou PC tem de autenticar um usuário para obter recursos de uma API web que está protegida pelo Azure AD.
* Aplicação Web API Web: uma aplicação web tem de obter recursos de uma API web protegida pelo Azure AD.
* O daemon ou servidor de aplicativo web API: precisa de um aplicativo de daemon ou um aplicativo de servidor sem interface do usuário de web obter recursos de uma API web protegida pelo Azure AD.

A biblioteca de autenticação do Windows Azure oferece várias formas de adquirir os tokens de Active Directory. Para um aprofundamento sobre a biblioteca, bem como código, exemplos de dar uma olhada [aqui](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="calling-digital-twins-from-a-middle-tier-web-api"></a>Chamar duplos digitais de uma API da web de camada intermediária

Ao desenvolver a arquitetura os desenvolvedores de soluções digitais Twins comumente optar por criar uma aplicação de camada intermediária ou a API que, em seguida, chama a API de duplos Digital de downstream. Os utilizadores seriam primeiro autenticar para o aplicativo de camada média e, em seguida, um fluxo de token em-nome-de seria usado ao chamar a jusante. Para obter instruções detalhadas sobre como orquestrar o fluxo em-nome-de visite [esta página](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Também pode ver exemplos de código [aqui](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).


## <a name="test-with-the-postman-client"></a>Testar com o cliente do Postman

Para começar a trabalhar com as APIs de duplos Digital, pode utilizar o cliente, como o Postman como ambiente de API. Postman ajuda-o a criar pedidos HTTP complexos rapidamente. As instruções abaixo irão focar-se sobre como obter um token do Azure AD necessário para chamar duplos digitais dentro da interface do Usuário do Postman.


1. Navegue para https://www.getpostman.com/ para transferir a aplicação
1. Siga os passos [aqui](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) para criar uma aplicação do Azure Active Directory (ou pode optar por utilizar um registo existente novamente). 
1. Em permissões necessárias, adicione "Duplos Digital do Azure" e selecione permissões delegadas. Não se esqueça de clicar em conceder permissões para finalizar.
1. Abra o manifesto do aplicativo e defina oauth2AllowImplicitFlow como verdadeiro
1. Configurar um url de resposta para [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
1. Selecione o **separador de autorização**, clique em **OAuth 2.0**e selecione **obter novo Token de acesso**.

    |**Campo**  |**Valor** |
    |---------|---------|
    | Tipo de Concessão | Implícito |
    | Url de chamada de retorno | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Auth URL | [https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0](https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0)
    | ID de Cliente | Utilizar o Id da aplicação para a aplicação do Azure AD que foi criada ou redirecionado para outros objetivos do passo 1 |
    | Âmbito | Deixe em branco |
    | Estado | Deixe em branco |
    | Autenticação de Cliente | Enviar como cabeçalho de autenticação básica |

1. Clique em **pedir o Token**.

    >[!NOTE]
    >Se receber a mensagem de erro "Não foi possível concluir o OAuth 2", experimente o seguinte:
    > * Feche o Postman e abri-lo novamente e tente novamente.
   
1. Desloque para baixo e clique em **utilização Token**.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre segurança de duplos Digital do Azure, leia [criar e gerir atribuições de funções](./security-create-manage-role-assignments.md).
