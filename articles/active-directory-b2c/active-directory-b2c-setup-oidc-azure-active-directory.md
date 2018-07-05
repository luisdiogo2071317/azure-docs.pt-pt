---
title: Adicionar um fornecedor do Azure AD com as políticas incorporadas no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba como adicionar um fornecedor de identidade abrir ID Connect (Azure AD).
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 70d3a19b715052fe658102929a1c29cf3db2d595
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443734"
---
# <a name="azure-active-directory-b2c-sign-in-using-azure-ad-accounts-through-a-built-in-policy"></a>O Azure Active Directory B2C: Inicie sessão com contas do Azure AD através de uma política incorporada

>[!NOTE]
> Esta funcionalidade está em pré-visualização pública. Não utilize a funcionalidade em ambientes de produção.

Este artigo mostra como ativar o início de sessão para os utilizadores de um diretivas internas da organização do Azure Active Directory (Azure AD) específico.

## <a name="create-an-azure-ad-app"></a>Criar uma aplicação do Azure AD

Para ativar o início de sessão para que os utilizadores específicos de um organização do Azure AD, terá de registar uma aplicação organizacionais no inquilino do Azure AD.

>[!NOTE]
> Utilizamos "contoso.com" o organizacional para inquilino do Azure AD e "fabrikamb2c.onmicrosoft.com" nome do inquilino do Azure AD B2C nas instruções seguintes.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Na barra superior, selecione a sua conta. Do **Directory** lista, escolha o organizacional inquilino do Azure AD em que irá registar a sua aplicação (contoso.com).
1. Selecione **todos os serviços** no painel esquerdo e procure "Registos de aplicações".
1. Selecione **Novo registo de aplicação**.
1. Introduza um nome para a sua aplicação (por exemplo, `Azure AD B2C App`).
1. Selecione **Aplicação/API Web** para o tipo de aplicação.
1. Para **URL de início de sessão**, introduza o URL seguinte, onde `yourtenant` é substituída pelo nome do inquilino do Azure AD B2C (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >O valor para "yourtenant" tem de estar todo em minúsculo no **URL de início de sessão**.

    ```Console
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Guarde o ID da aplicação, que irá utilizar na secção seguinte como o ID de cliente.
1. Sob o **configurações** painel, selecione **chaves**.
1. Introduza um **descrição da chave** sob a **palavras-passe** secção e defina o **duração** para "Nunca expira". 
1. Clique em **salvar**e anote a chave resultante **valor**, que irá utilizar na secção seguinte como o segredo do cliente.

## <a name="configure-azure-ad-as-an-identity-provider-in-your-tenant"></a>Configurar o Azure AD como fornecedor de identidade no seu inquilino

1. Na barra superior, selecione a sua conta. Partir do **Directory** lista, escolha o inquilino do Azure AD B2C (fabrikamb2c.onmicrosoft.com).
1. [Navegue para o menu de definições do Azure AD B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) no portal do Azure.
1. No menu de definições do Azure AD B2C, clique em **fornecedores de identidade**.
1. Clique em **+ Adicionar** na parte superior do painel.
1. Fornecer um amigável **nome** para a configuração do fornecedor de identidade. Por exemplo, introduza "Contoso do Azure AD".
1. Clique em **tipo de fornecedor de identidade**, selecione **abrir ID Connect**e clique em **OK**.
1. Clique em **configurar este fornecedor de identidade**
1. Para **url de metadados**, introduza o URL seguinte, onde `yourtenant` é substituída pelo nome do inquilino do Azure AD (por exemplo, `contoso.com`):

    ```Console
    https://login.microsoftonline.com/yourtenant/.well-known/openid-configuration
    ```
1. Para o **ID de cliente** e **segredo do cliente**, introduza o ID da aplicação e a chave da secção anterior.
1. Mantenha o valor predefinido para **âmbito**, que deve ser definido como `openid`.
1. Mantenha o valor predefinido para **tipo de resposta**, que deve ser definido como `code`.
1. Mantenha o valor predefinido para **modo de resposta**, que deve ser definido como `form_post`.
1. Opcionalmente, introduza um valor para **domínio** (por exemplo, `ContosoAD`). Este é o valor a utilizar quando nos Referimos a este fornecedor de identidade usando *domain_hint* no pedido. 
1. Clique em **OK**.
1. Clique em **mapear declarações do fornecedor de identidade**.
1. Para **ID de utilizador**, introduza `oid`.
1. Para **nome a apresentar**, introduza `name`.
1. Para **nome fornecido**, introduza `given_name`.
1. Para **Apelido**, introduza `family_name`.
1. Para **E-Mail**, introduza `unique_name`
1. Clique em **OK**e, em seguida **criar** para guardar a configuração.

## <a name="next-steps"></a>Passos Seguintes

Adicionar o recém-criado fornecedor de identidade do Azure AD para uma política incorporada e fornecer comentários para [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
