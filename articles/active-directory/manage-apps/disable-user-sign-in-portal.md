---
title: Desativar utilizador-inícios de sessão de uma aplicação empresarial no Azure Active Directory | Documentos da Microsoft
description: Como desativar uma aplicação empresarial, para que os utilizadores não poderão iniciar sessão na mesma no Azure Active Directory
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: celested
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdbabc1a1ccf4bf27172a4db53255eeb1576def9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56180509"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Desativar utilizador-inícios de sessão de uma aplicação empresarial no Azure Active Directory
É fácil desativar uma aplicação empresarial, para que os utilizadores não poderão iniciar sessão na mesma no Azure Active Directory (Azure AD). Tem de ter as permissões adequadas para gerir a aplicação da empresa e tem de ser administrador global do diretório.

## <a name="how-do-i-disable-user-sign-ins"></a>Como posso desativar inícios de sessão do utilizador?
1. Iniciar sessão no [Portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **todos os serviços**, introduza **Azure Active Directory** na caixa de texto e, em seguida, selecione **Enter**.
3. Sobre o **do Azure Active Directory** -  ***directoryname*** painel (ou seja, o Azure AD para o diretório que está a gerir), selecione **aplicações empresariais**.

    ![Aplicações empresariais de abertura](./media/disable-user-sign-in-portal/open-enterprise-apps.png)
4. Sobre o **aplicações empresariais** painel, selecione **todos os aplicativos**. Verá uma lista de aplicações que pode gerir.
5. Sobre o **aplicações empresariais - todas as aplicações** painel, selecione uma aplicação.
6. Sobre o ***appname*** painel (ou seja, o painel com o nome da aplicação selecionada no título), selecione **propriedades**.

    ![Selecionar o todos os comandos de aplicativos](./media/disable-user-sign-in-portal/select-app.png)
7. Sobre o ***appname*** - **propriedades** painel, selecione **não** para **ativado para os utilizadores para início de sessão?**.
8. Selecione o **guardar** comando.

## <a name="next-steps"></a>Passos Seguintes
* [Ver todos os meus grupos](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Atribuir um utilizador ou grupo a uma aplicação empresarial](assign-user-or-group-access-portal.md)
* [Remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial](remove-user-or-group-access-portal.md)
* [Alterar o nome ou logótipo de uma aplicação empresarial](change-name-or-logo-portal.md)
