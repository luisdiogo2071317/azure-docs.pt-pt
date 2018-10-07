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
ms.openlocfilehash: 66aaabb942e6296243aada9951a232496e7734e9
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843472"
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
