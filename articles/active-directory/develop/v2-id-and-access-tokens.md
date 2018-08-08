---
title: Referência de tokens do Azure Active Directory v2.0 | Documentos da Microsoft
description: Os tipos de tokens e afirmações emitidas pelo ponto final v2.0 do Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 815311797e1897259b961debc8a0f81157495570
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39596505"
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Referência de tokens de v2.0 do Azure Active Directory
O ponto de final de v2.0 do Azure Active Directory (Azure AD) emite vários tipos de tokens de segurança em cada [fluxo de autenticação](v2-app-types.md). Esta referência descreve o formato, características de segurança e conteúdo de cada tipo de token.

> [!NOTE]
> O ponto final v2.0 não suporta todos os cenários do Azure Active Directory e funcionalidades. Para determinar se deve utilizar o ponto final v2.0, leia sobre [v2.0 limitações](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>Tipos de tokens
O ponto final v2.0 oferece suporte a [protocolo de autorização de OAuth 2.0](active-directory-v2-protocols.md), que utiliza tokens de acesso e tokens de atualização. O ponto final v2.0 também suporta a autenticação e início de sessão por meio [OpenID Connect](active-directory-v2-protocols.md). OpenID Connect introduz um terceiro tipo de token, o token de ID. Cada um destes tokens é representada como um *portador* token.

Um token de portador é um token de segurança simples que concede o acesso de portador a um recurso protegido. O portador é capaz de apresentar o token de terceiros. Embora uma entidade tem de ser autenticado com o Azure AD para receber o token de portador, se não são tidas em passos para proteger o token durante a transmissão e o armazenamento, podem ser intercetado e utilizado por uma entidade não-intencionais. Alguns tokens de segurança tem um mecanismo interno para evitar que partes não autorizadas a utilizá-los, mas não os tokens de portador. Os tokens de portador devam ser transportados num canal seguro, como a segurança de camada de transporte (HTTPS). Se um token de portador é transmitido sem esse tipo de segurança, mal-intencionados poderiam usar um ataque"man-in-the-middle" para adquirir o token e utilizá-lo para o acesso não autorizado a um recurso protegido. Os mesmos princípios de segurança se aplicam ao armazenar ou de colocação em cache os tokens de portador para utilização posterior. Certifique-se sempre de que a aplicação em segurança transmite de colunas e armazena os tokens de portador. Para obter mais considerações de segurança para os tokens de portador, consulte [RFC 6750 secção 5](http://tools.ietf.org/html/rfc6750).

Muitos dos tokens emitidos pelo ponto final v2.0 são implementados como JSON Web Tokens (JWTs). Um JWT é uma forma segura para URL compact para transferir informações entre duas partes. As informações num JWT são chamadas um *afirmação*. É uma asserção de informações sobre o portador e o assunto do token. As declarações num JWT são objetos de JavaScript Object Notation (JSON) que são codificados e serializados para transmissão. Porque JWTs emitidos pelo ponto final v2.0 são assinados, mas não encriptados, pode facilmente inspecionar o conteúdo de um JWT para fins de depuração. Para obter mais informações sobre JWTs, consulte a [especificação do JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Tokens de ID
Um token de ID é uma forma de token de segurança de início de sessão que a aplicação recebe quando efetua a autenticação utilizando [OpenID Connect](active-directory-v2-protocols.md). Tokens de ID são representados como [JWTs](#types-of-tokens), e que contêm afirmações que pode utilizar para a sessão do utilizador para a sua aplicação. Pode utilizar as declarações num token de ID de várias formas. Normalmente, os administradores utilizar tokens de ID para exibir informações de conta ou para tomar decisões de controlo de acesso numa aplicação. O ponto final v2.0 emite apenas um tipo de token de ID, que tem um conjunto consistente de afirmações, independentemente do tipo de utilizador que tem sessão iniciada. O formato e conteúdo de tokens de ID são os mesmos para utilizadores com contas Microsoft pessoais e para contas escolares ou profissionais.

Atualmente, os tokens de ID são assinados, mas não encriptados. Quando a aplicação recebe um token de ID, deve [validar a assinatura](#validating-tokens) para provar a autenticidade do token e validar algumas declarações no token para provar a sua validade. As declarações validadas por uma aplicação variam consoante os requisitos de cenários, mas a sua aplicação tem de efetuar algumas [validações de afirmação comuns](#validating-tokens) em todos os cenários.

Damos-lhe os detalhes completos sobre afirmações nos tokens de ID nas seções a seguir, além de um token de ID de exemplo. Tenha em atenção que afirmações nos tokens de ID não são devolvidas por uma ordem específica. Além disso, as novas declarações podem ser introduzidas em tokens de ID em qualquer altura. A aplicação não deve interromper quando são introduzidas novas declarações. A lista seguinte inclui as afirmações que a aplicação atualmente confiável pode interpretar. Pode encontrar mais detalhes a [especificação do OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Token de ID de exemplo
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Para a prática, para inspecionar as afirmações no token de ID de exemplo, cole o token de ID de exemplo para [jwt.ms](http://jwt.ms/).
>
>

#### <a name="claims-in-id-tokens"></a>Afirmações nos tokens de ID
| Nome | Afirmação | Valor de exemplo | Descrição |
| --- | --- | --- | --- |
| Público-alvo |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Identifica o destinatário do token. Nos tokens de ID, o público-alvo é a ID da aplicação da sua aplicação, atribuído à sua aplicação no Portal de registo de aplicação do Microsoft. A aplicação deve validar este valor e rejeitar o token, se o valor não corresponde. |
| Emissor |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Identifica o serviço de token de segurança (STS) que constrói e devolve o token e o inquilino do Azure AD em que o utilizador foi autenticado. A aplicação deve validar a afirmação do emissor para se certificar de que o token provém o ponto final v2.0. Ele também deve usar a parte GUID da afirmação para restringir o conjunto de inquilinos que pode iniciar sessão aplicação. O GUID que indica que o utilizador é um utilizador de consumidor de uma conta Microsoft é `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Emitida em |`iat` |`1452285331` |O tempo em que o token foi emitido, representado na hora "Epoch". |
| Hora de expiração |`exp` |`1452289231` |O tempo em que o token se torna inválido, representado na hora "Epoch". A aplicação deve utilizar esta afirmação para verificar a validade da duração do token. |
| Não antes |`nbf` |`1452285331` |A hora em que o token se torna válido, representado na hora "Epoch". Normalmente, é o mesmo que a hora de emissão. A aplicação deve utilizar esta afirmação para verificar a validade da duração do token. |
| versão |`ver` |`2.0` |A versão do token de ID, conforme definido pelo Azure AD. Para o ponto final v2.0, o valor é `2.0`. |
| ID do inquilino |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Um GUID que representa o que o utilizador é de inquilino do Azure AD. Para contas profissionais e escolares, o GUID é o ID de inquilino imutável da organização que o utilizador pertence. Contas pessoais, o valor é `9188040d-6c67-4c5b-b112-36a304b66dad`. O `profile` âmbito é necessário para receber esta afirmação. |
| Hash de código |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |O hash de código está incluído nos tokens de ID apenas quando o token de ID é emitido com um código de autorização de OAuth 2.0. Ele pode ser usado para validar a autenticidade de um código de autorização. Para obter detalhes sobre como efetuar esta validação, consulte a [especificação do OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html). |
| Hash de token de acesso |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |O hash de token está incluído no ID de acesso tokens apenas quando o token de ID é emitido com um token de acesso de OAuth 2.0. Ele pode ser usado para validar a autenticidade de um token de acesso. Para obter detalhes sobre como efetuar esta validação, consulte a [especificação do OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html). |
| Valor de uso único |`nonce` |`12345` |O valor de uso único é uma estratégia de mitigação de ataques de repetição de token. A aplicação, pode especificar um valor de uso único numa solicitação de autorização utilizando a `nonce` parâmetro de consulta. O valor que fornecer no pedido é emitido no token de ID `nonce` afirmação, sem modificações. A aplicação pode verificar o valor com o valor-especificado na solicitação, que associa a sessão da aplicação com um token de ID específico. A aplicação deve realizar esta validação durante o processo de validação do token de ID. |
| nome |`name` |`Babe Ruth` |A afirmação de nome fornece um valor legível por humanos que identifica o assunto do token. O valor não é garantido para que seja exclusivo, é mutável e foi concebido para ser utilizado apenas para fins de exibição. O `profile` âmbito é necessário para receber esta afirmação. |
| e-mail |`email` |`thegreatbambino@nyy.onmicrosoft.com` |O endereço de e-mail principal associado a conta de utilizador, se existir. Seu valor é mutável e podem ser alterados ao longo do tempo. O `email` âmbito é necessário para receber esta afirmação. |
| nome de utilizador preferencial |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |O nome de utilizador principal que represente o usuário o ponto final de v2.0. Pode ser um endereço de e-mail, número de telefone ou um nome de utilizador genérica sem um formato especificado. Seu valor é mutável e podem ser alterados ao longo do tempo. Uma vez que é mutável, este valor não pode ser utilizado para tomar decisões de autorização. O `profile` âmbito é necessário para receber esta afirmação. |
| Assunto |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | O principal sobre o qual o token declara informações, como o utilizador de uma aplicação. Este valor é imutável e não pode ser reatribuído ou reutilizado. Ele pode ser utilizado para realizar verificações de autorização com segurança, por exemplo, quando o token é utilizado para aceder a um recurso e pode ser utilizado como uma chave em tabelas de base de dados. Uma vez que o assunto é sempre presente os tokens que problemas do Azure AD, recomendamos que utilize este valor num sistema de autorização para fins gerais. O assunto é, no entanto, um identificador pairwise - é exclusivo para um ID de aplicação em particular. Por conseguinte, se um único utilizador se inscreve para duas aplicações diferentes com dois IDs de cliente diferentes, essas aplicações irão receber dois valores diferentes para a afirmação do requerente. Isto pode ou não pode ser desejável dependendo dos requisitos de arquitetura e a privacidade. |
| ID de objeto |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | O identificador imutável para um objeto no Microsoft identity system, neste caso, uma conta de utilizador. Também pode ser utilizado para realizar verificações de autorização com segurança e como uma chave nas tabelas de base de dados. Este ID identifica exclusivamente o utilizador em todas as aplicações – dois aplicativos diferentes, iniciar sessão no mesmo utilizador irão receber o mesmo valor no `oid` de afirmação. Isso significa que pode ser utilizado ao fazer consultas ao Microsoft online services, como o Microsoft Graph. O Microsoft Graph irá devolver este ID como o `id` propriedade para uma determinada conta de utilizador. Uma vez que o `oid` permite que várias aplicações correlacionar os utilizadores, o `profile` âmbito é necessário para receber esta afirmação. Tenha em atenção que, se existir um único utilizador em vários inquilinos, o utilizador irá conter um ID de objeto diferentes em cada inquilino - que são consideradas diferentes contas, mesmo que o usuário faz logon em cada conta com as mesmas credenciais. |

### <a name="access-tokens"></a>Tokens de acesso

O ponto final v2.0 permite que as aplicações de terceiros que estão registadas com o Azure AD para emitir tokens de acesso para recursos protegidos, como as APIs da Web. Para obter mais informações sobre como configurar uma aplicação para emitir tokens de acesso, veja [como registar uma aplicação com o ponto final v2.0](quickstart-v2-register-an-app.md). Após registar a aplicação com o ponto final v2.0, o desenvolvedor pode especificar os níveis de acesso, chamado **âmbitos**, para que o acesso podem ser emitidos tokens. Por exemplo, o **calendars.read** âmbito definido no Microsoft Graph API concede permissão para ler o calendário do usuário. Quando seu aplicativo recebe um token de acesso a partir do ponto final v2.0, é necessário validar a assinatura do token, emissor, público-alvo, a hora de expiração e quaisquer outras afirmações dependendo do seu cenário. 

Quando solicita um token de acesso a partir do ponto final v2.0, o ponto final v2.0 devolve também metadados sobre o token de acesso para a sua aplicação utilizar. Estas informações incluem a hora de expiração do token de acesso e os âmbitos para a qual é válido. A aplicação utiliza estes metadados para efetuar a colocação em cache inteligente de tokens de acesso sem ter que analisar a abrir o token de acesso em si.

### <a name="refresh-tokens"></a>Tokens de atualização
Atualizar os tokens são tokens de segurança que seu aplicativo pode usar para obter novos tokens de acesso num fluxo de OAuth 2.0. A aplicação pode utilizar tokens de atualização para atingir a longo prazo acesso a recursos em nome de um utilizador sem a necessidade de interação com o utilizador.

Tokens de atualização são vários recursos. Um token de atualização recebido durante um pedido de token para um recurso pode ser resgatado para tokens de acesso a um recurso completamente diferente.

Para receber uma atualização numa resposta de token, a aplicação tem de pedir e receber o `offline_access` âmbito. Para saber mais sobre o `offline_access` escopo, consulte a [âmbitos e consentimento](v2-permissions-and-consent.md) artigo.

Tokens de atualização e sempre serão, completamente opaco para a sua aplicação. Eles são emitidos pelo ponto final v2.0 do Azure AD e podem apenas ser inspecionados e são interpretados pelo ponto final v2.0. Eles são vida útil longa, mas a aplicação não deve ser gravada esperar que um token de atualização irá durar por qualquer período de tempo. Tokens de atualização podem ser invalidadas a qualquer momento por vários motivos - para obter detalhes, consulte [revogação do token](v1-id-and-access-tokens.md#token-revocation). A única forma para a sua aplicação saber se um token de atualização é válido é tentar resgatá-lo fazendo um pedido de token para o ponto final v2.0.

Quando utilizar um token de atualização para um novo token de acesso (e se a sua aplicação foi concedida a `offline_access` âmbito), receberá um novo token de atualização na resposta de token. Guarde o token de atualização de emitidos recentemente, para substituir o que utilizou no pedido. Isso garante que os tokens de atualização mantêm-se válidas durante mais tempo.

## <a name="validating-tokens"></a>Tokens de validação
Atualmente, a validação de token apenas que as suas aplicações devem ser realizada está a validar os tokens de ID. Para validar um token de ID, a aplicação deve validar a assinatura do token de ID e as afirmações no token de ID.

<!-- TODO: Link --> A Microsoft fornece as bibliotecas e exemplos de código que mostram como lidar facilmente com validação do token. As secções seguintes, descrevemos o processo de subjacente. Várias bibliotecas de código-fonte aberto de terceiros também estão disponíveis para a validação do JWT. A opção, pelo menos, uma biblioteca para quase todas as plataformas e idiomas está.

### <a name="validate-the-signature"></a>Validar a assinatura
Um JWT contém três segmentos, que são separados pelo `.` caráter. O primeiro segmento é conhecido como o *cabeçalho*, o segundo segmento é o *corpo*, e o segmento de terceiro é o *assinatura*. O segmento de assinatura pode ser utilizado para validar a autenticidade do token de ID, de modo a que podem ser fidedignos pela sua aplicação.

Tokens de ID são assinados com algoritmos de criptografia assimétrica de norma da indústria, como RSA 256. O cabeçalho do token de ID tem informações sobre o método de encriptação e a chave utilizada para assinar o token. Por exemplo:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

O `alg` afirmação indica o algoritmo que foi utilizado para assinar o token. O `kid` afirmação indica a chave pública que foi utilizada para assinar o token.

O ponto final v2.0 assina os tokens de acesso e ID através da utilização de um conjunto específico de pares de chaves públicas-privadas. O ponto final v2.0 periodicamente gira o conjunto possível de chaves, para que a sua aplicação deve ser escrita para lidar com essas alterações principais automaticamente. Uma frequência razoável para procurar atualizações para as chaves públicas usadas pelo ponto final v2.0 é a cada 24 horas.

Pode adquirir os dados de chave de assinatura que precisa de validar a assinatura usando o documento de metadados OpenID Connect localizado em:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Experimente o URL num browser!

Este documento de metadados é um objeto JSON que tem várias partes úteis de informações, como a localização dos vários pontos finais necessários para a autenticação OpenID Connect. O documento também inclui um *jwks_uri*, que dá a localização do conjunto de chaves públicas usadas para assinar os tokens. O documento JSON localizado no jwks_uri tem todas as informações de chave públicas está atualmente em utilização. A aplicação pode utilizar o `kid` no cabeçalho do JWT para selecionar qual a chave pública neste documento foi utilizada para assinar um token de afirmação. Depois, ele faz validação da assinatura, utilizando a chave pública correta e o algoritmo indicado.

> [!NOTE]
> O `x5t` afirmação é preterida no ponto final v2.0. Recomendamos que utilize o `kid` validar o token de afirmação.

Realizar validação de assinatura está fora do escopo deste documento. Muitas bibliotecas de código-fonte aberto estão disponíveis para ajudá-lo com isso.

### <a name="validate-the-claims"></a>Validar as afirmações
Quando a aplicação recebe um token de ID após o início de sessão do utilizador, ele também deve executar algumas verificações contra as afirmações no token de ID. Estes incluem, mas não sejam limitam a:

* **público-alvo** alegam, certifique-se de que o token de ID destinava-se a ser dado à sua aplicação
* **não antes** e **hora de expiração** afirma, certifique-se de que o token de ID não expirou
* **emissor** alegam, certifique-se de que o token foi emitido para a sua aplicação, o ponto final v2.0
* **valor de uso único**, como um token de mitigação de ataques de repetição.

Para obter uma lista completa de validações de afirmação que deve executar a sua aplicação, consulte a [especificação do OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Detalhes dos valores esperados para essas declarações estão incluídas na [tokens de ID](# ID tokens) secção.

## <a name="token-lifetimes"></a>Durações de token
Podemos fornecer durações de token seguintes apenas para sua informação. As informações podem ajudar à medida que desenvolve e depurar aplicações. As suas aplicações não devem ser escritas esperar qualquer um desses tempos de vida permaneça constante. Pode durações de token e será alterado em qualquer altura.

| Certificado de | Tempo de vida | Descrição |
| --- | --- | --- |
| Tokens de ID (contas profissionais ou escolares) |1 hora |Tokens de identidade normalmente são válidos para 1 hora. A aplicação web pode utilizar esta mesma duração para manter a sua própria sessão com o utilizador (recomendado) ou pode escolher uma duração da sessão completamente diferente. Se a sua aplicação precisar de obter um novo ID de token, ele precisa fazer um novo início de sessão pedido para a versão 2.0 autorizar o ponto final. Se o utilizador tiver uma sessão de browser válido com o ponto final v2.0, o utilizador não poderá ser necessário introduzam as credenciais novamente. |
| Tokens de ID (contas pessoais) |24 horas |Tokens de ID para contas pessoais normalmente são válidos durante 24 horas. A aplicação web pode utilizar esta mesma duração para manter a sua própria sessão com o utilizador (recomendado) ou pode escolher uma duração da sessão completamente diferente. Se a sua aplicação precisar de obter um novo ID de token, ele precisa fazer um novo início de sessão pedido para a versão 2.0 autorizar o ponto final. Se o utilizador tiver uma sessão de browser válido com o ponto final v2.0, o utilizador não poderá ser necessário introduzam as credenciais novamente. |
| Tokens de acesso (contas profissionais ou escolares) |1 hora |Indicado nas respostas do token como parte dos metadados do token. |
| Tokens de acesso (contas pessoais) |1 hora |Indicado nas respostas do token como parte dos metadados do token. Tokens de acesso que são emitidos em nome de contas pessoais podem ser configurados para um ciclo de vida diferente, mas 1 hora é típica. |
| (Conta escolar ou profissional) de tokens de atualização |Até 14 dias |Um token de atualização é válido para um máximo de 14 dias. No entanto, o token de atualização pode se tornar inválido a qualquer momento por várias razões, para que a aplicação deve continuar a tentar utilizar um token de atualização até falhar, ou até que a sua aplicação substitui por um novo token de atualização. Um token de atualização também se torna inválido se passaram 90 dias, uma vez que o usuário digitar suas credenciais. |
| Tokens (contas pessoais) de atualização |Até 1 ano |Um token de atualização é válido para um máximo de 1 ano. No entanto, o token de atualização pode se tornar inválido a qualquer momento por várias razões, para que a aplicação deve continuar a tentar utilizar um token de atualização até falhar. |
| Códigos de autorização (contas profissionais ou escolares) |10 minutos |Códigos de autorização são propositadamente curta duração e devem ser resgatados imediatamente para tokens de acesso e tokens de atualização quando os tokens são recebidos. |
| Códigos de autorização (contas pessoais) |5 minutos |Códigos de autorização são propositadamente curta duração e devem ser resgatados imediatamente para tokens de acesso e tokens de atualização quando os tokens são recebidos. Códigos de autorização que são emitidos em nome de contas pessoais são para utilização de uma única vez. |
