---
title: "Criar uma aplicação Web Ruby e MySQL no Serviço de Aplicações do Azure no Linux | Microsoft Docs"
description: "Saiba como pôr aplicações Ruby a funcionar no Azure, com ligação a uma base de dados MySQL no Azure."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: ruby
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 951e66e47cf8fbe9d2cdf1606a8d63054bcada13
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Criar uma aplicação Web Ruby e MySQL no Serviço de Aplicações do Azure no Linux

O [Serviço de Aplicações no Linux](app-service-linux-intro.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática através do sistema operativo Linux. Este tutorial mostra como criar uma aplicação Web Ruby e ligá-la a uma base de dados MySQL. Quando tiver terminado, terá uma aplicação [Ruby on Rails](http://rubyonrails.org/) em execução no Serviço de Aplicações no Linux.

![Aplicação Ruby on Rails em execução no Serviço de Aplicações do Azure](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma base de dados MySQL no Azure
> * Ligar uma aplicação Ruby on Rails ao MySQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Transmitir os registos de diagnóstico em fluxo a partir do Azure
> * Gerir a aplicação no portal do Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* [Instale o Git](https://git-scm.com/).
* [Instalar o Ruby 2.3](https://www.ruby-lang.org/documentation/installation/)
* [Instalar o Ruby on Rails 5.1](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [Instalar e iniciar o MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Preparar o MySQL local

Neste passo, vai criar uma base de dados no seu servidor MySQL local para utilizar neste tutorial.

### <a name="connect-to-local-mysql-server"></a>Ligar ao servidor MySQL local

Numa janela de terminal, ligue ao seu servidor MySQL local. Pode utilizar esta janela de terminal para executar todos os comandos deste tutorial.

```bash
mysql -u root -p
```

Se lhe for pedida uma palavra-passe, introduza a palavra-passe da conta `root`. Se não se lembrar da sua palavra-passe da conta de raiz, veja [MySQL: Como Repor a Palavra-passe de Raiz](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Se o comando for executado com êxito, significa que o servidor MySQL está em execução. Se não, siga os [passos de pós-instalação do MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) para verificar se o servidor MySQL local é iniciado.

Escreva `quit` para sair da ligação ao servidor.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Criar uma aplicação Ruby on Rails localmente
Neste passo, vai obter uma aplicação Ruby on Rails de exemplo, configurar a respetiva ligação à base de dados e executá-la localmente. 

### <a name="clone-the-sample"></a>Clonar o exemplo

Na janela de terminal, `cd` num diretório de trabalho.

Execute o seguinte comando para clonar o repositório de exemplo.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd` para o diretório clonado. Instale os pacotes necessários.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>Configurar a ligação ao MySQL

No repositório, abra _config/database.yml_ e forneça o nome de utilizador e a palavra-passe de raiz do MySQL local (linha 13). Se tiver instalado o MySQL com uma ferramenta como o [Homebrew](https://brew.sh/), as credenciais no ficheiro já estão definidas para os valores predefinidos (`root` e uma palavra-passe em branco).

```txt
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock
```

### <a name="run-the-sample-locally"></a>Executar o exemplo localmente

Execute as [migrações do Rails](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) para criar as tabelas de que a aplicação precisa. Para ver que tabelas são criadas nas migrações, veja o diretório _db/migrate_ no repositório Git.

```bash
rake db:create
rake db:migrate
```

Execute a aplicação.

```bash
rails server
```

Navegue para `http://localhost:3000` num browser. Adicione algumas tarefas à página.

![O Ruby on Rails liga-se com êxito ao MySQL](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Para parar o servidor do Rails, escreva `Ctrl + C` no terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Criar o MySQL no Azure

Neste passo, vai criar uma base de dados MySQL na [Base de Dados do Azure para MySQL (Pré-visualização)](/azure/mysql). Posteriormente, vai configurar a aplicação Ruby on Rails para se ligar a esta base de dados.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Criar um servidor MySQL

Crie um servidor na Base de Dados do Azure para MySQL (Pré-visualização) com o comando [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create).

No comando seguinte, substitua o nome do servidor MySQL onde vê o marcador de posição _&lt;mysql_server_name>_ (os carateres válidos são `a-z`, `0-9` e `-`). Este nome faz parte do nome de anfitrião do servidor MySQL (`<mysql_server_name>.mysql.database.azure.com`); tem de ser globalmente exclusivo.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

Quando o servidor MySQL tiver sido criado, a CLI do Azure mostra informações semelhantes às do exemplo abaixo:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Configurar a firewall do servidor

Crie uma regra de firewall para o servidor MySQL permitir ligações ao cliente com o comando [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create).

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Atualmente, a Base de Dados do Azure para MySQL (Pré-visualização) não limita ligações apenas aos serviços do Azure. Uma vez que os endereços IP são atribuídos dinamicamente no Azure, é melhor ativar todos os endereços. O serviço está em pré-visualização. Estão previstos métodos melhores para proteger a sua base de dados.
>

### <a name="connect-to-production-mysql-server-locally"></a>Ligar ao servidor MySQL de produção localmente

Na janela de terminal, ligue ao servidor MySQL no Azure. Utilize o valor que especificou anteriormente em _&lt;mysql_server_name>_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Quando lhe for pedida uma palavra-passe, utilize _My5up3r$tr0ngPa$w0rd!_, que especificou quando criou o servidor de base de dados.

### <a name="create-a-production-database"></a>Criar uma base de dados de produção

No comando `mysql`, crie uma base de dados.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Criar um utilizador com permissões

Crie um utilizador de base de dados com o nome _railsappuser_ e conceda-lhe todos os privilégios da base de dados `sampledb`.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

Escreva `quit` para sair da ligação ao servidor.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Ligar a aplicação ao MySQL do Azure

Neste passo, vai ligar a aplicação Ruby on Rails à base de dados MySQL que criou na Base de Dados do Azure para MySQL (Pré-visualização).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Configurar a ligação à base de dados

No repositório, abra _config/database.yml_. Na parte inferior do ficheiro, substitua as variáveis de produção pelo código seguinte. Para o marcador de posição _&lt;mysql_server_name>_, utilize o nome do servidor MySQL que criou.

```txt
production:
  <<: *default
  host: <%= ENV['DB_HOST'] %>
  port: 3306
  database: <%= ENV['DB_DATABASE'] %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  sslca: ssl/BaltimoreCyberTrustRoot.crt.pem
```

Guarde as alterações.

> [!NOTE]
> É adicionado `sslca`, que aponta para um ficheiro _.pem_ existente no repositório de exemplo. Por predefinição, a Base de Dados do Azure para MySQL impõe ligações SSL dos clientes. Este certificado `.pem` é como efetua ligações SSL à Base de Dados do Azure para MySQL. Para obter mais informações, veja [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../../mysql/howto-configure-ssl.md) (Configurar a conectividade SSL na sua aplicação para ligar em segurança à Base de Dados do Azure para MySQL).
>

### <a name="test-the-application-locally"></a>Testar a aplicação localmente

No terminal local, defina as seguintes variáveis de ambiente:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

Execute migrações de bases de dados do Rails com os valores de produção que acabou de configurar para criar as tabelas na sua base de dados MySQL na Base de Dados do Azure para MySQL (Pré-visualização). 

```bash
rake db:migrate RAILS_ENV=production
```

Quando estiver em execução no ambiente de produção, a aplicação Rails precisa de ativos pré-compilados. Gere os recursos necessários com o seguinte comando:

```bash
rake assets:precompile
```

O ambiente de produção do Rails também utiliza segredos para gerir a segurança. Gere uma chave secreta.

```bash
rails secret
```

Guarde a chave secreta para as respetivas variáveis utilizadas pelo ambiente de produção do Rails. Para sua comodidade, utilize a mesma chave para ambas as variáveis.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

Ative o ambiente de produção do Rails para servir ficheiros JavaScript e CSS.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Execute a aplicação de exemplo no ambiente de produção.

```bash
rails server -e production
```

Navegue para `http://localhost:3000`. Se a página for carregada sem erros, a aplicação Ruby on Rails está a ligar-se à base de dados MySQL no Azure.

Adicione algumas tarefas à página.

![O Ruby on Rails liga-se com êxito à Base de Dados do Azure para MySQL (Pré-visualização)](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Para parar o servidor do Rails, escreva `Ctrl + C` no terminal.

### <a name="commit-your-changes"></a>Consolidar as alterações

Execute os comandos Git seguintes para consolidar as alterações:

```bash
git add .
git commit -m "database.yml updates"
```

A sua aplicação está pronta para ser implementada.

## <a name="deploy-to-azure"></a>Implementar no Azure

Neste passo, vai implementar a aplicação Rails ligada ao MySQL no Serviço de Aplicações do Azure.

### <a name="configure-a-deployment-user"></a>Configurar um utilizador de implementação

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Criar uma aplicação Web

No Cloud Shell, crie uma aplicação Web no plano do Serviço de Aplicações `myAppServicePlan` com o comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). 

No exemplo a seguir, substitua `<app_name>` com um nome de aplicação globalmente exclusivo (os carateres válidos são `a-z`, `0-9` e `-`). O runtime está definido como `RUBY|2.3`, o que implementa a [imagem Ruby predefinida](https://hub.docker.com/r/appsvc/ruby/). Para ver todos os runtimes suportados, execute [`az webapp list-runtimes`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "RUBY|2.3" --deployment-local-git
```

Quando a aplicação Web tiver sido criada, a CLI do Azure mostra informações semelhantes ao seguinte exemplo:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Criou uma nova aplicação Web vazia, com a implementação de git ativada.

> [!NOTE]
> O URL do Git remoto é apresentado na propriedade `deploymentLocalGitUrl`, com o formato `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Guarde este URL, uma vez que vai precisar dele mais tarde.
>

### <a name="configure-database-settings"></a>Configurar as definições da base de dados

No Serviço de Aplicações, as variáveis de ambiente são definidas como _definições da aplicação_ com o comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) no Cloud Shell.

O comando do Cloud Shell seguinte configura as definições da aplicação `DB_HOST`, `DB_DATABASE`, `DB_USERNAME` e `DB_PASSWORD`. Substitua os marcadores de posição _&lt;appname>_ e _&lt;mysql_server_name>_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Configurar as variáveis de ambiente do Rails

No terminal local, gere uma nova chave secreta para o ambiente de produção do Rails no Azure.

```bash
rails secret
```

Configure as variáveis necessárias para o ambiente de produção do Rails.

No comando do Cloud Shell seguinte, substitua os dois marcadores de posição _&lt;output_of_rails_secret>_ pela nova chave secreta gerada no terminal local.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"` indica o contentor Ruby predefinido para pré-compilar ativos em cada implementação do Git.

### <a name="push-to-azure-from-git"></a>Enviar para o Azure a partir do Git

No terminal local, adicione um Azure remoto ao seu repositório Git local.

```bash
git remote add azure <paste_copied_url_here>
```

Envie para o Azure remoto para implementar a aplicação Ruby on Rails. É-lhe pedida a palavra-passe que indicou anteriormente como parte da criação do utilizador de implementação.

```bash
git push azure master
```

Durante a implementação, o Serviço de Aplicações do Azure comunica o respetivo progresso com o Git.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

### <a name="browse-to-the-azure-web-app"></a>Navegar para a aplicação Web do Azure

Navegue para `http://<app_name>.azurewebsites.net` e adicione algumas tarefas à lista.

![Aplicação Ruby on Rails em execução no Serviço de Aplicações do Azure](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Parabéns! Está agora a executar uma Aplicação Ruby on Rails condicionada por dados no Serviço de Aplicações do Azure.

## <a name="update-model-locally-and-redeploy"></a>Atualizar o modelo localmente e reimplementar

Neste passo, vai fazer uma pequena alteração ao modelo de dados `task` e à aplicação Web e, depois, vai publicá-la no Azure.

No cenário de tarefas, vai modificar a aplicação de modo a poder marcar uma tarefa como concluída.

### <a name="add-a-column"></a>Adicionar uma coluna

No terminal, navegue para a raiz do repositório Git.

Gere uma nova migração que adicione uma coluna booleana denominada `Done` à tabela `Tasks`:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Este comando gera um novo ficheiro de migração no diretório _db/migrate_.


No terminal, execute migrações de bases de dados do Rails para fazer a alteração na base de dados local.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Atualizar a lógica da aplicação

Abra o ficheiro *app/controllers/tasks_controller.rb*. No final do ficheiro, localize a seguinte linha:

```rb
params.require(:task).permit(:Description)
```

Modifique esta linha para incluir o novo parâmetro `Done`.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>Atualizar as vistas

Abra o ficheiro *app/views/tasks/_form.html.erb*, que é o Formulário de edição.

Localize a linha `<%=f.error_span(:Description) %>` e insira o seguinte código diretamente abaixo da mesma:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Abra o ficheiro *app/views/tasks/show.html.erb*, que é a Página de visualização de registo único. 

Localize a linha `<dd><%= @task.Description %></dd>` e insira o seguinte código diretamente abaixo da mesma:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Abra o ficheiro *app/views/tasks/index.html.erb*, que é a Página de índice de todos os registos.

Localize a linha `<th><%= model_class.human_attribute_name(:Description) %></th>` e insira o seguinte código diretamente abaixo da mesma:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

No mesmo ficheiro, localize a linha `<td><%= task.Description %></td>` e insira o seguinte código diretamente abaixo da mesma:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>Testar as alterações localmente

No terminal local, execute o servidor do Rails.

```bash
rails server
```

Para ver a alteração do estado da tarefa, navegue para `http://localhost:3000` e adicione ou edite os itens.

![Caixa de verificação adicionada à tarefa](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Para parar o servidor do Rails, escreva `Ctrl + C` no terminal.

### <a name="publish-changes-to-azure"></a>Publicar alterações no Azure

No terminal, execute migrações de bases de dados do Rails para o ambiente de produção para fazer a alteração na base de dados do Azure.

```bash
rake db:migrate RAILS_ENV=production
```

Consolide todas as alterações no Git e envie as alterações ao código para o Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Quando `git push` estiver concluído, navegue para a aplicação Web do Azure e teste a funcionalidade nova.

![Alterações ao modelo e à base de dados publicadas no Azure](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Se tiver adicionado tarefas, estas são mantidas na base de dados. As atualizações ao esquema de dados não afetam os dados já existentes.

## <a name="manage-the-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para gerir a aplicação Web que criou.

No menu à esquerda, clique em **Serviços de Aplicações** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/tutorial-php-mysql-app/access-portal.png)

É apresentada a página de descrição geral da sua aplicação Web. Aqui, pode realizar tarefas de gestão básicas, como parar, iniciar, reiniciar, navegar e eliminar.

O menu do lado esquerdo disponibiliza páginas para configurar a aplicação.

![Página Serviço de Aplicações no portal do Azure](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma base de dados MySQL no Azure
> * Ligar uma aplicação Ruby on Rails ao MySQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Transmitir os registos de diagnóstico em fluxo a partir do Azure
> * Gerir a aplicação no portal do Azure

Avance para o tutorial seguinte para aprender a mapear um nome DNS personalizado para uma aplicação Web.

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para as Aplicações Web do Azure](../app-service-web-tutorial-custom-domain.md)
