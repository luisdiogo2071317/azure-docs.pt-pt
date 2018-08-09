---
title: Compilar uma aplicação Web em Python e PostgreSQL no Serviço de Aplicações do Azure | Microsoft Docs
description: Saiba como executar uma aplicação Python condicionada por dados no Azure, com ligação a uma base de dados PostgreSQL.
services: app-service\web
documentationcenter: python
author: berndverst
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: beverst;cephalin
ms.custom: mvc
ms.openlocfilehash: ce84498ab89891bd7b96cfcc6b0c7ac029c93cbd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423084"
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Criar uma aplicação Web Docker Python com PostgreSQL no Azure

A Aplicação Web para Contentores fornece um serviço de alojamento na Web altamente dimensionável e com correção automática. Este tutorial mostra como criar uma aplicação Web em Python condicionada por dados, com o PostgreSQL como o back-end da base de dados. Quando tiver terminado, terá uma aplicação do Python Flask em execução num contentor de Docker no [Serviço de Aplicações no Linux](app-service-linux-intro.md).

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

Numa janela do terminal local, execute `psql` para ligar ao seu servidor PostgreSQL local.

```bash
sudo -u postgres psql
```

Se a ligação for bem-sucedida, a base de dados PostgreSQL está em execução. Caso contrário, certifique-se de que a base de dados PostgresQL local é iniciada conforme os passos indicados em [Transferências - Distribuição do PostgreSQL Core](https://www.postgresql.org/download/).

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

Execute os seguintes comandos para clonar o repositório de exemplo e ir para a versão *0.1-initialapp*.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Este repositório de exemplo contém uma aplicação [Flask](http://flask.pocoo.org/). 

### <a name="run-the-app-locally"></a>Executar a aplicação localmente

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

Crie um servidor PostgreSQL com o comando [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) no Cloud Shell.

No seguinte exemplo de linha de comandos, substitua *\<postgresql_name>* por um nome de servidor exclusivo, e substitua *\<admin_username>* e *\<admin_password>* pelas credenciais de utilizador pretendidas. O nome do servidor é utilizado como parte do ponto final do PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), por isso, o nome tem de ser exclusivo em todos os servidores no Azure. As credenciais de utilizador são para a conta de utilizador administrador da base de dados. 

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --location "West Europe" --admin-user <admin_username> --admin-password <admin_password> --sku-name GP_Gen4_2
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

### <a name="create-a-firewall-rule-for-the-postgresql-server"></a>Criar uma regra de firewall para o servidor PostgreSQL

No Cloud Shell, execute o seguinte comando da CLI do Azure, para permitir o acesso à base de dados a partir de todos os endereços IP. Quando os IPs inicial e final estão definidos como `0.0.0.0`, a firewall é aberta apenas para outros recursos do Azure. 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAzureIPs
```

> [!TIP] 
> Pode ser ainda mais restritivo na sua regra de firewall ao [utilizar apenas os endereços IP de saída que a aplicação utiliza](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

No Cloud Shell, execute o comando novamente para permitir o acesso à base de dados a partir do seu computador local, substituindo *\<you_ip_address>* pelo [seu endereço IP IPv4 local](https://whatismyipaddress.com/). 

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=<you_ip_address> --end-ip-address=<you_ip_address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Ligar a aplicação Python base de dados de produção

Neste passo, vai ligar a aplicação de exemplo Flask à Base de Dados do Azure para servidor PostgreSQL que criou.

### <a name="create-empty-database-and-user-access"></a>Criar base de dados vazia e acesso de utilizador

No Cloud Shell, ligue à base de dados ao executar `psql`. Quando a sua palavra-passe de administrador lhe for pedida, utilize a mesma palavra-passe que especificou em [Criar uma Base de Dados do Azure para servidor PostgreSQL](#create-an-azure-database-for-postgresql-server).

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

![Aplicação do Python Flask em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app.png)

## <a name="upload-app-to-a-container-registry"></a>Carregar a aplicação para um registo de contentor

Neste passo, vai criar uma imagem do Docker e carregá-la para o Azure Container Registry. Também pode utilizar registos populares, como o Hub do Docker.

### <a name="build-the-docker-image-and-test-it"></a>Criar a imagem do Docker e testá-la

Na janela do terminal local, crie a imagem do Docker.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

O Docker apresenta uma confirmação de que criou com êxito a imagem.

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

Execute a imagem localmente num contentor do Docker. O seguinte comando especifica o ficheiro de variáveis de ambiente e a porta Flask 5000 predefinida é mapeada para a porta local 5000.

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

Agora que verificou que o contentor funciona localmente, elimine _db.env_. No Serviço de Aplicações do Azure, irá utilizar as definições da aplicação para definir as variáveis de ambiente.  

### <a name="create-an-azure-container-registry"></a>Criar um Azure Container Registry

No Cloud Shell, crie um registo no Azure Container Registry com o seguinte comando. Substitua *\<registry_name>* por um nome de registo exclusivo.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

### <a name="retrieve-registry-credentials"></a>Obter credenciais de registo

No Cloud Shell, execute os seguintes comandos para obter as credenciais de registo. Precisa delas para enviar e extrair as imagens.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Na saída, vê duas palavras-passe. Anote o nome de utilizador (que é o nome do registo por predefinição) e a primeira palavra-passe.

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

### <a name="upload-docker-image-to-registry"></a>Carregar imagem do Docker para registo

Na janela do terminal local, inicie sessão no seu novo registo com `docker`. Quando lhe for pedido, indique a palavra-passe que obteve.

```bash
docker login <registry_name>.azurecr.io -u <registry_name>
```

Enviar a imagem do Docker para o registo.

```bash
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="create-web-app-with-uploaded-image"></a>Criar aplicação Web com imagem carregada

Neste passo, vai criar uma aplicação no Serviço de Aplicações do Azure e configurá-la para utilizar a imagem carregada do Docker no Azure Container Registry.

### <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Criar uma aplicação Web

No Cloud Shell, crie uma aplicação Web no plano do Serviço de Aplicações *myAppServicePlan* com o comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create).

No seguinte comando, substitua o marcador de posição *\<app_name>* por um nome de aplicação exclusivo. Este nome faz parte do URL da aplicação Web, pelo que o nome tem de ser exclusivo relativamente a todas as aplicações no Serviço de Aplicações do Azure.

```azurecli-interactive
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

### <a name="configure-environment-variables"></a>Configurar as variáveis de ambiente

No início do tutorial, definiu as variáveis de ambiente para ligar à base de dados PostgreSQL.

No Serviço de Aplicações, as variáveis de ambiente são definidas como _definições da aplicação_ com o comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set).

O exemplo seguinte especifica os detalhes de ligação de base de dados, como as definições da aplicação. Também utiliza a variável *WEBSITES_PORT* para a porta de contentor 5000, o que permite ao contentor receber tráfego HTTP na porta 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" WEBSITES_PORT=5000
```

### <a name="configure-custom-container-deployment"></a>Configurar implementação de contentor personalizada

Embora já tenha especificado o nome da imagem de contentor, ainda tem de especificar o URL de registo personalizado e as credenciais de utilizador. No Cloud Shell, execute o comando [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set).

```azurecli-interactive
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

No Cloud Shell, reinicie a aplicação. Reiniciar assegura que todas as definições são aplicadas e que o contentor mais recente é extraído do registo.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Navegar para a aplicação Web do Azure 

Navegue para a aplicação Web implementada. 

```bash 
http://<app_name>.azurewebsites.net 
```

> [!NOTE]
> A aplicação Web demora algum tempo a iniciar porque o contentor tem de ser transferido e executado quando a aplicação é solicitada pela primeira vez. Se, incialmente, vir um erro após um longo período de tempo, atualize a página.

Verá os convidados registados anteriormente que foram guardados na base de dados de produção do Azure no passo anterior.

![Aplicação do Python Flask baseada em contentor de Docker, em execução localmente](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Parabéns!** Está a executar uma aplicação Python na Aplicação Web para Contentores.

## <a name="update-data-model-and-redeploy"></a>Atualizar o modelo de dados e voltar a implementar

Neste passo, adiciona o número de participantes de cada registo de evento, ao atualizar o modelo `Guest`.

Na janela do terminal local, veja a versão *0.2-migration* com o seguinte comando do git:

```bash
git checkout tags/0.2-migration
```

Esta versão já aplicou as alterações necessárias ao modelo, às vistas e aos controladores. Também inclui uma migração de base de dados gerada através do *alembic* (`flask db migrate`). Pode ver todas as alterações efetuadas através do seguinte comando do git:

```bash
git diff 0.1-initialapp 0.2-migration
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

![Aplicação do Python Flask baseada em contentor de Docker, em execução localmente](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publicar alterações no Azure

Na janela do terminal local, crie a nova imagem do docker e envie-a para o seu registo.

```bash
cd ..
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

No Cloud Shell, reinicie a aplicação para certificar-se de que o contentor mais recente é extraído do registo.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
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
