---
title: Token de referência no Azure Active Directory B2C | Documentos da Microsoft
description: Os tipos de tokens emitidos no Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 81927c4e69f36fbc4859ac2865bc15cbfacb17fb
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843874"
---
# <a name="azure-ad-b2c-token-reference"></a>Do Azure AD B2C: Referência de Token

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

O Azure Active Directory B2C (Azure AD B2C) emite vários tipos de tokens de segurança, à medida que processa cada [fluxo de autenticação](active-directory-b2c-apps.md). Este documento descreve o formato, características de segurança e conteúdo de cada tipo de token.

## <a name="types-of-tokens"></a>Tipos de tokens
O Azure AD B2C suporta o [protocolo de autorização de OAuth 2.0](active-directory-b2c-reference-protocols.md), que faz uso dos dois tokens de acesso e tokens de atualização. Também suporta a autenticação e início de sessão por meio [OpenID Connect](active-directory-b2c-reference-protocols.md), que introduz um terceiro tipo de token: o token de ID. Cada um destes tokens é representada como um token de portador.

Um token de portador é um token de segurança simples que concede o acesso de "bearer" a um recurso protegido. O portador é capaz de apresentar o token de terceiros. O Azure AD B2C devem primeiro autenticar uma parte antes de pode receber um token de portador. Mas se os passos necessários não serão direcionados para proteger o token na transmissão e o armazenamento, pode ser interceptado e utilizado por uma entidade não-intencionais. Alguns tokens de segurança tem um mecanismo interno para impedir que partes não autorizadas a utilizá-los, mas os tokens de portador não tem esse mecanismo. Eles devam ser transportados num canal seguro, tais como a segurança de camada de transporte (HTTPS).

Se um token de portador é transmitido fora de um canal seguro, mal-intencionados podem utilizar um ataque man-in-the-middle adquirir o token e utilizá-lo a obter acesso não autorizado a um recurso protegido. Os mesmos princípios de segurança aplicam-se quando os tokens de portador são armazenados ou colocados em cache para utilização posterior. Certifique-se sempre de que a aplicação transmite e armazena os tokens de portador de forma segura.

Para considerações de segurança adicionais em tokens de portador, consulte [RFC 6750 secção 5](https://tools.ietf.org/html/rfc6750).

Muitos dos tokens emitidos pelo Azure AD B2C são implementados como tokens de web JSON (JWTs). Um JWT é um meio de compacto e seguras para URL de transferência de informações entre duas partes. JWTs contêm informações conhecidas como afirmações. Estas são algumas hipóteses de obter informações sobre o portador e o assunto do token. As afirmações no JWTs são objetos JSON, que são codificados e serializados para transmissão. Porque JWTs emitidos pelo Azure AD B2C são assinados, mas não encriptados, pode inspecionar o conteúdo de um JWT para depurá-lo facilmente. Estão disponíveis várias ferramentas que pode fazê-lo, incluindo [jwt.ms](https://jwt.ms). Para obter mais informações sobre JWTs, consulte [especificações JWT](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Tokens de ID

Um token de ID é uma forma de token de segurança que a aplicação recebe do Azure AD B2C `/authorize` e `/token` pontos de extremidade. Tokens de ID são representados como [JWTs](#types-of-tokens), e que contêm afirmações que pode utilizar para identificar os utilizadores na sua aplicação. Quando os tokens de ID são adquiridos do `/authorize` ponto de extremidade, eles são efetuados utilizando o [fluxo implícito](active-directory-b2c-reference-spa.md), que é utilizado frequentemente para os utilizadores iniciar sessão no javascript baseado em aplicativos web. Quando os tokens de ID são adquiridos do `/token` ponto de extremidade, eles são efetuados utilizando o [fluxo de código confidenciais](active-directory-b2c-reference-oidc.md), que mantém o token ocultado a partir do browser. Isso permite que o token a ser enviadas de forma segura em pedidos de HTTP para a comunicação entre dois componentes do mesmo aplicativo ou serviço. Pode utilizar as declarações no token de ID como quiser. Elas são comumente usadas para exibir informações de conta ou para tomar decisões de controlo de acesso numa aplicação.  

Tokens de ID tem sessão iniciadas, mas eles não são atualmente criptografados. Quando a sua aplicação ou a API recebe um token de ID, deve [validar a assinatura](#token-validation) para provar que o token é autêntico. A aplicação ou API também deve validar algumas declarações no token para provar que é válido. Consoante os requisitos de cenário, as afirmações validadas por uma aplicação podem variar, mas a aplicação tem de efetuar algumas [validações de afirmação comuns](#token-validation) em todos os cenários.

#### <a name="sample-id-token"></a>Token de ID de exemplo
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Tokens de acesso

Um token de acesso também é uma forma de token de segurança que a aplicação recebe do Azure AD B2C `/authorize` e `/token` pontos de extremidade. Tokens de acesso também são representados como [JWTs](#types-of-tokens), e que contêm afirmações que pode utilizar para identificar as permissões concedidas às suas APIs. Tokens de acesso tem sessão iniciados, mas eles não são atualmente criptografados. Tokens de acesso devem ser utilizados para fornecer acesso aos servidores APIs e recursos. Saiba mais sobre como [utilizar tokens de acesso](active-directory-b2c-access-tokens.md). 

Quando a API recebe um token de acesso, tem [validar a assinatura](#token-validation) para provar que o token é autêntico. A API também deve validar algumas declarações no token para provar que é válido. Consoante os requisitos de cenário, as afirmações validadas por uma aplicação podem variar, mas a aplicação tem de efetuar algumas [validações de afirmação comuns](#token-validation) em todos os cenários.

### <a name="claims-in-id-and-access-tokens"></a>Afirmações nos tokens de acesso e ID

Quando utilizar o Azure AD B2C, terá um controle refinado sobre o conteúdo dos seus tokens. Pode configurar [fluxos de utilizador](active-directory-b2c-reference-policies.md) e as políticas personalizadas para enviar determinados conjuntos de dados de utilizador nas afirmações que a sua aplicação necessita para suas operações. Essas declarações podem incluir propriedades padrão, como o usuário `displayName` e `emailAddress`. Eles também podem incluir [atributos de utilizador personalizada](active-directory-b2c-reference-custom-attr.md) que pode definir no diretório do B2C. Cada acesso e o ID de token que recebe contém um determinado conjunto de declarações relacionadas à segurança. Seus aplicativos podem usar essas declarações de forma a autenticar os utilizadores e pedidos.

Tenha em atenção que as afirmações nos tokens de ID não são devolvidas em qualquer ordem específica. Além disso, podem ser introduzidas nova afirmações nos tokens de ID em qualquer altura. A aplicação não deve interromper conforme novas declarações são introduzidas. Aqui estão as afirmações que espera existe nos tokens de acesso e de ID emitidos pelo Azure AD B2C. Qualquer afirmações adicionais são determinadas por políticas. Para a prática, tente inspecionar as afirmações no token de ID de exemplo através da colagem-lo para [jwt.ms](https://jwt.ms). Detalhes adicionais podem ser encontrados no [especificação do OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html).

| Nome | Afirmação | Valor de exemplo | Descrição |
| --- | --- | --- | --- |
| Audiência |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |Uma afirmação de audiência identifica o destinatário do token. Para o Azure AD B2C, o público-alvo é a ID da aplicação da sua aplicação, como atribuído à sua aplicação no portal de registo de aplicação. A aplicação deve validar este valor e rejeitar o token, se não corresponde. Público-alvo é sinônimo de recursos. |
| Emissor |`iss` |`https://{tenantname}.b2clogin.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |Esta afirmação identifica o serviço de token de segurança (STS) que constrói e devolve o token. Ele também identifica o diretório do Azure AD em que o utilizador foi autenticado. A aplicação deve validar a afirmação do emissor para se certificar de que o token provém do ponto de final de v2.0 do Azure Active Directory. |
| Emitida em |`iat` |`1438535543` |Esta afirmação é a hora em que o token foi emitido, representado na hora "Epoch". |
| Hora de expiração |`exp` |`1438539443` |Hora de expiração da afirmação é a hora em que o token se torna inválido, representado na hora "Epoch". A aplicação deve utilizar esta afirmação para verificar a validade da duração do token. |
| Não antes |`nbf` |`1438535543` |Esta afirmação é a hora em que o token se torna válido, representado na hora "Epoch". Isso normalmente é o mesmo que o tempo que o token foi emitido. A aplicação deve utilizar esta afirmação para verificar a validade da duração do token. |
| Versão |`ver` |`1.0` |Esta é a versão do token de ID, conforme definido pelo Azure AD. |
| Hash de código |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Um hash de código está incluído num token de ID apenas quando o token for emitido, juntamente com um código de autorização de OAuth 2.0. Um hash de código pode ser utilizado para validar a autenticidade de um código de autorização. Para obter mais detalhes sobre como efetuar esta validação, consulte a [especificação do OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html).  |
| Hash de token de acesso |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Um hash de token de acesso está incluído num token de ID apenas quando o token for emitido, juntamente com um token de acesso de OAuth 2.0. Um hash de token de acesso pode ser utilizado para validar a autenticidade de um token de acesso. Para obter mais detalhes sobre como efetuar esta validação, consulte o [especificação OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html)  |
| Valor de uso único |`nonce` |`12345` |Um valor de uso único é uma estratégia utilizada para mitigar ataques de repetição de token. A aplicação, pode especificar um valor de uso único numa solicitação de autorização utilizando a `nonce` parâmetro de consulta. O valor que fornecer no pedido será emitido sem modificações no `nonce` de afirmação de apenas um token de ID. Isso permite que a sua aplicação verificar o valor com o valor que ele especificado na solicitação, que associa sessão da aplicação um determinado token de ID. A aplicação deve realizar esta validação durante o processo de validação do token de ID. |
| Requerente |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Este é o principal sobre o qual o token declara informações, como o utilizador de uma aplicação. Este valor é imutável e não pode ser reatribuído ou reutilizado. Ele pode ser usado para realizar verificações de autorização com segurança, por exemplo, quando o token é utilizado para aceder a um recurso. Por predefinição, a afirmação do requerente é preenchida com o ID de objeto do utilizador no diretório. Para obter mais informações, consulte [do Azure Active Directory B2C: Token, sessão e configuração de início de sessão único](active-directory-b2c-token-session-sso.md). |
| Referência de classe de contexto de autenticação |`acr` |Não aplicável |Não utilizado atualmente, exceto no caso de políticas mais antigas. Para obter mais informações, consulte [do Azure Active Directory B2C: Token, sessão e configuração de início de sessão único](active-directory-b2c-token-session-sso.md). |
| Política de estrutura de confiança |`tfp` |`b2c_1_sign_in` |Este é o nome da política que foi utilizado para adquirir o token de ID. |
| Tempo de autenticação |`auth_time` |`1438535543` |Esta afirmação é a hora em que um último introduzido credenciais de utilizador, representado na hora "Epoch". |

### <a name="refresh-tokens"></a>Tokens de atualização
Atualizar os tokens são tokens de segurança que seu aplicativo pode usar para adquirir novos tokens de ID e tokens num fluxo de OAuth 2.0 de acesso. Eles fornecem a aplicação com acesso de longo prazo para recursos em nome dos utilizadores sem a necessidade de interação com esses utilizadores.

Para receber uma atualização de token numa resposta de token, a aplicação tem de solicitar o `offline_acesss` âmbito. Para saber mais sobre o `offline_access` escopo, consulte a [referência do protocolo do Azure AD B2C](active-directory-b2c-reference-protocols.md).

Tokens de atualização e sempre serão, completamente opaco para a sua aplicação. São emitidos pelo Azure AD e podem ser inspecionadas e interpretados apenas pelo Azure AD. Eles são vida longa, mas a aplicação não deve ser escrita com a expectativa de que um token de atualização duram um determinado período de tempo. Tokens de atualização podem ser invalidadas a qualquer momento para uma variedade de motivos. A única forma para a sua aplicação saber se um token de atualização é válido é tentar resgatá-lo ao fazer um pedido de token para o Azure AD.

Quando utilizar um token de atualização para um novo token (e se a sua aplicação tenha sido concedida a `offline_access` âmbito), receberá um novo token de atualização na resposta de token. Deve guardar o token de atualização recém-emitidos. Ele deve substituir o utilizou anteriormente no pedido de token de atualização. Isto ajuda a garantir que seus tokens de atualização mantêm-se válidas durante mais tempo.

## <a name="token-validation"></a>Validação do token
Para validar um token, seu aplicativo deve verificar a assinatura e de declarações do token.

Muitas bibliotecas de código-fonte aberto estão disponíveis para validar JWTs, dependendo do seu idioma preferencial. Recomendamos que explorar essas opções em vez de implementar sua própria lógica de validação. As informações neste guia podem ajudar a aprender como usar adequadamente dessas bibliotecas.

### <a name="validate-the-signature"></a>Validar a assinatura
Um JWT contém três segmentos, separados pelo `.` caráter. O primeiro segmento é o *cabeçalho*, o segundo é o *corpo*, e o terceiro é o *assinatura*. O segmento de assinatura pode ser utilizado para validar a autenticidade do token, para que podem ser fidedignos pela sua aplicação.

O Azure AD B2C tokens são assinados com algoritmos de criptografia assimétrica de norma da indústria, como RSA 256. O cabeçalho do token contém informações sobre o método de encriptação e a chave utilizada para assinar o token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

O `alg` afirmação indica o algoritmo que foi utilizado para assinar o token. O `kid` afirmação indica a chave pública específica que foi utilizada para assinar o token.

Em qualquer momento, do Azure AD pode iniciar sessão um token ao utilizar qualquer um de um determinado conjunto de pares de chaves públicas-privadas. Do Azure AD gira periodicamente, o conjunto possível de chaves, para que a sua aplicação deve ser escrita para lidar com essas alterações principais automaticamente. Uma frequência razoável para procurar atualizações para as chaves públicas usadas pelo Azure AD é a cada 24 horas.

O Azure AD B2C tem um ponto final de metadados OpenID Connect. Isso permite que as aplicações obter informações sobre o Azure AD B2C no tempo de execução. Estas informações incluem pontos finais, conteúdo de token e chaves de assinatura de tokens. O diretório do B2C contém um documento de metadados JSON para cada política. Por exemplo, o documento de metadados para o `b2c_1_sign_in` política no `fabrikamb2c.onmicrosoft.com` está localizado em:

```
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com` é o diretório de B2C utilizado para autenticar o usuário, e `b2c_1_sign_in` é a política utilizada para adquirir o token. Para determinar qual a política foi utilizada para assinar um token (e onde posso para obter os metadados), tem duas opções. Em primeiro lugar, o nome da política está incluído no `acr` de afirmações no token. Pode analisar afirmações fora do corpo do JWT por base-64 decodificar o corpo e a cadeia de caracteres do JSON que resulta ao anular a serialização. O `acr` afirmação será o nome da política que foi utilizado para emitir o token.  A outra opção é codificar a política no valor do `state` parâmetro quando emitir o pedido e, em seguida, decodificá-la para determinar qual a política foi utilizada. Qualquer um dos métodos é válido.

O documento de metadados é um objeto JSON que contém várias partes úteis de informações. Estes incluem a localização dos pontos finais necessários para efetuar a autenticação OpenID Connect. Eles também incluem `jwks_uri`, que dá a localização do conjunto de chaves públicas que são utilizados para assinar os tokens. Que a localização é fornecida aqui, mas é melhor obter a localização dinamicamente usando o documento de metadados e analisando `jwks_uri`:

```
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

O documento JSON localizado neste URL contém todas as informações de chaves públicas em uso num momento específico. A aplicação pode utilizar o `kid` no cabeçalho do JWT para selecionar a chave pública no documento JSON que é utilizado para assinar um determinado token de afirmação. Em seguida, ele pode realizar validação da assinatura usando a chave pública correta e o algoritmo indicado.

Uma descrição de como realizar a validação de assinatura está fora do escopo deste documento. Muitas bibliotecas de código-fonte aberto estão disponíveis para ajudá-lo com isso se precisar dele.

### <a name="validate-the-claims"></a>Validar as afirmações
Quando a sua aplicação ou a API recebe um token de ID, ele também deve executar diversas verificações contra as afirmações no token de ID. Esses incluem, mas não sejam limitam a:

* O **público-alvo** de afirmação: Isto verifica que o token de ID destinava-se a ser dado à sua aplicação.
* O **não antes** e **hora de expiração** afirmações: estes Certifique-se de que o token de ID não expirou.
* O **emissor** de afirmação: Isto verifica se o token foi emitido para a sua aplicação pelo Azure AD.
* O **nonce**: Esta é uma estratégia de mitigação de ataques de repetição de token.

Para obter uma lista completa de validações deve executar a sua aplicação, consulte a [especificação do OpenID Connect](https://openid.net). Detalhes dos valores esperados para essas declarações estão incluídas no precedente [secção do token](#types-of-tokens).  

## <a name="token-lifetimes"></a>Durações de token
Durações de token seguintes são fornecidas para aumentar seu conhecimento. Eles podem ajudá-lo ao desenvolver e depurar aplicações. Tenha em atenção que as suas aplicações não devem ser escritas esperar qualquer um desses tempos de vida permaneça constante. Podem e será alterado. Leia mais sobre o [personalização das durações dos tokens](active-directory-b2c-token-session-sso.md) no Azure AD B2C.

| Certificado de | Tempo de vida | Descrição |
| --- | --- | --- |
| Tokens de ID |Uma hora |Tokens de ID são normalmente válidos durante uma hora. A aplicação web pode utilizar este tempo de vida para manter o seus próprio sessões com os utilizadores (recomendados). Também pode escolher uma duração de sessão diferente. Se a sua aplicação precisar de obter um novo ID de token, ele simplesmente precisa fazer um novo pedido de início de sessão para o Azure AD. Se um utilizador tiver uma sessão do browser válido com o Azure AD, que o utilizador não poderá ser necessário para introduzir as credenciais novamente. |
| Tokens de atualização |Até 14 dias |Um token de atualização é válido para um máximo de 14 dias. No entanto, um token de atualização pode se tornar inválido a qualquer momento por uma série de motivos. A aplicação deve continuar a tentar utilizar um token de atualização até que o pedido falhar, ou até que a sua aplicação substitui o token de atualização por um novo. Um token de atualização também pode se tornar inválido se 90 dias tiver passado, uma vez que o usuário inserir pela última vez credenciais. |
| Códigos de autorização |Cinco minutos |Códigos de autorização são intencionalmente curta duração. Eles devem ser resgatados imediatamente para tokens de acesso, os tokens de ID ou tokens de atualização quando eles são recebidos. |

