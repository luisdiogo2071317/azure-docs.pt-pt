---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 8d0b0c99b743461da41a518feae9c781f90c8b63
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988445"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Chamar a API do Microsoft Graph a partir de uma aplicação de página única (SPA) de JavaScript

Este guia demonstra como uma aplicação de página única (SPA) de JavaScript pode iniciar sessão no trabalho pessoal, e as contas de instituição de ensino, obter um token de acesso e chamar a API do Microsoft Graph ou outras APIs que precisam de tokens de acesso do ponto final v2.0 do Azure Active Directory.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Como funciona a aplicação de exemplo gerada por este guia

![Como funciona a aplicação de exemplo gerada por este guia](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Mais Informações

O aplicativo de exemplo criado por este guia permite que um SPA do JavaScript consultar o Graph API da Microsoft ou uma API Web que aceita tokens de ponto final de v2.0 do Azure Active Directory. Para este cenário, depois de um utilizador inicia sessão, um token de acesso é solicitado e adicionado a pedidos de HTTP por meio do cabeçalho de autorização. Aquisição do token e a renovação são processadas pelo Microsoft Authentication Library (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotecas

Este guia utiliza a biblioteca do seguinte:

|Biblioteca|Descrição|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteca de autenticação da Microsoft para pré-visualização do JavaScript|

> [!NOTE]
> *msal* destinos a *ponto final de v2.0 do Azure Active Directory* -que permite que as contas pessoais, escolares e profissionais iniciar sessão e adquirir tokens. O *ponto final de v2.0 do Azure Active Directory* tem [algumas limitações](..\articles\active-directory\develop\active-directory-v2-limitations.md).
> Para compreender as diferenças entre pontos finais v1.0 e v2.0, leia os [guia de comparação de ponto final](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
