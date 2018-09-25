---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 2a4c389d063bb63f2fa2293d54236f99d7035e0e
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060581"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Chamar o Microsoft Graph API a partir de uma aplicação de página única de JavaScript (SPA)

Este guia demonstra como uma aplicação JavaScript página única (SPA) pode iniciar sessão no trabalho pessoal, e as contas de instituição de ensino, obter um token de acesso e chamar a API do Microsoft Graph ou outras APIs que precisam de tokens de acesso do ponto final do Azure Active Directory v2.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Como funciona a aplicação de exemplo gerada por este guia

![Como funciona a aplicação de exemplo gerada por este guia](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Mais Informações

O aplicativo de exemplo criado por este guia permite que um SPA do JavaScript consultar o Graph API da Microsoft ou uma API Web que aceita tokens de ponto final do Azure Active Directory v2. Para este cenário, depois de um utilizador inicia sessão, um token de acesso é solicitado e adicionado a pedidos de HTTP com o cabeçalho de autorização. Aquisição do token e a renovação são processadas pelo Microsoft Authentication Library (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotecas

Este guia utiliza a biblioteca do seguinte:

|Biblioteca|Descrição|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteca de autenticação da Microsoft para pré-visualização do JavaScript|

> [!NOTE]
> *msal* destinos a *ponto final do Azure Active Directory v2* -que permite que as contas pessoais, escolares e profissionais iniciar sessão e adquirir tokens. O *ponto final do Azure Active Directory v2* tem [algumas limitações](..\articles\active-directory\develop\active-directory-v2-limitations.md).
> Para compreender as diferenças entre os pontos de extremidade v1 e v2 de leitura a [v1 e v2 comparação](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
