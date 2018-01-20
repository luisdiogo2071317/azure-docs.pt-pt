---
title: "Erro ao processar as melhores práticas para os clientes do Azure Active Directory Authentication Library (ADAL)"
description: "Fornece orientações e melhores práticas para as aplicações cliente ADAL de processamento de erros."
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mtillman
ms.author: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.custom: 
ms.openlocfilehash: 275ab65569a1861f046c8ee77914e0859d41d5f7
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="error-handling-best-practices-for-azure-active-directory-authentication-library-adal-clients"></a>Erro ao processar as melhores práticas para os clientes do Azure Active Directory Authentication Library (ADAL)

Este artigo fornece orientação no tipo de erros que os programadores podem encontrar, ao utilizar a ADAL para autenticar os utilizadores. Quando utilizar a ADAL, existem vários cenários em que um programador pode precisar passo na e processar erros. Processamento de erros adequada garante uma experiência de utilizador final excelente e limita o número de vezes que o utilizador final tem de iniciar sessão.

Neste artigo, vamos explorar cenários específicos para cada plataforma suportada pelo ADAL e a forma como a aplicação pode processar cada caso corretamente. As orientações de erro é dividida em duas categorias mais vasto, com base nos padrões de aquisição do token fornecidos pelo ADAL APIs:

- **AcquireTokenSilent**: cliente tentar obter um token automaticamente (sem IU) e pode falhar se não for bem-sucedida ADAL. 
- **AcquireToken**: cliente tentar aquisição automática, mas também pode efetuar pedidos interativos que necessitem de início de sessão.

> [!TIP]
> É uma boa ideia para registar todos os erros e exceções ao utilizar a ADAL e o Azure AD. Os registos apenas não são úteis para compreender o estado de funcionamento geral da sua aplicação, mas também são importantes quando os problemas mais amplo de depuração. Enquanto a aplicação pode recuperar de determinados erros, eles podem sugestão em mais amplo problemas de conceção que necessitem de alterações de código para resolver. 
> 
> Quando implementar as condições de erro abrangidas neste documento, deve iniciar sessão o código de erro e a descrição para os motivos pelos quais debatidos anteriormente. Consulte o [erros e de referência de registo](#error-and-logging-reference) para obter exemplos de código de registo. 
>

## <a name="acquiretokensilent"></a>AcquireTokenSilent

AcquireTokenSilent tenta obter um token com a garantia de que o utilizador final não consegue ver uma Interface de utilizador (IU). Existem vários cenários em que a aquisição automática pode falhar e tem de ser processada através de pedidos interativos ou por um processador de predefinição. Iremos aprofundar as especificações quando e como utilizar cada cenário nas secções que se seguem.

Não há um conjunto de erros gerados pelo sistema operativo, que podem necessitar de específicas para a aplicação de processamento de erros. Para obter mais informações, consulte a secção erros "Sistema operativo" [erros e de referência de registo](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Cenários de aplicações

- [O Native client](active-directory-dev-glossary.md#native-client) aplicações (iOS, Android, ambiente de trabalho .NET ou Xamarin)
- [Cliente Web](active-directory-dev-glossary.md#web-client) aplicações chamar um [recursos](active-directory-dev-glossary.md#resource-server) (.NET)

### <a name="error-cases-and-actionable-steps"></a>Nos casos de erro e passos passíveis de ação

Fundamentalmente, existem dois cenários de AcquireTokenSilent erros:

| Maiúsculas/Minúsculas | Descrição |
|------|-------------|
| **Caso 1**: erro é resolvível com um interativa início de sessão | Para erros causados por uma falta de tokens válidos, é necessário um pedido de interativo. Especificamente, a pesquisa de cache e um token de atualização inválido/expirado necessitam de uma chamada de AcquireToken para resolver.<br><br>Nestes casos, o utilizador final tem de ser-lhe pedido que inicie sessão. A aplicação pode optar por fazer um pedido de interativo imediatamente, após a interação do utilizador final (tais como a atingir um botão de início de sessão) ou posterior. A opção depende o comportamento pretendido da aplicação.<br><br>Consulte o código na secção seguinte para este cenário específico e os erros que diagnosticá-lo.|
| **Cenário 2**: erro não é resolvível com um interativa início de sessão | Para a rede e erros de transitório/temporário ou outras falhas, executar um pedido de AcquireToken interativo não resolver o problema. Desnecessários interativa início de sessão pede também pode frustrar utilizadores finais. ADAL tenta automaticamente uma repetição único para a maioria dos erros AcquireTokenSilent falhas.<br><br>A aplicação de cliente também pode tentar uma repetição em algum momento posterior, mas quando e como fazê-lo está dependente o comportamento da aplicação e experiência do utilizador final pretendido. Por exemplo, a aplicação pode efetuar uma repetição AcquireTokenSilent após alguns minutos, ou em resposta a qualquer ação do utilizador final. Uma repetição imediata resultará na aplicação a ser limitada e não deve ser tentada.<br><br>Uma repetição subsequente falha com o mesmo erro não significa que o cliente deve fazer um pedido de interativo utilizando AcquireToken, não resolver o erro.<br><br>Consulte o código na secção seguinte para este cenário específico e os erros que diagnosticá-lo. |

### <a name="net"></a>.NET

As seguintes orientações fornece exemplos de processamento em conjunto com o ADAL métodos de erros: 

- acquireTokenSilentAsync(…)
- acquireTokenSilentSync(…) 
- acquireTokenSilent(...) [preterido]
- acquireTokenByRefreshToken(...) [preterido] 

O código teria de ser implementado da seguinte forma:

```csharp
try{
    AcquireTokenSilentAsync(…);
} 

catch (AdalSilentTokenAcquisitionException e) {
    // Exception: AdalSilentTokenAcquisitionException
    // Caused when there are no tokens in the cache or a required refresh failed. 

    // Action: Case 1, resolvable with an interactive request.  
} 

catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
} 
    
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET. 
    // e.ErrorCode contains the error code. 

    // Action: Case 2, not resolvable with an interactive request.
    // Attempt retry after a timed interval or user action.
    // Example Error: network_not_available, default case.
}
```

### <a name="android"></a>Android

As seguintes orientações fornece exemplos de processamento em conjunto com o ADAL métodos de erros: 

- acquireTokenSilentSync(…)
- acquireTokenSilentAsync(...)
- acquireTokenSilent(...) [preterido]

O código teria de ser implementado da seguinte forma:

```java
// *Inside callback*
public void onError(Exception e) {

    if (e instanceof AuthenticationException) {
        // Exception: AdalException
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode().

        // Errors: ADALError.ERROR_SILENT_REQUEST,
        // ADALError.AUTH_REFRESH_FAILED_PROMPT_NOT_ALLOWED,
        // ADALError.INVALID_TOKEN_CACHE_ITEM
        // Description: Request failed due to no tokens in
        // cache or failed a required refresh. 

        // Action: Case 1, resolvable with an interactive request. 

        // Action: Case 2, not resolvable with an interactive request.
        // Attempt retry after a timed interval or user action.
        // Example Errors: default case,
        // DEVICE_CONNECTION_IS_NOT_AVAILABLE, 
        // BROKER_AUTHENTICATOR_ERROR_GETAUTHTOKEN,
    }
}
```

### <a name="ios"></a>iOS

As seguintes orientações fornece exemplos de processamento em conjunto com o ADAL métodos de erros: 

- acquireTokenSilentWithResource(…)

O código teria de ser implementado da seguinte forma:

```objc
[context acquireTokenSilentWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL Objective-C.
            // Error Code: result.error.code

            // Errors: AD_ERROR_SERVER_REFRESH_TOKEN_REJECTED, AD_ERROR_CACHE_NO_REFRESH_TOKEN
            // Description: No tokens in cache or failed a required token refresh failed. 
            // Action: Case 1, resolvable with an interactive request. 

            // Error: AD_ERROR_CACHE_MULTIPLE_USERS
            // Description: There was ambiguity in the silent request resulting in multiple cache items.
            // Action: Special Case, application should perform another silent request and specify the user using ADUserIdentifier. 
            // Can be caused in cases of a multi-user application.  

            // Action: Case 2, not resolvable with an interactive request.
            // Attempt retry after some time or user action.
            // Example Errors: default case,
            // AD_ERROR_CACHE_BAD_FORMAT
        }
    }
}]
```

## <a name="acquiretoken"></a>AcquireToken

AcquireToken método é a predefinição ADAL utilizado para obter os tokens. Nos casos em que a identidade do utilizador é necessária, AcquireToken tentar obter um token primeiro silenciosamente, em seguida, apresenta IU se for necessário (a menos que é transmitida PromptBehavior.Never). Nos casos em que a identidade da aplicação é necessária, AcquireToken tenta obter um token, mas não mostrar a IU vez que não há nenhum utilizador final.  

Quando o processamento de erros de AcquireToken, processamento de erros é depende da plataforma e o cenário a aplicação está a tentar alcançar.  

O sistema operativo também pode gerar um conjunto de erros, o que requer processamento depende a aplicação específica de erros. Para obter mais informações, consulte "Erros de sistema operativo" em [erros e de referência de registo](#error-and-logging-reference). 

### <a name="application-scenarios"></a>Cenários de aplicações

- Aplicações de cliente nativo (iOS, Android, ambiente de trabalho .NET ou Xamarin)
- Aplicações Web que chamar um recurso de API (.NET)
- Aplicações de página única (JavaScript)
- Aplicações de serviços (.NET, Java)
  - Todos os cenários, incluindo em-nome-de
  - Em-nome-de cenários específicos

### <a name="error-cases-and-actionable-steps-native-client-applications"></a>Nos casos de erro e passos acionáveis: aplicações de cliente nativo

Se estiver a criar uma aplicação cliente nativa, existem alguns casos de processamento de erros a ter em consideração que se relacionam com problemas de rede, falhas transitórias e outros erros específicos da plataforma. Na maioria dos casos, uma aplicação não deve executar tentativas imediatas, mas em vez disso, aguarde interação do utilizador final que solicita um início de sessão.  

Existem alguns casos especiais em que uma única repetição poderão ajudar a resolver o problema. Por exemplo, quando um utilizador tem de ativar os dados num dispositivo ou concluir o mediador do Azure AD transfiram após a falha inicial. 

Em caso de falha, uma aplicação pode apresentar IU para permitir ao utilizador final efetuar algumas interação que pede uma repetição. Por exemplo, se o dispositivo falhou para um erro offline, um botão "Tente voltar a iniciar sessão" pedir um AcquireToken novamente em vez de repetir imediatamente a falha. 

Erro ao processar aplicações nativas pode ser definido por dois cenários:

|  |  |
|------|-------------|
| **Caso 1**:<br>Erro de não repetição do (maioria dos casos) | 1. Não tente repetição imediata. Apresente o utilizador final que IU com base no erro específico que invoca uma repetição ("Tentar iniciar sessão no novo", "Aplicação de Mediador de transferir o Azure AD", etc.). |
| **Caso 2**:<br>Erro de repetição | 1. Efetue uma repetição único, tal como o utilizador final pode ter a entrou num Estado que resulta em concluído com êxito.<br><br>2. Se a tentativa falhar, apresentam o utilizador final que IU com base no erro específico que invoca uma repetição ("Tentar iniciar sessão no novo", "Aplicação de Mediador de transferir o Azure AD", etc.). |

> [!IMPORTANT]
> Se uma conta de utilizador é transmitida para a ADAL uma chamada automática e falhar, o pedido subsequente interativo permite ao utilizador final iniciar sessão com uma conta diferente. Após um AcquireToken com êxito através de uma conta de utilizador, a aplicação tem de verificar que o utilizador com sessão iniciada corresponde ao objeto de utilizador local as aplicações. Um erro de correspondência não gerar uma exceção (exceto no Objective C), mas devem ser consideradas nos casos em que um utilizador é conhecido localmente antes dos pedidos de autenticação (por exemplo, uma falha de chamada automática).
>

#### <a name="net"></a>.NET

As seguintes orientações fornece exemplos de juntamente com todos os não silencioso AcquireToken(...) de processamento de erros Métodos de ADAL, *exceto*: 

- AcquireTokenAsync(…, IClientAssertionCertification, …)
- AcquireTokenAsync(…,ClientCredential, …)
- AcquireTokenAsync(…,ClientAssertion, …)
- AcquireTokenAsync(…,UserAssertion,…)   

O código teria de ser implementado da seguinte forma:

```csharp
try {
    AcquireTokenAsync(…);
} 
    
catch(AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.
    
    // Design time consideration: Certain errors may be caused at development and exposed through this exception. 
    // Looking inside the description will give more guidance on resolving the specific issue. 

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

    } 

catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // e.ErrorCode contains the error code

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

> [!NOTE]
> ADAL .NET tem uma consideração adicional conforme suporta PromptBehavior.Never, que tem o comportamento como AcquireTokenSilent.
>

As seguintes orientações fornece exemplos de processamento em conjunto com o ADAL métodos de erros: 

- acquireToken(…, PromptBehavior.Never)

O código teria de ser implementado da seguinte forma:

```csharp
    try {acquireToken(…, PromptBehavior.Never);
    } 

catch(AdalServiceException e) {
    // Exception: AdalServiceException represents 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Action: Case 1: Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: default case

} catch (AdalException e) {
    // Error Code: e.ErrorCode == "user_interaction_required"
    // Description: user_interaction_required indicates the silent request failed 
    // in a way that's resolvable with an interactive request.
    // Action: Resolvable with an interactive request. 

    // Action: Case 1, Non-Retryable 
    // Do not perform an immediate retry. Only retry after user action. 
    // Example Errors: network_not_available, default case
}
```

#### <a name="android"></a>Android

As seguintes orientações fornece exemplos de juntamente com todos os não silencioso AcquireToken(...) de processamento de erros Métodos ADAL. 

O código teria de ser implementado da seguinte forma:

```java
AcquireTokenAsync(…);

// *Inside callback*
public void onError(Exception e) {
    if (e instanceof AuthenticationException) {
        // Exception: AdalException 
        // Represents a library exception generated by ADAL Android.
        // Error Code: e.getCode();

        // Error: ADALError.BROKER_APP_INSTALLATION_STARTED
        // Description: Broker app not installed, user will be prompted to download the app. 

        // Action: Case 2, Retriable Error 
        // Perform a single retry. If that fails, only try again after user action. 

        // Action: Case 1, Non-Retriable 
        // Do not perform an immediate retry. Only retry after user action. 
        // Example Errors: default case, DEVICE_CONNECTION_IS_NOT_AVAILABLE
    }
}
```

#### <a name="ios"></a>iOS

As seguintes orientações fornece exemplos de juntamente com todos os não silencioso AcquireToken(...) de processamento de erros Métodos ADAL. 

O código teria de ser implementado da seguinte forma:

```objc
[context acquireTokenWithResource:[ARGS], completionBlock:^(ADAuthenticationResult *result) {
    if (result.status == AD_FAILED) {
        if ([error.domain isEqualToString:ADAuthenticationErrorDomain]){
            // Exception: AD_FAILED 
            // Represents a library error generated by ADAL ObjC.
            // Error Code: result.error.code 

            // Error: AD_ERROR_SERVER_WRONG_USER
            // Description: App passed a user into ADAL and the end user signed in with a different account. 
            // Action: Case 1, Non-retriable (as is) and up to the application on how to handle this case. 
            // It can attempt a new request without specifying the user, or use UI to clarify the user account to sign in. 

            // Action: Case 1, Non-Retriable 
            // Do not perform an immediate retry. Only retry after user action. 
            // Example Errors: default case
        }
    }
}]
```

### <a name="error-cases-and-actionable-steps-web-applications-that-call-a-resource-api-net"></a>Nos casos de erro e passos acionáveis: aplicações Web que chamar um recurso de API (.NET)

Se estiver a criar uma aplicação web do .NET que chama obtém um token com um código de autorização para um recurso, o código apenas necessário é um processador predefinido para o cenário genérico. 

As seguintes orientações fornece exemplos de processamento em conjunto com o ADAL métodos de erros: 

- AcquireTokenByAuthorizationCodeAsync(…)

O código teria de ser implementado da seguinte forma:

```csharp
try {
    AcquireTokenByAuthorizationCodeAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action or wait until much later. 
    // Example Errors: default case
}
```

### <a name="error-cases-and-actionable-steps-single-page-applications-adaljs"></a>Nos casos de erro e passos acionáveis: aplicações de página única (adal.js)

Se estiver a criar uma aplicação de página única com adal.js AcquireToken, o erro do código de processamento é semelhante de uma chamada automática típica.  Especificamente no adal.js, AcquireToken nunca mostra uma IU. 

Uma falha AcquireToken tem os seguintes casos:

|  |  |
|------|-------------|
| **Caso 1**:<br>Resolvível com um pedido de interativo | 1. Se login() falhar, não efetue a repetição imediata. Repita apenas depois de ação do utilizador pede uma repetição.|
| **Caso 2**:<br>Não Resolvable com um pedido de interativo. Erro é repetição. | 1. Efetue uma repetição único, tal como o utilizador final principais ter entrou num Estado que resulta em concluído com êxito.<br><br>2. Se a tentativa falhar, apresentam o utilizador final com uma ação com base no erro específico que possa invocar uma repetição ("tente voltar a iniciar sessão"). |
| **Caso 3**:<br>Não Resolvable com um pedido de interativo. Erro não é repetição. | 1. Não tente repetição imediata. Apresentar o utilizador final com uma ação com base no erro específico que possa invocar uma repetição ("tente voltar a iniciar sessão"). |

O código teria de ser implementado da seguinte forma:

```javascript
AuthContext.acquireToken(…, function(error, errorDesc, token) {
    if (error || errorDesc) {
        // Represents any token acquisition failure that occurred. 
        // Error Code: error.indexOf("<ERROR_STRING>")

        // Errors: if (error.indexOf("interaction_required"))
        //         if (error.indexOf("login required"))
        // Description: ADAL wasn't able to silently acquire a token because of expire or fresh session. 
        // Action: Case 1, Resolvable with an interactive login() request. 

        // Error: if (error.indexOf("Token Renewal Failed")) 
        // Description: Timeout when refreshing the token.
        // Action: Case 2, Not resolvable interactively, error is retriable.
        // Perform a single retry. Only try again after user action.

        // Action: Case 3, Not resolvable interactively, error is not retriable. 
        // Do not perform an immediate retry. Only retry after user action.
        // Example Errors: default case
    }
}
```

### <a name="error-cases-and-actionable-steps-service-to-service-applications-net-only"></a>Nos casos de erro e passos acionáveis: serviço a serviço de aplicações (apenas para o .NET)

Se estiver a criar uma aplicação de serviço a serviço que utiliza AcquireToken, existem alguns erros de chaves que deve processar o seu código. O incumprimento apenas a falha é para devolver o erro para a aplicação de chamada (para casos em-nome-de) ou aplicar uma estratégia de repetição. 

#### <a name="all-scenarios"></a>Todos os cenários

Para *todos os* cenários de aplicação de serviço a serviço, incluindo em-nome-de:

- Não tente uma repetição imediata. Tentativas ADAL um único Repita para determinados pedidos falhados. 
- Apenas continue a repetir a operação depois de uma ação de utilizador ou aplicação pede uma repetição. Por exemplo, uma aplicação de daemon que funcionam em determinado intervalo de conjunto deve aguardar até que o intervalo seguinte para repetir.

As seguintes orientações fornece exemplos de processamento em conjunto com o ADAL métodos de erros: 

- AcquireTokenAsync(…, IClientAssertionCertification, …)
- AcquireTokenAsync(…,ClientCredential, …)
- AcquireTokenAsync(…,ClientAssertion, …)
- AcquireTokenAsync(…,UserAssertion, …)

O código teria de ser implementado da seguinte forma:

```csharp
try {
    AcquireTokenAsync(…);
} 

catch (AdalException e) {
    // Exception: AdalException
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Errors: default case
}  
```

#### <a name="on-behalf-of-scenarios"></a>Em-nome-de cenários

Para *em-nome-de* cenários de aplicação de serviço ao serviço.

As seguintes orientações fornece exemplos de processamento em conjunto com o ADAL métodos de erros: 

- AcquireTokenAsync(…, UserAssertion, …)

O código teria de ser implementado da seguinte forma:

```csharp
try {
AcquireTokenAsync(…);
} 

catch (AdalServiceException e) {
    // Exception: AdalServiceException 
    // Represents an error produced by the STS. 
    // e.ErrorCode contains the error code and description, which can be used for debugging. 
    // NOTE: Do not code a dependency on the contents of the error description, as it can change over time.

    // Error: On-Behalf-Of Error Handler
    if (e.ErrorCode == "interaction_required") {
        // Description: The client needs to perform some action due to a config from admin. 
        // Action: Capture `claims` parameter inside ex.InnerException.InnerException.
        // Generate HTTP error 403 with claims, throw back HTTP error to client.
        // Wait for client to retry. 
    }
} 
        
catch (AdalException e) {
    // Exception: AdalException 
    // Represents a library exception generated by ADAL .NET.
    // Error Code: e.ErrorCode

    // Action: Do not perform an immediate retry. Only try again after user action (if applicable) or wait until much later. 
    // Example Error: default case
}
```

Iremos compilou uma [exemplo completo](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof-ca) demonstra que neste cenário.

## <a name="error-and-logging-reference"></a>Registo de erros e de referência

### <a name="net"></a>.NET

#### <a name="adal-library-errors"></a>Erros de biblioteca da ADAL

Para explorar os erros específicos ADAL, o código de origem no [repositório azure-activedirectory-library-para-dotnet](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/8f6d560fbede2247ec0e217a21f6929d4375dcaa/src/ADAL.PCL/Utilities/Constants.cs#L58) é a melhor referência de erro.

#### <a name="guidance-for-error-logging-code"></a>Orientações para o código de registo de erro

Alterações de registo da ADAL .NET consoante a plataforma que está a ser trabalhados. Consulte o [registo documentação](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet#diagnostics) código sobre como ativar o registo.

### <a name="android"></a>Android

#### <a name="adal-library-errors"></a>Erros de biblioteca da ADAL

Para explorar os erros específicos ADAL, o código de origem no [repositório do azure activedirectory-library-para-android](https://github.com/AzureAD/azure-activedirectory-library-for-android/blob/dev/adal/src/main/java/com/microsoft/aad/adal/ADALError.java#L33) é a melhor referência de erro.

#### <a name="operating-system-errors"></a>Erros de sistema operativo

Erros de SO Android são expostos através de AuthenticationException na ADAL, serem identificáveis como "SERVER_INVALID_REQUEST" e podem ser mais granulares através de descrições de erro. As duas mensagens prominent que uma aplicação pode optar por mostrar a IU são:

- Erros SSL 
  - [Utilizador final está a utilizar o Chrome 53](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/SSL-Certificate-Validation-Issue)
  - [Cadeia de certificados tem um certificado marcado como extra transferir (utilizador tem de contactar o administrador de TI)](https://vkbexternal.partners.extranet.microsoft.com/VKBWebService/ViewContent.aspx?scid=KB;EN-US;3203929)
  - AC de raiz não é considerada fidedigna pelo dispositivo. Contacte o administrador de TI. 
- Erros relacionados com de rede 
  - [Problema potencialmente relacionado com a validação de certificado de SSL de rede](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/SSL-Certificate-Validation-Issue), pode tentar uma repetição único

#### <a name="guidance-for-error-logging-code"></a>Orientações para o código de registo de erro

```java
// 1. Configure Logger
Logger.getInstance().setExternalLogger(new ILogger() {    
    @Override   
    public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) { 
    // …
    // You can write this to logfile depending on level or errorcode.     
    writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);    
    }
}

// 2. Set the log level
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

// 3. Send logs to logcat.
adb logcat > "C:\logmsg\logfile.txt";
```

### <a name="ios"></a>iOS

#### <a name="adal-library-errors"></a>Erros de biblioteca da ADAL

Para explorar os erros específicos ADAL, o código de origem no [repositório azure-activedirectory-library-para-objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc/blob/dev/ADAL/src/ADAuthenticationError.m#L295) é a melhor referência de erro.

#### <a name="operating-system-errors"></a>Erros de sistema operativo

erros de iOS podem surgir durante o início de sessão quando os utilizadores utilizam vistas web e a natureza de autenticação. Isto pode dever-se a condições como erros SSL, tempos limite ou erros de rede:

- Para a partilha de elegibilidade, inícios de sessão não são persistentes e cache aparece em branco. Pode resolver, adicionando a seguinte linha de código para a keychain:`[[ADAuthenticationSettings sharedInstance] setSharedCacheKeychainGroup:nil];`
- Para o conjunto de NsUrlDomain de erros, as alterações de ação dependendo da lógica de aplicação. Consulte o [documentação de referência NSURLErrorDomain](https://developer.apple.com/documentation/foundation/nsurlerrordomain#declarations) para instâncias específicas que podem ser processadas.
- Consulte [ADAL problemas comuns de Obj-C](https://github.com/AzureAD/azure-activedirectory-library-for-objc#adauthenticationerror) para a lista de erros comuns mantida pela equipa do ADAL Objective-C.

#### <a name="guidance-for-error-logging-code"></a>Orientações para o código de registo de erro

```objc
// 1. Enable NSLogging
[ADLogger setNSLogging:YES];

// 2. Set the log level (if you want verbose)
[ADLogger setLevel:ADAL_LOG_LEVEL_VERBOSE];

// 3. Set up a callback block to simply print out
[ADLogger setLogCallBack:^(ADAL_LOG_LEVEL logLevel, NSString *message, NSString *additionalInformation, NSInteger errorCode, NSDictionary *userInfo) {
     NSString* log = [NSString stringWithFormat:@"%@ %@", message, additionalInformation];    
     NSLog(@"%@", log);
}];
```

### <a name="guidance-for-error-logging-code---javascript"></a>Orientações para o registo de erro código - JavaScript 

```javascript
0: Error1: Warning2: Info3: Verbose
window.Logging = {
    level: 3,
    log: function (message) {
        console.log(message);
    }
};
```
## <a name="related-content"></a>Conteúdo relacionado

* [Guia de AD para programadores do Azure][AAD-Dev-Guide]
* [Bibliotecas de autenticação do Azure AD][AAD-Auth-Libraries]
* [Cenários de autenticação do Azure AD][AAD-Auth-Scenarios]
* [Integrar aplicações com o Azure Active Directory][AAD-Integrating-Apps]

Utilize a secção de comentários que se segue para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.

[![Botão Iniciar sessão][AAD-Sign-In]][AAD-Sign-In]
<!--Reference style links -->
[AAD-Auth-Libraries]: ./active-directory-authentication-libraries.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AZURE-portal]: https://portal.azure.com

<!--Image references-->
[AAD-Sign-In]:./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png

