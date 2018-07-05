---
title: Adicionar o ADFS como um fornecedor de identidade com as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Um artigo que mostra como configurar 2016 AD FS utilizando o protocolo SAML e as políticas personalizadas
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a81baae553bbf9c58d42372e25e90cd7588f2952
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445080"
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>O Azure Active Directory B2C: Adicionar o ADFS como um fornecedor de identidade com as políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra-lhe como ativar o início de sessão para os utilizadores da conta do AD FS através da utilização de [políticas personalizadas](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Pré-requisitos

Conclua os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

Estes passos incluem:

1.  A criação de uma entidade Confiadora de ADFS.
2.  Adiciona o certificado do ADFS entidade Confiadora para o Azure AD B2C.
3.  Adicionar o fornecedor de afirmações para uma política.
4.  Registar o AD FS conta afirmações do fornecedor para um percurso do utilizador.
5.  Carregar a política para um Azure AD B2C de inquilino e testá-lo.

## <a name="to-create-a-claims-aware-relying-party-trust"></a>Para criar um suporte para afirmações entidade Confiadora

Para utilizar o ADFS como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar um ADFS entidade Confiadora e fornecer os parâmetros certos.

Para adicionar uma nova confiança da entidade confiadora, utilizando o snap-in Gestão do AD FS e configurar manualmente as definições, execute o seguinte procedimento num servidor de Federação.

A associação **administradores**, ou equivalente, no computador local é o mínimo requerido para concluir este procedimento. Veja os detalhes sobre as contas adequadas e associações a grupos em [locais e grupos predefinidos do domínio](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  No Gestor de servidor, clique em **ferramentas**e, em seguida, selecione **gestão de ADFS**.

2.  Clique em **adicionar confiança de entidade Confiadora**.
    ![Adicionar confiança de entidade Confiadora](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  Sobre o **bem-vindo** página, selecione **com suporte para afirmações** e clique em **iniciar**.
    ![Na página de boas-vindas, selecione com suporte para afirmações](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  Sobre o **selecionar origem de dados** página, clique em **introduzir dados sobre a entidade confiadora manualmente**e, em seguida, clique em **seguinte**.
    ![Introduzir dados sobre a entidade confiadora](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  Sobre o **Especificar nome a apresentar** página, escreva um nome na **nome a apresentar**, em **notas** escreva uma descrição para esta fidedignidade de entidade confiadora e, em seguida, clique em **seguinte** .
    ![Especifique o nome a apresentar e notas](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  Opcional. Se tiver um certificado de encriptação de tokens opcional, em seguida, na **configurar certificado** página, clique em **procurar** para localizar o ficheiro de certificado e, em seguida, clique em **seguinte**.
    ![Configurar certificado](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  Sobre o **configurar URL** página, selecione a **ativar o suporte para o protocolo SAML 2.0 WebSSO** caixa de verificação. Sob **URL do serviço Relying festa SAML 2.0 SSO**, escreva o URL de ponto final de serviço de Security Assertion Markup Language (SAML) para esta fidedignidade de entidade confiadora e, em seguida, clique em **próxima**.  Para o **URL do serviço Relying festa SAML 2.0 SSO**, cole o `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`. Substitua {inquilino} com o nome do inquilino (por exemplo, contosob2c.onmicrosoft.com) e substitua {política} pelo nome da sua política extensões (por exemplo, B2C_1A_TrustFrameworkExtensions).
    > [!IMPORTANT]
    >O nome da política é o que política signup_or_signin herda, neste caso é: `B2C_1A_TrustFrameworkExtensions`.
    >Por exemplo poderia ser o URL: https://login.microsoftonline.com/te/ **contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![URL do serviço de SAML 2.0 SSO de terceiros de entidade confiadora](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. Sobre o **configurar identificadores** página, especifique o mesmo URL que o passo anterior, clique em **Add** adicioná-los à lista e, em seguida, clique em **seguinte**.
    ![Identificadores de confiança de entidade confiadora](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  Sobre o **escolher política de controlo de acesso** Selecione uma política e clique em **próxima**.
    ![Escolha a política de controlo de acesso](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  Sobre o **pronto para adicionar confiança** página, reveja as definições e, em seguida, clique em **próxima** para guardar a sua entidade confiadora confiar informações.
    ![Guardar as suas informações de fidedignidade de terceiros entidade confiadora](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  Sobre o **concluir** página, clique em **fechar**, esta ação apresenta automaticamente o **editar regras de afirmação** caixa de diálogo.
    ![Editar regras de afirmação](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Clique em **Adicionar regra**.  
      ![Adicionar nova regra](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  Na **modelo de regra de afirmação**, selecione **enviar atributos LDAP como afirmações**.
    ![Selecione enviar atributos LDAP como afirmações regra de modelo](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Fornecer **nome da regra de afirmação**. Para o **arquivo de atributos** selecionar **selecione Active Directory** adicione as seguintes declarações, em seguida, clique em **concluir** e **OK**.
    ![Propriedades da regra de conjunto](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  No Gestor de servidor, selecione **entidade Confiadora confianças** , em seguida, selecione a entidade confiadora confia que criou e clique em **propriedades**.
    ![Propriedades de edição de terceiros entidade confiadora](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Um clique a janela de propriedades de fidedignidade (demonstração de B2C) de terceiros entidade confiadora **assinatura** separador e clique em **Add**.  
    ![Assinatura de conjunto](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  Adicione o certificado de assinatura (ficheiro .cert, sem a chave privada).  
    ![Adicionar o certificado de assinatura](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  Na janela de propriedades de fidedignidade (demonstração de B2C) de terceiros entidade confiadora, clique em **avançadas** separador e altere a **algoritmo hash seguro** para **SHA-1**, clique em **Ok**.  
    ![Definir o algoritmo hash seguro para SHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Adicionar a chave de conta da aplicação do AD FS para o Azure AD B2C
Federação com contas do AD FS requer um segredo do cliente para a conta do AD FS à confiança do Azure AD B2C em nome do aplicativo. Precisa armazenar o certificado ADFS no seu inquilino do Azure AD B2C. 

1.  Aceda ao seu inquilino do Azure AD B2C e selecione **definições do B2C** > **Framework de experiência de identidade**
2.  Selecione **chaves de política** para exibir as chaves disponíveis no seu inquilino.
3.  Clique em **+ adicionar**.
4.  Para **opções**, utilize **carregar**.
5.  Para **Name**, utilize `ADFSSamlCert`.  
    O prefixo `B2C_1A_` podem ser adicionados automaticamente.
6.  No carregamento de ficheiros, * * selecione o ficheiro. pfx de certificado com chave privada. Nota: este certificado (com a chave privada) deve ser a mesma que emitido e utilizado para a terceira parte confiável ADFS.
![Carregar a chave da política](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  Clique em **Criar**.
8.  Confirme que criou a chave `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Adicionar um fornecedor de afirmações na sua política de extensão
Se pretender que os utilizadores iniciem sessão com a conta do AD FS, terá de definir a conta ADFS como um fornecedor de afirmações. Em outras palavras, terá de especificar um ponto de final do Azure AD B2C se comunica com. O ponto final fornece um conjunto de afirmações que são utilizadas pelo Azure AD B2C para verificar que um utilizador específico foi autenticado.

Definir o ADFS como um fornecedor de afirmações, adicionando `<ClaimsProvider>` nó em seu arquivo de política de extensão:

1. Abra o ficheiro de política de extensão (TrustFrameworkExtensions.xml) a partir do diretório de trabalho. Se precisar de um editor de XML, [Experimente o Visual Studio Code](https://code.visualstudio.com/download), um editor simples de várias plataformas.
2. Encontrar o `<ClaimsProviders>` secção
3. Adicione o seguinte fragmento XML sob o `ClaimsProviders` elemento e substitua `identityProvider` com o seu DNS (valor arbitrário que indica o seu domínio) e guarde-o. 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
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

## <a name="register-the-adfs-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registar o fornecedor de afirmações do ADFS conta para início de sessão se ou iniciar sessão no percurso do utilizador
Neste momento, o fornecedor de identidade foi configurado.  No entanto, não está disponível em qualquer uma dos ecrãs de início de sessão-inscrição/início de sessão. Agora precisa adicionar o fornecedor de identidade de conta do AD FS para o seu utilizador `SignUpOrSignIn` percurso do utilizador. Para tornar disponível, criamos um duplicado de um percurso do utilizador modelo existente.  Em seguida, podemos modificá-lo para que ele inclui o fornecedor de identidade do AD FS:

>[!NOTE]
>Se copiou anteriormente o `<UserJourneys>` elemento do ficheiro de base da sua política de para o ficheiro de extensão (TrustFrameworkExtensions.xml), pode ignorar esta secção.

1.  Abra o ficheiro de base da sua política (por exemplo, TrustFrameworkBase.xml).
2.  Encontrar o `<UserJourneys>` elemento e copie todo o conteúdo do `<UserJourneys>` nó.
3.  Abra o ficheiro de extensão (por exemplo, TrustFrameworkExtensions.xml) e localize o `<UserJourneys>` elemento. Se o elemento não existir, adicione um.
4.  Cole o conteúdo inteiro do `<UserJournesy>` nó que copiou como subordinado do `<UserJourneys>` elemento.

### <a name="display-the-button"></a>Exibir o botão
O `<ClaimsProviderSelections>` elemento define a lista de opções de seleção do fornecedor de afirmações e sua ordem.  `<ClaimsProviderSelection>` elemento é semelhante a um botão do fornecedor de identidade numa página de início de sessão-inscrição/início de sessão. Se adicionar um `<ClaimsProviderSelection>` elemento para a conta ADFS, um novo botão exibido quando um utilizador que chegam na página. Para adicionar este elemento:

1.  Encontrar o `<UserJourney>` nó que inclui `Id="SignUpOrSignIn"` no percurso do utilizador que copiou.
2.  Localize o `<OrchestrationStep>` nó que inclui `Order="1"`
3.  Adicionar o seguinte fragmento XML em `<ClaimsProviderSelections>` nó:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação

Agora que tem um botão no local, terá de ligá-lo a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com a conta do AD FS para receber um token. Ligue o botão a uma ação, o perfil técnico de ligação para o seu fornecedor de afirmações do ADFS conta:

1.  Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
2.  Adicionar o seguinte fragmento XML em `<ClaimsExchanges>` nó:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Certifique-se a `Id` tem o mesmo valor que `TargetClaimsExchangeId` na secção anterior.
> * Certifique-se de `TechnicalProfileReferenceId` está definido para o perfil técnico que criou anteriormente (Contoso-SAML2).

## <a name="upload-the-policy-to-your-tenant"></a>Carregar a política para o seu inquilino
1.  Na [portal do Azure](https://portal.azure.com), mudar para o [contexto do inquilino do Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)e abra a **do Azure AD B2C** painel.
2.  Selecione **arquitetura de experiências de identidade**.
3.  Abra o **todas as políticas** painel.
4.  Selecione **carregar política**.
5.  Verifique **substituir a política, se existir** caixa.
6.  **Carregar** TrustFrameworkExtensions.xml e certifique-se de que ele não falha a validação

## <a name="test-the-custom-policy-by-using-run-now"></a>Testar a política personalizada com executar agora
1.  Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.
2.  Open **B2C_1A_signup_signin**, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**.
3.  Deverá conseguir iniciar sessão com a conta do AD FS.

## <a name="optional-register-the-adfs-account-claims-provider-to-profile-edit-user-journey"></a>[Opcional] Registar o fornecedor de afirmações do ADFS conta para percurso do utilizador de edição de perfil
Pode querer adicionar o fornecedor de identidade de conta ADFS também ao seu utilizador `ProfileEdit` percurso do utilizador. Para tornar disponível, repetimos os dois últimos passos:

### <a name="display-the-button"></a>Exibir o botão
1.  Abra o ficheiro de extensão da sua política (por exemplo, TrustFrameworkExtensions.xml).
2.  Encontrar o `<UserJourney>` nó que inclui `Id="ProfileEdit"` no percurso do utilizador que copiou.
3.  Localize o `<OrchestrationStep>` nó que inclui `Order="1"`
4.  Adicionar o seguinte fragmento XML em `<ClaimsProviderSelections>` nó:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação
1.  Encontrar o `<OrchestrationStep>` que inclua `Order="2"` no `<UserJourney>` nó.
2.  Adicionar o seguinte fragmento XML em `<ClaimsExchanges>` nó:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testar a política de edição de perfil personalizada com o executar agora
1.  Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.
2.  Open **B2C_1A_ProfileEdit**, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**.
3.  Deverá conseguir iniciar sessão com a conta do AD FS.

## <a name="download-the-complete-policy-files"></a>Transferir os ficheiros de política concluída
Opcional: Recomendamos que crie o seu cenário com sua própria política personalizada do percorrer a ficheiros depois de concluir o guia de introdução políticas personalizadas. [Ficheiros de exemplo de política apenas para referência](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
