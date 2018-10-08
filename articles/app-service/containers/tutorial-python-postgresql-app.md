---
title: Compilar uma aplicação Web em Python e PostgreSQL no Serviço de Aplicações do Azure | Microsoft Docs
description: Saiba como executar uma aplicação Python condicionada por dados no Azure, com ligação a uma base de dados PostgreSQL.
services: app-service\web
documentationcenter: python
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 09/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: f4ce197d541b8573e38fd85dcebb575c8ee99f59
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435791"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Criar uma aplicação Web Docker Python com PostgreSQL no Azure

[O Serviço de Aplicações no Linux](app-service-linux-intro.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática. Este tutorial mostra como criar uma aplicação Web em Python condicionada por dados, com o PostgreSQL como o back-end da base de dados. Quando tiver terminado, terá uma aplicação do Python Flask em execução num contentor de Docker no Serviço de Aplicações no Linux.

![Aplicação do Docker Python Flask no Serviço de Aplicações no Linux](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma base de dados PostgreSQL no Azure
> * Ligar uma aplicação Python ao PostgreSQL
> * Implementar a aplicação no Azure
> * Ver registos de diagnóstico
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Gerir a aplicação no portal do Azure

Pode seguir os passos neste artigo no macOS. As instruções do Linux e Windows são as mesmas na maioria dos casos, mas as diferenças não estão descritas neste tutorial.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

1. [Instalar o Git](https://git-scm.com/)
1. [Instalar o Python](https://www.python.org/downloads/)
1. [Instalar e executar o PostgreSQL](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testar a instalação do PostgreSQL local e criar uma base de dados

Numa janela do terminal local, execute `psql` para ligar ao seu servidor PostgreSQL local.

```bash
sudo -u postgres psql postgres
```

Se obtiver uma mensagem de erro semelhante a `unknown user: postgres`, a instalação do PostgreSQL pode ser configurada com o seu nome de utilizador com sessão iniciada. Em alternativa, experimente o comando seguinte.

```bash
psql postgres
```

Se a ligação for bem-sucedida, a base de dados PostgreSQL está em execução. Caso contrário, certifique-se de que a base de dados PostgresQL local é iniciada conforme as instruções para o seu sistema operativo em [Transferências - Distribuição do PostgreSQL Core](https://www.postgresql.org/download/).

Crie uma base de dados denominada *eventregistration* e configure um utilizador de base de dados em separado denominado *gestor* com a palavra-passe *supersecretpass*.

```sql
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Escreva `\q` para sair do cliente PostgreSQL.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Criar uma aplicação Python local

Neste passo, vai configurar o projeto Python Flask local.

### <a name="clone-the-sample-app"></a>Clonar a aplicação de exemplo

Abra a janela de terminal e `CD` num diretório de trabalho.

Execute os seguintes comandos para clonar o repositório de exemplo.

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app.git
cd flask-postgresql-app
```

Este repositório de exemplo contém uma aplicação [Flask](http://flask.pocoo.org/).

### <a name="run-the-app-locally"></a>Executar a aplicação localmente

Instale os pacotes necessários e inicie a aplicação.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run

# PowerShell
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
Set-Item Env:FLASK_APP ".\app.py"
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Quando a aplicação estiver totalmente carregada, verá algo semelhante à mensagem seguinte:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navegue para `http://localhost:5000` num browser. Clique em **Registar!** e criar um utilizador de teste.

![Aplicação do Python Flask em execução localmente](./media/tutorial-python-postgresql-app/local-app.png)

A aplicação Flask de exemplo armazena os dados do utilizador na base de dados. Se registar um utilizador com êxito, a aplicação escreve dados na base de dados PostgreSQL local.

Para parar o servidor do Flask em qualquer altura, escreva Ctrl+C no terminal.

## <a name="create-a-production-postgresql-database"></a>Criar uma base de dados PostgreSQL de produção

Neste passo, vai criar uma base de dados PostgreSQL no Azure. Quando a aplicação for implementada no Azure, utiliza esta base de dados na cloud.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>Criar uma Base de Dados do Azure para o servidor PostgreSQL

Crie um servidor PostgreSQL com o comando [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) no Cloud Shell.

No seguinte exemplo de linha de comandos, substitua *\<postgresql_name>* por um nome de servidor exclusivo, e substitua *\<admin_username>* e *\<admin_password>* pelas credenciais de utilizador pretendidas. As credenciais de utilizador são para a conta de administrador da base de dados. O nome do servidor é utilizado como parte do ponto final do PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), por isso, o nome tem de ser exclusivo em todos os servidores no Azure.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name B_Gen4_1
```

Após criar o servidor da Base de Dados do Azure para PostgreSQL, a CLI do Azure mostra informações semelhantes ao exemplo seguinte:

```json
{
  "administratorLogin": "<admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> Memorize \<admin_username> e \<admin_password> para utilizar mais tarde. Precisará deles para iniciar sessão no servidor e bases de dados Postgre.


### <a name="create-firewall-rules-for-the-postgresql-server"></a>Criar regras de firewall para o servidor PostgreSQL

No Cloud Shell, execute o seguinte comando da CLI do Azure, para permitir o acesso à base de dados a partir dos recursos do Azure.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Esta definição permite ligações de rede de todos os IPs na rede do Azure. Para utilização em produção, experimente configurar as regras de firewall o mais restritivas possível ao [utilizar apenas os endereços IP de saída que a aplicação utiliza](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

No Cloud Shell, execute o comando novamente para permitir o acesso a partir do seu computador local, substituindo *\<your_ip_address>* pelo [seu endereço IP IPv4 local](http://www.whatsmyip.org/).

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<your_ip_address> --end-ip-address=<your_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Ligar a aplicação Python base de dados de produção

Neste passo, vai ligar a aplicação de exemplo Flask à Base de Dados do Azure para servidor PostgreSQL que criou.

### <a name="create-empty-database-and-user-access"></a>Criar base de dados vazia e acesso de utilizador

Na janela de terminal local, ligue à base de dados ao executar o comando abaixo. Quando a sua palavra-passe de administrador lhe for pedida, utilize a mesma palavra-passe que especificou em [Criar uma Base de Dados do Azure para servidor PostgreSQL](#create-an-azure-database-for-postgresql-server).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Assim como no seu servidor Postgres local, crie a base de dados e um utilizador no servidor Postgres do Azure.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Escreva `\q` para sair do cliente PostgreSQL.

> [!NOTE]
> Uma prática recomendada é criar utilizadores de base de dados com permissões restritas para aplicações específicas, em vez de utilizar o utilizador administrador. Neste exemplo, o utilizador `manager` tem todos os privilégios _só_ para a base de dados `eventregistration`.

### <a name="test-app-connectivity-to-production-database"></a>Testar a conectividade da aplicação à base de dados de produção

Novamente na janela do terminal local, execute os seguintes comandos para executar a migração da base de dados do Flask e o servidor Flask.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Quando a aplicação estiver totalmente carregada, verá algo semelhante à mensagem seguinte:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Navegue para http://localhost:5000 num browser. Clique em **Registar!** e crie um registo de teste. Está agora a escrever dados na base de dados no Azure.

![Aplicação do Python Flask em execução localmente](./media/tutorial-python-postgresql-app/local-app.png)

## <a name="deploy-to-azure"></a>Implementar no Azure

Neste passo, vai implementar a aplicação Python ligada ao Postgres no Serviço de Aplicações do Azure.

### <a name="configure-repository"></a>Configurar o repositório

O motor de implementação do Git no Serviço de Aplicações invoca a automatização `pip` quando existe um _application.py_ na raiz do repositório. Neste tutorial, vai permitir que o motor de implementação execute a automatização por si. Na janela do terminal local, navegue até à raiz do repositório, crie um _application.py_ de teste e consolide as suas alterações.

```bash
cd ..
touch application.py
git add .
git commit -m "ensure azure automation"
```

### <a name="configure-a-deployment-user"></a>Configurar um utilizador de implementação

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações 

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Criar uma aplicação Web 

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Configurar as variáveis de ambiente

No início do tutorial, definiu as variáveis de ambiente para ligar à base de dados PostgreSQL.

No Serviço de Aplicações, as variáveis de ambiente são definidas como _definições da aplicação_ com o comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) no Cloud Shell.

O exemplo seguinte especifica os detalhes de ligação de base de dados, como as definições da aplicação. 

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Enviar para o Azure a partir do Git

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 5, done. 
Delta compression using up to 4 threads. 
Compressing objects: 100% (5/5), done. 
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done. 
Total 5 (delta 3), reused 0 (delta 0) 
remote: Updating branch 'master'. 
remote: Updating submodules. 
remote: Preparing deployment for commit id '6c7c716eee'. 
remote: Running custom deployment command... 
remote: Running deployment command... 
remote: Handling node.js deployment. 
. 
. 
. 
remote: Deployment successful. 
To https://<app_name>.scm.azurewebsites.net/<app_name>.git 
 * [new branch]      master -> master 
```  

### <a name="configure-entry-point"></a>Configurar o ponto de entrada

Por predefinição, a imagem incorporada procura um _wsgi.py_ ou _application.py_ no diretório de raiz como o ponto de entrada, mas o ponto de entrada é _app/app.py_. O _application.py_ adicionado anteriormente está vazio e não faz nada.

No Cloud Shell, execute o comando [`az webapp config set`](/cli/azure/webapp/config?view=azure-cli-latest#az-webapp-config-set) para definir um script de arranque personalizado.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --startup-file "gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/app app:app"
```

O parâmetro `--startup-file` assume um comando personalizado ou o caminho para o ficheiro que contém o comando personalizado. O comando personalizado deve ter o seguinte formato:

```
gunicorn '--bind=0.0.0.0' --chdir /home/site/wwwroot/<subdirectory> <module>:<variable>
```

No comando personalizado, `--chdir` é necessário se o ponto de entrada não estiver no diretório de raiz, e `<subdirectory>` é o subdiretório. `<module>` é o nome do ficheiro _.py_ e `<variable>` é a variável no módulo que representa a sua aplicação Web.

### <a name="browse-to-the-azure-web-app"></a>Navegar para a aplicação Web do Azure

Navegue para a aplicação Web implementada. Demora algum tempo a iniciar porque o contentor precisa de ser transferido e executado quando a aplicação é solicitada pela primeira vez. Se a página exceder o limite de tempo ou apresentar uma mensagem de erro, aguarde alguns minutos e atualize a página.

```bash
http://<app_name>.azurewebsites.net
```

Verá os convidados registados anteriormente que foram guardados na base de dados de produção do Azure no passo anterior.

![Aplicação do Python Flask em execução no Azure](./media/tutorial-python-postgresql-app/docker-app-deployed.png)

**Parabéns!** Está a executar uma aplicação Python no Serviço de Aplicações para Linux.

## <a name="access-diagnostic-logs"></a>Aceder aos registos de diagnósticos

Uma vez que a aplicação Python está em execução num contentor, o Serviço de Aplicações no Linux permite-lhe aceder aos registos da consola gerados no contentor. Para localizar os registos, navegue para este URL:

```
https://<app_name>.scm.azurewebsites.net/api/logs/docker
```

Deverá ver dois objetos JSON, cada um com uma propriedade `href`. Um `href` aponta para os registos de consola do Docker (termina com `_docker.log`) e o outro `href` aponta para os registos da consola gerados no contentor de Python. 

```json
[  
   {  
      "machineName":"RD0003FF61ACD0_default",
      "lastUpdated":"2018-09-27T16:48:17Z",
      "size":4766,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_default_docker.log"
   },
   {  
      "machineName":"RD0003FF61ACD0",
      "lastUpdated":"2018-09-27T16:48:19Z",
      "size":2589,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log",
      "path":"/home/LogFiles/2018_09_27_RD0003FF61ACD0_docker.log"
   }
]
```

Copie o valor `href` pretendido para uma janela do browser, para navegar para os registos. Os registos não foram transmitidos, pelo que pode ocorrer algum atraso. Para ver os registos novos, atualize a página do browser.

## <a name="update-data-model-and-redeploy"></a>Atualizar o modelo de dados e voltar a implementar

Neste passo, vai adicionar o número de participantes de cada registo de evento, ao atualizar o modelo `Guest` e, em seguida, volta a implementar a atualização no Azure.

Na janela de terminal local, confira os ficheiros do ramo `modelChange` com o seguinte comando do git:

```bash
git checkout origin/modelChange -- .
```

Esta verificação já faz as alterações necessárias ao modelo, às vistas e aos controladores. Também inclui uma migração de base de dados gerada através do *alembic* (`flask db migrate`). Pode ver todas as alterações através do seguinte comando do git:

```bash
git diff master origin/modelChange
```

### <a name="test-your-changes-locally"></a>Testar as alterações localmente

Na janela do terminal local, execute os seguintes comandos para testar as suas alterações localmente, mediante a execução do servidor Flask.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navegue até http://localhost:5000 no seu browser para ver as alterações. Crie um registo de teste.

![Aplicação do Python Flask baseada em contentor de Docker, em execução localmente](./media/tutorial-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publicar alterações no Azure

Na janela terminal local, consolide todas as alterações no Git e envie as alterações do código para o Azure.

```bash 
git add . 
git commit -m "updated data model" 
git push azure master 
``` 

Navegue para a aplicação Web do Azure e experimente a nova funcionalidade novamente. Certifique-se de que atualiza a página.

```bash
http://<app_name>.azurewebsites.net
```

![Aplicação do Docker Python Flask no Serviço de Aplicações do Azure](./media/tutorial-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-web-app-in-the-azure-portal"></a>Gerir a aplicação Web no Portal do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para ver a aplicação Web que criou.

No menu à esquerda, clique em **Serviços Aplicacionais** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/tutorial-python-postgresql-app/app-resource.png)

Por predefinição, o portal mostra a página **Descrição Geral** da aplicação Web. Esta página proporciona-lhe uma vista do desempenho da aplicação. Aqui, também pode realizar tarefas de gestão básicas, como navegar, parar, iniciar, reiniciar e eliminar. Os separadores no lado esquerdo da página mostram as várias páginas de configuração que pode abrir.

![Página Serviço de Aplicações no portal do Azure](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma base de dados PostgreSQL no Azure
> * Ligar uma aplicação Python ao PostgreSQL
> * Implementar a aplicação no Azure
> * Ver registos de diagnóstico
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Gerir a aplicação no portal do Azure

Avance para o próximo tutorial para saber como mapear um nome DNS personalizado para a aplicação Web.

> [!div class="nextstepaction"]
> [Configurar a imagem incorporada do Python](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para as Aplicações Web do Azure](../app-service-web-tutorial-custom-domain.md)

