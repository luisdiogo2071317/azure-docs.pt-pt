---
title: Como remover o acesso de um utilizador a uma aplicação | Documentos da Microsoft
description: Aprenda a remover o acesso de um utilizador a uma aplicação
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0deb5215c1379ac552a492f4b9e90df83201aebf
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364486"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Como remover o acesso de um utilizador a uma aplicação

Este artigo ajuda-o a compreender como remover o acesso de um utilizador a uma aplicação.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Quero remover a atribuição de um utilizador específico ou do grupo a uma aplicação

Para remover um utilizador ou a atribuição de grupo a uma aplicação, siga os passos apresentados no [remover uma atribuição de utilizador ou grupo a partir de uma aplicação empresarial no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artigo.

. # # quero desativar todos os acessos a uma aplicação para cada utilizador

Para desativar todos os utilizadores inícios de sessão a uma aplicação, siga os passos apresentados no [desativar utilizador-inícios de sessão de uma aplicação empresarial no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artigo.

## <a name="i-want-to-delete-an-application-entirely"></a>Quero eliminar uma aplicação totalmente

Para **eliminar uma aplicação**, siga estas instruções:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Coadministrador.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

   * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação que pretende eliminar.

7.  Assim que o aplicativo é carregado, clique em **elimine** ícone da aplicação superior **descrição geral** painel.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Quero desativar todas as operações de consentimento de utilizador futuras para qualquer aplicação

Desativar o consentimento do utilizador para o diretório completo impedir que os utilizadores finais consentir qualquer aplicação. Os administradores ainda podem consentir em behalves do utilizador. Para saber mais sobre o consentimento da aplicação e, por isso que pode ou não querer fazer isso, leia [consentimento de administrador e de utilizador de compreensão](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

Para **desativar todas as operações de consentimento de utilizador futuras no seu diretório inteiro**, siga estas instruções:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **utilizadores e grupos** no menu de navegação.

5.  Clique em **definições de utilizador**.

6.  Desativar todas as operações de consentimento do utilizador futuras ao definir o **os utilizadores podem permitir que as aplicações acedam aos seus dados** alternar para **não** e clique nas **guardar** botão.


# <a name="next-steps"></a>Passos Seguintes
[Gerir o acesso a aplicações](manage-apps/what-is-access-management.md)
