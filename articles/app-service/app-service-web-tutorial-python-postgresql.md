---
title: Compilar uma aplicação Web em Python e PostgreSQL no Azure | Microsoft Docs
description: Saiba como colocar uma aplicação Python a funcionar no Azure, com ligação a uma base de dados PostgreSQL.
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/25/2018
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: 49ec67d06446d6c48e45aef90e2bd528a1b541a9
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
ms.locfileid: "33203515"
---
# <a name="tutorial-build-a-python-and-postgresql-web-app-in-azure"></a>Tutorial: Compilar uma aplicação Web em Python e PostgreSQL no Azure

> [!NOTE]
> Este artigo implementa uma aplicação no Serviço de Aplicações no Windows. Para implementar no Serviço de Aplicações no _Linux_, veja [Compilar uma aplicação Web do Docker Python e PostgreSQL no Azure](./containers/tutorial-docker-python-postgresql-app.md).
>

O [Serviço de Aplicações do Azure](app-service-web-overview.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática. Este tutorial mostra como criar uma aplicação Web básica do Python no Azure. Ligue esta aplicação a uma base de dados PostgreSQL. Quando tiver terminado, terá uma aplicação do Python Flask em execução no Serviço de Aplicações.

![Aplicação do Python Flask no Serviço de Aplicações no Linux](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma base de dados PostgreSQL no Azure
> * Ligar uma aplicação Python ao MySQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Gerir a aplicação no portal do Azure

Pode seguir os passos neste tutorial no macOS. As instruções do Linux e Windows são as mesmas na maioria dos casos, mas as diferenças não estão descritas neste tutorial.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

1. [Instalar o Git](https://git-scm.com/)
1. [Instalar o Python](https://www.python.org/downloads/)
1. [Instalar e executar o PostgreSQL](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testar instalação do PostgreSQL local e criar uma base de dados

Abra a janela do terminal e execute `psql` para ligar ao seu servidor do PostgreSQL local.

```bash
sudo -u postgres psql
```

Se a ligação for bem-sucedida, a base de dados PostgreSQL está em execução. Caso contrário, certifique-se de que a base de dados PostgresQL local é iniciada conforme os passos indicados em [Transferências - Distribuição do PostgreSQL Core](https://www.postgresql.org/download/).

Crie uma base de dados denominada *eventregistration* e configure um utilizador de base de dados em separado denominado *gestor* com a palavra-passe *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Escreva `\q` para sair do cliente PostgreSQL. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Criar uma aplicação local do Python Flask

Neste passo, vai configurar o projeto Python Flask local.

### <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Abra a janela de terminal e `CD` num diretório de trabalho.

Execute os seguintes comandos para clonar o repositório de exemplo e, em seguida, reverta para a consolidação da aplicação inicial (antes `modelChange`).

```bash
git clone https://github.com/Azure-Samples/flask-postgresql-app
cd flask-postgresql-app
git revert modelChange --no-edit
```

Este repositório de exemplo contém uma aplicação [Flask](http://flask.pocoo.org/). 

### <a name="run-the-application"></a>Executar a aplicação

Instale os pacotes necessários e inicie a aplicação.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
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

![Aplicação do Python Flask em execução localmente](./media/app-service-web-tutorial-python-postgresql/local-app.png)

A aplicação Flask de exemplo armazena os dados do utilizador na base de dados. Se registar um utilizador com êxito, a aplicação escreve dados na base de dados PostgreSQL local.

Para parar o servidor do Flask em qualquer altura, escreva Ctrl+C no terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-postgresql-in-azure"></a>Criar PostgreSQL no Azure

Neste passo, vai criar uma base de dados PostgreSQL no Azure. Quando a aplicação for implementada no Azure, utiliza esta base de dados na cloud.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-postgresql-server"></a>Criar um servidor PostgreSQL

Crie um servidor PostgreSQL com o comando [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create).

No seguinte comando, substitua um nome de servidor exclusivo para o marcador de posição *\<postgresql_name>*, um nome de utilizador para o marcador de posição *\<admin_username>* e uma palavra-passe para o marcador de posição *\<admin_password>*. O nome do servidor é utilizado como parte do ponto final do PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), por isso, o nome tem de ser exclusivo em todos os servidores no Azure.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <server_admin_password> --sku-name GP_Gen4_2
```

Após criar o servidor da Base de Dados do Azure para PostgreSQL, a CLI do Azure mostra informações semelhantes ao exemplo seguinte:

```json
{
  "location": "westeurope",
  "name": "<postgresql_server_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "additionalProperties": {},
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  ...   +  
  -  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Configurar firewall do servidor

Execute o seguinte comando da CLI do Azure, para permitir o acesso à base de dados a partir de todos os endereços IP. Quando os IPs inicial e final estão definidos como 0.0.0.0, a firewall apenas é aberta para outros recursos do Azure. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0 --name AllowAzureIPs
```

A CLI do Azure confirma a criação da regra de firewall com o resultado semelhante ao seguinte exemplo:

```json
{
  "additionalProperties": {},
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
 "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

> [!TIP] 
> Pode ser ainda mais restritivo na sua regra de firewall ao [utilizar apenas os endereços IP de saída que a aplicação utiliza](app-service-ip-addresses.md#find-outbound-ips).
>

### <a name="create-a-production-database-and-user"></a>Criar uma base de dados de produção e de utilizador

Criar um utilizador de base de dados com acesso a apenas uma base de dados. Utilize estas credenciais para evitar conceder o acesso total da aplicação ao servidor.

Ligue à base de dados (é-lhe pedida a palavra-passe de administrador).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <admin_username>@<postgresql_name> postgres
```

Criar a base de dados e o utilizador a partir da CLI do PostgreSQL.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Escreva `\q` para sair do cliente PostgreSQL.

### <a name="test-app-locally-with-azure-postgresql"></a>Testar a aplicação localmente com o PostgreSQL do Azure

Volte agora para a pasta *aplicação* do repositório do Github clonado, pode executar a aplicação do Python Flask ao atualizar as variáveis de ambiente da base de dados.

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

![Aplicação do Python Flask em execução localmente](./media/app-service-web-tutorial-python-postgresql/local-app.png)

## <a name="deploy-to-azure"></a>Implementar no Azure

Neste passo, implementa a aplicação Python ligada ao PostgreSQL no Serviço de Aplicações do Azure.

O repositório de Git já contém os seguintes ficheiros que precisa para executar a aplicação Web do Flask no Serviço de Aplicações:

- `.deployment`: especifica o script de implementação personalizado a executar.
- `deploy.cmd`: o script de implementação. É onde `pip install` é executado.
- `web.config`: especifica o script do ponto de entrada para executar num `httpPlatformHandler` no IIS.
- `run_waitress_server.py`: o script do ponto de entrada. Inicia a aplicação Web do Flask num servidor [`waitress`](https://docs.pylonsproject.org/projects/waitress).

### <a name="configure-a-deployment-user"></a>Configurar um utilizador de implementação

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

<a name="create"></a>
### <a name="create-a-web-app"></a>Criar uma aplicação Web

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)] 

### <a name="install-python"></a>Instalar o Python

Neste passo, instala o Python 3.6.2 com [extensões de site](https://www.siteextensions.net/packages?q=Tags%3A%22python%22) no Serviço de Aplicações. Irá utilizar as credenciais que configurou em [Configurar um utilizador de implementação](#configure-a-deployment-user) para autenticar relativamente ao ponto final REST.

No Cloud Shell, execute o comando seguinte para obter as informações de pacote do Python 3.6.2. Substitua *\<deployment_user>* pelo nome de utilizador de implementação configurado e *\<app_name* pelo nome da aplicação. Quando lhe for pedido, utilize a palavra-passe de implementação que configurou.

```bash
packageinfo=$(curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/extensionfeed/python362x86)
```

No Cloud Shell, execute o comando seguinte para instalar o pacote do Python. Substitua *\<deployment_user>* pelo nome de utilizador de implementação configurado e *\<app_name* pelo nome da aplicação. Quando lhe for pedido, utilize a palavra-passe de implementação que configurou.

```bash
curl -X PUT -u <deployment_user> -H "Content-Type: application/json" -d '$packageinfo' https://<app_name>.scm.azurewebsites.net/api/siteextensions/python362x86
```

Na saída do comando, pode ver que o Python está instalado no caminho `D:\home\python362x86\python.exe`.

### <a name="configure-database-settings"></a>Configurar as definições da base de dados

No início do tutorial, definiu as variáveis de ambiente para ligar à base de dados PostgreSQL.

No Serviço de Aplicações, as variáveis de ambiente são definidas como _definições da aplicação_ com o comando [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set).

O exemplo seguinte especifica os detalhes de ligação de base de dados, como as definições da aplicação. Substitua *\<app_name>* pelo nome da aplicação e *\<postgresql_name>* pelo nome do servidor PostgreSQL.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration"
```

### <a name="push-to-azure-from-git"></a>Enviar para o Azure a partir do Git

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

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

### <a name="browse-to-the-azure-web-app"></a>Navegar para a aplicação Web do Azure 

Utilize o browser para navegar para a aplicação Web implementada. 

```bash 
http://<app_name>.azurewebsites.net 
```

Verá os convidados registados anteriormente que foram guardados na base de dados de produção do Azure no passo anterior.

![Aplicação do Python Flask em execução localmente](./media/app-service-web-tutorial-python-postgresql/docker-app-deployed.png)

**Parabéns!** Está a executar uma aplicação do Python Flask no Serviço de Aplicações do Azure.

## <a name="update-data-model-and-redeploy"></a>Atualizar o modelo de dados e voltar a implementar

Neste passo, adiciona o número de participantes de cada registo de evento, ao atualizar o modelo `Guest`.

Veja os ficheiros marcados pela consolidação `modelChange`:

```bash
git checkout modelChange -- *
```

Esta versão já aplicou as alterações necessárias para as vistas, os controladores e o modelo. Também inclui uma migração de base de dados gerada através do *alembic* (`flask db migrate`). Pode ver as alterações em todos os ficheiros na [Vista de consolidação do GitHub](https://github.com/Azure-Samples/flask-postgresql-app/commit/139a53023688631c3cc2caefd70086f4722ecd7e).

### <a name="test-your-changes-locally"></a>Testar as alterações localmente

Execute os seguintes comandos para testar as suas alterações localmente ao executar o servidor Flask.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navegue até http://localhost:5000 no seu browser para ver as alterações. Crie um registo de teste.

![Aplicação do Python Flask em execução localmente](./media/app-service-web-tutorial-python-postgresql/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publicar alterações no Azure

Na janela terminal local, consolide todas as alterações no Git e envie as alterações do código para o Azure.

```bash
git add .
git commit -m "updated data model"
git push azure master
```

Navegue para a aplicação Web do Azure e experimente a nova funcionalidade novamente. Crie outro registo de eventos.

```bash 
http://<app_name>.azurewebsites.net 
```

![Aplicação do Python Flask no Serviço de Aplicações do Azure](./media/app-service-web-tutorial-python-postgresql/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para ver a aplicação Web que criou.

No menu à esquerda, clique em **Serviços Aplicacionais** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/app-service-web-tutorial-python-postgresql/app-resource.png)

Por predefinição, o portal mostra a página **Descrição Geral** da aplicação Web. Esta página proporciona-lhe uma vista do desempenho da aplicação. Aqui, também pode realizar tarefas de gestão básicas, como navegar, parar, iniciar, reiniciar e eliminar. Os separadores no lado esquerdo da página mostram as várias páginas de configuração que pode abrir.

![Página Serviço de Aplicações no portal do Azure](./media/app-service-web-tutorial-python-postgresql/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Passos seguintes

Avance para o próximo tutorial para saber como mapear um nome DNS personalizado para a aplicação Web.

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para as Aplicações Web do Azure](app-service-web-tutorial-custom-domain.md)
