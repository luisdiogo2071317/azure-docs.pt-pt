---
title: Executar a CLI do Azure com o Jenkins | Documentos da Microsoft
description: Saiba como utilizar a CLI do Azure para implementar uma aplicação web Java no Azure no Pipeline do Jenkins
services: app-service\web
documentationcenter: ''
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 1796e9f76e39334c8bbdd03463a0f91e9b47cb17
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421309"
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>Implementar no serviço de aplicações do Azure com o Jenkins e a CLI do Azure
Para implementar uma aplicação web Java no Azure, pode utilizar o CLI do Azure no [Jenkins Pipeline](https://jenkins.io/doc/book/pipeline/). Neste tutorial, vai criar um pipeline de CI/CD numa VM do Azure, incluindo como:

> [!div class="checklist"]
> * Criar uma VM do Jenkins
> * Configurar o Jenkins
> * Criar uma aplicação web no Azure
> * Preparar um repositório do GitHub
> * Criar o pipeline do Jenkins
> * Execução do pipeline e certifique-se a aplicação web

Este tutorial requer a versão do módulo 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Criar e configurar o Jenkins instância
Se ainda não tiver um Jenkins master, começar com o [modelo de solução](install-jenkins-solution-template.md), que inclui o necessária [credenciais do Azure](https://plugins.jenkins.io/azure-credentials) Plug-in, por predefinição. 

O plug-in de credenciais do Azure permite-lhe armazenar credenciais de principal de serviço do Microsoft Azure no Jenkins. Na versão 1.2, adicionámos o suporte para que esse Jenkins Pipeline possa obter as credenciais do Azure. 

Certifique-se de que tem a versão 1.2 ou posterior:
* No painel do Jenkins, clique em **gerir Jenkins -> Gestor de plug-in ->** e procure **credenciais do Azure**. 
* Atualize o plug-in se a versão é anterior ao 1.2.

JDK do Java e Maven, também são necessários no Jenkins master. Para instalar, inicie sessão no mestre do Jenkins através de SSH e execute os seguintes comandos:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a>Adicionar principal de serviço do Azure para Jenkins credenciais

Uma credencial do Azure é necessário para executar a CLI do Azure.

* No painel do Jenkins, clique em **credenciais -> sistema ->**. Clique em **Global credentials(unrestricted)**.
* Clique em **adicionar credenciais** para adicionar um [principal de serviço do Microsoft Azure](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) ao preencher o ID de subscrição, o ID de cliente, o segredo do cliente e o ponto final de Token do OAuth 2.0. Forneça um ID para utilização num passo subsequente.

![Adicionar credenciais](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Criar um serviço de aplicações do Azure para implementar a aplicação web de Java

Crie um plano de serviço de aplicações do Azure com o **gratuito** usando o escalão de preço a [criar plano de serviço de aplicações de az](/cli/azure/appservice/plan#az-appservice-plan-create) comando da CLI. O plano do serviço de aplicações define os recursos físicos utilizados para alojar as suas aplicações. Todas as aplicações atribuídas a um plano do serviço de Aplicações partilham estes recursos, o que lhe permite economizar quando alojar várias aplicações. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Quando o plano estiver pronto, a CLI do Azure mostra um resultado semelhante ao seguinte exemplo:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Criar uma aplicação Web do Azure

 Utilize o [criar az webapp](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) comando da CLI para criar uma definição de aplicação web no `myAppServicePlan` plano do serviço de aplicações. A definição da aplicação Web fornece um URL com o qual pode aceder à sua aplicação e configura várias opções para implementar o código no Azure. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Substitua o marcador de posição `<app_name>` pelo nome exclusivo da sua aplicação. Este nome exclusivo faz parte do nome de domínio predefinido da aplicação Web, pelo que o nome tem de ser exclusivo relativamente a todas as aplicações no Azure. Pode mapear qualquer entrada de nome de domínio personalizada para a aplicação Web antes de a expor aos utilizadores.

Quando a definição de aplicação Web estiver pronta, a CLI do Azure mostra informações semelhantes ao seguinte exemplo: 

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

### <a name="configure-java"></a>Configurar o Java 

Definir a configuração de tempo de execução do Java que a sua aplicação precisa com o [atualização do az appservice web config](/cli/azure/appservice/web/config#az-appservice-web-config-update) comando.

O comando seguinte configura a aplicação Web para ser executada num JDK recente do Java 8 e [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>Preparar um repositório do GitHub
Abra o [aplicação Web Java simples para o Azure](https://github.com/azure-devops/javawebappsample) repositório. Para bifurcar o repositório para sua própria conta GitHub, clique nas **Fork** botão no canto superior direito.

* Na IU do GitHub web, abra **Jenkinsfile** ficheiro. Clique no ícone de lápis para editar este ficheiro para atualizar o grupo de recursos e o nome da sua aplicação web na linha 20 e 21, respetivamente.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Altere a linha 23 para atualizar o ID de credencial na instância do Jenkins

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Criar o pipeline do Jenkins
Abra o Jenkins num browser, clique em **Novo Item**. 

* Forneça um nome para a tarefa e selecione **Pipeline**. Clique em **OK**.
* Clique nas **Pipeline** separador, em seguida. 
* Para **definição**, selecione **Pipeline de script do SCM**.
* Para **SCM**, selecione **Git**.
* Introduza o URL do GitHub para o seu repositório bifurcado: https:\<seu repositório bifurcado\>. Git
* Clicar em **Guardar**

## <a name="test-your-pipeline"></a>Testar o pipeline
* Vá para o pipeline que criou, clique em **compilar agora**
* Uma compilação deve ter êxito em poucos segundos, e pode ir para a compilação e clique em **saída da consola** para ver os detalhes

## <a name="verify-your-web-app"></a>Certifique-se a sua aplicação web
Para verificar o WAR o ficheiro é implementado com êxito à sua aplicação web. Abra um navegador da web:

* Aceda a http://&lt;app_name >.azurewebsites.net/api/calculator/ping  
Verá:

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Aceda a http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (substitua &lt;x > e &lt;y > com quaisquer números) para obter a soma de x e y

![Calculadora: adicionar](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a>Implementar aplicação Web do Azure no Linux
Agora que sabe como utilizar a CLI do Azure no seu pipeline de Jenkins, pode modificar o script para implementar uma aplicação Web do Azure no Linux.

A aplicação Web no Linux suporta de forma diferente para efetuar a implementação, que é utilizar o Docker. Para implementar, terá de fornecer um Dockerfile que empacota a sua aplicação web com o tempo de execução do serviço numa imagem do Docker. O plug-in, em seguida, irá criar a imagem, enviá-la para um registo do Docker e implantar a imagem à sua aplicação web.

* Siga os passos [aqui](../app-service/containers/quickstart-nodejs.md) para criar uma aplicação Web do Azure em execução no Linux.
* Instalar o Docker na sua instância do Jenkins, seguindo as instruções neste [artigo](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Criar um registo de contentor no portal do Azure, utilizando os passos [aqui](/azure/container-registry/container-registry-get-started-azure-cli).
* Da mesma [aplicação Web Java simples para o Azure](https://github.com/azure-devops/javawebappsample) repositório bifurcou, edite a **Jenkinsfile2** ficheiro:
    * Linha 18 21, atualizar os nomes do seu grupo de recursos, a aplicação web e o ACR, respetivamente. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Linha 24, update \<azsrvprincipal\> ao seu ID de credencial
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Criar um novo pipeline do Jenkins, como ao implementar a aplicação web do Azure no Windows, só que desta vez, utilizou **Jenkinsfile2** em vez disso.
* Execute o seu novo trabalho.
* Para verificar, na CLI do Azure, execute:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Obtém o resultado seguinte:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Aceda a http://&lt;app_name >.azurewebsites.net/api/calculator/ping. Verá a mensagem: 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Aceda a http://&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (substitua &lt;x > e &lt;y > com quaisquer números) para obter a soma de x e y
    
## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, configurou um pipeline do Jenkins que verifica o código de origem no repositório do GitHub. Executa o Maven para criar um ficheiro war e, em seguida, utiliza a CLI do Azure para implementar no serviço de aplicações do Azure. Aprendeu a:

> [!div class="checklist"]
> * Criar uma VM do Jenkins
> * Configurar o Jenkins
> * Criar uma aplicação web no Azure
> * Preparar um repositório do GitHub
> * Criar o pipeline do Jenkins
> * Execução do pipeline e certifique-se a aplicação web
