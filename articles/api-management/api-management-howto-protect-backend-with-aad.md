---
title: Proteger uma API utilizando o OAuth 2.0 com o Azure Active Directory e a gestão de API | Microsoft Docs
description: Saiba como proteger um back-end de Web API com o Azure Active Directory e a API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2018
ms.author: apimpm
ms.openlocfilehash: 2c05407d761a8848f9e032aa219960cd7ea6fa93
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-protect-an-api-using-oauth-20-with-azure-active-directory-and-api-management"></a>Como proteger uma API utilizando o OAuth 2.0 com o Azure Active Directory e a API Management

Este guia mostra como configurar a sua instância de gestão de API (APIM) para proteger uma API utilizando o protocolo OAuth 2.0 com o Azure Active Directory (AAD). 

## <a name="prerequisite"></a>Pré-requisito
Siga os passos neste artigo, tem de ter para:
* Uma instância APIM
* Uma API que está a ser publicado utilizando a instância APIM
* Um inquilino do Azure AD

## <a name="overview"></a>Descrição geral

Este guia mostra como proteger uma API com OAuth 2.0 no APIM. Neste artigo, é utilizado o Azure AD como o servidor de autorização (OAuth servidor). Segue-se uma rápida descrição geral dos passos:

1. Registar uma aplicação (aplicação de back-end) no Azure AD para representar a API
2. Registar outra aplicação (aplicação de cliente) no Azure AD para representar uma aplicação cliente que precisa para chamar a API
3. No Azure AD, conceder permissões para permitir que a aplicação de cliente chamar a aplicação de back-end
4. Configurar a consola de programador para utilizar a autorização de utilizador de OAuth 2.0
5. Adicionar política jwt validar para validar o token OAuth para cada pedido recebido

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Registar uma aplicação no Azure AD para representar a API

Para proteger uma API com o Azure AD, o primeiro passo é registar uma aplicação no Azure AD que representa a API. 

Navegue para o inquilino do Azure AD, em seguida, navegue para **registos de aplicação**.

Selecione **novo registo de aplicação**. 

Forneça um nome da aplicação. Neste exemplo, `backend-app` é utilizado.  

Escolha **aplicação Web / API** como o **tipo de aplicação**. 

Para **URL de início de sessão**, pode utilizar `https://localhost` como um marcador de posição.

Clique em **criar**.

Assim que a aplicação é criada, anote o **ID da aplicação** para utilização num passo subsequente. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Registar outra aplicação no Azure AD para representar uma aplicação cliente

Tem de ser registado como uma aplicação no Azure AD, bem como todas as aplicações de cliente que precisa para chamar a API. Neste guia, irá utilizamos a consola de Programador no Portal do programador APIM como exemplo de aplicação de cliente. 

É necessário registar outra aplicação no Azure AD para representar a consola de programador.

Clique em **novo registo de aplicação** novamente. 

Forneça um nome da aplicação e escolha **aplicação Web / API** como o **tipo de aplicação**. Neste exemplo, `client-app` é utilizado.  

Para **URL de início de sessão**, pode utilizar `https://localhost` como um marcador de posição ou utilizar o URL de início de sessão da sua instância APIM. Neste exemplo, `https://contoso5.portal.azure-api.net/signin` é utilizado.

Clique em **criar**.

Assim que a aplicação é criada, anote o **ID da aplicação** para utilização num passo subsequente. 

Agora, é necessário criar um segredo do cliente para esta aplicação para utilização num passo subsequente.

Clique em **definições** novamente e aceda a **chaves**.

Em **palavras-passe**, forneça um **descrição da chave**, escolha quando a chave deve expirar e clique em **guardar**.

Anote o valor da chave. 

## <a name="grant-permissions-in-aad"></a>Conceder permissões no AAD

Agora vamos registou duas aplicações para representar a API (ou seja, back-end da aplicação) e a consola de programador (ou seja,-as aplicações cliente), é necessário conceder permissões para permitir que a aplicação de cliente chamar a aplicação de back-end.  

Navegue para **registos de aplicação** novamente. 

Clique em `client-app` e aceda a **definições**.

Clique em **as permissões necessárias** e, em seguida, **adicionar**.

Clique em **selecionar um API** e procure `backend-app`.

Verifique `Access backend-app` em **permissões delegadas**. 

Clique em **selecione** e, em seguida, **feito**. 

> [!NOTE]
> Se **Windows** **do Azure Active Directory** não é listados na permissões para outras aplicações, clique em **adicionar** e adicioná-la a partir da lista.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>Ativar a autorização de utilizador de OAuth 2.0 na consola de programador

Neste momento, estamos a ter criado as nossas aplicações no Azure AD e conceder permissões adequadas para permitir que a aplicação de cliente chamar a aplicação de back-end. 

Neste guia, utilizaremos a consola de programador do que a aplicação de cliente. Passos abaixo descrevem como ativar a autorização do OAuth 2.0 utilizador na consola de programador 

Navegue até à sua instância APIM.

Clique em **OAuth 2.0** e, em seguida, **adicionar**.

Forneça um **nome a apresentar** e **Descrição**.

Para o registo de cliente URL, da página * * Introduza um valor do marcador de posição como `http://localhost`.  O **URL de página de registo de cliente** aponta para a página que os utilizadores podem utilizar para criar e configurar as suas próprias contas para fornecedores de OAuth 2.0 que suportem gestão de contas de utilizadores. Neste exemplo, os utilizadores não criar e configurar as suas próprias contas, pelo que é utilizado um marcador de posição.

Verifique **código de autorização** como o **tipos de conceder de autorização**.

Em seguida, especifique **URL de ponto final de autorização** e **URL de ponto final do Token**.

Estes valores podem ser obtidos a partir de **pontos finais** página no inquilino do Azure AD. Para aceder os pontos finais, navegue para o **registos de aplicação** novamente a página e clique em **pontos finais**.

Copiar o **ponto final de autorização do OAuth 2.0** e cole-o para o **URL de ponto final de autorização** caixa de texto.

Copiar o **ponto final de tokens do OAuth 2.0** e cole-o para o **URL de ponto final do Token** caixa de texto.

Para além de colar no ponto final do token, adicionar um parâmetro de corpo adicionais com o nome **recursos** e a utilização de valor de **ID da aplicação** para a aplicação de back-end.

Em seguida, especifique as credenciais de cliente. Estas são as credenciais para a aplicação de cliente.

Para **Id de cliente**, utilize o **ID da aplicação** para a aplicação de cliente.

Para **segredo do cliente**, utilize a chave que criou para a aplicação cliente anteriormente. 

Imediatamente a seguir o segredo do cliente é a **redirect_url** para a autorização de tipo de concessão de códigos.

Tome nota deste URL.

Clique em **criar**.

Navegue de volta para o **definições** página da sua aplicação de cliente.

Clique em **URLs de resposta** e cole o **redirect_url** na primeira linha. Neste exemplo, é substituído `https://localhost` com o URL na primeira linha.  

Agora iremos configurar um servidor de autorização do OAuth 2.0, a consola de programador deverá conseguir obter os tokens de acesso do Azure AD. 

O passo seguinte consiste em Ativar autorização de utilizador de OAuth 2.0 para a nossa API, para que a consola de programador sabe tem de obter um token de acesso em nome do utilizador antes de efetuar chamadas à nossa API.

Navegue até à sua instância APIM, aceda a **APIs**.

Clique na API que pretende proteger. Neste exemplo, utilizaremos o `Echo API`.

Aceda a **definições**.

Em **segurança**, escolha **OAuth 2.0** e selecione o servidor de OAuth 2.0 que configurámos anteriormente. 

Clique em **Guardar**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Chamar com êxito a API a partir do portal do Programador

Agora que a autorização de utilizador de OAuth 2.0 está ativada no `Echo API`, a consola de programador irá obter um token de acesso em nome do utilizador antes de chamar a API.

Navegue para todas as operações no `Echo API` no Portal do programador e clique **experimente**, que será apresentada-nos para a consola de programador.

Tenha em atenção um novo item no **autorização** secção correspondente para o servidor de autorização que acabou de adicionar.

Selecione **código de autorização** da autorização na lista pendente e será pedido que inicie sessão no inquilino do Azure AD. Se já tem sessão iniciada com a conta, poderá não ser lhe.

Após o início de sessão com êxito, um `Authorization` cabeçalho será adicionado ao pedido com um token de acesso do Azure AD. 

Parece um token de exemplo abaixo, é codificado em Base64.

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
```

Clique em **enviar** e deve ser capaz de chamar a API com êxito.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configurar a política de validação de JWT para autorizar previamente pedidos

Neste momento, quando um utilizador tenta efetuar uma chamada a partir da consola do programador, o utilizador será avisado para iniciar sessão e a consola de programador irá obter um Token de acesso em nome do utilizador. Tudo está a funcionar conforme esperado. No entanto, e se alguém chama nossa API sem um token ou com um token inválido? Por exemplo, pode tentar eliminar o `Authorization` cabeçalho e irá encontrar ainda consegue chamar a API. O motivo é porque APIM não valida o Token de acesso neste momento. Simplesmente, transmite o `Auhtorization` cabeçalho para o API de back-end.

Podemos utilizar o [validar JWT](api-management-access-restriction-policies.md#ValidateJWT) política para autorizar previamente os pedidos APIM ao validar os tokens de acesso de cada pedido recebido. Se um pedido não tem um token válido, é bloqueada pela API Management e não é transmitido para o back-end. Por exemplo, vamos adicionar o abaixo política para o `<inbound>` secção política do `Echo API`. Verifica a afirmação de audiência num token de acesso e devolve uma mensagem de erro se o token não é válido. Para obter informações sobre como configurar políticas, consulte [definir ou editar políticas](set-edit-policies.md).

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/{aad-tenant}/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>{Application ID of backend-app}</value>
        </claim>
    </required-claims>
</validate-jwt>
```

## <a name="build-an-application-to-call-the-api"></a>Criar uma aplicação para chamar a API

Neste guia, utilizamos a consola de programador APIM como exemplo de aplicação de cliente para chamar o `Echo API` protegidos pelo OAuth 2.0. Para obter mais informações sobre como criar uma aplicação e implementa o fluxo de OAuth 2.0, consulte [exemplos de código do Azure Active Directory](../active-directory/develop/active-directory-code-samples.md).

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [Azure Active Directory e OAuth2.0](../active-directory/develop/active-directory-authentication-scenarios.md)
* Consulte mais [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre a API Management.
* Para outras formas de proteger o seu serviço de back-end, consulte [autenticação de certificados mútuos](api-management-howto-mutual-certificates.md).

[Create an API Management service instance]: get-started-create-service-instance.md
[Manage your first API]: import-and-publish.md
