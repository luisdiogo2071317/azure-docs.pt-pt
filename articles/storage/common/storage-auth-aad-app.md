---
title: Autenticar com o Azure AD a partir de uma aplicação de armazenamento (pré-visualização) | Microsoft Docs
description: Autenticar com o Azure AD a partir de uma aplicação do Storage do Azure (pré-visualização).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 1bf4a8bba3b93c16f67d46f65292709ef2a1bba2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661009"
---
# <a name="authenticate-with-azure-ad-from-an-azure-storage-application-preview"></a>Autenticar com o Azure AD a partir de uma aplicação do Storage do Azure (pré-visualização)

Das principais vantagens da utilização do Azure Active Directory (Azure AD) com o Storage do Azure é que as credenciais já não precisam de ser armazenados no seu código. Em vez disso, pode pedir um token de acesso de OAuth 2.0 do Azure AD. Azure AD processa a autenticação de principal de segurança (um utilizador, grupo ou principal de serviço) com a aplicação. Se a autenticação for bem sucedida, do Azure AD devolve o token de acesso à aplicação e a aplicação, em seguida, pode utilizar o token de acesso para autorizar pedidos para o Storage do Azure.

Este artigo mostra como configurar a sua aplicação para autenticação com o Azure AD. O .NET de funcionalidades de exemplo de código, mas outros idiomas utilizam uma abordagem semelhante.

Antes de pode autenticar um principal de segurança da sua aplicação do Storage do Azure, configure as definições de controlo (RBAC) de acesso baseado em funções para esse principal de segurança. Armazenamento do Azure define funções do RBAC que abranger permissões para contentores e pelas filas. Quando a função RBAC é atribuída a um principal de segurança, esse principal de segurança é concedido acesso a esse recurso. Para obter mais informações, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).

Para obter uma descrição geral do fluxo de concessão do código de OAuth 2.0, consulte [fluxo de concessão de acesso de autorizar a aplicações web do Azure Active Directory utilizando o código de OAuth 2.0](../../active-directory/develop/active-directory-protocols-oauth-code.md).

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o Storage do Azure está declarada geralmente disponível. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a utilizar a autorização de chave partilhada ou SAS tokens nas suas aplicações. Para obter informações adicionais sobre a pré-visualização, consulte [autenticar o acesso ao Storage do Azure utilizando o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
>
> Durante a pré-visualização, atribuições de funções RBAC podem demorar até cinco minutos para propagar.

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registar a aplicação com um inquilino do Azure AD

O primeiro passo para utilizar o Azure AD para autorizar o acesso aos recursos de armazenamento está a registar a aplicação cliente num inquilino do Azure AD. Registar a aplicação permite-lhe chamar do Azure [Active Directory Authentication Library](../../active-directory/active-directory-authentication-libraries.md) (ADAL) a partir do código. A ADAL fornece uma API para autenticar com o Azure AD da sua aplicação. Registar a aplicação também lhe permite autorizar as chamadas a partir dessa aplicação para as APIs de armazenamento do Azure com um token de acesso.

Ao registar a aplicação, fornecer informações sobre a sua aplicação para o Azure AD. Em seguida, do Azure AD fornece um ID de cliente (também designado por um *ID da aplicação*) que utiliza para associar a aplicação com o Azure AD no tempo de execução. Para saber mais sobre o ID de cliente, consulte o artigo [aplicação e objetos de principal de serviço no Azure Active Directory](../../active-directory/develop/active-directory-application-objects.md).

Para registar a aplicação do Storage do Azure, siga os passos a [adicionar uma aplicação](../../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) secção [integrar aplicações com o Azure Active Directory](../../active-directory/active-directory-integrating-applications.md). Se registar a aplicação como uma aplicação nativa, pode especificar qualquer URI válido para o **URI de redirecionamento**. O valor não precisa de ser um ponto final real.

![Captura de ecrã que mostra como registar a aplicação de armazenamento com o Azure AD](./media/storage-auth-aad-app/app-registration.png)

Depois de já registou a aplicação, verá o ID da aplicação (ou o ID de cliente) em **definições**:

![Captura de ecrã que mostra o ID de cliente](./media/storage-auth-aad-app/app-registration-client-id.png)

Para obter mais informações sobre como registar uma aplicação com o Azure AD, consulte [integrar aplicações com o Azure Active Directory](../../active-directory/develop/active-directory-integrating-applications.md). 

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Conceder as permissões de aplicação registada para o armazenamento do Azure

Em seguida, tem de conceder as permissões de aplicação para chamar as APIs de armazenamento do Azure. Este passo permite que a sua aplicação autorizar chamadas para o Storage do Azure com o Azure AD.

1. No painel de navegação esquerdo do portal do Azure, escolha **todos os serviços**e procure **registos de aplicação**.
2. Procure o nome da aplicação registada que criou no passo anterior.
3. Selecione a sua aplicação registada e clique em **definições**. No **acesso à API** secção, selecione **as permissões necessárias**.
4. No **as permissões necessárias** painel, clique em de **adicionar** botão.
5. Em **selecionar um API**, procure "Storage do Azure" e selecione **Storage do Azure** da lista de resultados.

    ![Captura de ecrã que mostra permissões para o armazenamento](media/storage-auth-aad-app/registered-app-permissions-1.png)

6. Em **selecionar permissões**, a caixa de verificação junto a **Storage do Azure de acesso**e clique em **selecione**.
7. Clique em **Concluído**.

O **permissões obrigatórias** agora mostra que a aplicação do Azure AD tem acesso ao Azure Active Directory e o armazenamento do Azure. São concedidas permissões para o Azure AD automaticamente quando tem primeiro de registar a aplicação com o Azure AD.

![Captura de ecrã que mostra registar permissões de aplicações](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="net-code-example-create-a-block-blob"></a>Exemplo de código do .NET: criar um blob de bloco

O exemplo de código mostra como obter acesso token do Azure AD. O token de acesso é utilizado para autenticar o utilizador especificado e, em seguida, autorizar um pedido de criação de um blob de blocos. Para obter este trabalho de exemplo, primeiro siga os passos descritos nas secções anteriores.

> [!NOTE]
> Como um proprietário da sua conta do Storage do Azure, a não atribui automaticamente permissões para aceder a dados. Deve explicitamente atribuir manualmente uma função RBAC para o Storage do Azure. Pode atribuí-la ao nível da sua subscrição, grupo de recursos, conta de armazenamento, ou contentor ou fila. 
>
> Por exemplo, para executar o código de exemplo numa conta de armazenamento onde tem um proprietário e com a seus próprios identidade de utilizador, tem de atribuir a função RBAC para contribuinte de dados de BLOBs a próprio. Caso contrário, a chamada para criar o blob irá falhar com o código de estado HTTP 403 (proibido). Para obter mais informações, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Valores well-known para autenticação com o Azure AD

Para autenticar um principal de segurança com o Azure AD, tem de incluir alguns valores conhecidos no seu código.

#### <a name="azure-ad-oauth-endpoint"></a>Ponto final de AD OAuth do Azure

A base de ponto final de autoridade do Azure AD para OAuth 2.0 é o seguinte, onde *id do inquilino* é o seu ID de inquilino do Active Directory (ou o ID de diretório):

`https://login.microsoftonline.com/<tenant-id>/oauth2/token`

O ID do inquilino identifica o inquilino do Azure AD a utilizar para autenticação. Para obter o ID do inquilino, siga os passos descritos em **obter o ID de inquilino do seu Azure Active Directory**.

#### <a name="storage-resource-id"></a>ID de recurso de armazenamento

Utilize o ID de recurso de armazenamento do Azure para adquirir um token para autenticar pedidos para o Storage do Azure:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>Obter o ID de inquilino do seu Azure Active Directory

Para obter o ID do inquilino, siga estes passos:

1. No portal do Azure, selecione o Active Directory.
2. Clique em **Propriedades**.
3. Copie o valor GUID fornecido para o **ID de diretório**. Este valor é também denominado o ID do inquilino.

![Captura de ecrã que mostra como copiar o ID de inquilino](./media/storage-auth-aad-app/aad-tenant-id.png)

### <a name="add-references-and-using-statements"></a>Adicione referências e instruções de utilização  

No Visual Studio, instale a versão de pré-visualização da biblioteca de clientes de armazenamento do Azure. Do **ferramentas** menu, selecione **Gestor de pacotes Nuget**, em seguida, **consola do Gestor de pacotes**. Escreva o seguinte comando para a consola:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Em seguida, adicione as seguintes instruções para o código de utilização:

```dotnet
using Microsoft.IdentityModel.Clients.ActiveDirectory; //ADAL client library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Obter um OAuth token do Azure AD

Em seguida, adicione um método que os pedidos de um token do Azure AD. Para o token de pedido, chame o [AuthenticationContext.AcquireTokenAsync](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync) método.

```dotnet
static string GetUserOAuthToken()
{
    const string ResourceId = "https://storage.azure.com/"; // Storage resource endpoint
    const string AuthEndpoint = "https://login.microsoftonline.com/{0}/oauth2/token"; // Azure AD OAuth endpoint
    const string TenantId = "<tenant-id>"; // Tenant or directory ID

    // Construct the authority string from the Azure AD OAuth endpoint and the tenant ID. 
    string authority = string.Format(CultureInfo.InvariantCulture, AuthEndpoint, TenantId);
    AuthenticationContext authContext = new AuthenticationContext(authority);

    // Acquire an access token from Azure AD. 
    AuthenticationResult result = authContext.AcquireTokenAsync(ResourceId, 
                                                                "<client-id>", 
                                                                new Uri(@"<client-redirect-uri>"), 
                                                                new PlatformParameters(PromptBehavior.Auto)).Result;

    return result.AccessToken;
}
```

### <a name="create-the-block-blob"></a>Criar o blob de bloco

Por fim, utilize o token de acesso para criar novas credenciais do armazenamento e utilizar essas credenciais para criar o blob:

```dotnet
// Get the access token.
string accessToken = GetUserOAuthToken();

// Use the access token to create the storage credentials.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using those credentials
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
```

> [!NOTE]
> Integração do AD do Azure com o Storage do Azure requer que utilize HTTPS para operações de armazenamento do Azure.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre as funções do RBAC para o armazenamento do Azure, consulte [gerir direitos de acesso aos dados de armazenamento com o RBAC (pré-visualização)](storage-auth-aad-rbac.md).
- Para saber mais sobre como utilizar identidade de serviço geridas com o Storage do Azure, consulte [autenticar com o Azure AD a partir de uma identidade de serviço gerida do Azure (pré-visualização)](storage-auth-aad-msi.md).
- Para saber como iniciar sessão na CLI do Azure e do PowerShell com uma identidade do Azure AD, consulte [utilizar uma identidade do Azure AD para aceder ao armazenamento do Azure com o CLI ou PowerShell (pré-visualização)](storage-auth-aad-script.md).
- Para obter informações adicionais sobre a integração do Azure AD para filas e Blobs do Azure, consulte o blogue da equipa do Storage do Azure post, [anunciar a autenticação de pré-visualização do Azure AD para o Storage do Azure](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).



