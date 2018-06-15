---
title: Compilar uma aplicação Web em Docker Python e PostgreSQL no Azure | Microsoft Docs
description: Saiba como colocar uma aplicação Docker Python a funcionar no Azure, com ligação a uma base de dados PostgreSQL.
services: app-service\web
documentationcenter: python
author: berndverst
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 01/28/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: 2728c354a84c4b13b0ad8509d038837733251975
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31788437"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Criar uma aplicação Web Docker Python com PostgreSQL no Azure

A Aplicação Web para Contentores fornece um serviço de alojamento na Web altamente dimensionável e com correção automática. Este tutorial mostra como criar uma aplicação Web básica do Docker Python no Azure. Ligue esta aplicação a uma base de dados PostgreSQL. Quando tiver terminado, terá uma aplicação do Python Flask em execução num contentor de Docker no [Serviço de Aplicações no Linux](app-service-linux-intro.md).

![Aplicação do Docker Python Flask no Serviço de Aplicações no Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma base de dados PostgreSQL no Azure
> * Ligar uma aplicação Python ao PostgreSQL
> * Implementar a aplicação no Azure
> * Atualizar o modelo de dados e voltar a implementar a aplicação
> * Gerir a aplicação no portal do Azure

Pode seguir os passos neste artigo no macOS. As instruções do Linux e Windows são as mesmas na maioria dos casos, mas as diferenças não estão descritas neste tutorial.
 
[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

1. [Instalar o Git](https://git-scm.com/)
1. [Instalar o Python](https://www.python.org/downloads/)
1. [Instalar e executar o PostgreSQL](https://www.postgresql.org/download/)
1. [Instalar a Edição de Comunidade do Docker](https://www.docker.com/community-edition)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testar a instalação do PostgreSQL local e criar uma base de dados

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

Execute os seguintes comandos para clonar o repositório de exemplo e ir para a versão *0.1-initialapp*.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Este repositório de exemplo contém uma aplicação [Flask](http://flask.pocoo.org/). 

### <a name="run-the-application"></a>Executar a aplicação

> [!NOTE] 
> Num passo posterior, irá simplificar este processo ao criar um contentor de Docker para utilizar com a base de dados de produção.

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

![Aplicação do Python Flask em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app.png)

A aplicação Flask de exemplo armazena os dados do utilizador na base de dados. Se registar um utilizador com êxito, a aplicação escreve dados na base de dados PostgreSQL local.

Para parar o servidor do Flask em qualquer altura, escreva Ctrl+C no terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-production-postgresql-database"></a>Criar uma base de dados PostgreSQL de produção

Neste passo, vai criar uma base de dados PostgreSQL no Azure. Quando a aplicação for implementada no Azure, utiliza esta base de dados na cloud.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-database-for-postgresql-server"></a>Criar uma Base de Dados do Azure para o servidor PostgreSQL

Crie um servidor PostgreSQL com o comando [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create).

No seguinte comando, substitua um nome de servidor exclusivo para o marcador de posição *\<postgresql_name>* e um nome de utilizador para o marcador de posição *\<admin_username>*. O nome do servidor é utilizado como parte do ponto final do PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), por isso, o nome tem de ser exclusivo em todos os servidores no Azure. O nome de utilizador para a conta de utilizador administrador de base de dados inicial. É-lhe pedido para escolher uma palavra-passe para este utilizador.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>  --storage-size 51200
```

Após criar o servidor da Base de Dados do Azure para PostgreSQL, a CLI do Azure mostra informações semelhantes ao exemplo seguinte:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>Criar uma regra de firewall para a Base de Dados do Azure para PostgreSQL

Execute o seguinte comando da CLI do Azure, para permitir o acesso à base de dados a partir de todos os endereços IP. Quando os IPs inicial e final estão definidos como 0.0.0.0, a firewall apenas é aberta para outros recursos do Azure. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAzureIPs
```

A CLI do Azure confirma a criação da regra de firewall com o resultado semelhante ao seguinte exemplo:

```json
{
  "endIpAddress": "0.0.0.0",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAzureIPs",
  "name": "AllowAzureIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

> [!TIP] 
> Pode ser ainda mais restritivo na sua regra de firewall ao [utilizar apenas os endereços IP de saída que a aplicação utiliza](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

## <a name="connect-your-python-flask-application-to-the-database"></a>Ligar a aplicação Python Flask à base de dados

Neste passo, vai ligar a aplicação de exemplo do Python Flask ao servidor da Base de Dados do Azure para PostgreSQL que criou.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Criar uma base de dados vazia e configurar um novo utilizador de aplicação de base de dados

Criar um utilizador de base de dados com acesso a apenas uma base de dados. Utilize estas credenciais para evitar conceder o acesso total da aplicação ao servidor.

Ligue à base de dados (é-lhe pedida a palavra-passe de administrador).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Criar a base de dados e o utilizador a partir da CLI do PostgreSQL.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Escreva `\q` para sair do cliente PostgreSQL.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Testar a aplicação localmente relativamente à base de dados PostgreSQL do Azure

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

![Aplicação do Python Flask em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>Executar a aplicação a partir de um Contentor de Docker

Criar a imagem de contentor de Docker.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

O Docker apresenta uma confirmação de que criou com êxito o contentor.

```bash
Successfully built 7548f983a36b
```

Na raiz do repositório, adicione um ficheiro de variáveis de ambiente com o nome _db.env_e, em seguida, adicione as seguintes variáveis de ambiente de base de dados ao mesmo. A aplicação estabelece ligação à base de dados de produção da Base de Dados do Azure para PostgreSQL.

```text
DBHOST=<postgresql_name>.postgres.database.azure.com
DBUSER=manager@<postgresql_name>
DBNAME=eventregistration
DBPASS=supersecretpass
```

Execute a aplicação a partir do contentor de Docker. O seguinte comando especifica o ficheiro de variáveis de ambiente e a porta Flask 5000 predefinida é mapeada para a porta local 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

O resultado será semelhante ao que vimos anteriormente. No entanto, a migração de base de dados inicial já não precisa de ser executada e, por conseguinte, é ignorada.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

A base de dados já contém o registo que criou anteriormente.

![Aplicação do Python Flask baseada em contentor de Docker, em execução localmente](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>Carregar o contentor de Docker para um registo de contentor

Neste passo, vai carregar o contentor de Docker para um registo de contentor. Utilize o Azure Container Registry, mas também poderia utilizar outros populares, como o Hub do Docker.

### <a name="create-an-azure-container-registry"></a>Criar um Azure Container Registry

No comando seguinte para criar um registo de contentor, substitua *\<registry_name>* por um nome de registo de contentor do Azure exclusivo, à sua escolha.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Saída

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>Obter as credenciais de registo para emitir e solicitar imagens Docker

Para mostrar as credenciais do registo, ative primeiro o modo de administração.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Vê duas palavras-passe. Tome nota do nome de utilizador e da primeira palavra-passe.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Carregar o contentor de Docker para o Azure Container Registry

Inicie sessão no registo. Quando lhe for pedido, indique a palavra-passe que obteve.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Emitir a imagem do docker para o registo de contentor.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>Implementar a aplicação Docker Python Flask no Azure

Neste passo, vai implementar a aplicação do Python Flask baseada em contentor de Docker para o Serviço de Aplicações do Azure.

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Criar uma aplicação Web

Crie uma aplicação Web no plano do Serviço de Aplicações *myAppServicePlan* com o comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create).

A aplicação Web dá-lhe espaço de alojamento para implementar o seu código, bem como um URL que lhe permite ver a aplicação implementada. Utilize para criar a aplicação Web.

No seguinte comando, substitua o marcador de posição *\<app_name>* por um nome de aplicação exclusivo. Este nome faz parte do URL da aplicação Web, pelo que o nome tem de ser exclusivo relativamente a todas as aplicações no Serviço de Aplicações do Azure.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-container-image-name "<registry_name>.azurecr.io/flask-postgresql-sample"
```

Quando a aplicação Web tiver sido criada, a CLI do Azure mostra informações semelhantes ao seguinte exemplo:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-the-database-environment-variables"></a>Configurar as variáveis de ambiente de base de dados

No início do tutorial, definiu as variáveis de ambiente para ligar à base de dados PostgreSQL.

No Serviço de Aplicações, as variáveis de ambiente são definidas como _definições da aplicação_ com o comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set).

O exemplo seguinte especifica os detalhes de ligação de base de dados, como as definições da aplicação. Também utiliza a variável *PORT* para mapear a PORTA 5000 do Contentor de Docker para receber o tráfego HTTP na PORTA 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Configurar a implementação do contentor de Docker

O Serviço de Aplicações pode transferir e executar um contentor de Docker automaticamente.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Sempre que atualizar o contentor de Docker ou alterar as definições, reinicie a aplicação. Reiniciar assegura que todas as definições são aplicadas e que o contentor mais recente é extraído do registo.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Navegar para a aplicação Web do Azure 

Utilize o browser para navegar para a aplicação Web implementada. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> A aplicação Web demora mais tempo a carregar porque o contentor tem de ser transferido e iniciado depois de a configuração do contentor ser alterada.

Verá os convidados registados anteriormente que foram guardados na base de dados de produção do Azure no passo anterior.

![Aplicação do Python Flask baseada em contentor de Docker, em execução localmente](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Parabéns!** Está a executar uma aplicação do Python Flask baseada no contentor de Docker no Serviço de Aplicações do Azure.

## <a name="update-data-model-and-redeploy"></a>Atualizar o modelo de dados e voltar a implementar

Neste passo, vai adicionar o número de participantes de cada registo de evento, ao atualizar o modelo Convidado.

Veja a versão *0.2-migration* com o seguinte comando do git:

```bash
git checkout tags/0.2-migration
```

Esta versão já aplicou as alterações necessárias às vistas, aos controladores e ao modelo. Também inclui uma migração de base de dados gerada através do *alembic* (`flask db migrate`). Pode ver todas as alterações efetuadas através do seguinte comando do git:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Testar as alterações localmente

Execute os seguintes comandos para testar as suas alterações localmente ao executar o servidor Flask.

```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navegue até http://localhost:5000 no seu browser para ver as alterações. Crie um registo de teste.

![Aplicação do Python Flask baseada em contentor de Docker, em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publicar alterações no Azure

Crie a nova imagem de docker, envie-a para o registo de contentor e reinicie a aplicação.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Navegue para a aplicação Web do Azure e experimente a nova funcionalidade novamente. Crie outro registo de eventos.

```bash 
http://<app_name>.azurewebsites.net 
```

![Aplicação do Docker Python Flask no Serviço de Aplicações do Azure](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para ver a aplicação Web que criou.

No menu à esquerda, clique em **Serviços Aplicacionais** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/tutorial-docker-python-postgresql-app/app-resource.png)

Por predefinição, o portal mostra a página **Descrição Geral** da aplicação Web. Esta página proporciona-lhe uma vista do desempenho da aplicação. Aqui, também pode realizar tarefas de gestão básicas, como navegar, parar, iniciar, reiniciar e eliminar. Os separadores no lado esquerdo da página mostram as várias páginas de configuração que pode abrir.

![Página Serviço de Aplicações no portal do Azure](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Passos seguintes

Avance para o próximo tutorial para saber como mapear um nome DNS personalizado para a aplicação Web.

> [!div class="nextstepaction"]
> [Mapear um nome DNS personalizado existente para as Aplicações Web do Azure](../app-service-web-tutorial-custom-domain.md)
