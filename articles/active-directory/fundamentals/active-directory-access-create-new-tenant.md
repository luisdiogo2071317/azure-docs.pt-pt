---
title: Início Rápido para aceder ao Microsoft Azure Active Directory e para criar um novo inquilino | Microsoft Docs
description: Início Rápido com os passos para localizar o Microsoft Azure Active Directory e criar um novo inquilino para a sua organização.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: lizross
custom: it-pro
ms.openlocfilehash: eacb2e7c52adae9de0b74c3dade59446cc8459ed
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50023558"
---
# <a name="quickstart-access-azure-active-directory-to-create-a-new-tenant"></a>Início Rápido: Aceder ao Microsoft Azure Active Directory para criar um novo inquilino
Pode fazer todas as suas tarefas administrativas com o portal do Microsoft Azure Active Directory (Microsoft Azure AD), incluindo a criação de um novo inquilino para a sua organização. 

Neste início rápido, ficará a saber como aceder ao portal do Azure e do Microsoft Azure Active Directory e aprenderá como criar um inquilino básico para a sua organização.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [portal do Azure](https://portal.azure.com/) da sua organização com uma conta de Administrador global.

![Ecrã do portal do Azure](media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>Criar um novo inquilino para a sua organização
Depois de iniciar sessão no portal do Azure, pode criar um novo inquilino para a sua organização. O novo inquilino representa a sua organização e ajuda-o a gerir uma instância específica dos serviços cloud da Microsoft para os seus utilizadores internos e externos.

### <a name="to-create-a-new-tenant"></a>Para criar um novo inquilino
1. Selecione **Azure Active Directory**, selecione **Criar recursos**, selecione **Identidade** e, em seguida, selecione **Azure Active Directory**.

    É apresentada a página **Criar diretório**.

    ![Criar página do Microsoft Azure Active Directory](media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

2.  Na página **Criar diretório**, introduza as seguintes informações:
    
    - Introduza _Contoso_ na caixa **Nome da organização**.

    - Introduza _Contoso_ na caixa **Nome de domínio inicial**.

    - Deixe a opção _Estados Unidos_ na caixa **País ou região**.

3. Selecione **Criar**.

O novo inquilino é criado com o domínio contoso.onmicrosoft.com.

## <a name="clean-up-resources"></a>Limpar recursos
Se não quiser continuar a utilizar esta aplicação, poderá eliminar o inquilino através dos seguintes passos:

- Selecione **Azure Active Directory** e, em seguida, na página **Contoso – Descrição Geral**, selecione **Eliminar diretório**.

    O inquilino e a informação associada são eliminados.

    ![Página Criar diretório](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>Passos seguintes
- Para alterar ou adicionar nomes de domínio, veja [Como adicionar um nome de domínio personalizado ao Microsoft Azure Active Directory](add-custom-domain.md)

- Para adicionar utilizadores, veja [Adicionar ou eliminar um novo utilizador](add-users-azure-active-directory.md)

- Para adicionar grupos e membros, veja [Criar um grupo básico e adicionar membros](active-directory-groups-create-azure-portal.md)

- Saiba mais sobre o [acesso baseado em funções com o Privileged Identity Management](../../role-based-access-control/pim-azure-resource.md) e o [Acesso condicional](../../role-based-access-control/conditional-access-azure-management.md) para ajudar a gerir o acesso a aplicações e recursos da sua organização.

- Saiba mais sobre o Azure AD, incluindo [informações básicas de licenciamento, terminologia e funcionalidades associadas](active-directory-whatis.md).
