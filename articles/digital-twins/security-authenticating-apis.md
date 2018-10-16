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
ms.openlocfilehash: 1d5b1869428cec6bf80b8518485f685e38ad5997
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324204"
---
# <a name="connect-and-authenticate-to-apis"></a>Ligar e autenticar para APIs

Duplos Digital do Azure utiliza o Azure Active Directory (Azure AD) para autenticar os utilizadores e proteger as aplicações.

Se não estiver familiarizado com o Azure AD, estão disponíveis em mais informações a [Guia do programador](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-developers-guide). A biblioteca de autenticação do Windows Azure oferece várias formas de adquirir os tokens de Active Directory. Para obter uma análise detalhada para a biblioteca, dê uma olhada [aqui](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

Este artigo fornece uma visão geral dos dois cenários: um cenário de produção com uma API de camada intermediária e a autenticação no aplicativo cliente do Postman para inicialização rápida e teste.

## <a name="authentication-in-production"></a>Autenticação na produção

Os desenvolvedores de soluções têm duas formas de ligar-se para Digital Twins.  Os desenvolvedores de soluções podem ligar-se para Twins Digital do Azure das seguintes formas:

* É possível criar uma aplicação cliente ou uma API de camada intermediária. Aplicações de cliente exigirem que os utilizadores autenticar e, em seguida, utilizar o [OAuth 2.0 On-Behalf-Of](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) fluxo de segurança para chamar uma API de downstream.
* Criar ou utilizar uma aplicação do AD do Azure existente. Ver a documentação [aqui](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad).
    1. Especifique a **início de sessão e URIs de redirecionamento** (se necessário).
    1. No aplicativo manifesto conjunto `oauth2AllowImplicitFlow` como true.
    1. Na **permissões obrigatórias**, adicionar duplos Digital ao pesquisar "Duplos Digital do Azure." Selecione **acesso de leitura/gravação de permissões delegada** e clique nas **conceder permissões** botão.

Para obter instruções detalhadas sobre como orquestrar o fluxo em-nome-de visite [esta página](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Também pode ver exemplos de código [aqui](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).

## <a name="test-with-the-postman-client"></a>Testar com o cliente do Postman

1. Siga as etapas iniciais acima para criar (ou modificar) uma aplicação do Azure Active Directory. Em seguida, defina `oauth2AllowImplicitFlow` como true no manifesto do aplicativo e conceder permissões para "Duplos Digital do Azure."
1. Definir um url de resposta [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
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

    >[!TIP]
    >Se receber a mensagem de erro "Não foi possível concluir o OAuth 2", experimente um dos seguintes:
    > * Feche o Postman e abri-lo novamente e tente novamente.
    > * Eliminar a chave secreta na sua aplicação, recriar uma nova e volte a introduzir o valor no formulário acima.

1. Desloque para baixo e clique em **utilização Token**.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre segurança de duplos Digital do Azure, leia [criar e gerir atribuições de funções](./security-create-manage-role-assignments.md).
