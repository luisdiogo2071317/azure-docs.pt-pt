---
title: Adicionar seus próprios atributos para as políticas personalizadas no Azure Active Directory B2C | Documentos da Microsoft
description: Um passo a passo sobre como utilizar propriedades de extensão, atributos personalizados e incluí-los na interface do usuário.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ecde4d8cd8ee454290b16b640ba05d310cf348fe
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450242"
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>O Azure Active Directory B2C: Criar e utilizar atributos personalizados num perfil personalizado do Editar política

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Neste artigo, cria um atributo personalizado no seu diretório do Azure AD B2C e usar esse novo atributo como uma declaração personalizada no percurso do utilizador de Editar perfil.

## <a name="prerequisites"></a>Pré-requisitos

Conclua os passos no artigo [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Utilizar atributos personalizados para recolher informações sobre os seus clientes no Azure Active Directory B2C com as políticas personalizadas
O diretório do Azure Active Directory (Azure AD) B2C vem com um conjunto interno de atributos: com o nome, sobrenome, cidade, Código Postal, userPrincipalName, etc.  Muitas vezes, terá de criar seus próprios atributos.  Por exemplo:
* Uma aplicação do lado do cliente precisa de manter um atributo, como "LoyaltyNumber."
* Um fornecedor de identidade tem um identificador exclusivo do utilizador que tem de ser guardado como "uniqueUserGUID"."
* Precisa de um percurso do utilizador personalizado para manter o estado de utilizador como "migrationStatus."

Com o Azure AD B2C, pode estender o conjunto de atributos armazenados em cada conta de utilizador. Também pode ler e gravar esses atributos utilizando o [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Propriedades de extensão expandem o esquema dos objetos de utilizador no diretório.  A propriedade de extensão de termos, o atributo personalizado e a afirmação personalizada referem-se para a mesma coisa no contexto deste artigo e o nome varia consoante o contexto (aplicação, objeto, política).

Propriedades de extensão só podem ser registadas num objeto de aplicativo, mesmo que eles podem conter dados para um utilizador. A propriedade é anexada à aplicação. O objeto de aplicativo deve ser concedido acesso de escrita para registar uma propriedade de extensão. 100 propriedades de extensão (em todos os tipos e todas as aplicações) podem ser escritas para qualquer objeto único. Propriedades de extensão são adicionados para o tipo de diretório de destino e fica imediatamente acessíveis no inquilino do diretório do Azure AD B2C.
Se a aplicação for eliminada, essas propriedades de extensão, juntamente com quaisquer dados contidos nos mesmos para todos os utilizadores também são removidas. Se uma propriedade de extensão for eliminada pela aplicação, esta é removida em objetos de diretório de destino e os valores eliminados.

Propriedades de extensão existem apenas no contexto de uma aplicação registada no inquilino. Id de objeto do aplicativo deve ser incluído no TechnicalProfile que a utilizam.

>[!NOTE]
>O diretório do Azure AD B2C normalmente inclui uma aplicação Web com o nome `b2c-extensions-app`.  Esta aplicação é principalmente utilizada pelas políticas internas de b2c para as afirmações personalizadas criadas através do portal do Azure.  É recomendado utilizar esta aplicação para registar as extensões para as políticas personalizadas do b2c apenas para utilizadores avançados.  Instruções para isso estão incluídas na secção passos seguintes neste artigo.


## <a name="creating-a-new-application-to-store-the-extension-properties"></a>Criando um novo aplicativo para armazenar as propriedades de extensão

1. Abra uma sessão de navegação e navegue para o [portal do Azure](https://portal.azure.com) e inicie sessão com credenciais administrativas do diretório B2C que pretende configurar.
2. Clique em **do Azure Active Directory** no menu de navegação esquerdo. Poderá ter de encontrá-lo ao selecionar mais serviços >.
3. Selecione **registos de aplicações** e clique em **novo registo de aplicação**
4. Forneça as seguintes entradas recomendadas:
    * Especifique um nome para a aplicação web: **WebApp-Graph-DirectoryExtensions**
    * Tipo de aplicação: aplicação/API Web
    * Início de sessão URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
5. Selecione **Criar**.
6. Selecione a aplicação web recentemente criada.
7. Selecione **configurações** > **permissões obrigatórias**.
8. Selecionar API **Windows do Azure Active Directory**.
9. Coloque uma marca de verificação nas permissões de aplicação: **dados de diretório de leitura e escrita**e, em seguida, selecione **guardar**.
10. Escolher **conceder permissões** e confirme **Sim**.
11. Copiar para a área de transferência e guarde identificadores a seguir:
    * **ID da aplicação** . Exemplo: `103ee0e6-f92d-4183-b576-8c3739027780`
    * **ID de objeto**. Exemplo: `80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a>Modificar a política personalizada para adicionar o ApplicationObjectId

Quando concluiu os passos em [introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md), transferiu e alterou [arquivos](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) com o nome *TrustFrameworkBase.xml*,  *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml*, e *PasswordReset.xml*. Nas etapas a seguir, continuar a fazer modificações a estes ficheiros.

1. Abra o *TrustFrameworkBase.xml* de ficheiros e adicionar o `Metadata` secção conforme mostrado no exemplo a seguir. Insira o ID de objeto que registou anteriormente para o `ApplicationObjectId` valor e o ID da aplicação que registou para o `ClientId` valor: 

    ```xml
    <ClaimsProviders>
        <ClaimsProvider>
          <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
          <DisplayName>Azure Active Directory</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              
          <!-- Provide objectId and appId before using extension properties. -->
          <Metadata>
            <Item Key="ApplicationObjectId">insert objectId here</Item>
            <Item Key="ClientId">insert appId here</Item>
          </Metadata>
          <!-- End of changes -->
              
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
          </CryptographicKeys>
          <IncludeInSso>false</IncludeInSso>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
    ```

>[!NOTE]
>Quando escreve o TechnicalProfile pela primeira vez para a propriedade de extensão recentemente criado, pode ocorrer um erro de uso individual. A propriedade de extensão é criada na primeira vez que é utilizado.  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a>Usando a nova propriedade de extensão / personalizado do atributo no percurso do utilizador

1. Abra o *ProfileEdit.xml* ficheiro.
2. Adicione uma declaração personalizada `loyaltyId`.  Ao incluir o personalizado de afirmação no `<RelyingParty>` elemento, está incluído no token para a aplicação.
    
    ```xml
    <RelyingParty>
      <DefaultUserJourney ReferenceId="ProfileEdit" />
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
          <OutputClaim ClaimTypeReferenceId="city" />

          <!-- Provide the custom claim identifier -->
          <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
          <!-- End of changes -->
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

3. Abra o *TrustFrameworkExtensions.xml* de ficheiros e adicionar o`<ClaimsSchema>` elemento e seus elementos subordinados para o `BuildingBlocks` elemento:

    ```xml
    <BuildingBlocks>
      <ClaimsSchema> 
        <ClaimType Id="extension_loyaltyId"> 
          <DisplayName>Loyalty Identification Tag</DisplayName> 
          <DataType>string</DataType> 
          <UserHelpText>Your loyalty number from your membership card</UserHelpText> 
          <UserInputType>TextBox</UserInputType> 
        </ClaimType> 
      </ClaimsSchema>
    </BuildingBlocks>
    ```

4. Adicionar a mesma `ClaimType` definição à *TrustFrameworkBase.xml*. Adicionar uma `ClaimType` definição na base e o arquivo de extensões normalmente não é necessária, no entanto, uma vez que irão adicionar os passos seguintes a `extension_loyaltyId` para os TechnicalProfiles no ficheiro de Base, o validador de política irão rejeitar o carregamento do ficheiro base sem ele é. Poderá ser útil para rastreio da execução do percurso do utilizador com o nome "ProfileEdit" no *TrustFrameworkBase.xml* ficheiro.  Procure o percurso do utilizador o mesmo nome no seu editor e observe que o passo 5 da orquestração invoca o TechnicalProfileReferenceID = "SelfAsserted ProfileUpdate".  Pesquise e inspecionar este TechnicalProfile para se familiarizar com o fluxo.

5. Abra o *TrustFrameworkBase.xml* de ficheiros e adicionar `loyaltyId` como uma entrada e saída de afirmações no TechnicalProfile "SelfAsserted ProfileUpdate":

    ```xml
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <DisplayName>User ID signup</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        
        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->

      </OutputClaims>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
      </ValidationTechnicalProfiles>
    </TechnicalProfile>
    ```

6. Na *TrustFrameworkBase.xml* do ficheiro, adicione o `loyaltyId` alegam TechnicalProfile "AAD-UserWriteProfileUsingObjectId" para manter o valor da afirmação na propriedade de extensão, para o utilizador atual no diretório:

    ```xml
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <Metadata>
        <Item Key="Operation">Write</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="givenName" />
        <PersistedClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </PersistedClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

7. Na *TrustFrameworkBase.xml* do ficheiro, adicione o `loyaltyId` alegam TechnicalProfile "AAD-UserReadUsingObjectId" para ler o valor do atributo de extensão, sempre que um utilizador inicia sessão. Até agora os TechnicalProfiles foram alterados no fluxo de apenas contas locais.  Se o novo atributo for o pretendido no fluxo de uma conta social federado, um conjunto diferente dos TechnicalProfiles precisa ser alterada. Veja os passos seguintes.

    ```xml
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <Metadata>
        <Item Key="Operation">Read</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="otherMails" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </OutputClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

## <a name="test-the-custom-policy"></a>Testar a política personalizada

1. Abra o **painel Azure AD B2C** e navegue até à **Framework de experiência de identidade > políticas personalizadas**.
1. Selecione a política personalizada que carregou e clique nas **executar agora** botão.
1. Deverá conseguir inscrever-se através de um endereço de e-mail.

O token de id enviadas de volta à sua aplicação inclui a nova propriedade de extensão, como uma declaração personalizada precedida por extension_loyaltyId. Veja o exemplo.

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Passos Seguintes

### <a name="add-the-new-claim-to-the-flows-for-social-account-logins-by-changing-the-technicalprofiles-listed-below-these-two-technicalprofiles-are-used-by-socialfederated-account-logins-to-write-and-read-the-user-data-using-the-alternativesecurityid-as-the-locator-of-the-user-object"></a>Adicione a nova afirmação aos fluxos para inícios de sessão de conta de redes sociais, alterando os TechnicalProfiles listados abaixo. Estes dois TechnicalProfiles são utilizados por inícios de sessão de conta social federado para escrever e ler os dados de utilizador utilizando o alternativeSecurityId como o localizador de objeto do usuário.
```xml
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Usando os mesmos atributos de extensão entre as políticas incorporadas e personalizadas.
Quando adiciona a atributos de extensão (também conhecido como atributos personalizados) através da experiência do portal, esses atributos são registrados com a * * b2c-extensions-app, que existe no cada inquilino do b2c.  Para utilizar estes atributos de extensão na sua política personalizada:
1. No seu inquilino de b2c em portal.azure.com, navegue até **do Azure Active Directory** e selecione **registos das aplicações**
2. Encontre seu **b2c-extensions-app** e selecioná-lo
3. Em registo "Essentials" a **ID da aplicação** e o **ID de objeto**
4. Incluí-los nos seus metadados do perfil técnico de AAD comuns, como da seguinte forma:

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
              </Metadata>
```

Para manter a consistência com a experiência do portal, crie esses atributos com o portal da interface do Usuário *antes de* usá-los em suas políticas personalizadas.  Quando cria um atributo "ActivationStatus" no portal, deve fazer referência a ele da seguinte forma:

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a>Referência

* R **perfil técnico (TP)** é um tipo de elemento que pode ser considerado como um *função* que define o nome de um ponto de extremidade, seus metadados, o protocolo e fornece detalhes sobre a troca de afirmações que a identidade Deve executar o Framework de experiência.  Quando isso *função* denomina-se um passo de orquestração ou de outro TechnicalProfile, o InputClaims e OutputClaims são fornecidos como parâmetros ao chamador.


* Para dados completos sobre nas propriedades de extensão, consulte o artigo [EXTENSÕES de esquema do | CONCEITOS DA GRAPH API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Atributos de extensão no Graph API são nomeados usando a Convenção `extension_ApplicationObjectID_attributename`. As políticas personalizadas referir aos atributos de extensões como extension_attributename, assim, omitindo o ApplicationObjectId no XML
