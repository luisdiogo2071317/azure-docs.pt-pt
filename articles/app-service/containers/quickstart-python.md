---
title: Implementar uma aplicação Python na Aplicação Web para Contentores do Azure
description: Como implementar uma imagem de Docker a executar uma aplicação Python para uma Aplicação Web para Contentores.
keywords: serviço de aplicações do azure, aplicação web, python, docker, contentor
services: app-service
author: cephalin
manager: jeconnoc
ms.service: app-service
ms.devlang: python
ms.topic: quickstart
ms.date: 07/13/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6d328d8a3556f565e7eac8ee079bd191b7dcadef
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433447"
---
# <a name="deploy-a-python-web-app-in-web-app-for-containers"></a>Implementar uma aplicação Web Python na Aplicação Web para Contentores

O [Serviço de Aplicações no Linux](app-service-linux-intro.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática através do sistema operativo Linux. Este início rápido mostra como criar uma aplicação Web e implementar uma aplicação Flask simples na mesma com uma imagem personalizada do Hub do Docker. Irá criar a aplicação Web com a [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![Aplicação de exemplo em execução no Azure](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* <a href="https://git-scm.com/" target="_blank">Instale o Git</a>
* <a href="https://www.docker.com/community-edition" target="_blank">Instalar a Edição de Comunidade do Docker</a>
* <a href="https://hub.docker.com/" target="_blank">Inscrever numa conta do Docker Hub</a>

## <a name="download-the-sample"></a>Transferir o exemplo

Numa janela do terminal, execute os seguintes comandos para clonar o exemplo de aplicação no seu computador local e é navegue para o diretório que contém o código de exemplo.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
cd python-docs-hello-world
```

Este repositório contém uma aplicação Flask simples na pasta _/aplicação_ e um _Dockerfile_ que especifica três itens:

- Utilize a imagem de base [tiangolo/uwsgi-nginx-flask:python3.6-alpine3.7](https://hub.docker.com/r/tiangolo/uwsgi-nginx-flask/).
- O contentor deve escutar na porta 8000.
- Copie o diretório `/app` para o diretório do contentor `/app`.

A configuração segue as [instruções para a imagem de base](https://hub.docker.com/r/tiangolo/uwsgi-nginx-flask/).

## <a name="run-the-app-locally"></a>Executar a aplicação localmente

Execute a aplicação num contentor do Docker.

```bash
docker build --rm -t flask-quickstart .
docker run --rm -it -p 8000:8000 flask-quickstart
```

Abra um browser e navegue para a aplicação de exemplo em `http://localhost:8000`.

Pode ver a mensagem **Hello Wordl** da aplicação de exemplo apresentada na página.

![Aplicação de exemplo em execução localmente](media/quickstart-python/localhost-hello-world-in-browser.png)

Na janela do terminal, prima **Ctrl+C** para parar o contentor.

## <a name="deploy-image-to-docker-hub"></a>Implementar a imagem para o Hub do Docker

Inicie sessão na sua conta do Hub do Docker. Siga o pedido para introduzir as suas credenciais do Hub do Docker.

```bash
docker login
```

Coloque uma etiqueta na imagem e envie-a para um novo repositório _público_ da sua conta do Hub do Docker, para um repositório chamado `flask-quickstart`. Substitua *\<dockerhub_id>* pelo ID do Hub do Docker.

```bash
docker tag flask-quickstart <dockerhub_id>/flask-quickstart
docker push <dockerhub_id>/flask-quickstart
```

> [!NOTE]
> `docker push` cria um repositório público se o repositório especificado não for encontrado. Este início rápido pressupõe um repositório público no Hub do Docker. Se preferir enviar para um repositório privado, terá de configurar as credenciais do Hub do Docker no Serviço de Aplicações do Azure mais tarde. Veja [Criar uma aplicação Web](#create-a-web-app).

Assim que o envio da imagem for concluído, está pronto para utilizá-lo na sua aplicação Web do Azure.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Criar uma aplicação Web

Crie uma [aplicação Web](../app-service-web-overview.md) no plano do `myAppServicePlan`Serviço de Aplicações com o comando[az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Substitua *\<app name>* por um nome de aplicação globalmente exclusivo e substitua *\<dockerhub_id>* pelo ID do Hub do Docker.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name <dockerhub_id>/flask-quickstart
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
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Se tiver carregado para um repositório privado anteriormente, também terá de configurar as credenciais do Hub do Docker no Serviço de Aplicações. Para obter mais informações, veja [Utilizar uma imagem privada do Hub do Docker](tutorial-custom-docker-image.md#use-a-private-image-from-docker-hub-optional).

### <a name="specify-container-port"></a>Especifique a porta do contentor

Conforme especificado no _Dockerfile_, o seu contentor escuta na porta 8000. Para o Serviço de Aplicações encaminhar o seu pedido para a porta à direita, tem de definir a *WEBSITES_PORT* na aplicação.

Na Cloud Shell, execute o comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set).


```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings WEBSITES_PORT=8000
```

## <a name="browse-to-the-app"></a>Navegar para a aplicação

```bash
http://<app_name>.azurewebsites.net/
```

![Aplicação de exemplo em execução no Azure](media/quickstart-python/hello-world-in-browser.png)

> [!NOTE]
> A aplicação Web demora algum tempo a iniciar porque a imagem do Hub do Docker tem de ser transferida e executada quando a aplicação é pedida pela primeira vez. Se, incialmente, vir um erro após um longo período de tempo, atualize a página.

**Parabéns!** Implementou uma imagem personalizada do Docker a executar uma aplicação Python para uma Aplicação Web para Contentores.

## <a name="update-locally-and-redeploy"></a>Atualizar localmente e reimplementar

Através de um editor de texto local, abra o ficheiro `app/main.py` na aplicação Python e faça uma pequena alteração ao texto junto à declaração `return`:

```python
return 'Hello, Azure!'
```

Recriar a imagem e enviá-la novamente para o Hub do Docker.

```bash
docker build --rm -t flask-quickstart .
docker tag flask-quickstart <dockerhub_id>/flask-quickstart
docker push <dockerhub_id>/flask-quickstart
```

No Cloud Shell, reinicie a aplicação. Reiniciar a aplicação assegura que todas as definições são aplicadas e que o contentor mais recente é extraído do registo.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

Aguarde cerca de 15 segundos para o Serviço de Aplicações pedir a imagem atualizada. Volte para a janela do browser aberta que abriu no passo **Navegar para a aplicação** e atualize a página.

![Aplicação de exemplo atualizada em execução no Azure](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para ver a aplicação Web que criou.

No menu à esquerda, clique em **Serviços Aplicacionais** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/quickstart-python/app-service-list.png)

Por predefinição, o portal mostra a página **Descrição Geral** da aplicação Web. Esta página proporciona-lhe uma vista do desempenho da aplicação. Aqui, também pode realizar tarefas de gestão básicas, como navegar, parar, iniciar, reiniciar e eliminar. Os separadores no lado esquerdo da página mostram as várias páginas de configuração que pode abrir.

![Página Serviço de Aplicações no portal do Azure](./media/quickstart-python/app-service-detail.png)

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Python com PostgreSQL](tutorial-docker-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Utilizar imagens personalizadas](tutorial-custom-docker-image.md)