---
title: Definições de cookies de Proxy de aplicação - Azure Active Directory | Documentos da Microsoft
description: Azure Active Directory (Azure AD) tem os cookies de acesso e de sessão para aceder a aplicações no local através do Proxy de aplicações. Neste artigo, descobrirá como utilizar e configurar as definições de cookie.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: concept
ms.date: 01/16/2019
ms.author: celested
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1d97e2542200703201b1c20738581a938ba209e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165999"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>Definições de cookies para aceder a aplicações no local no Azure Active Directory

Azure Active Directory (Azure AD) tem os cookies de acesso e de sessão para aceder a aplicações no local através do Proxy de aplicações. Saiba como utilizar as definições de cookie do Proxy de aplicações. 

## <a name="what-are-the-cookie-settings"></a>Quais são as definições de cookie?

[Proxy de aplicações](application-proxy.md) utiliza as seguintes definições de cookie do acesso e de sessão.

| Definição de cookie | Predefinição | Descrição | Recomendações |
| -------------- | ------- | ----------- | --------------- |
| Utilizar Cookie Apenas HTTP | **Não** | **Sim** permite que o Proxy de aplicações incluir o sinalizador de HTTPOnly nos cabeçalhos de resposta HTTP. Este sinalizador fornece benefícios de segurança adicional, por exemplo, ele impede que o lado do cliente scripts (CSS) copiar ou modifiquem os cookies.<br></br><br></br>Antes que suporte a definição apenas para HTTP, o Proxy de aplicações encriptados e transmitidas cookies através de um canal seguro do SSL para proteger contra modificação. | Uso **Sim** por causa dos benefícios de segurança adicionais.<br></br><br></br>Uso **não** para os clientes ou agentes de utilizador que necessitam de acesso para o cookie de sessão. Por exemplo, usar **não** para um cliente RDP ou MTSC que se liga a um servidor de Gateway de ambiente de trabalho remoto através do Proxy de aplicações.|
| Utilizar Cookie Seguro | **Não** | **Sim** permite que o Proxy de aplicações incluir o Secure sinalizador nos cabeçalhos de resposta HTTP. Cookies seguros aprimorou a segurança com a transmissão de cookies através de um canal seguro do TLS, tal como HTTPS. Isto impede que cookies que está a ser observado pelo partes não autorizadas devido a transmissão do cookie em texto não criptografado. | Uso **Sim** por causa dos benefícios de segurança adicionais.|
| Utilizar Cookie Persistente | **Não** | **Sim** permite que o Proxy de aplicações definir seus cookies de acesso para não expirarem quando o navegador da web está fechado. A persistência dura até que o token de acesso expira, ou até que o utilizador o elimine manualmente os cookies persistentes. | Uso **não** devido ao risco de segurança associado a mantém os usuários autenticados.<br></br><br></br>Sugerimos que utilize apenas **Sim** para aplicativos mais antigos que não é possível partilhar cookies entre processos. É melhor atualizar a sua aplicação para processar cookies partilha entre processos em vez de utilizar cookies persistentes. Por exemplo, poderá ter cookies persistentes para permitir que um utilizador abrir documentos do Office na vista de Explorador de um site SharePoint. Sem cookies persistentes, esta operação poderá falhar se os cookies de acesso não são partilhados entre o navegador, o processo explorer e o processo do Office. |

## <a name="set-the-cookie-settings---azure-portal"></a>Configurar as definições de cookie - portal do Azure
Para configurar as definições de cookie no portal do Azure:

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 
2. Navegue para **do Azure Active Directory** > **aplicações empresariais** > **todas as aplicações**.
3. Selecione a aplicação para o qual pretende ativar uma definição de cookie.
4. Clique em **Proxy de aplicações**.
5. Sob **definições adicionais**, configure a definição de cookie **Sim** ou **não**.
6. Clique em **guardar** para aplicar as alterações. 

<!---

## View current cookie settings - PowerShell

To see the current cookie settings for the application, use this PowerShell command:  

```PowerShell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## Set cookie settings - PowerShell

In the following PowerShell commands, ```<ObjectId>``` is the ObjectId of the application. 

**Http-Only Cookie** 

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Secure Cookie**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Persistent Cookies**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```

-->
