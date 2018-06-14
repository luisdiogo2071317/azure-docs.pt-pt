---
title: Proteger uma API utilizando o OAuth 2.0 com o Azure Active Directory e a gestão de API | Microsoft Docs
description: Saiba como proteger um back-end de API web com o Azure Active Directory e a API Management.
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
ms.openlocfilehash: f5662a4082487137dfd642cc3264a90f8ab19054
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30928041"
---
# <a name="protect-an-api-by-using-oauth-20-with-azure-active-directory-and-api-management"></a>Proteger uma API utilizando o OAuth 2.0 com o Azure Active Directory e a API Management

Este guia mostra como configurar a sua instância de API Management do Azure para proteger uma API, utilizando o protocolo OAuth 2.0 no Azure Active Directory (Azure AD). 

## <a name="prerequisites"></a>Pré-requisitos
Siga os passos neste artigo, tem de ter para:
* Uma instância de API Management
* Uma API que está a ser publicada que utiliza a instância de API Management
* Um inquilino do Azure AD

## <a name="overview"></a>Descrição geral

Eis uma rápida descrição geral dos passos:

1. Registe uma aplicação (aplicação de back-end) no Azure AD para representar a API.
2. Outra aplicação (aplicação de cliente) a registar no Azure AD para representar uma aplicação cliente que precisa para chamar a API.
3. No Azure AD, conceda permissões para permitir que a aplicação de cliente chamar a aplicação de back-end.
4. Configure a consola de programador para utilizar a autorização de utilizador de OAuth 2.0.
5. Adicionar o **jwt validar** política para validar o token OAuth para cada pedido recebido.

## <a name="register-an-application-in-azure-ad-to-represent-the-api"></a>Registar uma aplicação no Azure AD para representar a API

Para proteger uma API com o Azure AD, o primeiro passo é registar uma aplicação no Azure AD que representa a API. 

1. Navegue até ao seu inquilino do Azure AD e, em seguida, procure **registos de aplicação**.

2. Selecione **Novo registo de aplicação**. 

3. Forneça um nome da aplicação. (Este exemplo, o nome é `backend-app`.)  

4. Escolha **aplicação Web / API** como o **tipo de aplicação**. 

5. Para **URL de início de sessão**, pode utilizar `https://localhost` como um marcador de posição.

6. Selecione **Criar**.

Quando a aplicação é criada, anote o **ID da aplicação**, para utilização num passo subsequente. 

## <a name="register-another-application-in-azure-ad-to-represent-a-client-application"></a>Registar outra aplicação no Azure AD para representar uma aplicação cliente

Tem de ser registado como uma aplicação no Azure AD, bem como todas as aplicações de cliente que chama a API. Neste exemplo, a aplicação de cliente de exemplo é a consola de Programador no portal do Programador de API Management. Eis como registar outra aplicação no Azure AD para representar a consola de programador.

1. Selecione **Novo registo de aplicação**. 

2. Forneça um nome da aplicação. (Este exemplo, o nome é `client-app`.)

3. Escolha **aplicação Web / API** como o **tipo de aplicação**.  

4. Para **URL de início de sessão**, pode utilizar `https://localhost` como um marcador de posição ou um URL de utilizar o início de sessão da sua instância da API Management. (Este exemplo, o URL é `https://contoso5.portal.azure-api.net/signin`.)

5. Selecione **Criar**.

Quando a aplicação é criada, anote o **ID da aplicação**, para utilização num passo subsequente. 

Agora, crie um segredo do cliente para esta aplicação, para utilização num passo subsequente.

1. Selecione **definições** novamente e aceda a **chaves**.

2. Em **palavras-passe**, forneça um **descrição da chave**. Escolher quando a chave deve expirar e selecione **guardar**.

Anote o valor da chave. 

## <a name="grant-permissions-in-azure-ad"></a>Conceder permissões no Azure AD

Agora que registou duas aplicações para representar a API e a consola de programador, tem de conceder permissões para permitir que a aplicação de cliente chamar a aplicação de back-end.  

1. Navegue até à **registos de aplicação**. 

2. Selecione `client-app`e aceda a **definições**.

3. Selecione **as permissões necessárias** > **adicionar**.

4. Selecione **selecionar um API**e procure `backend-app`.

5. Em **permissões delegadas**, selecione `Access backend-app`. 

6. Selecione **selecione**e, em seguida, selecione **feito**. 

> [!NOTE]
> Se **do Azure Active Directory** não está listado em permissões para outras aplicações, selecionadas **adicionar** adicioná-lo a partir da lista.
> 
> 

## <a name="enable-oauth-20-user-authorization-in-the-developer-console"></a>Ativar a autorização de utilizador de OAuth 2.0 na consola de programador

Neste momento, pode criar as suas aplicações no Azure AD e conceder as permissões adequadas para permitir que a aplicação de cliente chamar a aplicação de back-end. 

Neste exemplo, a consola de programador é a aplicação de cliente. Os passos seguintes descrevem como ativar a autorização de utilizador de OAuth 2.0 na consola de programador. 

1. Navegue até à sua instância da API Management.

2. Selecione **OAuth 2.0** > **adicionar**.

3. Forneça um **nome a apresentar** e **Descrição**.

4. Para o **URL de página de registo de cliente**, introduza um valor do marcador de posição, como `http://localhost`. O **URL de página de registo de cliente** aponta para a página que os utilizadores podem utilizar para criar e configurar as suas próprias contas para fornecedores de OAuth 2.0 que suportem este. Neste exemplo, os utilizadores não criar e configurar as suas próprias contas, para utilizar em vez disso, um marcador de posição.

5. Para **tipos de conceder de autorização**, selecione **código de autorização**.

6. Especifique o **URL de ponto final de autorização** e **URL de ponto final do Token**. Obter estes valores no **pontos finais** página no inquilino do Azure AD. Navegue para o **registos de aplicação** novamente a página e selecione **pontos finais**.

7. Copiar o **ponto final de autorização do OAuth 2.0**e cole-o para o **URL de ponto final de autorização** caixa de texto.

8. Copiar o **ponto final de tokens do OAuth 2.0**e cole-o para o **URL de ponto final do Token** caixa de texto. Para além de colar no ponto final do token, adicionar um parâmetro de corpo denominado **recursos**. Para o valor deste parâmetro, utilize o **ID da aplicação** para a aplicação de back-end.

9. Em seguida, especifique as credenciais de cliente. Estas são as credenciais para a aplicação de cliente.

10. Para **ID de cliente**, utilize o **ID da aplicação** para a aplicação de cliente.

11. Para **segredo do cliente**, utilize a chave que criou para a aplicação cliente anteriormente. 

12. Imediatamente a seguir o segredo do cliente é a **redirect_url** para a autorização de tipo de concessão de códigos. Tome nota deste URL.

13. Selecione **Criar**.

14. Volte à **definições** página da sua aplicação de cliente.

15. Selecione **URLs de resposta**e cole o **redirect_url** na primeira linha. Neste exemplo, substituiu `https://localhost` com o URL na primeira linha.  

Agora que configurou um servidor de autorização do OAuth 2.0, a consola de programador pode obter os tokens de acesso do Azure AD. 

O passo seguinte consiste em Ativar autorização de utilizador de OAuth 2.0 para a API. Isto permite que a consola de programador para saber o que precisa de obter um token de acesso em nome do utilizador, antes de efetuar chamadas à sua API.

1. Navegue até à sua instância da API Management e aceda a **APIs**.

2. Selecione a API que pretende proteger. Neste exemplo, utilize o `Echo API`.

3. Aceda a **definições**.

4. Em **segurança**, escolha **OAuth 2.0**e selecione o servidor de OAuth 2.0 configurados anteriormente. 

5. Selecione **Guardar**.

## <a name="successfully-call-the-api-from-the-developer-portal"></a>Chamar com êxito a API a partir do portal do Programador

Agora que a autorização de utilizador de OAuth 2.0 está ativada no `Echo API`, a consola de programador obtém um token de acesso em nome do utilizador, antes de chamar a API.

1. Procurar a qualquer operação sob o `Echo API` no programador portal e selecione **experimente**. Isto proporciona para a consola de programador.

2. Tenha em atenção um novo item no **autorização** secção, correspondente para o servidor de autorização que acabou de adicionar.

3. Selecione **código de autorização** da autorização na lista pendente e, são-lhe pedido que inicie sessão no inquilino do Azure AD. Se já tem sessão iniciada com a conta, poderá não ser lhe.

4. Após o início de sessão com êxito, um `Authorization` cabeçalho é adicionado ao pedido, com um token de acesso do Azure AD. Segue-se um token de exemplo (codificação Base64):

   ```
   Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCIsImtpZCI6IlNTUWRoSTFjS3ZoUUVEU0p4RTJnR1lzNDBRMCJ9.eyJhdWQiOiIxYzg2ZWVmNC1jMjZkLTRiNGUtODEzNy0wYjBiZTEyM2NhMGMiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC80NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgvIiwiaWF0IjoxNTIxMTUyNjMzLCJuYmYiOjE1MjExNTI2MzMsImV4cCI6MTUyMTE1NjUzMywiYWNyIjoiMSIsImFpbyI6IkFWUUFxLzhHQUFBQUptVzkzTFd6dVArcGF4ZzJPeGE1cGp2V1NXV1ZSVnd1ZXZ5QU5yMlNkc0tkQmFWNnNjcHZsbUpmT1dDOThscUJJMDhXdlB6cDdlenpJdzJLai9MdWdXWWdydHhkM1lmaDlYSGpXeFVaWk9JPSIsImFtciI6WyJyc2EiXSwiYXBwaWQiOiJhYTY5ODM1OC0yMWEzLTRhYTQtYjI3OC1mMzI2NTMzMDUzZTkiLCJhcHBpZGFjciI6IjEiLCJlbWFpbCI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiSmlhbmciLCJnaXZlbl9uYW1lIjoiTWlhbyIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJpcGFkZHIiOiIxMzEuMTA3LjE3NC4xNDAiLCJuYW1lIjoiTWlhbyBKaWFuZyIsIm9pZCI6IjhiMTU4ZDEwLWVmZGItNDUxMS1iOTQzLTczOWZkYjMxNzAyZSIsInNjcCI6InVzZXJfaW1wZXJzb25hdGlvbiIsInN1YiI6IkFGaWtvWFk1TEV1LTNkbk1pa3Z3MUJzQUx4SGIybV9IaVJjaHVfSEM1aGciLCJ0aWQiOiI0NDc4ODkyMC05Yjk3LTRmOGItODIwYS0yMTFiMTMzZDk1MzgiLCJ1bmlxdWVfbmFtZSI6Im1pamlhbmdAbWljcm9zb2Z0LmNvbSIsInV0aSI6ImFQaTJxOVZ6ODBXdHNsYjRBMzBCQUEiLCJ2ZXIiOiIxLjAifQ.agGfaegYRnGj6DM_-N_eYulnQdXHhrsus45QDuApirETDR2P2aMRxRioOCR2YVwn8pmpQ1LoAhddcYMWisrw_qhaQr0AYsDPWRtJ6x0hDk5teUgbix3gazb7F-TVcC1gXpc9y7j77Ujxcq9z0r5lF65Y9bpNSefn9Te6GZYG7BgKEixqC4W6LqjtcjuOuW-ouy6LSSox71Fj4Ni3zkGfxX1T_jiOvQTd6BBltSrShDm0bTMefoyX8oqfMEA2ziKjwvBFrOjO0uK4rJLgLYH4qvkR0bdF9etdstqKMo5gecarWHNzWi_tghQu9aE3Z3EZdYNI_ZGM-Bbe3pkCfvEOyA
   ```

5. Selecione **enviar**, e pode chamar a API com êxito.


## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Configurar a política de validação de JWT para autorizar previamente pedidos

Neste momento, quando um utilizador tenta efetuar uma chamada a partir da consola do programador, é pedido ao utilizador para iniciar sessão. A consola de programador obtém um token de acesso em nome do utilizador.

Mas e se alguém chama a API sem um token ou com um token inválido? Por exemplo, pode ainda chamar a API, mesmo se eliminar o `Authorization` cabeçalho. O motivo é que a gestão de API não valida o token de acesso neste momento. Simplesmente, transmite o `Authorization` cabeçalho para a API de back-end.

Pode utilizar o [validar JWT](api-management-access-restriction-policies.md#ValidateJWT) política para autorizar previamente pedidos na API Management, ao validar os tokens de acesso de cada pedido recebido. Se um pedido não tem um token válido, a API Management bloqueia-lo. Por exemplo, pode adicionar a seguinte política para o `<inbound>` secção política a `Echo API`. Verifica a afirmação de audiência num token de acesso e devolve uma mensagem de erro se o token não é válido. Para obter informações sobre como configurar políticas, consulte [definir ou editar políticas](set-edit-policies.md).

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

Neste guia, utilizou a consola de programador na API Management como exemplo de aplicação de cliente para chamar o `Echo API` protegidos pelo OAuth 2.0. Para obter mais informações sobre como criar uma aplicação e implementa o OAuth 2.0, consulte [exemplos de código do Azure Active Directory](../active-directory/develop/active-directory-code-samples.md).

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [Azure Active Directory e OAuth2.0](../active-directory/develop/active-directory-authentication-scenarios.md).
* Consulte mais [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre a API Management.
* Para outras formas de proteger o seu serviço de back-end, consulte [autenticação de certificados mútuos](api-management-howto-mutual-certificates.md).

* [Criar uma instância de serviço de API Management](get-started-create-service-instance.md).

* [Gerir a sua primeira API](import-and-publish.md).
