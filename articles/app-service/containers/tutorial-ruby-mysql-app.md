---
title: "Criar uma aplicação de web Ruby e o MySQL no Azure App Service no Linux | Microsoft Docs"
description: "Saiba como obter uma aplicação Ruby a funcionar no Azure, com uma ligação à base de dados MySQL no Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 03b2b4e8f8827a08e1414512d848bd5bed48d674
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Criar uma aplicação de web Ruby e o MySQL no Azure App Service no Linux

O [Serviço de Aplicações no Linux](app-service-linux-intro.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática através do sistema operativo Linux. Este tutorial mostra como criar uma aplicação Ruby web e ligue-o a uma base de dados MySQL. Quando tiver terminado, terá um [Ruby no Rails](http://rubyonrails.org/) aplicação em execução no serviço de aplicações no Linux.

![Ruby na aplicação Rails em execução no App Service do Azure](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma base de dados MySQL no Azure
> * Ligar um Ruby na aplicação Rails MySQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e volte a implementar a aplicação
> * Registos de diagnóstico de fluxo a partir do Azure
> * Gerir a aplicação no portal do Azure

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* [Instale o Git](https://git-scm.com/).
* [Instalar Ruby 2.3](https://www.ruby-lang.org/documentation/installation/)
* [Instalar Ruby Rails 5.1](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [Instalar e iniciar o MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Preparar o local MySQL

Neste passo, vai criar uma base de dados no seu servidor MySQL local para a utilização deste tutorial.

### <a name="connect-to-local-mysql-server"></a>Ligar ao servidor local do MySQL

Numa janela de terminal, ligue ao servidor local MySQL. Pode utilizar esta janela de terminal para executar todos os comandos neste tutorial.

```bash
mysql -u root -p
```

Se lhe for pedida uma palavra-passe, introduza a palavra-passe para o `root` conta. Se não se lembra da sua palavra-passe da conta de raiz, consulte o artigo [MySQL: como repor a palavra-passe de raiz](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Se o comando for executado com êxito, em seguida, o servidor de MySQL está em execução. Se não, certifique-se de que o servidor de MySQL local é iniciado, seguindo o [passos de pós-instalação MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

Sair da sua ligação ao servidor introduzindo `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Criar um Ruby Rails aplicação localmente
Neste passo, obtenha um Ruby na aplicação de exemplo Rails, configurar a ligação de base de dados e executá-la localmente. 

### <a name="clone-the-sample"></a>Clonar o exemplo

Na janela de terminal, `cd` para um diretório de trabalho.

Execute o seguinte comando para clonar o repositório de exemplo.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd`para o seu diretório clonado. Instale os pacotes necessários.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>Configurar a ligação de MySQL

No repositório de abrir _config/database.yml_ e fornecer o nome de utilizador de raiz do local MySQL e a palavra-passe (linha 13). Se tiver instalado o MySQL utilizando uma ferramenta como [Homebrew](https://brew.sh/), em seguida, as credenciais no ficheiro já estão definidas para os valores predefinidos (que é `root` e uma palavra-passe em branco).

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

Executar [as migrações de Rails](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) tem de criar as tabelas a aplicação. Para ver as tabelas são criadas nas migrações, consulte o _migrar/db_ diretório no repositório de Git.

```bash
rake db:create
rake db:migrate
```

Execute a aplicação.

```bash
rails server
```

Navegue para `http://localhost:3000` num browser. Adicione algumas tarefas na página.

![Ruby no Rails liga-se com êxito ao MySQL](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Para parar o servidor de Rails, escreva `Ctrl + C` no terminal.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Criar o MySQL no Azure

Neste passo, vai criar uma base de dados MySQL no [MySQL (pré-visualização) na base de dados Azure](/azure/mysql). Posteriormente, configurar o Ruby na aplicação Rails para ligar a esta base de dados.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Criar um servidor de MySQL

Criar um servidor na base de dados do Azure para MySQL (pré-visualização) com o [az mysql servidor criar](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) comando.

O seguinte comando, substitua o nome do servidor MySQL onde vir o  _&lt;mysql_server_name >_ marcador de posição (carateres válidos são `a-z`, `0-9`, e `-`). Este nome é parte do nome de anfitrião do servidor MySQL (`<mysql_server_name>.mysql.database.azure.com`), tem de ser globalmente exclusivo.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

Quando o servidor de MySQL é criado, a CLI do Azure mostra as informações semelhante ao seguinte exemplo:

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

Criar uma regra de firewall para o servidor de MySQL permitir ligações de cliente utilizando o [az mysql servidor-regra de firewall criar](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) comando.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Base de dados do Azure para MySQL (pré-visualização) atualmente não limita ligações apenas para os serviços do Azure. Como dinamicamente forem atribuídos a endereços IP no Azure, é melhor ativar todos os endereços IP. O serviço está em pré-visualização. Melhor métodos para proteger a sua base de dados estão a ser planeados.
>

### <a name="connect-to-production-mysql-server-locally"></a>Ligar ao servidor de MySQL de produção localmente

Na janela de terminal, ligar ao servidor MySQL no Azure. Utilize o valor que especificou anteriormente para  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Quando lhe for pedido para uma palavra-passe, utilize _My5up3r$ tr0ngPa$ w0rd!_, que especificou quando criou o servidor de base de dados.

### <a name="create-a-production-database"></a>Criar uma base de dados de produção

Na `mysql` solicitar, crie uma base de dados.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Criar um utilizador com permissões

Criar um utilizador de base de dados chamado _railsappuser_ e conceda-lhe todos os privilégios `sampledb` base de dados.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

Sair a ligação ao servidor introduzindo `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Ligar a aplicação para MySQL do Azure

Neste passo, ligue o Ruby na aplicação Rails à base de dados MySQL que criou na base de dados do Azure para MySQL (pré-visualização).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Configurar a ligação de base de dados

No repositório de abrir _config/database.yml_. Na parte inferior do ficheiro, substitua as variáveis de produção com o seguinte código. Para o  _&lt;mysql_server_name >_ marcador de posição, utilize o nome do servidor MySQL que criou.

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
> O `sslca` é adicionada e de pontos a um existente _. pem_ ficheiro no repositório de exemplo. Por predefinição, a base de dados do Azure para MySQL impõe ligações de SSL de clientes. Isto `.pem` certificado é como efetuar ligações de SSL à base de dados do Azure para MySQL. Para obter mais informações, veja [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../../mysql/howto-configure-ssl.md) (Configurar a conectividade SSL na sua aplicação para ligar em segurança à Base de Dados do Azure para MySQL).
>

### <a name="test-the-application-locally"></a>Testar a aplicação localmente

No local terminal, defina as seguintes variáveis de ambiente:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

Execute migrações de base de dados de Rails com os valores de produção que acabou de configurar para criar as tabelas na base de dados MySQL na base de dados do Azure para MySQL (pré-visualização). 

```bash
rake db:migrate RAILS_ENV=production
```

Quando em execução no ambiente de produção, a aplicação de Rails tem ativos pré-compilada. Gerar os recursos necessários com o seguinte comando:

```bash
rake assets:precompile
```

O ambiente de produção Rails também utiliza os segredos para gerir a segurança. Gere uma chave secreta.

```bash
rails secret
```

Guarde a chave secreta para as respetivas variáveis utilizadas pelo ambiente de produção Rails. Para sua comodidade, utilize a mesma chave para ambas as variáveis.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

Ative o ambiente de produção Rails servir os ficheiros de JavaScript e CSS.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Execute a aplicação de exemplo no ambiente de produção.

```bash
rails server -e production
```

Navegue para `http://localhost:3000`. Se a página for carregada sem erros, o Ruby na aplicação Rails está a ligar à base de dados MySQL no Azure.

Adicione algumas tarefas na página.

![Ruby no Rails estabelece ligação com êxito à base de dados do Azure para MySQL (pré-visualização)](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Para parar o servidor de Rails, escreva `Ctrl + C` no terminal.

### <a name="commit-your-changes"></a>Consolidar as alterações

Execute os seguintes comandos de Git para consolidar as alterações:

```bash
git add .
git commit -m "database.yml updates"
```

A aplicação está pronta para ser implementada.

## <a name="deploy-to-azure"></a>Implementar no Azure

Neste passo, a aplicação é implementada ligados MySQL Rails App Service do Azure.

### <a name="configure-a-deployment-user"></a>Configurar um utilizador de implementação

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Criar uma aplicação Web

No Cloud Shell, crie uma aplicação Web no plano do Serviço de Aplicações `myAppServicePlan` com o comando [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). 

No exemplo a seguir, substitua `<app_name>` com um nome de aplicação globalmente exclusivo (os carateres válidos são `a-z`, `0-9` e `-`). O tempo de execução está definido como `RUBY|2.3`, que implementa o [predefinido imagem Ruby](https://hub.docker.com/r/appsvc/ruby/). Para ver todos os tempos de execução suportados, execute [az webapp list-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes). 

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

### <a name="configure-database-settings"></a>Configurar as definições de base de dados

No App Service, definir variáveis de ambiente como _as definições de aplicação_ utilizando o [az webapp configuração appsettings conjunto](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) comando na Shell de nuvem.

O seguinte comando na Shell de nuvem configura as definições de aplicação `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, e `DB_PASSWORD`. Substitua os marcadores de posição  _&lt;appname >_ e  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Configurar variáveis de ambiente de Rails

No local terminal, gere uma nova chave secreta para o ambiente de produção Rails no Azure.

```bash
rails secret
```

Configure as variáveis de ambiente de produção Rails.

No comando Shell de nuvem seguinte, substitua as duas  _&lt;output_of_rails_secret >_ marcadores de posição com a nova chave secreta gerados no local terminal.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"`Indica a predefinição Ruby contentor para pré-compilar ativos em cada implementação de Git.

### <a name="push-to-azure-from-git"></a>Enviar para o Azure a partir do Git

No local terminal, adicione um remoto do Azure para o seu repositório de Git local.

```bash
git remote add azure <paste_copied_url_here>
```

Push para o Azure remoto para implementar o Ruby na aplicação Rails. É-lhe pedida a palavra-passe que forneceu anteriormente como parte da criação do utilizador de implementação.

```bash
git push azure master
```

Durante a implementação, o App Service do Azure comunica o progresso da mesma com o Git.

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

### <a name="browse-to-the-azure-web-app"></a>Navegue para a aplicação web do Azure

Navegue até à `http://<app_name>.azurewebsites.net` e adicionar algumas tarefas à lista.

![Ruby na aplicação Rails em execução no App Service do Azure](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Parabéns, que está a executar um Ruby condicionada por dados na aplicação de Rails no App Service do Azure.

## <a name="update-model-locally-and-redeploy"></a>Atualizar o modelo localmente e volte a implementar

Neste passo, efetuar uma alteração simples para o `task` modelo de dados e a webapp e, em seguida, publicar a atualização para o Azure.

Para o cenário de tarefas, modificar a aplicação de modo a que pode marcar uma tarefa como concluídos.

### <a name="add-a-column"></a>Adicionar uma coluna

No terminal, navegue para a raiz do repositório de Git.

Gerar uma nova migração que adiciona uma coluna booleana denominada `Done` para o `Tasks` tabela:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Este comando gera um novo ficheiro de migração no _migrar/db_ diretório.


No terminal, execute migrações de base de dados de Rails para efetuar a alteração na base de dados local.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Lógica da aplicação de atualização

Abra o *app/controllers/tasks_controller.rb* ficheiro. No final do ficheiro, localize a seguinte linha:

```rb
params.require(:task).permit(:Description)
```

Modificar esta linha para incluir o novo `Done` parâmetro.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>As vistas de atualização

Abra o *app/views/tasks/_form.html.erb* ficheiro, que é a forma de edição.

Localizar a linha `<%=f.error_span(:Description) %>` e insira o seguinte código diretamente abaixo do mesmo:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Abra o *app/views/tasks/show.html.erb* ficheiro, que é a página de vista único registo. 

Localizar a linha `<dd><%= @task.Description %></dd>` e insira o seguinte código diretamente abaixo do mesmo:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Abra o *app/views/tasks/index.html.erb* ficheiro, que é a página de índice para todos os registos.

Localizar a linha `<th><%= model_class.human_attribute_name(:Description) %></th>` e insira o seguinte código diretamente abaixo do mesmo:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

O mesmo ficheiro, localize a linha `<td><%= task.Description %></td>` e insira o seguinte código diretamente abaixo do mesmo:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>Testar as alterações localmente

No local terminal, execute o servidor de Rails.

```bash
rails server
```

Para ver o estado da tarefa alterar, navegue para `http://localhost:3000` e adicionar ou editar itens.

![Caixa de verificação foi adicionada à tarefa](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Para parar o servidor de Rails, escreva `Ctrl + C` no terminal.

### <a name="publish-changes-to-azure"></a>Publicar as alterações para Azure

No terminal, execute migrações de base de dados de Rails para o ambiente de produção efetuar a alteração na base de dados do Azure.

```bash
rake db:migrate RAILS_ENV=production
```

Consolidar todas as alterações no Git e, em seguida, emita as alterações de código para o Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Uma vez a `git push` está concluída, navegue para a aplicação web do Azure e testar a nova funcionalidade.

![As alterações de modelo e a base de dados publicados para o Azure](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Se tiver adicionado quaisquer tarefas, estes são retidos na base de dados. Atualizações para o esquema de dados, deixe dados existentes intactas.

## <a name="manage-the-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para gerir a aplicação Web que criou.

No menu à esquerda, clique em **Serviços de Aplicações** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/tutorial-php-mysql-app/access-portal.png)

É apresentada a página de descrição geral da sua aplicação Web. Aqui, pode efetuar tarefas de gestão básicas, como parar, iniciar, reiniciar, procurar e eliminar.

O menu à esquerda fornece páginas para configurar a sua aplicação.

![Página Serviço de Aplicações no portal do Azure](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma base de dados MySQL no Azure
> * Ligar um Ruby na aplicação Rails MySQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e volte a implementar a aplicação
> * Registos de diagnóstico de fluxo a partir do Azure
> * Gerir a aplicação no portal do Azure

Avançar para o próximo tutorial para saber como mapear um nome DNS personalizado para uma aplicação web.

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para as Aplicações Web do Azure](../app-service-web-tutorial-custom-domain.md)
