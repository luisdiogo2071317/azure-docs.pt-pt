---
title: Introdução ao Azure AD no Visual Studio end WebApi projetos
description: Como começar a utilizar o Azure Active Directory nos projetos de end WebApi depois de ligar ou criar um Azure AD com o Visual Studio ligada a serviços
services: active-directory
author: ghogen
manager: douge
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: 109de9fb78ae3abfc09a37c6b8cb38c554f7613c
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Introdução ao Azure Active Directory (end WebApi projetos)

> [!div class="op_single_selector"]
> - [Introdução](vs-active-directory-webapi-getting-started.md)
> - [O que aconteceu](vs-active-directory-webapi-what-happened.md)

Este artigo fornece orientação adicional depois de acrescentar do Active Directory para um projeto de end de ASP.NET WebAPI através de **projeto > Serviços ligados** comando do Visual Studio. Se ainda não adicionou o serviço ao seu projeto, pode fazê-lo em qualquer altura.

Consulte [o que aconteceu ao meu projeto end WebAPI?](vs-active-directory-webapi-what-happened.md) para que as alterações efetuadas ao seu projeto ao adicionar o serviço ligado.

## <a name="requiring-authentication-to-access-controllers"></a>Exigir autenticação para os controladores de acesso

Foram adorned todos os controladores no seu projeto com o `[Authorize]` atributo. Este atributo exige que o utilizador ser autenticado antes de aceder a APIs definido por estes controladores. Para permitir que o controlador ser acedida anonimamente, remova este atributo do controlador de. Se pretender definir as permissões de um nível mais granular, aplique o atributo para cada método de que necessita de autorização em vez de aplicá-la para a classe de controlador.

## <a name="next-steps"></a>Passos Seguintes

- [Cenários de autenticação do Azure Active Directory](active-directory-authentication-scenarios.md)
- [Adicionar início de sessão com a Microsoft para uma aplicação web ASP.NET](guidedsetups/active-directory-aspnetwebapp-v1.md)
