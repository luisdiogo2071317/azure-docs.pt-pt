---
title: Migrar os utilizadores com redes sociais identidades no Azure Active Directory B2C | Microsoft Docs
description: Aborda conceitos nucleares na migração de utilizadores com redes sociais identidades no Azure AD B2C, utilizando a Graph API.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/03/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7c83afba1f027771b3407aecf94fefffdc951664
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34710564"
---
# <a name="azure-active-directory-b2c-migrate-users-with-social-identities"></a>O Azure Active Directory B2C: Migrar os utilizadores com redes sociais identidades
Quando planeia migrar o seu fornecedor de identidade para o Azure AD B2C, também poderá ter de migrar os utilizadores com redes sociais identidades. Este artigo explica como migrar as contas de identidades de redes sociais existentes, tais como: contas do Facebook, LinkedIn, Microsoft e Google ao Azure AD B2C. Este artigo também se aplica a identidades federadas, no entanto, estes migrações são menos comuns.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo é uma continuação do artigo de migração de utilizador e centra-se na migração da identidade de redes sociais. Antes de começar, leia [migração de utilizador](active-directory-b2c-user-migration.md).

## <a name="social-identities-migration-introduction"></a>Introdução de migração de identidades de redes sociais

* No Azure AD B2C, **contas locais** nomes início de sessão (utilizador nome ou endereço de e-mail) estão armazenados no `signInNames` coleção no registo de utilizador. O `signInNames` contém um ou mais `signInName` registos que especificar os nomes de início de sessão do utilizador. Cada nome de início de sessão tem de ser exclusivo em toda o inquilino.

* **Redes sociais contas** identidades são armazenadas no `userIdentities` coleção. Especifica a entrada de `issuer` (nome do fornecedor de identidade) como facebook.com e `issuerUserId`, que é um identificador de utilizador exclusivo para o emissor. O `userIdentities` atributo contém um ou mais registos UserIdentity que especificam o tipo de conta de redes sociais e o identificador exclusivo do utilizador do fornecedor de identidade de redes sociais.

* **Combinar conta local com a identidade de redes social**. Conforme mencionado, nomes de início de sessão de conta local e identidades de redes sociais conta são armazenados em atributos diferentes. `signInNames` é utilizado para a conta local, enquanto `userIdentities` para a conta de redes social. Uma única conta do Azure AD B2C, pode ser uma conta local apenas, apenas a conta de redes social ou combinar uma conta local com redes social identidade no registo de um utilizador. Este comportamento permite-lhe gerir uma conta única, enquanto que um utilizador pode iniciar sessão com a conta local credential(s) ou com as identidades de redes sociais.

* `UserIdentity` Escreva - contém informações sobre a identidade de um utilizador de conta de redes sociais num inquilino do Azure AD B2C:
    * `issuer` A representação de cadeia do fornecedor de identidade que emitiu o identificador de utilizador, tais como facebook.com.
    * `issuerUserId` O identificador de utilizador exclusivo utilizado pelo fornecedor de identidade de redes sociais em formato base64.

    ```JSON
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
    ```

* Consoante o fornecedor de identidade, o **ID de utilizador de redes sociais** é um valor exclusivo para um determinado utilizador por conta de desenvolvimento ou de aplicação. Configure a política do Azure AD B2C com o mesmo ID de aplicação, que anteriormente era atribuído pelo fornecedor de redes social. Outra aplicação ou dentro da mesma conta de desenvolvimento.

## <a name="use-graph-api-to-migrate-users"></a>Utilize a Graph API para migrar os utilizadores
Criar a conta de utilizador através do Azure AD B2C [Graph API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Para comunicar com a Graph API, primeiro tem de ter uma conta de serviço com privilégios administrativos. No Azure AD, registar uma aplicação e a autenticação para o Azure AD. As credenciais de aplicação são o ID de aplicação e o segredo de aplicação. A aplicação funciona como o próprio, não como um utilizador, para chamar a API de gráfico. Siga as instruções no passo 1 [migração de utilizador](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-user-migration#step-1-use-graph-api-to-migrate-users) artigo.

## <a name="required-properties"></a>Propriedades necessárias
A lista seguinte mostra as propriedades que são necessárias quando criar um utilizador.
* **accountEnabled** - verdadeiro
* **displayName** -o nome a apresentar no livro de endereços do utilizador.
* **passwordProfile** -o perfil de palavra-passe para o utilizador. 

> [!NOTE]
> Para a conta de redes social apenas (sem credenciais de conta local), ainda tem de especificar a palavra-passe. O Azure AD B2C ignora a palavra-passe que especificar para contas de redes sociais.

* **userPrincipalName** -o nome principal de utilizador (someuser@contoso.com). O nome principal de utilizador tem de conter um dos domínios verificados para o inquilino. Para especificar o UPN, gerar novo valor GUID, concatenate com `@` e o nome do inquilino.
* **mailNickname** -o alias de correio para o utilizador. Este valor pode ser o mesmo ID que utilizar para userPrincipalName. 
* **signInNames** -um ou mais registos SignInName que especificar os nomes de início de sessão do utilizador. Cada nome de início de sessão tem de ser exclusivo em toda o empresa/inquilino. Para redes social conta, esta propriedade pode ser deixada em branco.
* **userIdentities** -um ou mais registos UserIdentity que especificam o social conta tipo e o identificador exclusivo do utilizador do fornecedor de identidade de redes sociais.
* [opcional] **otherMails** - sociais conta apenas, endereços de e-mail do utilizador 

Para obter mais informações, consulte: [referência da Graph API](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser)

## <a name="migrate-social-account-only"></a>Migrar redes social conta (apenas)
Para criar uma conta social apenas, sem credenciais de conta local. Envie o pedido POST de HTTPS para a Graph API. O corpo do pedido contém as propriedades do utilizador para criar conta de redes sociais. No mínimo, tem de especificar as propriedades necessárias. 


**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Submeta os formulário de dados seguintes: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8",
    "signInNames": [],
    "creationType": null,
    "displayName": "Sara Bell",
    "givenName": "Sara",
    "surname": "Bell",
    "passwordProfile": {
        "password": "Test1234",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": null,
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ],
    "otherMails": ["sara@live.com"],
    "userPrincipalName": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8@tenant-name.onmicrosoft.com"
}
```
## <a name="migrate-social-account-with-local-account"></a>Migrar redes social conta com a conta local
Para criar uma conta local combinada com redes sociais identidades. Envie o pedido POST de HTTPS para a Graph API. O corpo do pedido contém as propriedades do utilizador para criar conta de redes sociais. No mínimo, tem de especificar as propriedades necessárias. 

**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Submeta os seguintes dados do formulário: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "5164db16-3eee-4629-bfda-dcc3326790e9",
    "signInNames": [{
            "type": "emailAddress",
            "value": "david@contoso.com"
        }
    ],
    "creationType": "LocalAccount",
    "displayName": "David Hor",
    "givenName": "David",
    "surname": "Hor",
    "passwordProfile": {
        "password": "1234567",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": "DisablePasswordExpiration,DisableStrongPassword",
    "userIdentities": [{
            "issuer": "contoso.com",
            "issuerUserId": "ZGF2aWRAY29udG9zby5jb20="
        }
    ],
    "otherMails": [],
    "userPrincipalName": "5164db16-3eee-4629-bfda-dcc3326790e9@tenant-name.onmicrosoft.com"
}
```

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes
### <a name="how-can-i-know-the-issuer-name"></a>Como posso saber o nome do emissor?
O nome do emissor, ou o nome do fornecedor de identidade, está configurado na sua política. Se não souber o valor para especificar `issuer`, siga este procedimento:
1. Inicie sessão com uma das contas de redes sociais
2. Do JWT token, copie o `sub` valor. O `sub` contém, normalmente, o ID de objeto do utilizador no Azure AD B2C. Ou a partir do portal do Azure, abra as propriedades do utilizador e copie o ID de objeto.
3. Abra [Explorador do Azure AD Graph](https://graphexplorer.azurewebsites.net)
4. Inicie sessão com o administrador. N
5. Execute o seguinte pedido GET. Substitua o userObjectId com o ID de utilizador que copiou. **INTRODUÇÃO** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId
6. Localize o `userIdentities` elemento dentro de retorno JSON do Azure AD B2C.
7. [Opcional] Pode também querer descodificar o `issuerUserId` valor.

> [!NOTE]
> Utilize uma conta de administrador de inquilino B2C é local para o inquilino do B2C. A sintaxe do nome de conta é admin@tenant-name.onmicrosoft.com.

### <a name="is-it-possible-to-add-social-identity-to-an-existing-local-account"></a>É possível adicionar a identidade de redes social para uma conta local existente?
Sim. Pode adicionar a identidade de redes social depois de criada a conta local. Execute o pedido de PATCH de HTTPS. Substitua o userObjectId com o ID de utilizador que pretende atualizar. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Submeta os seguintes dados do formulário: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

### <a name="is-it-possible-to-add-multiple-social-identities"></a>É possível adicionar várias identidades de redes sociais?
Sim. Pode adicionar múltiplas identidades de redes sociais para uma única conta do Azure AD B2C. Execute o pedido de PATCH de HTTPS. Substitua o userObjectId com o ID de utilizador. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Submeta os seguintes dados do formulário: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "google.com",
            "issuerUserId": "MjQzMjE2NTc4NTQ="
        },
        {
            "issuer": "facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

## <a name="optional-user-migration-application-sample"></a>[Opcional] Exemplo de aplicação de migração de utilizador
[Transferir e executar a aplicação de exemplo V2](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). A aplicação de exemplo V2 utiliza um ficheiro JSON que contém dados de utilizador fictício, incluindo: conta local, conta de redes social e identidades locais e redes sociais numa única conta.  Para editar o ficheiro JSON, abra o `AADB2C.UserMigration.sln` solução do Visual Studio. No `AADB2C.UserMigration` projeto, abra o `UsersData.json` ficheiro. O ficheiro contém uma lista de entidades do utilizador. Cada entidade de utilizador tem as seguintes propriedades:
* **signInName** - para a conta local, o endereço de correio eletrónico para início de sessão
* **displayName** -nome a apresentar do utilizador
* **firstName** -nome próprio do utilizador
* **lastName** -apelido do utilizador
* **palavra-passe** para a conta local, palavra-passe de utilizador (pode estar vazio)
* **emissor** - para a conta de redes social, o nome do fornecedor de identidade
* **issuerUserId** - para a conta de redes social, o identificador de utilizador exclusivo utilizado pelo fornecedor de identidade de redes sociais. O valor deve estar em texto não encriptado. A aplicação de exemplo codifica este valor para base64.
* **e-mail** para do redes sociais apenas (não combinado), utilizador da conta endereço de e-mail

```JSON
{
  "userType": "emailAddress",
  "Users": [
    {
      // Local account only
      "signInName": "James@contoso.com",
      "displayName": "James Martin",
      "firstName": "James",
      "lastName": "Martin",
      "password": "Pass!w0rd"
    },
    {
      // Social account only
      "issuer": "Facebook.com",
      "issuerUserId": "1234567890",
      "email": "sara@contoso.com",
      "displayName": "Sara Bell",
      "firstName": "Sara",
      "lastName": "Bell"
    },
    {
      // Combine local account with social identity
      "signInName": "david@contoso.com",
      "issuer": "Facebook.com",
      "issuerUserId": "0987654321",
      "displayName": "David Hor",
      "firstName": "David",
      "lastName": "Hor",
      "password": "Pass!w0rd"
    }
  ]
}
```

> [!NOTE]
> Se não atualizar o ficheiro de UsersData.json na amostra com os seus dados, que pode iniciar sessão com as credenciais de conta local de exemplo, mas não com os exemplos de conta de redes sociais. Para migrar as contas de redes sociais, forneça dados reais.

Para obter mais informações, como utilizar a aplicação de exemplo, consulte [Azure Active Directory B2C: migração de utilizador](active-directory-b2c-user-migration.md)
