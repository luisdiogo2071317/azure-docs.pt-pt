---
title: Criar uma Aplicação Ruby e implementar no Serviço de Aplicações no Linux | Microsoft Docs
description: Saiba como criar aplicações Ruby com o Serviço de Aplicações no Linux.
keywords: serviço de aplicações do azure, linux, oss, ruby
services: app-service
documentationcenter: ''
author: SyntaxC4
manager: cfowler
editor: ''
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 6668f02bb7ac9588e1bb11b3848d0a3e25cbed67
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Criar uma Aplicação Ruby no Serviço de Aplicações no Linux

[O Serviço de Aplicações no Linux](app-service-linux-intro.md) oferece um serviço de alojamento na Web altamente dimensionável e com correção automática. Este guia de introdução mostra como criar uma aplicação básica no Ruby on Rails e, em seguida, implementá-la no Azure como uma Aplicação Web no Linux.

![Hello-world](./media/quickstart-ruby/hello-world-updated.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Instalar Ruby 2.4.1 ou superior</a>
* <a href="https://git-scm.com/" target="_blank">Instale o Git</a>.

## <a name="download-the-sample"></a>Transferir o exemplo

Numa janela do terminal, execute o seguinte comando para clonar o repositório da aplicação de exemplo para o seu computador local:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Executar a aplicação localmente

Execute o servidor de rails para a aplicação funcionar. Mude para o diretório *hello-world* e o comando `rails server` inicia o servidor.

```bash
cd hello-world\bin
rails server
```

Com o seu browser, navegue para `http://localhost:3000` para testar a aplicação localmente.

![Hello-world](./media/quickstart-ruby/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Modificar a aplicação para apresentar a mensagem de boas-vindas

Modifique a aplicação para apresentar uma mensagem de boas-vindas. Em primeiro lugar, tem de configurar uma rota ao modificar o ficheiro *~/workspace/ruby-docs-hello-world/config/routes.rb* para incluir uma rota chamada `hello`.

  ```ruby
  Rails.application.routes.draw do
      #For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
      root 'application#hello'
  end
  ```

Altere o controlador da aplicação, de modo a que devolva a mensagem como HTML no browser. 

Abra *~/workspace/hello-world/app/controllers/application_controller.rb* para editar. Modifique a classe `ApplicationController` para se parecer ao exemplo de código seguinte:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

A sua aplicação está agora configurada. No browser, navegue para `http://localhost:3000` para confirmar a página de destino de raiz.

![Hello World configurado](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Criar uma aplicação Web

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-ruby-linux-no-h.md)] 

Navegue para o site para ver a sua aplicação Web criada recentemente com imagem incorporada. Substitua _&lt;nome da aplicação>_ pelo nome da sua aplicação Web.

```bash
http://<app_name>.azurewebsites.net
```

A aplicação Web deve ter o seguinte aspeto:

![Página inicial](./media/quickstart-ruby/splash-page.png)

## <a name="deploy-your-application"></a>Implementar a sua aplicação

Execute os seguintes comandos para implementar a aplicação local no seu site do Azure:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Certifique-se de que as operações de implementação remota são bem-sucedidas. Os comandos produzem um resultado semelhante ao seguinte texto:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Assim que a implementação é concluída, reinicie a aplicação Web para a implementação ter efeito, ao utilizar o comando [`az webapp restart`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_restart), conforme mostrado aqui:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Navegue para o site e verifique os resultados.

```bash
http://<app name>.azurewebsites.net
```

![aplicação Web atualizada](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Enquanto a aplicação está a reiniciar, tente procurar os resultados do site num código de estado HTTP `Error 503 Server unavailable`. Pode demorar alguns minutos a reiniciar completamente.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Ruby no Rails com MySQL](tutorial-ruby-mysql-app.md)
