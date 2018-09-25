---
title: Referência de alterações de última hora de Active Directory do Azure | Documentos da Microsoft
description: Alterações feitas para os protocolos do Azure AD que podem afetar a sua aplicação.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 14217d03cdb56c5c641ab8f8c3fc0748e8e815a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987781"
---
# <a name="whats-new-for-authentication"></a>O que há de novo para a autenticação? 

|
>Ser notificado sobre quando rever esta página para obter atualizações adicionando isso [URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) para seu RSS feed leitor.

O sistema de autenticação altera e adiciona recursos de forma contínua para melhorar a segurança e melhorar a conformidade com os padrões. Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

- As funcionalidades mais recentes
- Problemas conhecidos
- Alterações de protocolo
- Funcionalidades preteridas

> [!TIP] 
> Esta página é atualizada regularmente, por isso, muitas vezes a rever. Salvo indicação em contrário, estas alterações apenas são colocadas no local para aplicações recentemente registadas.  

## <a name="upcoming-changes"></a>Alterações futuras

### <a name="authorization-codes-can-no-longer-be-reused"></a>Códigos de autorização já não podem ser reutilizados

**Data efetiva**: 10 de Outubro de 2018 **pontos de extremidade afetados**: tanto v1.0 e v2.0 **protocolo afetado**: [fluxo de código](v2-oauth2-auth-code-flow.md)

A partir de 10 de Outubro de 2018, do Azure AD irá parar abertos ao recebimento de códigos de autenticação utilizado anteriormente para novas aplicações. Qualquer aplicação criada antes de 10 de Outubro de 2018 ainda será capaz de reutilizar os códigos de autenticação. Esta alteração de segurança ajuda a trazer do Azure AD em conformidade com a especificação de OAuth e será imposta em pontos finais de ambos o v1 e v2.

Se a sua aplicação reutiliza os códigos de autorização para obter os tokens de vários recursos, recomendamos que utilize o código para obter um token de atualização e, em seguida, utilizar esse token de atualização para adquirir tokens adicionais para outros recursos. Códigos de autorização só podem ser utilizados uma vez, mas os tokens de atualização podem ser utilizadas várias vezes em vários recursos. Os novos aplicativos que tentam reutilizar um código de autenticação durante o fluxo de código do OAuth obterá um erro de invalid_grant.

Para obter mais informações sobre os tokens de atualização, consulte [atualizar os tokens de acesso](v1-protocols-oauth-code.md#refreshing-the-access-tokens).

## <a name="may-2018"></a>Maio de 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>Tokens de ID não podem ser utilizados para o fluxo OBO

**Data**: 1 de Maio de 2018 **pontos de extremidade afetados**: tanto v1.0 e v2.0 **protocolos afetados**: fluxo implícito e [fluxo OBO](v1-oauth2-on-behalf-of-flow.md)

Após 1 de Maio de 2018, id_tokens não pode ser utilizado como a asserção num fluxo de OBO novos aplicativos.  Tokens de acesso devem ser utilizados em vez disso, para proteger APIs, até mesmo entre um cliente e a camada intermediária do mesmo aplicativo.  Aplicações registadas antes de 1 de Maio de 2018 continuarão a funcionar e ser capaz de trocar id_tokens para um token de acesso - no entanto, isso não é considerado uma prática recomendada.

Para contornar esta alteração, pode fazer o seguinte:

1. Crie uma API Web para a sua aplicação de camada intermediária, com um ou mais âmbitos.  Isso permitirá que a segurança e melhor controlo detalhado.
1. No manifesto da sua aplicação, o [Portal do Azure](https://portal.azure.com), ou o [Portal de registo de aplicação](https://apps.dev.microsoft.com), certifique-se de que a aplicação tem permissões para emitir tokens de acesso via o fluxo implícito. Isso é controlado por meio do `oauth2AllowImplicitFlow` chave.
1. Quando a aplicação cliente solicita um id_token via `response_type=id_token`, também, solicitar um token de acesso (`response_type=token`) para a API Web criado acima.  Portanto, ao utilizar o ponto final v2.0 a `scope` parâmetro deve ser semelhante à `api://GUID/SCOPE`.  No ponto de extremidade v1.0, o `resource` parâmetro deve ser a URI da web API de aplicação.
1. Passe esse token de acesso para a camada intermediária em vez do id_token.  