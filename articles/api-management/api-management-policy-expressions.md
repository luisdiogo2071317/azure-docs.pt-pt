---
title: "Expressões de política de gestão de API do Azure | Microsoft Docs"
description: "Saiba mais sobre as expressões de política na API Management do Azure."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: fb50ba3f292a390c45f1afe6259731d2b92cc335
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="api-management-policy-expressions"></a>Expressões de política de gestão de API
Sintaxe de expressões de política é c# 6.0. Cada expressão tem acesso ao implicitamente fornecido [contexto](api-management-policy-expressions.md#ContextVariables) variável e um permitido [subconjunto](api-management-policy-expressions.md#CLRTypes) dos tipos de .NET Framework.  
  
> [!TIP]
>  Para obter mais informações sobre as expressões de política, consulte o [expressões de política](https://azure.microsoft.com/documentation/videos/policy-expressions-in-azure-api-management/) vídeo.  
>   
>  Para demonstrações de configuração de políticas utilizando expressões de política, consulte [nuvem abrangem episódio 177: mais funcionalidades de gestão de API com Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Este vídeo contém as seguintes demonstrações de expressão de política:  
>   
>  -   10:30 - Consulte como fornecer informações de contexto ao seu serviço de back-end. Utilize o [defina o parâmetro de cadeia de consulta](api-management-transformation-policies.md#SetQueryStringParameter) e [cabeçalho de HTTP definir](api-management-transformation-policies.md#SetHTTPheader) políticas para fornecer estas informações. Às 12:10, não há uma demonstração da chamada de uma operação no portal do programador, onde poderá ver estas políticas no trabalho.  
> -   13:50 - ver como utilizar o [validar JWT](api-management-access-restriction-policies.md#ValidateJWT) política para pré-autorizar o acesso às operações de baseadas em afirmações de token. Reencaminhe rápido para 15:00 para ver como as políticas são configuradas no editor de políticas. Às 18:50, veja uma demonstração de chamar uma operação a partir do portal do programador com e sem o token de autorização necessário.  
> -   21:00 - utilize um [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) rastreio para ver como são avaliadas as políticas e os resultados desses avaliações.  
> -   25:25 - ver como utilizar expressões com o [aproveitar a cache](api-management-caching-policies.md#GetFromCache) e [arquivo para colocar em cache](api-management-caching-policies.md#StoreToCache) políticas para configurar a colocação em cache de resposta de API Management. Defina uma duração que corresponda a resposta a colocação em cache do serviço de back-end como especificado pelo serviço de cópia `Cache-Control` diretiva.  
> -   34:30 - Consulte como efetuar a filtragem de conteúdo. Remova os elementos de dados da resposta recebida do back-end com o [controlar o fluxo](api-management-advanced-policies.md#choose) e [definir corpo](api-management-transformation-policies.md#SetBody) políticas. Iniciar o 31:50 ver uma descrição geral do [a escuro Sky previsão API](https://developer.forecast.io/) utilizado para esta demonstração.  
> -   Para transferir as declarações de política utilizadas neste vídeo, consulte o [api management-samples/políticas](https://github.com/Azure/api-management-samples/tree/master/policies) repositório do github.  
  
  
##  <a name="Syntax"></a>Sintaxe  
 As expressões de instrução única estão incluídas no `@(expression)`, onde `expression` uma declaração de expressão do c# corretamente formado.  
  
 Expressões com múltiplas instruções estão incluídas no `@{expression}`. Todos os caminhos de código dentro de expressões com múltiplas instruções tem de terminar com um `return` instrução.  
  
##  <a name="PolicyExpressionsExamples"></a>Exemplos  
  
```  
@(true)  
  
@((1+1).ToString())  
  
@("Hi There".Length)  
  
@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)  
  
@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)  
  
@{   
  string value;   
  if (context.Request.Headers.TryGetValue("Authorization", out value))   
  {   
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));  
  }   
  else   
  {   
    return null;  
  }  
}  
```  
  
##  <a name="PolicyExpressionsUsage"></a>Utilização  
 As expressões podem ser utilizadas como valores de atributo ou valores de texto em qualquer API Management [políticas](api-management-policies.md) (a menos que a referência de política especifica caso contrário,).  
  
> [!IMPORTANT]
>  Quando utilizar expressões de política, há apenas limitada verificação das expressões de política quando a política estiver definida. As expressões são executadas pelo gateway em tempo de execução, quaisquer exceções geradas pelo resultado de expressões de política num erro de tempo de execução.  
  
##  <a name="CLRTypes"></a>Tipos de .NET framework permitidos em expressões de política  
 A tabela seguinte lista os tipos de .NET Framework e os respetivos membros que são permitidos em expressões de política.  
  
|Tipo CLR|Métodos suportados|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JArray|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JConstructor|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JContainer|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JObject|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JProperty|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JRaw|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JToken|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JTokenType|Todos os métodos são suportados|  
|Newtonsoft.Json.Linq.JValue|Todos os métodos são suportados|  
|System.Collections.Generic.IReadOnlyCollection<T\>|Todos|  
|System.Collections.Generic.IReadOnlyDictionary<TKey,  TValue>|Todos|  
|System.Collections.Generic.ISet<TKey, TValue>|Todos|  
|System.Collections.Generic.KeyValuePair<TKey,  TValue>|Chave, o valor|  
|System.Collections.Generic.List<TKey, TValue>|Todos|  
|System.Collections.Generic.Queue<TKey, TValue>|Todos|  
|System.Collections.Generic.Stack<TKey, TValue>|Todos|  
|System.Convert|Todos|  
|System.DateTime|Todos|  
|System.DateTimeKind|Utc|  
|System.DateTimeOffset|Todos|  
|System.Decimal|Todos|  
|System.Double|Todos|  
|System.Guid|Todos|  
|System.IEnumerable<T\>|Todos|  
|System.IEnumerator<T\>|Todos|  
|System.Int16|Todos|  
|System.Int32|Todos|  
|System.Int64|Todos|  
|System.Linq.Enumerable<T\>|Todos os métodos são suportados|  
|System.Math|Todos|  
|System.MidpointRounding|Todos|  
|System.Nullable<T\>|Todos|  
|System.Random|Todos|  
|System.SByte|Todos|  
|System.Security.Cryptography. HMACSHA384|Todos|  
|System.Security.Cryptography. HMACSHA512|Todos|  
|System.Security.Cryptography.HashAlgorithm|Todos|  
|System.Security.Cryptography.HMAC|Todos|  
|System.Security.Cryptography.HMACMD5|Todos|  
|System.Security.Cryptography.HMACSHA1|Todos|  
|System.Security.Cryptography.HMACSHA256|Todos|  
|System.Security.Cryptography.KeyedHashAlgorithm|Todos|  
|System.Security.Cryptography.MD5|Todos|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Todos|  
|System.Security.Cryptography.SHA1|Todos|  
|System.Security.Cryptography.SHA1Managed|Todos|  
|System.Security.Cryptography.SHA256|Todos|  
|System.Security.Cryptography.SHA256Managed|Todos|  
|System.Security.Cryptography.SHA384|Todos|  
|System.Security.Cryptography.SHA384Managed|Todos|  
|System.Security.Cryptography.SHA512|Todos|  
|System.Security.Cryptography.SHA512Managed|Todos|  
|System.Single|Todos|  
|System.String|Todos|  
|System.StringSplitOptions|Todos|  
|System.Text.Encoding|Todos|  
|System.Text.RegularExpressions.Capture|Valor de índice, comprimento,|  
|System.Text.RegularExpressions.CaptureCollection|Contagem de Item|  
|System.Text.RegularExpressions.Group|Capturas de êxito|  
|System.Text.RegularExpressions.GroupCollection|Contagem de Item|  
|System.Text.RegularExpressions.Match|Resultado vazio, grupos,|  
|System.Text.RegularExpressions.Regex|(Construtor), IsMatch, correspondência, correspondências, substituir|  
|System.Text.RegularExpressions.RegexOptions|Compilado, IgnoreCase, IgnorePatternWhitespace, múltiplas linhas, None, RightToLeft, Singleline|  
|System.TimeSpan|Todos|  
|System.Tuple|Todos|  
|System.UInt16|Todos|  
|System.UInt32|Todos|  
|System.UInt64|Todos|  
|System.Uri|Todos|  
|System.Xml.Linq.Extensions|Todos os métodos são suportados|  
|System.Xml.Linq.XAttribute|Todos os métodos são suportados|  
|System.Xml.Linq.XCData|Todos os métodos são suportados|  
|System.Xml.Linq.XComment|Todos os métodos são suportados|  
|System.Xml.Linq.XContainer|Todos os métodos são suportados|  
|System.Xml.Linq.XDeclaration|Todos os métodos são suportados|  
|System.Xml.Linq.XDocument|Todos os métodos são suportados|  
|System.Xml.Linq.XDocumentType|Todos os métodos são suportados|  
|System.Xml.Linq.XElement|Todos os métodos são suportados|  
|System.Xml.Linq.XName|Todos os métodos são suportados|  
|System.Xml.Linq.XNamespace|Todos os métodos são suportados|  
|System.Xml.Linq.XNode|Todos os métodos são suportados|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Todos os métodos são suportados|  
|System.Xml.Linq.XNodeEqualityComparer|Todos os métodos são suportados|  
|System.Xml.Linq.XObject|Todos os métodos são suportados|  
|System.Xml.Linq.XProcessingInstruction|Todos os métodos são suportados|  
|System.Xml.Linq.XText|Todos os métodos são suportados|  
|System.Xml.XmlNodeType|Todos|  
  
##  <a name="ContextVariables"></a>Variável de contexto  
 Uma variável com o nome `context` está implicitamente disponível em cada política [expressão](api-management-policy-expressions.md#Syntax). Os seus membros fornecem informações relevantes para o `\request`. Todas as `context` membros são só de leitura.  
  
|Variável de contexto|Permitidos métodos, propriedades e valores de parâmetros|  
|----------------------|-------------------------------------------------------|  
|context|Api: IApi<br /><br /> Implementação<br /><br /> Decorrido: TimeSpan - intervalo de tempo entre o valor de Timestamp e a hora atual<br /><br /> LastError<br /><br /> Operação<br /><br /> Produto<br /><br /> Pedir<br /><br /> RequestId: Guid-Identificador exclusivo de pedido<br /><br /> Resposta<br /><br /> Subscrição<br /><br /> Timestamp: DateTime - ponto no tempo quando o pedido foi recebido<br /><br /> Rastreio: bool - indica se o rastreio está ligado ou desligado <br /><br /> Utilizador<br /><br /> Variáveis: IReadOnlyDictionary < cadeia, objecto ><br /><br /> void Trace(message: string)|  
|context.Api|ID: cadeia<br /><br /> IsRevisionCurrent: bool<br /><br />  Nome: cadeia<br /><br /> Caminho: cadeia<br /><br /> Revisão: cadeia<br /><br /> ServiceUrl: IUrl<br /><br /> Versão: cadeia |  
|context.Deployment|Região: cadeia<br /><br /> ServiceName: string<br /><br /> Certificates: IReadOnlyDictionary<string, X509Certificate2>|  
|context.LastError|Origem: cadeia<br /><br /> Razão: cadeia<br /><br /> Mensagem: cadeia<br /><br /> Âmbito: cadeia<br /><br /> Secção: cadeia<br /><br /> Caminho: cadeia<br /><br /> PolicyId: string<br /><br /> Para obter mais informações sobre o contexto. LastError, consulte [processamento de erros](api-management-error-handling-policies.md).|  
|context.Operation|ID: cadeia<br /><br /> Método: cadeia<br /><br /> Nome: cadeia<br /><br /> UrlTemplate: cadeia|  
|context.Product|APIs: IEnumerable < IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Grupos: IEnumerable < IGroup\><br /><br /> ID: cadeia<br /><br /> Nome: cadeia<br /><br /> Estado: enumeração ProductState {NotPublished, publicada}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|  
|context.Request|Corpo: IMessageBody<br /><br /> Certificate: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Headers: IReadOnlyDictionary<string, string[]><br /><br /> IpAddress: cadeia<br /><br /> MatchedParameters: IReadOnlyDictionary<string, string><br /><br /> Método: cadeia<br /><br /> OriginalUrl:IUrl<br /><br /> Url: IUrl|  
|string context.Request.Headers.GetValueOrDefault(headerName: string, defaultValue: string)|headerName: string<br /><br /> defaultValue: string<br /><br /> Devolve os valores de cabeçalho de pedido de valores separados por vírgulas ou `defaultValue` se não for encontrado o cabeçalho.|  
|context.Response|Corpo: IMessageBody<br /><br /> Headers: IReadOnlyDictionary<string, string[]><br /><br /> StatusCode: int<br /><br /> StatusReason: cadeia|  
|string context.Response.Headers.GetValueOrDefault(headerName: string, defaultValue: string)|headerName: string<br /><br /> defaultValue: string<br /><br /> Devolve os valores de cabeçalho de resposta de valores separados por vírgulas ou `defaultValue` se não for encontrado o cabeçalho.|  
|context.Subscription|CreatedTime: DateTime<br /><br /> EndDate: DateTime?<br /><br /> ID: cadeia<br /><br /> Chave: cadeia<br /><br /> Nome: cadeia<br /><br /> PrimaryKey: string<br /><br /> SecondaryKey: string<br /><br /> StartDate: DateTime?|  
|context.User|E-mail: cadeia<br /><br /> FirstName: string<br /><br /> Grupos: IEnumerable < IGroup\><br /><br /> ID: cadeia<br /><br /> Identidades: IEnumerable < IUserIdentity\><br /><br /> LastName: string<br /><br /> Nota: cadeia<br /><br /> RegistrationDate: DateTime|  
|IApi|ID: cadeia<br /><br /> Nome: cadeia<br /><br /> Caminho: cadeia<br /><br /> Protocolos: IEnumerable < cadeia\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|ID: cadeia<br /><br /> Nome: cadeia|  
|IMessageBody|Como < T\>(preserveContent: bool = false): onde t: Temporary cadeia, JObject JToken, JArray, XNode, XElement, XDocument<br /><br /> O `context.Request.Body.As<T>` e `context.Response.Body.As<T>` métodos utilizados para ler um pedido e resposta corpos de mensagem num tipo especificado `T`. Por predefinição, o método utiliza o fluxo do corpo de mensagem original e composições indisponível depois devolve. Para evitar que fazendo com que o método funcionar numa cópia do fluxo do corpo, defina o `preserveContent` parâmetro `true`. Aceda [aqui](api-management-transformation-policies.md#SetBody) para ver um exemplo.|  
|IUrl|Host: string<br /><br /> Caminho: cadeia<br /><br /> Porta: int<br /><br /> Query: IReadOnlyDictionary<string, string[]><br /><br /> QueryString: cadeia<br /><br /> Esquema: cadeia|  
|IUserIdentity|ID: cadeia<br /><br /> Fornecedor: cadeia|  
|ISubscriptionKeyParameterNames|Cabeçalho: cadeia<br /><br /> Consulta: cadeia|  
|string IUrl.Query.GetValueOrDefault(queryParameterName: string, defaultValue: string)|queryParameterName: string<br /><br /> defaultValue: string<br /><br /> Devolve os valores de parâmetros de consulta de valores separados por vírgulas ou `defaultValue` se o parâmetro não for encontrado.|  
|T context.Variables.GetValueOrDefault<T\>(variableName: string, defaultValue: T)|variableName: string<br /><br /> defaultValue: T<br /><br /> Devolve o valor da variável convertido no tipo `T` ou `defaultValue` se a variável não for encontrada.<br /><br /> Este método emite uma exceção se o tipo especificado não corresponde ao tipo real da variável devolvido.|  
|BasicAuthCredentials AsBasic(input: this string)|entrada: cadeia<br /><br /> Se o parâmetro de entrada contém um valor de cabeçalho do pedido de autorização de autenticação básica de HTTP válido, o método devolve um objeto do tipo `BasicAuthCredentials`; caso contrário, o método devolve um valor nulo.|  
|bool TryParseBasic (entrada: Esta cadeia, o resultado: saída BasicAuthCredentials)|entrada: cadeia<br /><br /> resultado: saída BasicAuthCredentials<br /><br /> Se o parâmetro de entrada contém um valor válido para a autorização de autenticação básica de HTTP no cabeçalho do pedido o método devolve `true` e o parâmetro de resultado contém um valor de tipo `BasicAuthCredentials`; caso contrário, devolve o método `false`.|  
|BasicAuthCredentials|Palavra-passe: cadeia<br /><br /> UserId: cadeia|  
|Jwt AsJwt(input: this string)|entrada: cadeia<br /><br /> Se o parâmetro de entrada contém um valor de token válido JWT, o método devolve um objeto do tipo `Jwt`; caso contrário, devolve o método `null`.|  
|bool TryParseJwt (entrada: Esta cadeia, o resultado: saída Jwt)|entrada: cadeia<br /><br /> resultado: saída Jwt<br /><br /> Se o parâmetro de entrada contém um valor de token válido JWT, o método devolve `true` e o parâmetro de resultado contém um valor de tipo `Jwt`; caso contrário, devolve o método `false`.|  
|Jwt|Algoritmo: cadeia<br /><br /> Audiência: IEnumerable < cadeia\><br /><br /> Claims: IReadOnlyDictionary<string, string[]><br /><br /> ExpirationTime: DateTime?<br /><br /> ID: cadeia<br /><br /> Emissor: cadeia<br /><br /> NotBefore: DateTime?<br /><br /> Requerente: cadeia<br /><br /> Tipo: cadeia|  
|string Jwt.Claims.GetValueOrDefault(claimName: string, defaultValue: string)|claimName: string<br /><br /> defaultValue: string<br /><br /> Devolve separados por vírgulas de valores de afirmação ou `defaultValue` se não for encontrado o cabeçalho.|
|byte [] encriptar (entrada: esta byte [], alg: cadeia, a chave: byte [], iv:byte[])|entrada - texto simples para ser encriptada<br /><br />alg - nome de um algoritmo de encriptação simétrica<br /><br />chave - chave de encriptação<br /><br />IV - vetor de inicialização<br /><br />Devolve texto não encriptado.|
|byte [] encriptar (entrada: esta byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|entrada - texto simples para ser encriptada<br /><br />alg - algoritmo de encriptação<br /><br />Devolve texto não encriptado.|
|byte [] encriptar (entrada: esta byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, chave: byte [], iv:byte[])|entrada - texto simples para ser encriptada<br /><br />alg - algoritmo de encriptação<br /><br />chave - chave de encriptação<br /><br />IV - vetor de inicialização<br /><br />Devolve texto não encriptado.|
|byte [] desencriptar (entrada: esta byte [], alg: cadeia, a chave: byte [], iv:byte[])|entrada - texto de cifra a desencriptar<br /><br />alg - nome de um algoritmo de encriptação simétrica<br /><br />chave - chave de encriptação<br /><br />IV - vetor de inicialização<br /><br />Devolve texto simples.|
|byte [] desencriptar (entrada: esta byte [], alg: System.Security.Cryptography.SymmetricAlgorithm)|entrada - texto de cifra a desencriptar<br /><br />alg - algoritmo de encriptação<br /><br />Devolve texto simples.|
|byte [] desencriptar (entrada: esta byte [], alg: System.Security.Cryptography.SymmetricAlgorithm, chave: byte [], iv:byte[])|texto de cifra - entrada - entrada a desencriptar<br /><br />alg - algoritmo de encriptação<br /><br />chave - chave de encriptação<br /><br />IV - vetor de inicialização<br /><br />Devolve texto simples.|

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player] 
>
## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações para trabalhar com as políticas, consulte:

+ [Políticas de gestão de API](api-management-howto-policies.md)
+ [APIs de transformação](transform-api.md)
+ [Referência de política](api-management-policy-reference.md) para uma lista completa das declarações de política e as respetivas definições
+ [Exemplos de política](policy-samples.md)   
