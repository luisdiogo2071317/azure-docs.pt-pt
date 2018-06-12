---
title: Alterar o nome ou logótipo de uma aplicação empresarial no Azure Active Directory | Microsoft Docs
description: Como alterar o nome ou logótipo para uma aplicação empresarial personalizada no Azure Active Directory
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
ms.topic: article
ms.date: 08/28/2017
ms.author: barbkess
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: ad424d6ca8ea8c35aa502a3d1bd98940591c38e8
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303899"
---
# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Alterar o nome ou logótipo de uma aplicação empresarial no Azure Active Directory
É fácil de alterar o nome ou logótipo para uma aplicação empresarial personalizada no Azure Active Directory (Azure AD). Tem de ter as permissões adequadas para efetuar estas alterações, e tem de ser o criador da aplicação personalizada.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Como alterar nome ou logótipo de uma aplicação empresarial?
1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **todos os serviços**, introduza **do Azure Active Directory** na caixa de texto e, em seguida, selecione **Enter**.
3. No **Azure Active Directory - *directoryname***  painel (ou seja, o Azure AD para o diretório que está a gerir), selecione **aplicações empresariais**.

    ![Aplicações da empresa ao abrir](./media/change-name-or-logo-portal/open-enterprise-apps.png)
4. No **aplicações empresariais** painel, selecione **todas as aplicações**. É apresentada uma lista das aplicações que pode gerir.
5. No **aplicações da empresa - todas as aplicações** painel, selecione uma aplicação.
6. No ***appname*** painel (ou seja, o painel com o nome da aplicação selecionada no título), selecione **propriedades**.

    ![Selecionar o comando Propriedades](./media/change-name-or-logo-portal/select-app.png)
7. No ***appname*** **-propriedades** painel, procure um ficheiro para utilizar como um logótipo de novo, ou editar o nome da aplicação, ou ambos.

    ![Alterar o comando de logótipo ou nameproperties de aplicação](./media/change-name-or-logo-portal/change-logo.png)
8. Selecione o **guardar** comando.

## <a name="next-steps"></a>Passos Seguintes
* [Ver todos os meus grupos](../active-directory-groups-view-azure-portal.md)
* [Atribuir um utilizador ou grupo a uma aplicação empresarial](assign-user-or-group-access-portal.md)
* [Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial](remove-user-or-group-access-portal.md)
* [Desativar o utilizador inícios de sessão para uma aplicação empresarial](disable-user-sign-in-portal.md)
