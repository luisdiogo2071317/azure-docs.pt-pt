---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 7e7e9d078bf9339beb2ad5ac53ea858e843242ce
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293678"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Iniciar sessão dos utilizadores e chamar o Microsoft Graph a partir de uma aplicação Android

Neste tutorial, irá aprender como criar uma aplicação Android e integrá-lo na plataforma de identidade da Microsoft. Especificamente, esta aplicação irá iniciar sessão de um utilizador, obter um token de acesso para chamar a API do Microsoft Graph e fazer um pedido de básico para o Microsoft Graph API.  

Quando concluir o guia, a aplicação irá aceitar inícios de sessão de contas pessoais da Microsoft (incluindo o outlook.com, live.com e outros) e profissional ou escolar contas a partir de qualquer empresa ou organização que utiliza o Azure Active Directory. 

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Como funciona a aplicação de exemplo gerada por este guia
![Como funciona este exemplo](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

A aplicação neste exemplo irá iniciar sessão dos utilizadores e obter dados em seu nome.  Estes dados serão acedidos através de uma API remota (Microsoft Graph API neste caso) que requer autorização e é também protegida pela plataforma de identidade da Microsoft. 

Mais especificamente, 
* A aplicação abre uma página da web para iniciar a sessão do utilizador.
* A aplicação será emitida um token de acesso para a Microsoft Graph API.
* O token de acesso será incluído na solicitação HTTP para a API web.
* Processe a resposta do Microsoft Graph. 

Este exemplo usa a biblioteca de Microsoft Authentication para Android (MSAL) para ser coordenar e ajudando com autenticação A MSAL automaticamente irá renovar os tokens, fornecer SSO entre outras aplicações no dispositivo, ajudar a gerir a conta (s) e lidar com a maioria dos casos de acesso condicional. 

## <a name="prerequisites"></a>Pré-requisitos
* Esta configuração orientada utiliza o Android Studio 3.0. 
* É necessário o Android 21 ou posterior (25 + é recomendado).
* Google Chrome ou um navegador da web que utiliza os separadores de Custom é necessário para esta versão do MSAL para Android.

## <a name="library"></a>Biblioteca

Este guia utiliza a biblioteca de autenticação seguintes:

|Biblioteca|Descrição|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Biblioteca de autenticação da Microsoft (MSAL)|
