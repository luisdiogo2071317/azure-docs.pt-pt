---
title: Configurar contas de início de sessão no Azure Active Directory uma política incorporada no Azure Active Directory B2C | Documentos da Microsoft
description: Configure início de sessão no Azure Active Directory contas de uma política incorporada no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 64cd440947c95de92ea156c14e4c524ecdc8e76c
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268821"
---
# <a name="set-up-sign-in-azure-active-directory-accounts-a-built-in-policy-in-azure-active-directory-b2c"></a>Configurar contas de início de sessão no Azure Active Directory uma política incorporada no Azure Active Directory B2C

>[!NOTE]
> Esta funcionalidade está em pré-visualização pública. Não utilize a funcionalidade em ambientes de produção.

Este artigo mostra como ativar o início de sessão para os utilizadores de uma organização específica do Azure Active Directory (Azure AD) com uma política incorporada no Azure Active Directory (Azure AD) B2C.

## <a name="create-an-azure-ad-app"></a>Criar uma aplicação do Azure AD

Para ativar o início de sessão para que os utilizadores específicos de um organização do Azure AD, terá de registar uma aplicação organizacionais no inquilino do Azure AD.

>[!NOTE]
>`Contoso.com` é utilizado o organizacional para inquilino do Azure AD e `fabrikamb2c.onmicrosoft.com` é utilizado como o inquilino do Azure AD B2C nas instruções seguintes.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C (fabrikamb2c.onmicrosoft.com) ao clicar no filtro de diretório e subscrição no menu superior e escolher o diretório que contém o seu inquilino do Azure AD B2C.
3. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **registos das aplicações**.
4. Selecione **Novo registo de aplicação**.
5. Introduza um nome para a aplicação. Por exemplo, `Azure AD B2C App`.
6. Para o **tipo de aplicação**, selecione `Web app / API`.
7. Para o **URL de início de sessão**, introduza o seguinte URL em todas as letras minúsculas, onde `your-tenant` é substituído pelo nome do inquilino do Azure AD B2C (fabrikamb2c.onmicrosoft.com):

    ```
    https://yourtenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp
    ```

8. Clique em **Criar**. Copiar o **ID da aplicação** a ser utilizado mais tarde.
9. Selecione a aplicação e, em seguida, selecione **definições**.
10. Selecione **chaves**, introduza a descrição da chave, selecione uma duração e, em seguida, clique em **guardar**. Copie o valor da chave que é apresentada a ser utilizado mais tarde.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Configurar o Azure AD como fornecedor de identidade no seu inquilino

1. Certifique-se de que está a utilizar o diretório que contém o Azure organizacional inquilino do AD (contoso.com) ao clicar o **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
2. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **do Azure AD B2C**.
3. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
4. Introduza um **nome**. Por exemplo, introduza "Contoso do Azure AD".
5. Selecione **tipo de fornecedor de identidade**, selecione **abrir ID Connect**e clique em **OK**.
6. Clique em **configurar este fornecedor de identidade**
7. Para **url de metadados**, introduza o URL seguinte substituindo `your-tenant` com o nome do inquilino do Azure AD. Por exemplo, contoso.com:

    ```
    https://login.microsoftonline.com/your-tenant/.well-known/openid-configuration
    ```

8. Opcionalmente, introduza um valor para **domínio** (por exemplo, `ContosoAD`). Este é o valor a utilizar quando nos Referimos a este fornecedor de identidade usando *domain_hint* no pedido. 
9. Clique em **OK**.
10. Selecione **mapear declarações do fornecedor de identidade** e defina as seguintes declarações:
    
    - Para **ID de utilizador**, introduza `oid`.
    - Para **nome a apresentar**, introduza `name`.
    - Para **nome fornecido**, introduza `given_name`.
    - Para **Apelido**, introduza `family_name`.
    - Para **E-Mail**, introduza `unique_name`.

11. Clique em **OK**e, em seguida **criar** para guardar a configuração.
