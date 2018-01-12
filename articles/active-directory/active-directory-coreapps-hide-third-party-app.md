---
title: "Ocultar uma aplicação da experiência do utilizador no Azure Active Directory | Microsoft Docs"
description: "Como ocultar uma aplicação da experiência do utilizador no painel de acesso do Azure Active Directory ou Office 365 launchers."
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
ms.date: 01/04/2018
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 00307311ca2bc1432f2de3755575adf1804f9c77
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Ocultar uma aplicação da experiência do utilizador no Azure Active Directory

Se tiver uma aplicação que não pretende mostrar no painel de acesso ou do Office 365 launchers dos utilizadores, existem opções para ocultar este mosaico da aplicação.  As duas opções seguintes estão disponíveis para ocultar as aplicações de launchers de aplicação do utilizador.

- Ocultar uma aplicação de terceiros de painéis de acesso de utilizadores e launchers de aplicação do Office 365
- Ocultar todas as aplicações do Office 365 de painéis de acesso de utilizadores

Por ocultar os utilizadores da aplicação ainda tem permissões para a aplicação, mas não irão vê-las aparecer no respetivos launchers de aplicação. Tem de ter as permissões adequadas para gerir a aplicação da empresa e tem de ser um administrador global do diretório.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Ocultar uma aplicação a partir das experiências do utilizador final do utilizador
Pode utilizar os passos abaixo, dependendo da sua situação, para ocultar as aplicações a partir do painel de acesso.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Como posso ocultar uma aplicação de terceiros do painel de acesso de utilizador e launchers de aplicação do Office 365?
Utilize os seguintes passos para ocultar uma aplicação a partir do painel de acesso de um utilizador e launchers de aplicação do Office 365.

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

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Como posso ocultar aplicações do Office 365 do painel de acesso do utilizador?

Utilize os seguintes passos para ocultar todas as aplicações do Office 365 do painel de acesso. Estas aplicações continuarão a estar visíveis no portal do Office 365.

1.  Iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2.  Selecione **mais serviços**, introduza **do Azure Active Directory** na caixa de texto e, em seguida, selecione **Enter**.
3.  No **Azure Active Directory - *directoryname***  ecrã (ou seja, o Azure AD ecrã para o diretório que está a gerir), selecione **as definições de utilizador**.
4.  No **as definições de utilizador** ecrã em **aplicações empresariais** selecione **Sim** para **os utilizadores apenas podem ver as aplicações do Office 365 no portal do Office 365**.

![Aplicações empresariais](media/active-directory-coreapps-hide-third-party-app/apps4.png)

## <a name="next-steps"></a>Passos Seguintes
* [Ver todos os meus grupos](active-directory-groups-view-azure-portal.md)
* [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)
* [Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Alterar o nome ou logótipo de uma aplicação empresarial](active-directory-coreapps-change-app-logo-user-azure-portal.md)

