---
title: "A adição de um Azure Active Directory utilizando os serviços ligados no Visual Studio | Microsoft Docs"
description: "Adicionar um Azure Active Directory utilizando a caixa de diálogo do Visual Studio adicionar ligado serviços"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2018
ms.author: kraigb
ms.openlocfilehash: b21761b6fc166ecbb2fec9c13e5e207481fa9a39
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>A adição de um Azure Active Directory utilizando os serviços ligados no Visual Studio

Ao utilizar o Azure Active Directory (Azure AD), pode suportar único Sign-On (SSO) para aplicações web de MVC do ASP.NET, ou a autenticação do Active Directory nos serviços de Web API. Com a autenticação do AD do Azure, os utilizadores podem utilizar as contas do Azure Active Directory para ligar às suas aplicações web. As vantagens do Azure AD Authentication com Web API incluem a segurança de dados melhorado quando a exposição de uma API de uma aplicação web. Com o Azure AD, não é necessário que gerir um sistema de autenticação separado com a sua própria conta e o utilizador de gestão.

Este artigo e os respetivos artigos complementar fornecem detalhes de utilização da funcionalidade do Visual Studio ligado serviço para o Active Directory. A capacidade está disponível no Visual Studio 2017 e Visual Studio 2015.

Atualmente, o serviço ligado de Active Directory não suporta aplicações ASP.NET Core.

## <a name="prerequisites"></a>Pré-requisitos

- Conta do Azure: Se não tiver uma conta do Azure, pode [inscrever-se numa avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ou [ativar os benefícios de subscritor do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Ligar ao Azure Active Directory utilizando a caixa de diálogo Serviços ligados

1. No Visual Studio, criar ou abrir projeto MVC do ASP.NET ou um projeto API Web do ASP.NET. Pode utilizar o MVC, Web API, a aplicação de página única, aplicação API do Azure, aplicação móvel do Azure e modelos de serviço do Azure Mobile.

1. Selecione o **projeto > adicionar o serviço ligado...**  comandos de menu ou faça duplo clique o **serviços ligados** possível localizar o projeto no Explorador de soluções de nó.

1. No **serviços ligados** página, selecione **autenticação no Azure Active Directory**.

    ![Página de serviços ligada](./media/vs-azure-active-directory/connected-services-add-active-directory.png)

1. No **introdução** página, selecione **seguinte**. Se observar erros nesta página, consulte [diagnosticar erros com o serviço do Azure Active Directory ligado](vs-active-directory-error.md).

    ![Página de introdução](./media/vs-azure-active-directory/configure-azure-ad-wizard-1.png)

1. No **no início de sessão único** página, selecione um domínio do **domínio** na lista pendente. A lista contém todos os domínios acessíveis pelas contas listadas na caixa de diálogo Definições da conta do Visual Studio (**ficheiro > definições de conta...** ). Como alternativa, pode introduzir um nome de domínio, se não encontrar um que procura, tais como `mydomain.onmicrosoft.com`. Pode escolher a opção para criar uma aplicação do Azure Active Directory ou utiliza as definições de uma aplicação do Azure Active Directory existente. Selecione **seguinte** quando terminar.

    ![Início de sessão único na página](./media/vs-azure-active-directory/configure-azure-ad-wizard-2.png)

1. No **Directory acesso** página, selecione o **ler os dados de diretório** opção conforme pretendido. Os programadores incluem, geralmente, esta opção.

    ![Página de acesso de diretório](./media/vs-azure-active-directory/configure-azure-ad-wizard-3.png)

1. Selecione **concluir** para iniciar as modificações ao seu projeto para ativar a autenticação do Azure AD. Durante este período, o Visual Studio mostra progresso:

    ![Progresso de serviço do Active Directory ligado](./media/vs-azure-active-directory/active-directory-connected-service-output.png)

1. Quando o processo estiver concluído, o Visual Studio abre o browser para um dos seguintes artigos, conforme apropriado para o tipo de projeto:

    - [Começar com projetos de MVC do .NET](vs-active-directory-dotnet-getting-started.md)
    - [Introdução ao end WebAPI projetos](vs-active-directory-webapi-getting-started.md)

1. Também pode ver o domínio do Active Directory no [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

## <a name="how-your-project-is-modified"></a>Como o projeto é modificado

Ao adicionar o serviço ligado do assistente, o Visual Studio adiciona Azure Active Directory e as referências associadas ao seu projeto. Ficheiros de configuração e ficheiros de código no seu projeto também são modificados para adicionar suporte para o Azure AD. As modificações específicas que faz com que o Visual Studio dependem do tipo de projeto. Consulte os artigos seguintes para obter mais detalhes:

- [O que aconteceu ao meu projeto MVC do .NET?](vs-active-directory-dotnet-what-happened.md)
- [O que aconteceu ao meu projeto Web API?](vs-active-directory-webapi-what-happened.md)

## <a name="next-steps"></a>Passos seguintes

- [Cenários de autenticação do Azure Active Directory](active-directory-authentication-scenarios.md)
- [Adicionar início de sessão com a Microsoft para uma aplicação web ASP.NET](guidedsetups/active-directory-aspnetwebapp-v1.md)
