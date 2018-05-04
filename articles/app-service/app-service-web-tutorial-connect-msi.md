---
title: Utilizar identidades de serviço geridas para proteger a ligação da Base de Dados SQL do Azure a partir do Serviço de Aplicações | Microsoft Docs
description: Saiba como tornar a conectividade da base de dados mais segura com uma identidade de serviço gerido e como aplicá-la a outros serviços do Azure.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/17/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 1b51638754287d3359eaea7bd5da3f71bf15cc89
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-secure-sql-database-connection-with-managed-service-identity"></a>Tutorial: Proteger a ligação da Base de Dados SQL com a identidade de serviço gerida

O [Serviço de Aplicações](app-service-web-overview.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática no Azure. Também oferece uma [identidade de serviço gerida](app-service-managed-service-identity.md) para a sua aplicação, que é uma solução chave na mão para proteger o acesso à [Base de Dados SQL do Azure](/azure/sql-database/) e a outros serviços do Azure. As identidades de serviço geridas no Serviço de Aplicações retiram a necessidade de ter segredos nas suas aplicações, como credenciais nas cadeias de ligação, o que as torna mais seguras. Neste tutorial, vai adicionar a identidade de serviço gerida à aplicação Web ASP.NET de exemplo que criou em [Tutorial: Criar uma aplicação ASP.NET no Azure com a Base de Dados SQL](app-service-web-tutorial-dotnet-sqldatabase.md). Quando tiver terminado, a aplicação de exemplo irá ligar-se à Base de Dados SQL em segurança sem ser preciso o nome de utilizador e a palavras-passe.

Saiba como:

> [!div class="checklist"]
> * Ativar a identidade de serviço gerida
> * Conceder acesso à Base de Dados SQL à identidade de serviço
> * Configurar o código da aplicação para autenticar com a Base de Dados SQL através da autenticação do Azure Active Directory
> * Conceder privilégios mínimos à identidade de serviço na Base de Dados SQL

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Este artigo retoma onde o artigo [Tutorial: Criar uma aplicação ASP.NET no Azure com a Base de Dados SQL](app-service-web-tutorial-dotnet-sqldatabase.md) ficou. Se ainda não o fez, siga este tutorial primeiro. Em alternativa, pode adaptar os passos para a sua própria aplicação ASP.NET com a Base de Dados SQL.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-service-identity"></a>Ativar a identidade de serviço gerida

Para ativar uma identidade de serviço na sua aplicação do Azure, utilize o comando [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az_webapp_identity_assign) no Cloud Shell. No comando seguinte, substitua *\<nome da aplicação>*.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

Eis um exemplo da saída depois de a identidade ser criada no Azure Active Directory:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

Vai utilizar o valor de `principalId` no próximo passo. Se quiser ver os detalhes da identidade nova no Azure Active Directory, execute o seguinte comando opcional com o valor de `principalId`:

```azurecli-interactive
az ad sp show --id <principalid>`
```

## <a name="grant-database-access-to-identity"></a>Conceder acesso à base de dados à identidade

Em seguida, vai conceder acesso à base de dados à identidade de serviço da sua aplicação com o comando [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az_sql_server_ad-admin_create) no Cloud Shell. No comando seguinte, substitua *\<server_name>* e <principalid_from_last_step>. Escreva um nome de administrador em *\<admin_user>*.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

A identidade de serviço gerida tem agora acesso ao servidor da Base de Dados SQL do Azure.

## <a name="modify-connection-string"></a>Modificar a cadeia de ligação

Modifique a ligação que definiu anteriormente para a aplicação com o comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) no Cloud Shell. No comando seguinte, substitua *\<app name>* pelo nome da aplicação e *\<server_name>* e *\<db_name>* pelos nomes relativos à Base de Dados SQL.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>Modificar o código ASP.NET

No projeto **DotNetAppSqlDb** no Visual Studio, abra _packages.config_ e adicione a seguinte linha na lista de pacotes.

```xml
<package id="Microsoft.Azure.Services.AppAuthentication" version="1.1.0-preview" targetFramework="net461" />
```

Abra _Models\MyDatabaseContext.cs_ e adicione as seguintes instruções `using` à parte superior do ficheiro.

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

Na classe `MyDatabaseContext`, adicione o construtor seguinte:

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

Este construtor configura um objeto SqlConnection personalizado para utilizar um token de acesso para a Base de Dados SQL do Azure a partir do Serviço de Aplicações. Com o token de acesso, a aplicação do Serviço de Aplicações autentica-se com a Base de Dados SQL do Azure com a respetiva identidade de serviço gerida. Para obter mais informações, veja [Obtaining tokens for Azure resources](app-service-managed-service-identity.md#obtaining-tokens-for-azure-resources) (Obter tokens para recursos do Azure). A instrução `if` permite-lhe continuar a testar a aplicação localmente com LocalDB.

> [!NOTE]
> `SqlConnection.AccessToken` só é suportado atualmente em .NET Framework 4.6 e superior, não em [.NET Core](https://www.microsoft.com/net/learn/get-started/windows).
>

Para utilizar este construtor novo, abra `Controllers\TodosController.cs` e localize a linha `private MyDatabaseContext db = new MyDatabaseContext();`. O código existente utiliza o controlador `MyDatabaseContext` predefinido para criar uma base de dados com a cadeia de ligação padrão, que tinha o nome de utilizador e a palavra-passe em texto não encriptado antes de os ter [alterado](#modify-connection-string).

Substitua a linha anterior pelo código abaixo:

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new SqlConnection());
```

### <a name="publish-your-changes"></a>Publicar as alterações

Agora, só falta publicar as alterações no Azure.

No **Explorador de Soluções**, clique com o botão direito do rato no projeto **DotNetAppSqlDb** e selecione **Publicar**.

![Publicar a partir do Explorador de Soluções](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Na página de publicação, clique em **Publicar**. Quando a página Web nova mostrar a lista de tarefas, significa que a aplicação se está a ligar à base de dados com a identidade de serviço gerida.

![Aplicação Web do Azure após a Primeira Migração de Código](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Agora, deverá conseguir editar a lista de tarefas como antes.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Conceder privilégios mínimos à identidade

Durante os passos anteriores, reparou provavelmente que a identidade de serviço gerida está ligada ao SQL Server como a administradora do Azure AD. Para conceder privilégios mínimos à sua identidade de serviço gerida, tem de iniciar sessão no servidor da Base de Dados SQL do Azure como o administrador do Azure AD e, em seguida, adicionar um grupo do Azure Active Directory que contenha a identidade de serviço. 

### <a name="add-managed-service-identity-to-an-azure-active-directory-group"></a>Adicionar a identidade de serviço gerida a um grupo do Azure Active Directory

No Cloud Shell, adicione a identidade de serviço gerida da aplicação num grupo do Azure Active Directory novo, chamado _myAzureSQLDBAccessGroup_, mostrado no seguinte script:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiid
az ad group member list -g $groupid
```

Se quiser ver a saída JSON completa de cada comando, remova os parâmetros `--query objectId --output tsv`.

### <a name="reconfigure-azure-ad-administrator"></a>Reconfigurar o administrador do Azure AD

Anteriormente, atribuiu a identidade de serviço gerida como o administrador do Azure AD da sua Base de Dados SQL. Não pode utilizar esta identidade para início de sessão interativo (para adicionar utilizadores da base de dados), pelo que tem de utilizar o seu utilizador do Azure AD real. Para adicionar o utilizador do Azure AD, siga os passos em [Provision an Azure Active Directory administrator for your Azure SQL Database Server](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) (Aprovisionar um administrador do Azure Active Directory para o servidor da Base de Dados SQL do Azure). 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Conceder permissões ao grupo do Azure Active Directory

No Cloud Shell, utilize o comando SQLCMD para iniciar sessão na Base de Dados SQL. Substitua _\<servername>_ pelo nome do servidor da Base de Dados SQL e _\<AADusername>_ e _\<AADpassword>_ pelas credenciais do utilizador do Azure Ad.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Na linha de comandos do SQL, execute os seguintes comandos, que adicionam o grupo do Azure Active Directory que criou anteriormente como utilizador.

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
GO
```

Escreva `EXIT` para regressar à linha de comandos do Cloud Shell. Em seguida, execute novamente SQLCMD, mas especifique o nome da base de dados em _\<dbname>_.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

Na linha de comandos do SQL da base de dados que pretende, execute os seguintes comandos para conceder permissões de leitura e de escrita ao grupo do Azure Active Directory.

```sql
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

## <a name="next-steps"></a>Passos seguintes

O que aprendeu:

> [!div class="checklist"]
> * Ativar a identidade de serviço gerida
> * Conceder acesso à Base de Dados SQL à identidade de serviço
> * Configurar o código da aplicação para autenticar com a Base de Dados SQL através da autenticação do Azure Active Directory
> * Conceder privilégios mínimos à identidade de serviço na Base de Dados SQL

Avance para o próximo tutorial para saber como mapear um nome DNS personalizado para a aplicação Web.

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para as Aplicações Web do Azure](app-service-web-tutorial-custom-domain.md)