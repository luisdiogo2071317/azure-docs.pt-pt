---
title: 'Azure Active Directory dos serviços de domínio: Ativar a delegação restringida de kerberos | Microsoft Docs'
description: Ativar a delegação restringida de kerberos em domínios geridos de serviços de domínio do Active Directory do Azure
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: maheshu
ms.openlocfilehash: fc4e738e95799838c5761e8b1ee5973aefea7dc6
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213845"
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Configurar a delegação restringida de Kerberos (KCD) um domínio gerido
Muitas aplicações devem ter acesso a recursos no contexto do utilizador. Active Directory suporta um mecanismo de delegação de Kerberos, que permite que este caso de utilização de chamada. Além disso, pode restringir a delegação para que podem ser acedidos apenas os recursos específicos no contexto do utilizador. Domínios geridos de serviços de domínio do AD do Azure são diferentes das tradicionais domínios do Active Directory, uma vez que seja mais segura estejam bloqueados para baixo.

Este artigo mostra como configurar a delegação restringida de Kerberos num domínio gerido dos serviços de domínio do Azure AD.

## <a name="kerberos-constrained-delegation-kcd"></a>Delegação restringida de Kerberos (KCD)
A delegação de Kerberos permite a uma conta para representar outro principal de segurança (por exemplo, um utilizador) para aceder a recursos. Considere uma aplicação web que acede a uma API de web de back-end no contexto de um utilizador. Neste exemplo, a aplicação web (em execução no contexto de uma conta de serviço ou uma conta de computador /) representa o utilizador tem quando aceder ao recurso (web API de back-end). A delegação de Kerberos é inseguras, uma vez que não restringe os recursos que pode aceder a impersonating conta no contexto do utilizador.

Delegação restringida de Kerberos (KCD) restringe os serviços/recursos a que o servidor especificado pode agir em nome de um utilizador. KCD tradicional requer privilégios de administrador de domínio para configurar uma conta de domínio para um serviço e restringe a conta para um único domínio.

KCD tradicional também tem alguns problemas associados. Sistemas operativos anteriores, se o administrador de domínio configurada com base na conta KCD para o serviço, o administrador de serviços não teve nenhuma forma útil de saber que serviços front-end delegados aos serviços de recurso detinham. E qualquer serviço front-end que poderia delegar a um serviço de recursos representado um ponto de ataque potencial. Se um servidor que alojava um serviço front-end fosse comprometido e configurado para delegar a serviços de recursos, os serviços de recursos também podiam ser comprometidos.

> [!NOTE]
> Num domínio gerido dos serviços de domínio do Azure AD, não tiver privilégios de administrador de domínio. Por conseguinte, **KCD tradicional baseado na conta não é possível configurar um domínio gerido**. Utilize KCD baseada em recursos, conforme descrito neste artigo. Este mecanismo também é mais seguro.
>
>

## <a name="resource-based-kcd"></a>Baseada em recursos KCD
Partir do Windows Server 2012, os administradores de serviço obtêm a capacidade de configurar a delegação restrita para o seu serviço. Neste modelo, o administrador de serviço de back-end pode permitir ou negar serviços front-end específicos da utilização KCD. Este modelo é conhecido como **baseada em recursos KCD**.

Baseada em recursos KCD está configurado com o PowerShell. Utilizar o `Set-ADComputer` ou `Set-ADUser` cmdlets, dependendo se a conta impersonating é uma conta de computador ou uma conta de serviço/conta de utilizador.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Configurar KCD baseada em recursos de uma conta de computador um domínio gerido
Suponha que tem uma aplicação web em execução no computador ' contoso100-webapp.contoso100.com'. Precisa de aceder ao recurso (uma API web em execução no ' contoso100-api.contoso100.com') no contexto de utilizadores de domínio. Eis como poderia configurar KCD baseada em recursos para este cenário:

1. [Criar uma UO personalizada](active-directory-ds-admin-guide-create-ou.md). Pode delegar permissões para gerir essa UO personalizado aos utilizadores no domínio gerido.
2. Associe as duas máquinas virtuais (aquele que executa a aplicação web e um em execução na API web) para o domínio gerido. Crie estas contas de computador na UO personalizado.
3. Agora, configure KCD baseada em recursos utilizando o seguinte comando do PowerShell:

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> As contas de computador para a aplicação web e a API web tem de ser numa UO em que disponham de permissão para configurar KCD baseada em recursos personalizada. Não é possível configurar KCD baseada em recursos de uma conta de computador no contentor 'AAD DC computadores' incorporado.
>

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Configurar KCD baseada em recursos para uma conta de utilizador um domínio gerido
Suponha que tem uma aplicação web em execução como uma conta de serviço 'appsvc' e de que necessita aceder ao recurso (uma API web em execução como uma conta de serviço - 'backendsvc') no contexto de utilizadores de domínio. Eis como poderia configurar KCD baseada em recursos para este cenário.

1. [Criar uma UO personalizada](active-directory-ds-admin-guide-create-ou.md). Pode delegar permissões para gerir essa UO personalizado aos utilizadores no domínio gerido.
2. Associe a máquina virtual com o back-end web API/recursos para o domínio gerido. Crie a conta de computador na UO personalizado.
3. Crie a conta de serviço (por exemplo, ' appsvc') utilizada para executar a aplicação web na UO personalizado.
4. Agora, configure KCD baseada em recursos utilizando o seguinte comando do PowerShell:

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> A conta de computador para a API web de back-end e a necessidade de conta de serviço para ser numa UO em que disponham de permissão para configurar KCD baseada em recursos personalizada. Não é possível configurar KCD baseada em recursos para uma conta de computador no contentor 'AAD DC computadores' incorporado ou para contas de utilizador no contentor 'Utilizadores de DC do AAD' incorporado. Assim, não é possível utilizar contas de utilizador sincronizadas a partir do Azure AD para configurar KCD baseada em recursos.
>

## <a name="related-content"></a>Conteúdo relacionado
* [Serviços de domínio do Azure AD - guia de introdução](active-directory-ds-getting-started.md)
* [Descrição geral da delegação restrita de Kerberos](https://technet.microsoft.com/library/jj553400.aspx)
