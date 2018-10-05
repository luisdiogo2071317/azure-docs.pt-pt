---
title: Venda de SaaS através do Azure — APIs | Documentos da Microsoft
description: Explica como criar uma oferta SaaS através de APIs do marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 0368d9822df193fbf00d8a2069108e23100a58cd
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810594"
---
<a name="saas-sell-through-azure---apis"></a>Venda de SaaS através do Azure - APIs
==============================

Este artigo explica como criar uma oferta SaaS com as APIs. As APIs são necessárias para permitir que as assinaturas para sua oferta de SaaS, se tiver de venda através do Azure selecionada. Se quiser criar uma listagem de SaaS regular que não tenha commerce ativado, veja [SaaS Application Technical Guide]./cloud-partner-portal-saas-offers-tech-publishing-guide.md de publicação).

Este artigo está dividido em duas seções:

-   Autenticação serviço a serviço entre um serviço SaaS e o Azure Marketplace
-   Métodos da API e os pontos finais

As seguintes APIs são fornecidas para o ajudar a integrar o serviço SaaS com o Azure:

-   Resolver
-   Subscrever
-   Converter
-   Anular a subscrição

O diagrama seguinte mostra o fluxo de subscrição de um novo cliente e quando essas APIs são utilizadas:

![SaaS oferecem o fluxo de API](media/saas-offer-publish-with-subscription-apis/saas-offer-publish-api-flow.png)

<a name="service-to-service-authentication-between-saas-service-and-azure-marketplace"></a>Serviço de autenticação de serviço entre o serviço SaaS e o Azure marketplace
----------------------------------------------------------------------------

Azure não impõe quaisquer restrições a autenticação que o serviço SaaS expõe a seus usuários finais. No entanto, quando se trata o serviço de SaaS a comunicar com APIs do Azure Marketplace, a autenticação é feita no contexto de uma aplicação do Azure Active Directory (Azure AD).

A seguinte secção descreve como criar uma aplicação do Azure AD.

### <a name="register-an-azure-ad-application"></a>Registar uma aplicação do Azure AD

Qualquer aplicação que pretenda utilizar as capacidades do Azure AD, tem de estar registada previamente no inquilino do Azure AD. Este processo de registro envolve a fornecer os detalhes do Azure AD sobre a sua aplicação, como a URL onde este se encontra, o URL para enviar respostas após um utilizador é autenticado, o URI que identifica a aplicação e assim por diante.

Para registar uma nova aplicação com o portal do Azure, execute os seguintes passos:

1.  Inicie sessão no [Portal do Azure](https://portal.azure.com/).
2.  Se a sua conta permitir aceder a mais de uma, clique na sua conta no canto superior direito e defina a sua sessão no portal para o inquilino pretendido do Azure AD.
3.  No painel de navegação do lado esquerdo, clique nas **do Azure Active Directory** do serviço, clique em **registos das aplicações**e clique em **novo registo de aplicação**.

    ![Registos de aplicações do AD de SaaS](media/saas-offer-publish-with-subscription-apis/saas-offer-app-registration.png)

4.  Na página de criar, introduza o seu aplicativo\'informações de registo de s:
    -   **Nome**: introduza um nome de aplicação com significado
    -   **Tipo de aplicação**: selecione **aplicação Web / API** para [aplicativos de cliente](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) e [aplicações de API derecursos/](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) que são instaladas num servidor seguro. Esta definição é utilizada para OAuth confidencial [clientes web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) e públicas [baseada no utilizador-agente clientes](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client).
        A mesma aplicação também pode expor um cliente e um recurso/API.
    -   **URL de início de sessão**: aplicativos de aplicação/API para a Web, indique o URL base da sua aplicação. Por exemplo, **http://localhost:31544** pode ser o URL para uma aplicação web em execução no seu computador local. Os utilizadores, em seguida, usaria este URL para iniciar sessão a uma aplicação de cliente da web.
    -   **URI de redirecionamento**: aplicações para o nativas, forneça o URI utilizado pelo Azure AD para devolver respostas token. Introduza um valor específico para seu aplicativo, por exemplo **http://MyFirstAADApp**.

        Para obter exemplos específicos de aplicações web ou de aplicativos nativos, check-out de início rápido orientada instalações que estão disponíveis na secção começar a utilizar o [guia para programadores do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide#get-started).

5.  Quando terminar, clique em **Criar**. Azure AD atribui um ID de aplicação exclusivo para seu aplicativo e\'re direcionado para seu aplicativo\'página registo principal. Dependendo de se a sua aplicação é do tipo nativo ou Web, dispõe de várias opções para lhe adicionar mais capacidades.

    **Nota:** por predefinição, a aplicação recentemente registada é configurada para permitir que apenas os utilizadores a partir do mesmo inquilino para iniciar sessão na sua aplicação.

<a name="api-methods-and-endpoints"></a>Métodos da API e os pontos finais
-------------------------

As secções seguintes descrevem os métodos de API e os pontos finais disponíveis para ativar as subscrições para uma oferta SaaS.

### <a name="get-a-token-based-on-the-azure-ad-app"></a>Obter um token com base na aplicação do Azure AD

Método HTTP

`GET`

*Request URL* (URL do pedido)

**https://login.microsoftonline.com/*{tenantId}*  /oauth2/token**

*Parâmetro URI*

|  **Parameter name** (Nome do parâmetro)  | **Necessário**  | **Descrição**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| TenantId             | Verdadeiro          | ID do inquilino da aplicação registada do AAD   |
|  |  |  |


*Cabeçalho do pedido*

|  **Nome do cabeçalho**  | **Necessário** |  **Descrição**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  Content-Type     | Verdadeiro         | Tipo de conteúdo associado à solicitação. O valor predefinido é `application/x-www-form-urlencoded`.  |
|  |  |  |


*Corpo do pedido*

| **Nome da propriedade**   | **Necessário** |  **Descrição**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | Verdadeiro         | Tipo de concessão. O valor predefinido é `client_credentials`.                    |
|  Client_id          | Verdadeiro         |  Identificador de cliente/aplicação associado à aplicação do Azure AD.                  |
|  client_secret      | Verdadeiro         |  Palavra-passe associado à aplicação do Azure AD.                               |
|  Recurso           | Verdadeiro         |  Recurso de destino para a qual o token é solicitado. O valor predefinido é `b3cca048-ed2e-406c-aff2-40cf19fe7bf5`. |
|  |  |  |


*Resposta*

|  **Nome**  | **Tipo**       |  **Descrição**    |
| ---------- | -------------  | ------------------- |
| 200 OK /    | TokenResponse  | Pedido efetuado com êxito   |
|  |  |  |

*TokenResponse*

Token de resposta de exemplo:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "b3cca048-ed2e-406c-aff2-40cf19fe7bf5",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```

### <a name="marketplace-api-endpoint-and-api-version"></a>Ponto final de API do Marketplace e a versão de API

O ponto final para a API do Azure Marketplace está `https://marketplaceapi.microsoft.com`.

A versão de API atual é `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Resolver a subscrição

Ação de publicação em resolver o ponto final permite aos utilizadores resolver um token para um ID de recurso persistente.

*Pedido*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Nome do parâmetro** |     **Descrição**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  versão de API        |  A versão da operação para utilizar para este pedido.   |
|  |  |


*Headers* (Cabeçalhos)

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Não           | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do cliente, preferencialmente, um GUID. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
| x-ms-correlationid | Não           | Um valor de cadeia de caracteres exclusivo para a operação no cliente. Isso correlaciona todos os eventos da operação de cliente com eventos do lado do servidor. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| tipo de conteúdo       | Sim          | `application/json`                                        |
| Autorização      | Sim          | O JSON web token (JWT) token de portador.                    |
|  |  |  |
  

*Corpo da resposta*

 ``` json       
    { 
        “id”: “”, 
        “offerId”:””, 
         “planId”:””, 
    }     
```

| **Parameter name** (Nome do parâmetro) | **Tipo de dados** | **Descrição**                       |
|--------------------|---------------|---------------------------------------|
| ID                 | Cadeia        | ID da subscrição SaaS.          |
| OfferId            | Cadeia        | ID de oferta que o utilizador inscrito. |
| planId             | Cadeia        | ID do plano nos quais o utilizador inscrito.  |
|  |  |  |


*Códigos de resposta*

| **Código de estado de HTTP** | **Código de erro**     | **Descrição**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token está a ser resolvido com êxito.                                                            |
| 400                  | `BadRequest`         | É necessário um cabeçalhos estão em falta ou um api-version inválido especificado. Falha ao resolver o token, porque o token de qualquer um dos está incorretamente formulados ou expirados. |
| 403                  | `Forbidden`          | O chamador não está autorizado a efetuar esta operação.                                 |
| 429                  | `RequestThrottleId`  | O serviço está ocupado a processar pedidos, tente novamente mais tarde.                                |
| 503                  | `ServiceUnavailable` | Serviço para baixo temporariamente, tente novamente mais tarde.                                        |
|  |  |  |


*Cabeçalhos de resposta*

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sim          | ID recebida do cliente do pedido.                                                                   |
| x-ms-correlationid | Sim          | ID de correlação se transmitido pelo cliente, caso contrário, este valor é o ID de correlação de servidor.                   |
| x-ms-activityid    | Sim          | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do serviço. Isto é utilizado para qualquer reconciliations. |
| Retry-After        | Não           | Este valor é definido apenas para uma resposta 429.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Subscrever

O ponto de extremidade subscribe permite aos utilizadores iniciar uma subscrição para um serviço de SaaS para um determinado plano e ativar a faturação no sistema de comércio.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}* ? api-version = 2017-04-15**

| **Nome do parâmetro**  | **Descrição**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Subscrição de ID de SaaS.                              |
| versão de API         | A versão da operação para utilizar para este pedido. |
|  |  |

*Headers* (Cabeçalhos)

|  **Chave do cabeçalho**        | **Necessário** |  **Descrição**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   Não         | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do cliente, preferencialmente, um GUID. Se não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| x-ms-correlationid     |   Não         | Um valor de cadeia de caracteres exclusivo para a operação no cliente. Este valor é para correlacionar a todos os eventos da operação de cliente com eventos do lado do servidor. Se não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| If-Match/If-None-Match |   Não         |   Validador de valor de ETag forte.                                                          |
| tipo de conteúdo           |   Sim        |    `application/json`                                                                   |
|  Autorização         |   Sim        |    O JSON web token (JWT) token de portador.                                               |
|  |  |  |

*Corpo*

``` json
  { 
      “planId”:””, 
   }      
```

| **Nome do elemento** | **Tipo de dados** | **Descrição**                      |
|------------------|---------------|--------------------------------------|
| planId           | Cadeia        | ID do plano nos quais o utilizador inscrito. |
|  |  |  |

*Códigos de resposta*

| **Código de estado de HTTP** | **Código de erro**     | **Descrição**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Ativação de assinatura de SaaS recebida para um determinado plano.                   |
| 400                  | `BadRequest`         | É necessário um cabeçalhos estão em falta ou o corpo do JSON é um formato incorreto. |
| 403                  | `Forbidden`          | O chamador não está autorizado a efetuar esta operação.                   |
| 404                  | `NotFound`           | Subscrição não foi encontrada com o ID fornecido                                  |
| 409                  | `Conflict`           | Outra operação está em curso na subscrição.                     |
| 429                  | `RequestThrottleId`  | O serviço está ocupado a processar pedidos, tente novamente mais tarde.                  |
| 503                  | `ServiceUnavailable` | Serviço para baixo temporariamente, tente novamente mais tarde.                          |
|  |  |  |

*Cabeçalhos de resposta*

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sim          | ID recebida do cliente do pedido.                                                                   |
| x-ms-correlationid | Sim          | ID de correlação se transmitido pelo cliente, caso contrário, este valor é o ID de correlação de servidor.                   |
| x-ms-activityid    | Sim          | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do serviço. Este valor é utilizado para qualquer reconciliations. |
| Retry-After        | Sim          | Intervalo com o cliente pode verificar o estado.                                                       |
| Operação de localização | Sim          | Ligar a um recurso para obter o estado da operação.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Ponto final do plano de alteração

O ponto de extremidade de alteração permite ao utilizador converter o seu plano de atualmente subscrito para um novo plano.

**PATCH**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}* ? api-version = 2017-04-15**

| **Nome do parâmetro**  | **Descrição**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Subscrição de ID de SaaS.                              |
| versão de API         | A versão da operação para utilizar para este pedido. |
|  |  |

*Headers* (Cabeçalhos)

| **Chave do cabeçalho**          | **Necessário** | **Descrição**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | Não           | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do cliente. Recomendamos um GUID. Se não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.   |
| x-ms-correlationid      | Não           | Um valor de cadeia de caracteres exclusivo para a operação no cliente. Este valor é para correlacionar a todos os eventos da operação de cliente com eventos do lado do servidor. Se não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| If-Match /If-None-Match | Não           | Validador de valor de ETag forte.                              |
| tipo de conteúdo            | Sim          | `application/json`                                        |
| Autorização           | Sim          | O JSON web token (JWT) token de portador.                    |
|  |  |  |


*Corpo*

``` json
                { 
                    “planId”:””, 
                } 
```


|  **Nome do elemento** |  **Tipo de dados**  | **Descrição**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  Cadeia         | ID do plano nos quais o utilizador inscrito.         |
|  |  |  |

*Códigos de resposta*

| **Código de estado de HTTP** | **Código de erro**     | **Descrição**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Ativação de assinatura de SaaS recebida para um determinado plano.                   |
| 400                  | `BadRequest`         | É necessário um cabeçalhos estão em falta ou o corpo do JSON é um formato incorreto. |
| 403                  | `Forbidden`          | O chamador não está autorizado a efetuar esta operação.                   |
| 404                  | `NotFound`           | Subscrição não foi encontrada com o ID fornecido                                  |
| 409                  | `Conflict`           | Outra operação está em curso na subscrição.                     |
| 429                  | `RequestThrottleId`  | O serviço está ocupado a processar pedidos, tente novamente mais tarde.                  |
| 503                  | `ServiceUnavailable` | Serviço para baixo temporariamente, tente novamente mais tarde.                          |
|  |  |  |

*Cabeçalhos de resposta*

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sim          | ID recebida do cliente do pedido.                                                                   |
| x-ms-correlationid | Sim          | ID de correlação se transmitido pelo cliente, caso contrário, este valor é o ID de correlação de servidor.                   |
| x-ms-activityid    | Sim          | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do serviço. Este valor é utilizado para qualquer reconciliations. |
| Retry-After        | Sim          | Intervalo com o cliente pode verificar o estado.                                                       |
| Operação de localização | Sim          | Ligar a um recurso para obter o estado da operação.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Eliminar subscrição

A ação de eliminação no ponto de extremidade subscribe permite que um utilizador eliminar uma subscrição com uma ID especificada.

*Pedido*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}* ? api-version = 2017-04-15**

| **Nome do parâmetro**  | **Descrição**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Subscrição de ID de SaaS.                              |
| versão de API         | A versão da operação para utilizar para este pedido. |
|  |  |

*Headers* (Cabeçalhos)

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | Não           | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do cliente. Recomendamos um GUID. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.                                                           |
| x-ms-correlationid | Não           | Um valor de cadeia de caracteres exclusivo para a operação no cliente. Este valor é para correlacionar a todos os eventos da operação de cliente com eventos do lado do servidor. Se não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| Autorização      | Sim          | O JSON web token (JWT) token de portador.                    |
|  |  |  |
 

*Códigos de resposta*

| **Código de estado de HTTP** | **Código de erro**     | **Descrição**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Ativação de assinatura de SaaS recebida para um determinado plano.                   |
| 400                  | `BadRequest`         | É necessário um cabeçalhos estão em falta ou o corpo do JSON é um formato incorreto. |
| 403                  | `Forbidden`          | O chamador não está autorizado a efetuar esta operação.                   |
| 404                  | `NotFound`           | Subscrição não foi encontrada com o ID fornecido                                  |
| 429                  | `RequestThrottleId`  | O serviço está ocupado a processar pedidos, tente novamente mais tarde.                  |
| 503                  | `ServiceUnavailable` | Serviço está temporariamente indisponível. Tente novamente mais tarde.                          |
|  |  |  |

*Cabeçalhos de resposta*

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sim          | ID recebida do cliente do pedido.                                                                   |
| x-ms-correlationid | Sim          | ID de correlação se transmitido pelo cliente, caso contrário, isso é o ID de correlação de servidor.                   |
| x-ms-activityid    | Sim          | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do serviço. Isto é utilizado para qualquer reconciliations. |
| Retry-After        | Sim          | Intervalo com o cliente pode verificar o estado.                                                       |
| Operação de localização | Sim          | Ligar a um recurso para obter o estado da operação.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>Obter estado da operação

Este ponto final permite ao utilizador controlar o estado de uma operação de async acionadas (subscrever/anular a subscrição/alterar plano).

*Pedido*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/*{operationId}* ? api-version = 2017-04-15**

| **Nome do parâmetro**  | **Descrição**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | ID exclusivo para a operação acionada.                |
| versão de API         | A versão da operação para utilizar para este pedido. |
|  |  |


*Headers* (Cabeçalhos)

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Não           | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do cliente. Recomendamos um GUID. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.   |
| x-ms-correlationid | Não           | Um valor de cadeia de caracteres exclusivo para a operação no cliente. Este valor é para correlacionar a todos os eventos da operação de cliente com eventos do lado do servidor. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.  |
| Autorização      | Sim          | O JSON web token (JWT) token de portador.                    |
|  |  |  | 
  

*Corpo da resposta*

``` json
  { 
      “id”: “”, 
      “status”:””, 
       “resourceLocation”:””, 
      “created”:””, 
      “lastModified”:”” 
  } 
```

| **Parameter name** (Nome do parâmetro) | **Tipo de dados** | **Descrição**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| ID                 | Cadeia        | ID da operação.                                                                      |
| status             | Enum          | Estado da operação, um dos seguintes: `In Progress`, `Succeeded`, ou `Failed`.          |
| resourceLocation   | Cadeia        | Ligação para a subscrição que foi criada ou modificada. Isto ajuda o cliente para obter a operação de mensagem de estado atualizado. Este valor não está definido para `Unsubscribe` operações. |
| criado            | DateTime      | Hora de criação de operação em UTC.                                                           |
| lastModified       | DateTime      | Última atualização na operação em UTC.                                                      |
|  |  |  |

*Códigos de resposta*

| **Código de estado de HTTP** | **Código de erro**     | **Descrição**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Resolvido o pedido get com êxito e o corpo contém a resposta.    |
| 400                  | `BadRequest`         | É necessário um cabeçalhos estão em falta ou um api-version inválido foi especificado. |
| 403                  | `Forbidden`          | O chamador não está autorizado a efetuar esta operação.                      |
| 404                  | `NotFound`           | Subscrição não foi encontrada com o ID fornecido                                     |
| 429                  | `RequestThrottleId`  | O serviço está ocupado a processar pedidos, tente novamente mais tarde.                     |
| 503                  | `ServiceUnavailable` | Serviço para baixo temporariamente, tente novamente mais tarde.                             |
|  |  |  |

*Cabeçalhos de resposta*

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sim          | ID recebida do cliente do pedido.                                                                   |
| x-ms-correlationid | Sim          | ID de correlação se transmitido pelo cliente, caso contrário, isso é o ID de correlação de servidor.                   |
| x-ms-activityid    | Sim          | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do serviço. Isto é utilizado para qualquer reconciliations. |
| Retry-After        | Sim          | Intervalo com o cliente pode verificar o estado.                                                       |
| Operação de localização | Sim          | Ligar a um recurso para obter o estado da operação.                                                        |
|  |  |  |

### <a name="get-subscription"></a>Obter a subscrição

Assine a ação de Get no ponto final permite que um utilizador a obter uma subscrição com um identificador de recurso específico.

*Pedido*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/*{subscriptionId}* ? api-version = 2017-04-15**

| **Nome do parâmetro**  | **Descrição**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Subscrição de ID de SaaS.                              |
| versão de API         | A versão da operação para utilizar para este pedido. |
|  |  |

*Headers* (Cabeçalhos)

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Não           | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do cliente, preferencialmente, um GUID. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.                                                           |
| x-ms-correlationid | Não           | Um valor de cadeia de caracteres exclusivo para a operação no cliente. Este valor é para correlacionar a todos os eventos da operação de cliente com eventos do lado do servidor. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| Autorização      | Sim          | O JSON web token (JWT) token de portador.                                                                    |
|  |  |  |

*Corpo da resposta*

``` json
  { 
      “id”: “”, 
      “saasSubscriptionName”:””, 
      “offerId”:””, 
       “planId”:””, 
      “saasSubscriptionStatus”:”” 
      “created”:”” 
      “lastModified”: “”, 
  }
```
| **Parameter name** (Nome do parâmetro)     | **Tipo de dados** | **Descrição**                               |
|------------------------|---------------|-----------------------------------------------|
| ID                     | Cadeia        | Recursos de subscrição de ID de SaaS no Azure.    |
| offerId                | Cadeia        | ID de oferta que o utilizador inscrito.         |
| planId                 | Cadeia        | ID do plano nos quais o utilizador inscrito.          |
| saasSubscriptionName   | Cadeia        | Nome da subscrição SaaS.                |
| saasSubscriptionStatus | Enum          | Estado da operação.  Um dos seguintes:  <br/> - `Subscribed`: A subscrição está ativa.  <br/> - `Pending`: Utilizador crie o recurso, mas ele não está ativado por de ISV.   <br/> - `Unsubscribed`: Anulou a subscrição o utilizador.   <br/> - `Suspended`: O utilizador foi suspenso a subscrição.   <br/> - `Deactivated`: Subscrição o azure está suspensa.  |
| criado                | DateTime      | Valor de timestamp de criação de subscrição em UTC. |
| lastModified           | DateTime      | Subscrição alterar timestamp valor em UTC. |
|  |  |  |

*Códigos de resposta*

| **Código de estado de HTTP** | **Código de erro**     | **Descrição**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Resolvido o pedido get com êxito e o corpo contém a resposta.    |
| 400                  | `BadRequest`         | É necessário um cabeçalhos estão em falta ou um api-version inválido foi especificado. |
| 403                  | `Forbidden`          | O chamador não está autorizado a efetuar esta operação.                      |
| 404                  | `NotFound`           | Subscrição não foi encontrada com o ID fornecido                                     |
| 429                  | `RequestThrottleId`  | O serviço está ocupado a processar pedidos, tente novamente mais tarde.                     |
| 503                  | `ServiceUnavailable` | Serviço para baixo temporariamente, tente novamente mais tarde.                             |
|  |  |  |

*Cabeçalhos de resposta*

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sim          | ID recebida do cliente do pedido.                                                                   |
| x-ms-correlationid | Sim          | ID de correlação se transmitido pelo cliente, caso contrário, isso é o ID de correlação de servidor.                   |
| x-ms-activityid    | Sim          | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do serviço. Isto é utilizado para qualquer reconciliations. |
| Retry-After        | Não           | Intervalo com o cliente pode verificar o estado.                                                       |
| eTag               | Sim          | Ligar a um recurso para obter o estado da operação.                                                        |
|  |  |  |


### <a name="get-subscriptions"></a>Obter Subscrições

A ação de Get no ponto final de subscrições permite que um usuário recuperar todas as subscrições para todas as ofertas do ISV.

*Pedido*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/subscriptions?api-version=2017-04-15**

| **Nome do parâmetro**  | **Descrição**                                       |
|---------------------|-------------------------------------------------------|
| versão de API         | A versão da operação para utilizar para este pedido. |
|  |  |

*Headers* (Cabeçalhos)

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | Não           | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do cliente. Recomendamos um GUID. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta.             |
| x-ms-correlationid | Não           | Um valor de cadeia de caracteres exclusivo para a operação no cliente. Este valor é para correlacionar a todos os eventos da operação de cliente com eventos do lado do servidor. Se este valor não for fornecido, um será gerado e fornecido nos cabeçalhos de resposta. |
| Autorização      | Sim          | O JSON web token (JWT) token de portador.                    |
|  |  |  |


*Corpo da resposta*

``` json
  { 
      “id”: “”, 
      “saasSubscriptionName”:””, 
      “offerId”:””, 
       “planId”:””, 
      “saasSubscriptionStatus”:”” 
      “created”:”” 
      “lastModified”: “”, 
  }
```

| **Parameter name** (Nome do parâmetro)     | **Tipo de dados** | **Descrição**                               |
|------------------------|---------------|-----------------------------------------------|
| ID                     | Cadeia        | Recursos de subscrição de ID de SaaS no Azure.    |
| offerId                | Cadeia        | ID de oferta que o utilizador inscrito.         |
| planId                 | Cadeia        | ID do plano nos quais o utilizador inscrito.          |
| saasSubscriptionName   | Cadeia        | Nome da subscrição SaaS.                |
| saasSubscriptionStatus | Enum          | Estado da operação.  Um dos seguintes:  <br/> - `Subscribed`: A subscrição está ativa.  <br/> - `Pending`: Utilizador crie o recurso, mas ele não está ativado por de ISV.   <br/> - `Unsubscribed`: Anulou a subscrição o utilizador.   <br/> - `Suspended`: O utilizador foi suspenso a subscrição.   <br/> - `Deactivated`: Subscrição o azure está suspensa.  |
| criado                | DateTime      | Valor de timestamp de criação de subscrição em UTC. |
| lastModified           | DateTime      | Subscrição alterar timestamp valor em UTC. |
|  |  |  |

*Códigos de resposta*

| **Código de estado de HTTP** | **Código de erro**     | **Descrição**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | Resolvido o pedido get com êxito e o corpo contém a resposta.    |
| 400                  | `BadRequest`         | É necessário um cabeçalhos estão em falta ou um api-version inválido foi especificado. |
| 403                  | `Forbidden`          | O chamador não está autorizado a efetuar esta operação.                      |
| 404                  | `NotFound`           | Subscrição não foi encontrada com o ID fornecido                                     |
| 429                  | `RequestThrottleId`  | O serviço está ocupado a processar pedidos, tente novamente mais tarde.                     |
| 503                  | `ServiceUnavailable` | Serviço está temporariamente indisponível. Tente novamente mais tarde.                             |
|  |  |  |

*Cabeçalhos de resposta*

| **Chave do cabeçalho**     | **Necessário** | **Descrição**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sim          | ID recebida do cliente do pedido.                                                                   |
| x-ms-correlationid | Sim          | ID de correlação se transmitido pelo cliente, caso contrário, isso é o ID de correlação de servidor.                   |
| x-ms-activityid    | Sim          | Um valor de cadeia de caracteres exclusivo para o pedido de controlo do serviço. Isto é utilizado para qualquer reconciliations. |
| Retry-After        | Não           | Intervalo com o cliente pode verificar o estado.                                                       |
|  |  |  |
