---
title: "Criar uma aplicação Web da Base de Dados SQL e .NET Core no Serviço de Aplicações do Azure no Linux | Microsoft Docs"
description: "Saiba como obter uma aplicação .NET Core a funcionar no Serviço de Aplicações do Azure no Linux, com ligação a uma Base de Dados SQL."
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 10/10/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 1418914b2886ce3f896e62b5b4a3da573655e274
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="build-a-net-core-and-sql-database-web-app-in-azure-app-service-on-linux"></a>Criar uma aplicação Web da Base de Dados SQL e .NET Core no Serviço de Aplicações do Azure no Linux

O [Serviço de Aplicações no Linux](app-service-linux-intro.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática através do sistema operativo Linux. Este tutorial mostra como criar uma aplicação Web do .NET Core e liguá-la a uma Base de Dados SQL. Quando tiver terminado, terá uma aplicação de MVC do .NET Core em execução no Serviço de Aplicações no Linux.

![aplicação em execução no Serviço de Aplicações no Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

O que aprende com o saiba como:

> [!div class="checklist"]
> * Criar uma Base de Dados SQL no Azure
> * Ligar uma aplicação .NET Core à Base de Dados SQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Transmitir os registos de diagnóstico de transmissão em fluxo a partir do Azure
> * Gerir a aplicação no portal do Azure

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

1. [Instale o Git](https://git-scm.com/).
1. [Instalar o SDK de .NET Core 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.1.2-download.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-local-net-core-app"></a>Criar aplicação de .NET Core local

Neste passo, configurará o projeto de .NET Core local.

### <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Na janela de terminal, `cd` num diretório de trabalho.

Execute os seguintes comandos para clonar o repositório de exemplo e altere para esta raiz.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

O projeto de exemplo contém uma aplicação CRUD (create-read-update-delete (criar-ler-atualizar-eliminar)) básica [Entity Framework Core](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>Executar a aplicação

Execute os seguintes comandos para instalar os pacotes necessários, execute migrações de base de dados e inicie a aplicação.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

Navegue para `http://localhost:5000` num browser. Selecione a ligação **Criar Novo** e criar alguns itens _a fazer_.

![liga-se com êxito à Base de Dados SQL](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

Para parar o .NET Core em qualquer altura, prima `Ctrl+C` no terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Criar Base de Dados SQL de produção

Neste passo, criará uma Base de Dados SQL no Azure. Quando a aplicação for implementada no Azure, este utiliza esta base de dados de cloud.

Como Base de Dados SQL, neste tutorial é utilizada a [Base de Dados SQL do Azure](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>Criar um servidor lógico da Base de Dados SQL

No Cloud Shell, crie um servidor lógico de Base de Dados SQL do Azure com o comando [az sql server create](/cli/azure/sql/server?view=azure-cli-latest#az_sql_server_create).

Substitua o marcador de posição *\<server_name>* por um nome exclusivo da Base de Dados SQL. Este nome é utilizado como parte do ponto final da Base de Dados SQL, `<server_name>.database.windows.net`, por isso, o nome tem de ser exclusivo em todos os servidores lógicos no Azure. O nome deve conter apenas minúsculas, números, o caráter hífen (-) e tem de ter entre três e 50 carateres. Substitua também, *\<db_username>* e  *\<db_password>* por um nome de utilizador e palavra-passe à sua escolha. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

Após criar a o servidor lógico da Base de Dados SQL, a CLI do Azure mostra informações semelhantes ao exemplo seguinte:

```json
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/<server_name>",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "<server_name>",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

### <a name="configure-a-server-firewall-rule"></a>Configurar uma regra de firewall do servidor

Crie uma [regra de firewall ao nível do servidor da Base de Dados SQL do Azure](../../sql-database/sql-database-firewall-configure.md) com o comando [az sql server firewall create](/cli/azure/sql/server/firewall-rule?view=azure-cli-latest#az_sql_server_firewall_rule_create). Quando os IPs inicial e final estão definidos como 0.0.0.0, a firewall apenas é aberta para outros recursos do Azure. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Criar uma base de dados

Crie uma base de dados com um [nível de desempenho S0](../../sql-database/sql-database-service-tiers.md) no servidor com o comando [az sql db create](/cli/azure/sql/db?view=azure-cli-latest#az_sql_db_create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Crie uma cadeia de ligação

Substitua a seguinte string com o *\<server_name>*, *\<db_username>* e *\<db_password>* utilizados anteriormente.

```
Server=tcp:<server_name>.database.windows.net,1433;Initial Catalog=coreDB;Persist Security Info=False;User ID=<db_username>;Password=<db_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Esta é a cadeia de ligação para a sua aplicação .NET Core. Copie-a para utilização posterior.

## <a name="deploy-app-to-azure"></a>Implementar a aplicação no Azure

Neste passo, implemente a aplicação .NET Core ligada à Base de Dados SQL do Serviço de Aplicações no Linux.

### <a name="configure-local-git-deployment"></a>Configurar a implementação do git local

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Criar uma aplicação Web

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Configure uma variável de ambiente

Para definir cadeias de ligação para a sua aplicação do Azure, utilize o comando [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) no Cloud Shell. No seguinte comando, substitua  *\<*, bem como o parâmetro *\<connection_string>* com a cadeia de ligação que criou anteriormente.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection_string>' --connection-string-type SQLServer
```

Em seguida, configure a configuração de aplicação `ASPNETCORE_ENVIRONMENT` para _Produção_. Esta definição permite-lhe saber se está a em execução no Azure, uma vez que utiliza o SQLite para o seu ambiente de desenvolvimento local e a Base de Dados SQL para o seu ambiente do Azure.

O exemplo seguinte configura uma configuração de aplicação `ASPNETCORE_ENVIRONMENT` na sua aplicação Web do Azure. Substitua o marcador de posição *\<app_name>*.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

### <a name="connect-to-sql-database-in-production"></a>Ligar à Base de Dados SQL na produção

No seu repositório local, abra Startup.cs e localize o seguinte código:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Substitua-o com o código seguinte, que utiliza as variáveis de ambiente que configurou anteriormente.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
```

Se este código deteta que está a ser executado na produção (o que indica o ambiente do Azure), em seguida, utiliza a cadeia de ligação configurada para ligar à Base de Dados SQL.

A chamada `Database.Migrate()` ajuda-o quando é executada no Azure, porque esta cria automaticamente as bases de dados que a aplicações .NET Core necessita, com base na respetiva configuração de migração. 

Guardar as alterações e, em seguida, consolide-as no seu repositório de Git. 

```bash
git commit -am "connect to SQLDB in Azure"
```

### <a name="push-to-azure-from-git"></a>Enviar para o Azure a partir do Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-azure-web-app"></a>Navegue para a aplicação Web do Azure

Utilize o browser para navegar para a aplicação Web implementada.

```bash
http://<app_name>.azurewebsites.net
```

Adicione alguns itens a fazer.

![aplicação em execução no Serviço de Aplicações no Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Parabéns!** Está a executar uma aplicação de .NET Core condicionada por dados no Serviço de Aplicações no Linux.

## <a name="update-locally-and-redeploy"></a>Atualizar localmente e reimplementar

Neste passo, altere o esquema de base de dados e publique-o no Azure.

### <a name="update-your-data-model"></a>Atualizar o modelo de dados

Abra _Models\Todo.cs_ no editor de código. Adicione a seguinte propriedade à classe `ToDo`:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Execute Primeiras Migrações do Código localmente

Execute alguns comandos para disponibilizar as atualizações à base de dados local.

```bash
dotnet ef migrations add AddProperty
```

Atualize a base de dados local:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Utilize a nova propriedade

Efetuar algumas alterações no seu código para utilizar a propriedade `Done`. Por simplicidade neste tutorial, apenas vai alterar as vistas `Index` e `Create` para ver a propriedade em ação.

Abra _Controllers\TodosController.cs_.

Localize o método `Create()` e adicione `Done` à lista de propriedades no atributo `Bind`. Quando tiver terminado, a assinatura de método `Create()` tem o aspeto do seguinte código:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Abra _Views\Todos\Create.cshtml_.

No código Razor, deverá ver um elemento `<div class="form-group">` para `Description`e, em seguida, outro elemento `<div class="form-group">` para `CreatedDate`. Imediatamente a seguir estes dois elementos, adicione outro elemento `<div class="form-group">` para `Done`:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Abra _Views\Todos\Index.cshtml_.

Procure o elemento vazio `<th></th>`. Mesmo acima deste elemento, adicione o seguinte código Razor:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Localize o elemento `<td>` que contém a etiqueta `asp-action` de ajuda. Mesmo acima deste elemento, adicione o seguinte código Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.CreatedDate)
</td>
```

É tudo o que necessita para ver as alterações nas vistas `Index` e `Create`.

### <a name="test-your-changes-locally"></a>Teste as alterações localmente

Execute a aplicação localmente.

```bash
dotnet run
```

No seu browser, navegue para `http://localhost:5000/`. Agora pode adicionar um item de tarefas e marcar **Concluído**. Em seguida, deve mostrar cópias de segurança na sua home page como um item concluído. Lembre-se de que a vista `Edit` não apresenta o campo `Done`, porque a vista `Edit` não foi alterada.

### <a name="publish-changes-to-azure"></a>Publique as alterações no Azure

```bash

git commit -am "added done field"
git push azure master
```

Uma vez que `git push` é concluída, navegue para a aplicação Web do Azure e experimente a nova funcionalidade.

![Aplicação Web do Azure após a Primeira Migração do Código](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Todos os seus itens a fazer existentes ainda são apresentados. Quando voltar a publicar a aplicação .NET Core, os dados existentes na sua Base de Dados SQL não são perdidos. Além disso, as Migrações do Entity Framework Core apenas alteram o esquema de dados e mantêm os dados existentes intactos.

## <a name="manage-your-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para ver a aplicação Web que criou.

No menu à esquerda, clique em **Serviços Aplicacionais** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Por predefinição, o portal mostra a página **Descrição Geral** da aplicação Web. Esta página proporciona-lhe uma vista do desempenho da aplicação. Aqui, também pode realizar tarefas de gestão básicas, como navegar, parar, iniciar, reiniciar e eliminar. Os separadores no lado esquerdo da página mostram as várias páginas de configuração que pode abrir.

![Página Serviço de Aplicações no portal do Azure](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Passos seguintes

O que aprendeu:

> [!div class="checklist"]
> * Criar uma Base de Dados SQL no Azure
> * Ligar uma aplicação .NET Core à Base de Dados SQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Transmitir os registos do Azure para o seu terminal
> * Gerir a aplicação no portal do Azure

Avançar para o próximo tutorial para saber como mapear um nome DNS personalizado à aplicação Web.

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para as Aplicações Web do Azure](../app-service-web-tutorial-custom-domain.md)