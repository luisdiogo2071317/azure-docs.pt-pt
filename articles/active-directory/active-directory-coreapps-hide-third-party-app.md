---
title: "Ocultar uma aplicação da experiência do utilizador no Azure Active Directory | Microsoft Docs"
description: "Como ocultar uma aplicação da experiência do utilizador no Azure Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 667fdd45bc9eb1f01ce3883006bb29274478cb83
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Ocultar uma aplicação da experiência do utilizador no Azure Active Directory

Se tiver uma aplicação que não pretende mostrar no painel de acesso ou do Office 365 launchers dos utilizadores, é uma opção para ocultar este mosaico da aplicação. Esta opção só está disponível para aplicações de terceiros (aplicações não publicadas pela Microsoft). Por ocultar os utilizadores da aplicação ainda tem permissões para a aplicação, mas não irão vê-las aparecer no respetivos launchers de aplicação. Tem de ter as permissões adequadas para gerir a aplicação da empresa e tem de ser administrador global do diretório. 

## <a name="hiding-an-application-from-users-end-user-experiences"></a>Ocultar uma aplicação a partir das experiências do utilizador final do utilizador
Utilize os seguintes passos para ocultar uma aplicação a partir do painel de acesso de um utilizador e launchers de aplicação do Office 365

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Como posso ocultar uma aplicação de terceiros do painel de acesso de utilizador e launchers de aplicação do Office 365?

1.  Iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2.  Selecione **mais serviços**, introduza **do Azure Active Directory** na caixa de texto e, em seguida, selecione **Enter**.
3.  No **Azure Active Directory - *directoryname***  ecrã (ou seja, o Azure AD ecrã para o diretório que está a gerir), selecione **aplicações empresariais**.
![Aplicações da empresa](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  No **aplicações empresariais** ecrã, selecione **todas as aplicações**. É apresentada uma lista das aplicações que pode gerir.
5.  No **aplicações da empresa - todas as aplicações** ecrã, selecione uma aplicação.</br>
![Aplicações da empresa](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  No ***appname*** ecrã (ou seja, ecrã de com o nome da aplicação selecionada no título), selecione propriedades.
7.  No  ***appname* -propriedades** ecrã, selecione **Sim** para **Visible aos utilizadores?**.
![Aplicações da empresa](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Selecione o **guardar** comando.

## <a name="next-steps"></a>Passos seguintes
* [Ver todos os meus grupos](active-directory-groups-view-azure-portal.md)
* [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)
* [Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Alterar o nome ou logótipo de uma aplicação empresarial](active-directory-coreapps-change-app-logo-user-azure-portal.md)
