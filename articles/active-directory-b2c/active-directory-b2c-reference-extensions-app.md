---
title: As extensões aplicação no Azure Active Directory B2C | Microsoft Docs
description: Restaurar a aplicação de extensões de b2c.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 9/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ad908e7408c2d14a843af49bc091fea725bfba1d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712288"
---
# <a name="azure-ad-b2c-extensions-app"></a>Do Azure AD B2C: Aplicação de extensões

Quando é criado um diretório do Azure AD B2C, uma aplicação chamar `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` é criado automaticamente dentro do novo diretório. Esta aplicação, referida como a **aplicação de extensões de b2c**, fica visível no *registos de aplicação*. É utilizado pelo serviço do Azure AD B2C para armazenar informações sobre utilizadores e os atributos personalizados. Se a aplicação for eliminada, o Azure AD B2C não irão funcionar corretamente e ambiente de produção será afetado.

> [!IMPORTANT]
> Não elimine a aplicação de extensões de b2c, exceto se estiver a planear eliminar imediatamente o seu inquilino. Se a aplicação permanece eliminada mais de 30 dias, informações de utilizador serão perdidas permanentemente.

## <a name="verifying-that-the-extensions-app-is-present"></a>Verificar se a aplicação de extensões está presente

Para verificar que a aplicação de extensões de b2c está presente:

1. No interior do seu inquilino do Azure AD B2C, clique em **todos os serviços** no menu de navegação esquerdo.
1. Procure e abra **registos de aplicação**.
1. Procurar uma aplicação que comece com **aplicação de extensões de b2c**

## <a name="recover-the-extensions-app"></a>Recuperar a aplicação de extensões

Caso o tenha eliminado acidentalmente a aplicação de extensões de b2c, tem de 30 dias para recuperar. Pode restaurar a aplicação utilizar a Graph API:

1. Navegue até à [ https://graphexplorer.azurewebsites.net/ ](https://graphexplorer.azurewebsites.net/).
1. Inicie sessão no site como um administrador global para o diretório do Azure AD B2C que pretende restaurar a aplicação eliminada para. Este administrador global tem de ter um endereço de e-mail semelhante ao seguinte: `username@{yourTenant}.onmicrosoft.com`.
1. Emitir um GET de HTTP relativamente ao URL `https://graph.windows.net/myorganization/deletedApplications` com a api-version = 1.6. Esta operação irá listar todas as aplicações que tenham sido eliminadas nos últimos 30 dias.
1. Localizar a aplicação na lista onde o nome começa com 'aplicação da extensão b2c' e copie o `objectid` valor da propriedade.
1. Emitir um POST de HTTP relativamente ao URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Substitua o `{OBJECTID}` parte do URL com a `objectid` do passo anterior. 

Deve agora poderá [veem a aplicação restaurada](#verifying-that-the-extensions-app-is-present) no portal do Azure.

> [!NOTE]
> Uma aplicação só pode ser restaurada se foi eliminado nos últimos 30 dias. Se já tiverem mais de 30 dias, os dados serão perdidos permanentemente. Para obter mais assistência, um pedido de suporte de ficheiros.