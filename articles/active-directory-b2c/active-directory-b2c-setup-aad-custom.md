---
title: Adicionar um fornecedor do Azure AD ao utilizar políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre as políticas personalizadas do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: af97a85b4d5d9c38f0e2bf8947482a0585fa6ee1
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338316"
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>O Azure Active Directory B2C: Inicie sessão com contas do Azure AD

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra-lhe como ativar o início de sessão para os utilizadores de uma organização específica do Azure Active Directory (Azure AD) através da utilização de [políticas personalizadas](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Pré-requisitos

Conclua os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

Estes passos incluem:

1. Criar um Azure Active Directory B2C inquilino (Azure AD B2C).
2. Criar uma aplicação do Azure AD B2C.
3. A registar as duas aplicações de motor de política.
4. A configurar chaves.
5. A configurar o pacote de iniciante.

## <a name="create-an-azure-ad-app"></a>Criar uma aplicação do Azure AD

Para ativar o início de sessão para que os utilizadores específicos de um organização do Azure AD, terá de registar uma aplicação organizacionais no inquilino do Azure AD.

>[!NOTE]
> Utilizamos "contoso.com" o organizacional para inquilino do Azure AD e "fabrikamb2c.onmicrosoft.com" nome do inquilino do Azure AD B2C nas instruções seguintes.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Na barra superior, selecione a sua conta. Do **Directory** lista, escolha o organizacional inquilino do Azure AD onde pretende registar a sua aplicação (contoso.com).
3. Selecione **mais serviços** no painel esquerdo e procure "Registos de aplicações".
4. Selecione **Novo registo de aplicação**.
5. Introduza um nome para a sua aplicação (por exemplo, `Azure AD B2C App`).
6. Selecione **Aplicação/API Web** para o tipo de aplicação.
7. Para **URL de início de sessão**, introduza o URL seguinte, onde `yourtenant` é substituída pelo nome do inquilino do Azure AD B2C (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >O valor para "yourtenant" tem de estar todo em minúsculo no **URL de início de sessão**.

    ```
    https://yourtenant.b2clogin.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

8. Guarde o ID da aplicação.
9. Selecione a aplicação criada recentemente.
10. Sob o **configurações** painel, selecione **chaves**.
11. Introduza a descrição da chave, selecione uma duração e, em seguida, clique em **guardar**. O valor da chave é apresentado. Copie-a porque irá utilizá-lo nos passos na secção seguinte.

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a>Adicionar a chave do Azure AD para o Azure AD B2C

Precisa armazenar a chave da aplicação contoso.com no seu inquilino do Azure AD B2C. Para efetuar este procedimento:
1. Aceda ao seu inquilino do Azure AD B2C e selecione **definições do B2C** > **Framework de experiência de identidade** > **chaves da política**.
1. Selecione **+ adicionar**.
1. Selecione ou introduza estas opções:
   * Selecione **Manual**.
   * Para **Name**, escolha um nome que corresponde ao nome do seu inquilino do Azure AD (por exemplo, `ContosoAppSecret`).  O prefixo `B2C_1A_` é adicionado automaticamente o nome da sua chave.
   * Cole a chave de aplicação do **segredo** caixa.
   * Selecione **assinatura**.
1. Selecione **Criar**.
1. Confirme que criou a chave `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Adicionar um fornecedor de afirmações na sua política base

Se pretender que os utilizadores iniciem sessão através da utilização do Azure AD, terá de definir do Azure AD como um fornecedor de afirmações. Em outras palavras, terá de especificar um ponto de final do Azure AD B2C irá comunicar com. O ponto final irá fornecer um conjunto de afirmações que são utilizadas pelo Azure AD B2C para verificar que um utilizador específico foi autenticado. 

Pode definir do Azure AD como um fornecedor de afirmações ao Azure AD para adicionar o `<ClaimsProvider>` nó do arquivo de extensão da política:

1. Abra o ficheiro de extensão (TrustFrameworkExtensions.xml) a partir do diretório de trabalho.
1. Encontrar o `<ClaimsProviders>` secção. Se não existir, adicione-o sob o nó de raiz.
1. Adicionar um novo `<ClaimsProvider>` nó da seguinte forma:

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
                </OutputClaims>
                <OutputClaimsTransformations>
                    <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
                    <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
                    <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
                    <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
                </OutputClaimsTransformations>
                <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
            </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Sob o `<ClaimsProvider>` nó, atualize o valor para `<Domain>` para um valor exclusivo que pode ser utilizado para distinguir de outros fornecedores de identidade.
1. Sob o `<ClaimsProvider>` nó, atualize o valor para `<DisplayName>` para um nome amigável para o fornecedor de afirmações. Este valor não é atualmente utilizado.

### <a name="update-the-technical-profile"></a>Atualizar o perfil técnico

Para obter um token a partir do ponto final do Azure AD, terá de definir protocolos que o Azure AD B2C deve utilizar para comunicar com o Azure AD. Isso é feito no interior da `<TechnicalProfile>` elemento de `<ClaimsProvider>`.
1. O ID de atualização a `<TechnicalProfile>` nó. Este ID é utilizado para fazer referência a este perfil técnico de outras partes da política.
1. Atualize o valor para `<DisplayName>`. Este valor será apresentado no botão de início de sessão no seu ecrã de início de sessão.
1. Atualize o valor para `<Description>`.
1. Azure AD utiliza o protocolo OpenID Connect, por isso, certifique-se de que o valor para `<Protocol>` é `"OpenIdConnect"`.

Tem de atualizar o `<Metadata>` seção no arquivo XML referido anteriormente para refletir as definições de configuração específicas do seu inquilino do Azure AD. No ficheiro XML, atualize os valores de metadados da seguinte forma:

1. Definir `<Item Key="METADATA">` para `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, onde `yourAzureADtenant` é o nome do seu inquilino do Azure AD (contoso.com).
1. Abra o browser e aceda ao `METADATA` URL que acabou de atualizar.
1. No browser, procure o objeto "emissor" e copie o seu valor. Deve ter um aspeto semelhante ao seguinte: `https://sts.windows.net/{tenantId}/`.
1. Cole o valor para `<Item Key="ProviderName">` no arquivo XML.
1. Definir `<Item Key="client_id">` para o ID de aplicação do registo de aplicações.
1. Definir `<Item Key="IdTokenAudience">` para o ID de aplicação do registo de aplicações.
1. Certifique-se de que `<Item Key="response_types">` está definido como `id_token`.
1. Certifique-se de que `<Item Key="UsePolicyInRedirectUri">` está definido como `false`.

Também precisa de associar o segredo do Azure AD que registou no seu inquilino do Azure AD B2C para o Azure AD `<ClaimsProvider>` informações:

* Na `<CryptographicKeys>` secção no ficheiro XML anterior, atualize o valor para `StorageReferenceId` para o ID de referência do segredo do que definiu (por exemplo, `ContosoAppSecret`).

### <a name="upload-the-extension-file-for-verification"></a>Carregar o ficheiro de extensão de verificação

Agora, configurou sua política para que o Azure AD B2C sabe como se comunicar com o diretório do Azure AD. Tente carregar o ficheiro de extensão da sua política só para confirmar que ele não tem quaisquer problemas até agora. Para tal:

1. Abra o **todas as políticas** painel no seu inquilino do Azure AD B2C.
1. Verifique os **substituir a política, se existir** caixa.
1. Carregar o ficheiro de extensão (TrustFrameworkExtensions.xml) e certifique-se de que ele não falha a validação.

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a>Registar o fornecedor de afirmações do AD do Azure para um percurso do utilizador

Agora precisa adicionar o fornecedor de identidade do Azure AD para um dos seus jornadas de utilizador. Neste momento, o fornecedor de identidade tiver sido configurado, mas não está disponível em qualquer uma dos ecrãs de início de sessão-inscrição/início de sessão. Para tornar disponível, iremos criar um duplicado de um percurso do utilizador modelo existente e, em seguida, modificá-lo para que ele também tem o fornecedor de identidade do Azure AD:

1. Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).
1. Encontrar o `<UserJourneys>` elemento e copie todo o `<UserJourney>` nó que inclui `Id="SignUpOrSignIn"`.
1. Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e localize o `<UserJourneys>` elemento. Se o elemento não existir, adicione um.
1. Cole a toda `<UserJourney>` nó que copiou como subordinado do `<UserJourneys>` elemento.
1. Mudar o nome o ID do percurso do utilizador novo (por exemplo, mudar o nome como `SignUpOrSignUsingContoso`).

### <a name="display-the-button"></a>Exibir o "botão"

O `<ClaimsProviderSelection>` elemento é semelhante a um botão do fornecedor de identidade numa tela de início de sessão-inscrição/início de sessão. Se adicionar um `<ClaimsProviderSelection>` elemento para o Azure AD, um novo botão exibido quando um utilizador que chegam na página. Para adicionar este elemento:

1. Encontrar o `<OrchestrationStep>` nó que inclui `Order="1"` no percurso do utilizador que acabou de criar.
1. Adicione o seguinte:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Definir `TargetClaimsExchangeId` para um valor adequado. Recomendamos que siga a mesma Convenção outras pessoas:  *\[ClaimProviderName\]Exchange*.

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação

Agora que tem um botão no local, terá de ligá-lo a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com o Azure AD para receber um token. Ligue o botão a uma ação, o perfil técnico de ligação para o seu fornecedor de afirmações do AD do Azure:

1. Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
1. Adicione o seguinte:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Atualização `Id` para o mesmo valor que `TargetClaimsExchangeId` na secção anterior.
1. Atualização `TechnicalProfileReferenceId` para o ID do perfil técnico de que criou anteriormente (ContosoProfile).

### <a name="upload-the-updated-extension-file"></a>Carregue o ficheiro de extensão atualizada

Terminar de modificar o ficheiro de extensão. Guarde-o. Em seguida, carregue o ficheiro e certifique-se de que todas as validações tenha êxito.

### <a name="update-the-rp-file"></a>Atualizar o ficheiro RP

Agora precisa atualizar o ficheiro da entidade confiadora de terceiros (RP) que irá iniciar o percurso do utilizador que acabou de criar:

1. Faça uma cópia do SignUpOrSignIn.xml no diretório de trabalho e mude o nome (por exemplo, renomeá-lo para SignUpOrSignInWithAAD.xml).
1. Abra o ficheiro novo e a atualização do `PolicyId` atributo para `<TrustFrameworkPolicy>` com um valor exclusivo (por exemplo, SignUpOrSignInWithAAD). <br> Este será o nome da política (por exemplo, B2C\_1A\_SignUpOrSignInWithAAD).
1. Modificar a `ReferenceId` atributo `<DefaultUserJourney>` de acordo com o ID do percurso do utilizador novo que criou (SignUpOrSignUsingContoso).
1. Guardar as alterações e carregar o ficheiro.

## <a name="troubleshooting"></a>Resolução de problemas

Testar a política personalizada que acabou de carregar a abrir o painel e clicando em **executar agora**. Para diagnosticar problemas, leia sobre [resolução de problemas](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Passos Seguintes

Fornecer feedback para [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
