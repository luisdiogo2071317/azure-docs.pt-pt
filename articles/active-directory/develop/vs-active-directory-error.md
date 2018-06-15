---
title: Serviço de ligado como diagnosticar erros com o Azure Active Directory
description: O serviço do Active Directory ligado detetou um tipo de autenticação incompatível
services: active-directory
author: ghogen
manager: douge
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: 8cd15c59bbe536dba9adb6f44c07844eaf030b55
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31784892"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Erros de diagnóstico com o serviço do Azure Active Directory ligado

Ao detetar o código de autenticação anterior, ligar o diretório do Active Directory do Azure servidor detetou um tipo de autenticação incompatível.

Para detetar corretamente o código de autenticação anterior num projeto, o projeto tem de ser criado.  Se encontrou o erro e não tiver um código de autenticação anterior no seu projeto, reconstruir e tente novamente.

## <a name="project-types"></a>Tipos de projeto

O serviço ligado verifica o tipo de projeto que estiver a desenvolver, de modo que pode inserir a lógica de autenticação correto no projeto. Se não houver qualquer controlador que derive `ApiController` no projeto, o projeto é considerado um projeto de end WebAPI. Se existirem apenas os controladores que derivem de `MVC.Controller` no projeto, o projeto é considerado um projeto MVC. O serviço ligado não suporta qualquer tipo de projeto.

## <a name="compatible-authentication-code"></a>Código de autenticação compatível

O serviço ligado também verifica a existência de definições de autenticação que foram configuradas anteriormente ou que são compatíveis com o serviço. Se todas as definições estiverem presentes, é considerada um caso re-entrant e abre o serviço ligado visualize as definições.  Se apenas algumas das definições estiverem presentes, é considerada um caso de erro.

Num projeto MVC, o serviço ligado verifica a existência de qualquer uma das seguintes definições, que resultam da utilização anterior do serviço:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Além disso, o serviço ligado verifica a existência de qualquer uma das seguintes definições num projeto Web API, que resultam da utilização anterior do serviço:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

## <a name="incompatible-authentication-code"></a>Código de autenticação incompatível

Por fim, o serviço ligado tenta detetar as versões do código de autenticação que foram configuradas com versões anteriores do Visual Studio. Se receber este erro, significa que projecto contém um tipo de autenticação incompatível. O serviço ligado Deteta os seguintes tipos de autenticação de versões anteriores do Visual Studio:

* Autenticação do Windows
* Contas de utilizador individuais
* Contas organizacionais

Para detetar a autenticação do Windows num projeto MVC, o ligado procura o `authentication` elemento no seu `web.config` ficheiro.

```xml
<configuration>
    <system.web>
        <span style="background-color: yellow"><authentication mode="Windows" /></span>
    </system.web>
</configuration>
```

Para detetar a autenticação do Windows num projeto Web API, o serviço ligado procura o `IISExpressWindowsAuthentication` elemento no seu projeto `.csproj` ficheiro:

```xml
<Project>
    <PropertyGroup>
        <span style="background-color: yellow"><IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication></span>
    </PropertyGroup>
</Project>
```

Para detetar a autenticação de contas de utilizador individuais, o serviço ligado procura o elemento de pacote na sua `packages.config` ficheiro.

```xml
<packages>
    <span style="background-color: yellow"><package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /></span>
</packages>
```

Para detetar um formulário antigo de autenticação da conta institucional, o serviço ligado procura para o elemento seguinte`web.config`:

```xml
<configuration>
    <appSettings>
        <span style="background-color: yellow"><add key="ida:Realm" value="***" /></span>
    </appSettings>
</configuration>
```

Para alterar o tipo de autenticação, remova o tipo de autenticação incompatíveis e tente novamente adicionar o serviço ligado.

Para obter mais informações, consulte [cenários de autenticação para o Azure AD](active-directory-authentication-scenarios.md).