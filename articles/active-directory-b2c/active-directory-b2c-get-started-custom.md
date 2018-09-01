---
title: Introdução às políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Como começar com as políticas personalizadas do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 36fad697758273246d567dfa1010f0e6bfc68939
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344567"
---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>O Azure Active Directory B2C: Introdução às políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Depois de concluir os passos neste artigo, a diretiva personalizada oferecerá suporte a "conta local" inscrição ou início de sessão por meio de um endereço de e-mail e palavra-passe. Também irá preparar o ambiente para o adicionar fornecedores de identidade (como o Facebook ou do Azure Active Directory). Aconselhamo-lo para concluir estes passos antes de ler sobre outros usos do Framework de experiência de identidade do Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, certifique-se de que tem um inquilino do Azure AD B2C, que é um contentor para todos os seus utilizadores, aplicações, políticas e muito mais. Se ainda não tiver um, tem de [criar um inquilino do Azure AD B2C](active-directory-b2c-get-started.md). Recomendamos todos os desenvolvedores para concluir as instruções de política incorporada do Azure AD B2C e configurar seus aplicativos com as políticas incorporadas antes de continuar. Seus aplicativos funcionarão com ambos os tipos de políticas uma vez que fizer uma alteração menor ao nome da política para invocar a política personalizada.

>[!NOTE]
>Para aceder a edição de política personalizada, precisa de uma subscrição do Azure válida associada ao seu inquilino. Se ainda não o tiver [ligado o inquilino do Azure AD B2C para uma subscrição do Azure](active-directory-b2c-how-to-enable-billing.md) ou a sua subscrição do Azure está desativada, o botão de arquitetura de experiências de identidade não estará disponível.

## <a name="add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies"></a>Adicione chaves de assinatura e encriptação para o inquilino de B2C para utilização por políticas personalizadas

1. Abra o **arquitetura de experiências de identidade** painel nas definições de inquilino do Azure AD B2C.
2. Selecione **chaves de política** para exibir as chaves disponíveis no seu inquilino.
3. Se não existir, crie B2C_1A_TokenSigningKeyContainer:<br>
    a. Selecione **Adicionar**. <br>
    b. Selecione **gerar**.<br>
    c. Para **Name**, utilize `TokenSigningKeyContainer`. <br> 
    O prefixo `B2C_1A_` podem ser adicionados automaticamente.<br>
    d. Para **tipo de chave**, utilize **RSA**.<br>
    e. Para **datas**, utilize as predefinições. <br>
    f. Para **utilização de chave**, utilize **assinatura**.<br>
    g. Selecione **Criar**.<br>
4. Se não existir, crie B2C_1A_TokenEncryptionKeyContainer:<br>
 a. Selecione **Adicionar**.<br>
 b. Selecione **gerar**.<br>
 c. Para **Name**, utilize `TokenEncryptionKeyContainer`. <br>
   O prefixo `B2C_1A`_ podem ser adicionados automaticamente.<br>
 d. Para **tipo de chave**, utilize **RSA**.<br>
 e. Para **datas**, utilize as predefinições.<br>
 f. Para **utilização de chave**, utilize **encriptação**.<br>
 g. Selecione **Criar**.<br>
5. Crie B2C_1A_FacebookSecret. <br>
Se já tiver um segredo de aplicação do Facebook, adicione-o como uma chave de política com o seu inquilino. Caso contrário, tem de criar a chave com um valor de marcador de posição para que as políticas de passam na validação.<br>
 a. Selecione **Adicionar**.<br>
 b. Para **opções**, utilize **Manual**.<br>
 c. Para **Name**, utilize `FacebookSecret`. <br>
 O prefixo `B2C_1A_` podem ser adicionados automaticamente.<br>
 d. Na **segredo** , introduza o seu FacebookSecret de developers.facebook.com ou `0` como um marcador de posição. *Não se trata de sua ID de aplicação do Facebook.* <br>
 e. Para **utilização de chave**, utilize **assinatura**. <br>
 f. Selecione **criar** e confirme a criação.

## <a name="register-identity-experience-framework-applications"></a>Registar aplicações de arquitetura de experiências de identidade

O Azure AD B2C exige que Registre-se duas aplicações adicionais que são utilizadas pelo mecanismo para inscrição e início de sessão dos utilizadores.

>[!NOTE]
>Tem de criar duas aplicações que ativar início de sessão com contas locais: IdentityExperienceFramework (uma aplicação web) e ProxyIdentityExperienceFramework (uma aplicação nativa) com o delegado permissão da aplicação IdentityExperienceFramework. Contas locais existem apenas no seu inquilino. Os utilizadores inscrever-se com uma combinação de endereço/palavra-passe de e-mail exclusivo para aceder às suas aplicações de inquilino registado.

### <a name="create-the-identityexperienceframework-application"></a>Criar a aplicação de IdentityExperienceFramework

1. Na [portal do Azure](https://portal.azure.com), mudar para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md).
2. Abra o **do Azure Active Directory** painel (não o **do Azure AD B2C** painel). Poderá ter de selecionar **mais serviços** encontrá-lo.
3. Selecione **Registos das aplicações**.
4. Selecione **Novo registo de aplicação**.
   * Para **Name**, utilize `IdentityExperienceFramework`.
   * Para **tipo de aplicação**, utilize **aplicação/API Web**.
   * Para **URL de início de sessão**, utilize `https://yourtenant.b2clogin.com/yourtenant.onmicrosoft.com`, onde `yourtenant` é o seu nome de domínio de inquilino do Azure AD B2C.
5. Selecione **Criar**.
6. Depois de criado, selecione a aplicação criada recentemente **IdentityExperienceFramework**.<br>
   * Selecione **propriedades**.<br>
   * Copie o ID da aplicação e guarde-o para utilizar mais tarde.

### <a name="create-the-proxyidentityexperienceframework-application"></a>Criar a aplicação de ProxyIdentityExperienceFramework

1. Selecione **Registos das aplicações**.
1. Selecione **Novo registo de aplicação**.
   * Para **Name**, utilize `ProxyIdentityExperienceFramework`.
   * Para **tipo de aplicação**, utilize **nativo**.
   * Para **URI de redirecionamento**, utilize `https://yourtenant.b2clogin.com/yourtenant.onmicrosoft.com`, onde `yourtenant` é o seu inquilino do Azure AD B2C.
1. Selecione **Criar**.
1. Depois de este ter sido criado, selecione a aplicação **ProxyIdentityExperienceFramework**.<br>
   * Selecione **propriedades**. <br>
   * Copie o ID da aplicação e guarde-o para utilizar mais tarde.
1. Selecione **permissões obrigatórias**.
1. Selecione **Adicionar**.
1. Selecione **Selecionar uma API**.
1. Procure o nome IdentityExperienceFramework. Selecione **IdentityExperienceFramework** nos resultados e, em seguida, clique **selecione**.
1. Selecione a caixa de verificação junto a **acesso IdentityExperienceFramework**e, em seguida, clique em **selecione**.
1. Selecione **Done** (Concluído).
1. Selecione **conceder permissões**e, em seguida, confirme selecionando **Sim**.

## <a name="download-starter-pack-and-modify-policies"></a>Transferir o pacote de iniciante e modificar as políticas

As políticas personalizadas são um conjunto de arquivos XML que devem ser carregados para o inquilino do Azure AD B2C. Fornecemos pacotes starter para ajudá-lo a trabalhar rapidamente. Cada pacote de iniciante na lista seguinte contém o menor número de perfis técnicos e jornadas de utilizador necessárias para atingir os cenários descritos:
 * LocalAccounts. Permite a utilização de apenas contas locais.
 * SocialAccounts. Permite a utilização de apenas para contas de redes sociais (ou federadas).
 * **SocialAndLocalAccounts**. Podemos usar esse arquivo para o passo a passo.
 * SocialAndLocalAccountsWithMFA. Opções de redes sociais, o local e o multi-factor Authentication são incluídas aqui.

Cada pacote de iniciante contém:

* O [ficheiro base](active-directory-b2c-overview-custom.md#policy-files) da política. Algumas modificações são necessárias para a base.
* O [ficheiro de extensão](active-directory-b2c-overview-custom.md#policy-files) da política.  Este ficheiro é onde a maior parte das alterações de configuração são feitas.
* [Ficheiros de terceiros entidade confiadora](active-directory-b2c-overview-custom.md#policy-files) são chamados pelo seu aplicativo de ficheiros de tarefa específica.

>[!NOTE]
>Se o seu editor de XML dá suporte a validação, Valide os ficheiros no esquema de TrustFrameworkPolicy_0.3.0.0.xsd XML que está localizado no diretório de raiz do pacote de iniciante. Validação do esquema XML identifica os erros antes de carregar.

 Vamos começar:

1. Baixe active-directory-b2c-custom-policy-starterpack do GitHub. [Transfira o ficheiro. zip](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) ou executar

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. Abra a pasta de SocialAndLocalAccounts.  O ficheiro de base (TrustFrameworkBase.xml) nesta pasta contém conteúdo necessário para contas locais e social corporativa. O conteúdo social não interfere com os passos para colocar as contas locais em funcionamento.
3. Abra TrustFrameworkBase.xml. Se precisar de um editor de XML, [Experimente o Visual Studio Code](https://code.visualstudio.com/download), um editor simples de várias plataformas.
4. Na raiz `TrustFrameworkPolicy` elemento, atualize o `TenantId` e `PublicPolicyUri` atributos, substituindo `yourtenant.onmicrosoft.com` com o nome de domínio do inquilino do Azure AD B2C:
   ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="yourtenant.onmicrosoft.com"
    PolicyId="B2C_1A_TrustFrameworkBase"
    PublicPolicyUri="http://yourtenant.onmicrosoft.com">
    ```
   >[!NOTE]
   >`PolicyId` é o nome da política que vê no portal e o nome pelo qual este ficheiro de política é referenciado por outros ficheiros de política.

5. Guarde o ficheiro.
6. Abra TrustFrameworkExtensions.xml. Faça as alterações de dois mesmo, substituindo `yourtenant.onmicrosoft.com` com o seu inquilino do Azure AD B2C. Fazer a mesma substituição no `<TenantId>` elemento para um total de três alterações. Guarde o ficheiro.
7. Abra SignUpOrSignIn.xml. Tornar as mesmas alterações ao substituir `yourtenant.onmicrosoft.com` com o seu inquilino do Azure AD B2C em três locais. Guarde o ficheiro.
8. Abra a reposição de palavra-passe e editar ficheiros de perfil. Tornar as mesmas alterações ao substituir `yourtenant.onmicrosoft.com` com o seu inquilino do Azure AD B2C em três locais em cada arquivo. Guarde os ficheiros.

### <a name="add-the-application-ids-to-your-custom-policy"></a>Adicionar os IDs de aplicação à sua política personalizada
Adicionar os IDs de aplicação para o arquivo de extensões (`TrustFrameworkExtensions.xml`):

1. O arquivo de extensões (TrustFrameworkExtensions.xml), localize o elemento `<TechnicalProfile Id="login-NonInteractive">`.
2. Substitua as duas instâncias de `IdentityExperienceFrameworkAppId` com o ID de aplicação a arquitetura de experiências de identidade que criou anteriormente. Segue-se um exemplo:

   ```xml
   <Item Key="IdTokenAudience">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Substitua as duas instâncias de `ProxyIdentityExperienceFrameworkAppId` com o ID de aplicação a arquitetura de experiências de identidade de Proxy que criou anteriormente.
4. Guarde o ficheiro de extensões.

## <a name="upload-the-policies-to-your-tenant"></a>Carregar as políticas para o seu inquilino

1. Na [portal do Azure](https://portal.azure.com), mudar para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e abra a **do Azure AD B2C** painel.
2. Selecione **arquitetura de experiências de identidade**.
3. Selecione **carregar política**.

    >[!WARNING]
    >Os ficheiros de política personalizada tem de ser carregados na seguinte ordem:

1. Carregar TrustFrameworkBase.xml.
2. Carregar TrustFrameworkExtensions.xml.
3. Carregar SignUpOrSignin.xml.
4. Carregar os outros ficheiros de política.

Quando um ficheiro é carregado, o nome do ficheiro de política é anexado ao `B2C_1A_`.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testar a política personalizada com executar agora

1. Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.

   >[!NOTE]
   >**Executar agora** requer, pelo menos, um aplicativo para ser foi pré-registado no inquilino. Aplicações tem de estar registadas no inquilino do B2C, através da utilização a **aplicativos** seleção de menu no Azure AD B2C ou com o Framework de experiência de identidade para invocar as políticas incorporadas e personalizadas. É necessário apenas um registo por aplicação.<br><br>
   Para saber como registar aplicações, veja o Azure AD B2C [começar](active-directory-b2c-get-started.md) artigo ou o [registo de aplicação](active-directory-b2c-app-registration.md) artigo.  

2. Abra B2C_1A_signup_signin, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**.

3. Deverá conseguir inscrever-se através de um endereço de e-mail.

4. Inicie sessão com a mesma conta para confirmar que tem a configuração correta.

>[!NOTE]
>Uma causa comum de falha de início de sessão é uma aplicação de IdentityExperienceFramework configurada incorretamente.


## <a name="next-steps"></a>Passos Seguintes

### <a name="add-facebook-as-an-identity-provider"></a>Adicionar o Facebook como um fornecedor de identidade
Para configurar o Facebook:
1. [Configurar uma aplicação do Facebook no developers.facebook.com](active-directory-b2c-setup-fb-app.md).
2. [Adicione o segredo de aplicação do Facebook para o inquilino do Azure AD B2C](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. No ficheiro de política TrustFrameworkExtensions, substitua o valor do `client_id` com o ID de aplicação do Facebook:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. Carregue o ficheiro de política de TrustFrameworkExtensions.xml ao seu inquilino.
5. Teste usando **executar agora** ou ao invocar a política diretamente a partir da aplicação registada.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Adicionar o Azure Active Directory como um fornecedor de identidade
O ficheiro de base utilizado neste guia de introdução já contém alguns do conteúdo que precisa para adicionar outros fornecedores de identidade. Para obter informações sobre como configurar inícios de sessão, consulte a [do Azure Active Directory B2C: Inicie sessão com contas do Azure AD](active-directory-b2c-setup-aad-custom.md) artigo.

Para uma descrição geral das políticas personalizadas no Azure AD B2C que usam o Framework de experiência de identidade, consulte a [do Azure Active Directory B2C: políticas personalizadas](active-directory-b2c-overview-custom.md) artigo. 
