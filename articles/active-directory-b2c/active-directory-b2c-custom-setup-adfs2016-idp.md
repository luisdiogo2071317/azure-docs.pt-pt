---
title: Adicionar o ADFS como um fornecedor de identidade com as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Configurar 2016 AD FS com o protocolo SAML e as políticas personalizadas no Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2c2e6861fda42a9e8c1aabcba303bfede47ac3c1
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669231"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>Adicionar o ADFS como um fornecedor de identidade com as políticas personalizadas no Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo mostra-lhe como ativar o início de sessão para uma conta de utilizador do AD FS, utilizando [políticas personalizadas](active-directory-b2c-overview-custom.md) no Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Pré-requisitos

Conclua os passos a [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md) artigo.

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Adicionar a chave de conta da aplicação do AD FS para o Azure AD B2C

Federação com uma conta do AD FS requer um segredo do cliente para a conta à confiança do Azure AD B2C em nome do aplicativo. Precisa armazenar o certificado ADFS no seu inquilino do Azure AD B2C. 

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, ao trocá-lo no canto superior direito do portal do Azure. Selecione **trocar diretório**e, em seguida, selecione o diretório que contém o inquilino que criou. Neste tutorial, o *contoso* é utilizado o diretório que contém o inquilino com o nome *contoso0522Tenant.onmicrosoft.com*.

    ![Trocar diretórios](./media/active-directory-b2c-custom-setup-adfs2016-idp/switch-directories.png)

3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**. Deve agora ser com o seu inquilino.
4. Na página de descrição geral, selecione **arquitetura de experiências de identidade**.
5. Selecione **chaves de política** para exibir as chaves disponíveis no seu inquilino e, em seguida, clique em **Add**.
6. Escolher **carregar** como a opção.
7. Introduza `ADFSSamlCert` para o nome. O prefixo `B2C_1A_` podem ser adicionados automaticamente.
8. Procure e selecione o ficheiro. pfx de certificado com a chave privada. Este certificado com a chave privada deve ser a mesma que foi emitido e utilizado para a terceira parte confiável ADFS.
9. Clique em **Create** e confirme que criou o `B2C_1A_ADFSSamlCert` chave.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Adicionar um fornecedor de afirmações na sua política de extensão

Se pretender que os utilizadores iniciem sessão com uma conta do AD FS, terá de definir a conta como um fornecedor de afirmações. Pode fazê-lo ao especificar um ponto de final do Azure AD B2C se comunica com. O ponto final fornece um conjunto de afirmações que são utilizadas pelo Azure AD B2C para verificar que um utilizador específico foi autenticado.

Definir o ADFS como um fornecedor de afirmações, adicionando **ClaimsProvider** elemento em seu arquivo de política de extensão.

1. Abra o *TrustFrameworkExtensions.xml* ficheiro de política no diretório de trabalho. Se precisar de um editor de XML, [Experimente o Visual Studio Code](https://code.visualstudio.com/download), que é um editor simples de várias plataformas.
2. Adicione o seguinte XML sob o **ClaimsProviders** elemento e substitua **seu-ADFS-dominio** com o seu domínio do AD FS dê um nome e substitua o valor do **identityProvider** afirmações de saída com o seu DNS (valor arbitrário que indica o seu domínio) e guarde-o. 

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
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
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

## <a name="register-the-claims-provider-for-sign-up-and-sign-in"></a>Registar o fornecedor de afirmações para inscrição e início de sessão

Para disponibilizar o fornecedor de identidade de conta ADFS nas páginas de inscrição e início de sessão, terá de adicioná-lo à sua **SignUpOrSignIn** percurso do utilizador. 

Fazer uma cópia de um percurso do utilizador modelo existente e, em seguida, modificá-lo para que ele inclui o fornecedor de identidade do AD FS:

>[!NOTE]
>Se copiou anteriormente o **UserJourneys** elemento a partir do ficheiro de base da sua política para o ficheiro de extensão (*TrustFrameworkExtensions.xml*) pode ignorar esta secção.

1. Abra o ficheiro de base da sua política. Por exemplo, *TrustFrameworkBase.xml*.
2. Copie todo o conteúdo do **UserJourneys** elemento.
3. Abra o ficheiro de extensão (*TrustFrameworkExtensions.xml*) e cole o conteúdo inteiro do **UserJourneys** elemento que copiou no ficheiro de extensão.

### <a name="display-the-button"></a>Exibir o botão

O **ClaimsProviderSelections** elemento define a lista de seleções de fornecedor de afirmações e sua ordem.  O **ClaimsProviderSelection** elemento é semelhante a um botão de fornecedor de identidade numa página de inscrição e início de sessão. Se adicionar um **ClaimsProviderSelection** elemento para uma conta ADFS, um novo botão é apresentado quando um utilizador vê a página. Para adicionar este elemento:

1. Na **UserJourney** elemento com um identificador de `SignUpOrSignIn` no jornadas de utilizador que copiou, localize o **OrchestrationStep** elemento de `Order="1"`.
2. Adicionar seguintes **ClaimsProviderSelection** elemento sob o **ClaimsProviderSelections** elemento:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação

Agora que tem um botão no local, terá de ligá-lo a uma ação. A ação, neste caso, é para o Azure AD B2C comunicar com a conta do AD FS para receber um token. Ligue o botão a uma ação, o perfil técnico de ligação para o seu fornecedor de afirmações do ADFS conta:

1. Encontrar o **OrchestrationStep** dos `Order="2"` sob o **UserJourney** elemento.
2. Adicionar seguintes **ClaimsExchange** elemento sob o **ClaimsExchanges** elemento:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

> [!NOTE]
> * Certifique-se de que o `Id` tem o mesmo valor que `TargetClaimsExchangeId` na secção anterior.
> * Certifique-se de que o `TechnicalProfileReferenceId` está definido para o perfil técnico que criou anteriormente (Contoso-SAML2).


## <a name="optional-register-the-claims-provider-for-profile-edit"></a>[Opcional] Registar o fornecedor de afirmações para edição de perfil

Também poderá adicionar o fornecedor de identidade de conta do AD FS para o seu percurso de utilizador de edição de perfil.

### <a name="display-the-button"></a>Exibir o botão

1. Abra o ficheiro de extensão da sua política. Por exemplo, *TrustFrameworkExtensions.xml*.
2. Na **UserJourney** elemento com um identificador `ProfileEdit` no jornadas de utilizador que copiou, localize o **OrchestrationStep** elemento de `Order="1"`.
3. Adicionar seguintes **ClaimsProviderSelection** elemento sob **ClaimsProviderSelections** elemento:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Ligar o botão para uma ação

1. Encontrar o **OrchestrationStep** dos `Order="2"` sob o **UserJourney** elemento.
2. Adicionar seguintes **ClaimsExchange** elemento sob o **ClaimsExchanges** elemento:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

## <a name="upload-the-policy-to-your-tenant"></a>Carregar a política para o seu inquilino

1. No portal do Azure, selecione **todas as políticas**.
2. Selecione **carregar política**.
3. Ativar **substituir a política, se existir**.
4. Procure e selecione sua *TrustFrameworkExtensions.xml* ficheiro de política e, em seguida, selecione **carregar**. Certifique-se de que a validação é efetuada com êxito.


## <a name="configure-an-adfs-relying-party-trust"></a>Configurar uma entidade Confiadora de ADFS

Para utilizar o ADFS como um fornecedor de identidade no Azure AD B2C, terá de criar um ADFS entidade Confiadora com os metadados SAML do Azure AD B2C. O exemplo seguinte mostra um endereço de URL para os metadados SAML de um perfil técnico do Azure AD B2C:

```
https://login.microsoftonline.com/te/your-tenant/your-policy/samlp/metadata?idptp=your-technical-profile
```

Substitua os valores seguintes:

- **o inquilino** com o nome de inquilino, como seu tenant.onmicrosoft.com.
- **sua política** com o nome da sua política. Utilize a política de onde configurar o perfil técnico do fornecedor SAML ou uma política que herda dessa política.
- **seu perfil técnico** com o nome de tha do seu perfil de técnica de fornecedor de identidade SAML.
 
Abra um browser e navegue para o URL. Certifique-se de que digite a URL correta e de que tem acesso para o ficheiro de metadados XML.

Para adicionar uma nova confiança da entidade confiadora, utilizando o snap-in Gestão do AD FS e configurar manualmente as definições, execute o seguinte procedimento num servidor de Federação. A associação **administradores** ou equivalente no computador local é o mínimo requerido para concluir este procedimento. Veja os detalhes sobre as contas adequadas e associações a grupos em [locais e grupos predefinidos do domínio](http://go.microsoft.com/fwlink/?LinkId=83477).

1. No Gestor de servidor, selecione **ferramentas**e, em seguida, selecione **gestão de ADFS**.
2. Selecione **adicionar confiança de entidade Confiadora**.
3. Sobre o **bem-vindo** página, selecione **com suporte para afirmações**e, em seguida, clique em **iniciar**.
4. Sobre o **selecionar origem de dados** , selecione **importar dados sobre a entidade confiadora publicar online ou numa rede local**, forneça o URL de metadados do Azure AD B2C e, em seguida, clique em **próxima**.
5. Sobre o **Especificar nome a apresentar** página, introduza um **nome a apresentar**, em **notas**, introduza uma descrição para esta fidedignidade de entidade confiadora e, em seguida, clique em **seguinte**.
6. Sobre o **escolher política de controlo de acesso** página, selecione uma política e, em seguida, clique em **próxima**.
7. Sobre o **pronto para adicionar confiança** página, reveja as definições e, em seguida, clique em **próxima** para guardar a sua entidade confiadora confiar informações.
8. Sobre o **concluir** página, clique em **fechar**, esta ação apresenta automaticamente o **editar regras de afirmação** caixa de diálogo.
9. Selecione **Adicionar regra**.  
10. Na **modelo de regra de afirmação**, selecione **enviar atributos LDAP como afirmações**.
11. Fornecer um **nome da regra de afirmação**. Para o **arquivo de atributos**, selecione **selecione Active Directory**, adicione as seguintes declarações, em seguida, clique em **concluir** e **OK**.

    ![Propriedades da regra de conjunto](./media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)

12.  Com base no seu tipo de certificado, poderá ter de definir o algoritmo HASH. A janela Propriedades de confiança (demonstração de B2C) da parte da entidade confiadora, selecione o **avançadas** separador e altere a **algoritmo hash seguro** para `SHA-1` ou `SHA-256`e clique em **Ok**.  

### <a name="update-the-relying-party-metadata"></a>Atualizar os metadados da entidade confiadora de terceiros

Alterar o perfil técnico SAML necessita de atualizar o AD FS com a versão de metadados atualizados. Não é necessário atualizar os metadados quando criar o aplicativo de parte confiável, mas quando fizer uma alteração, atualizar os metadados no ADFS.

1. No Gestor de servidor, selecione **ferramentas**e, em seguida, selecione **gestão de ADFS**.
2. Selecione a confiança de entidade confiadora que criou, selecione **atualização de metadados de Federação**e, em seguida, clique em **atualização**. 

### <a name="test-the-policy-by-using-run-now"></a>Testar a política ao executar agora a utilizar

1.  Open **definições do Azure AD B2C** e aceda à **Framework de experiência de identidade**.
2.  Open **B2C_1A_ProfileEdit**, a política personalizada de terceiros (RP) da entidade confiadora que carregou. Selecione **executar agora**. Deverá conseguir iniciar sessão com a conta do AD FS.

## <a name="download-the-complete-policy-files"></a>Transferir os ficheiros de política concluída

Opcional: Pode criar o seu cenário usando seus próprios arquivos de política personalizada depois de concluir os passos em [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md). Por exemplo ficheiros, consulte [ficheiros de exemplo de política apenas para referência](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app).
