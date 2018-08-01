---
title: Ocultar uma aplicação da experiência do utilizador no Azure Active Directory | Documentos da Microsoft
description: Como ocultar uma aplicação da experiência do usuário em painéis de acesso do Azure Active Directory ou Office 365 iniciadores.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 55f80396df4cbfe7d0a16a6a5066b68aadc0bdd3
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369346"
---
# <a name="hide-an-application-from-users-experience-in-azure-active-directory"></a>Ocultar uma aplicação da experiência do utilizador no Azure Active Directory

Se tiver uma aplicação que não pretende mostrar nos painéis de acesso dos utilizadores ou os iniciadores do Office 365, existem opções para ocultar este mosaico da aplicação.  As duas opções seguintes estão disponíveis para ocultar aplicações a partir de iniciadores de aplicação do utilizador.

- Ocultar uma aplicação de terceiros de painéis de acesso de utilizadores e iniciadores de aplicação do Office 365
- Ocultar todas as aplicações do Office 365 a partir de painéis de acesso de utilizadores

Ocultando os utilizadores da aplicação ainda tem permissões para a aplicação, mas não irão vê-las aparecer na sua iniciadores de aplicação. Tem de ter as permissões adequadas para gerir a aplicação da empresa e tem de ser um administrador global do diretório.


## <a name="hiding-an-application-from-users-end-user-experiences"></a>Ocultar uma aplicação a partir das experiências de utilizador final do utilizador
Pode utilizar os passos abaixo, dependendo da sua situação, para ocultar aplicações a partir do painel de acesso.

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Como posso ocultar uma aplicação de terceiros a partir do painel de acesso e iniciadores de aplicação do Office 365 do utilizador?
Utilize os seguintes passos para ocultar uma aplicação a partir do painel de acesso de um usuário e iniciadores de aplicação do Office 365.

1.  Iniciar sessão no [Portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2.  Selecione **todos os serviços**, introduza **Azure Active Directory** na caixa de texto e, em seguida, selecione **Enter**.
3.  Sobre o **Azure Active Directory – *directoryname***  tela (ou seja, o Azure AD do ecrã para o diretório que está a gerir), selecione **aplicações empresariais**.
![Aplicações empresariais](./media/hide-application-from-user-portal/app1.png)
4.  Sobre o **aplicações empresariais** ecrã, selecione **todos os aplicativos**. Verá uma lista de aplicações que pode gerir.
5.  Sobre o **aplicações empresariais - todas as aplicações** ecrã, selecione uma aplicação.</br>
![Aplicações empresariais](./media/hide-application-from-user-portal/app2.png)
6.  Sobre o ***appname*** tela (ou seja, a tela com o nome da aplicação selecionada no título), selecione propriedades.
7.  Sobre o  ***appname* -propriedades** ecrã, selecione **Sim** para **visível aos utilizadores?**.
![Aplicações empresariais](./media/hide-application-from-user-portal/app3.png)
8.  Selecione o **guardar** comando.

### <a name="how-do-i-hide-office-365-applications-from-users-access-panel"></a>Como posso ocultar aplicações do Office 365, do painel de acesso do utilizador?

Utilize os seguintes passos para ocultar todos os aplicativos do Office 365 do painel de acesso. Estas aplicações continuarão a estar visíveis no portal do Office 365.

1.  Iniciar sessão no [Portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2.  Selecione **todos os serviços**, introduza **Azure Active Directory** na caixa de texto e, em seguida, selecione **Enter**.
3.  Na **Azure Active Directory – *directoryname***  tela (ou seja, o Azure AD do ecrã para o diretório que está a gerir), selecione **definições de utilizador**.
4.  Sobre o **as definições de utilizador** ecrã, em **aplicações empresariais** selecione **Sim** para **os utilizadores apenas podem ver aplicações do Office 365 no portal do Office 365**.

![Aplicações empresariais](./media/hide-application-from-user-portal/apps4.png)

## <a name="next-steps"></a>Passos Seguintes
* [Ver todos os meus grupos](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Atribuir um utilizador ou grupo a uma aplicação empresarial](assign-user-or-group-access-portal.md)
* [Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial](remove-user-or-group-access-portal.md)
* [Alterar o nome ou logótipo de uma aplicação empresarial](change-name-or-logo-portal.md)

