---
title: Implementar o serviço de aplicações do Azure com o plug-in do Jenkins
description: Saiba como utilizar o plug-in do Jenkins de serviço de aplicações do Azure para implementar uma aplicação web Java no Azure no Jenkins
ms.topic: article
ms.author: tarcher
author: tomarcher
manager: jpconnock
ms.service: devops
ms.custom: jenkins
ms.date: 07/25/2018
ms.openlocfilehash: 407ec2bbb145e73b1a903886204b660aadc9a65f
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284419"
---
# <a name="deploy-to-azure-app-service-by-using-the-jenkins-plugin"></a>Implementar o serviço de aplicações do Azure com o plug-in do Jenkins 

Para implementar uma aplicação web Java no Azure, pode utilizar a CLI do Azure no [Jenkins Pipeline](/azure/jenkins/execute-cli-jenkins-pipeline) ou pode utilizar os [Plug-in Jenkins de serviço de aplicações do Azure](https://plugins.jenkins.io/azure-app-service). O plug-in versão 1.0 do Jenkins suporta implementação contínua ao utilizar a funcionalidade de aplicações Web do serviço de aplicações do Azure através de:
* Git ou FTP.
* Docker para aplicações Web no Linux.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Configure o Jenkins para implementar aplicações Web através de Git ou FTP.
> * Configure o Jenkins para implementar a aplicação Web para contentores.

## <a name="create-and-configure-a-jenkins-instance"></a>Criar e configurar uma instância do Jenkins

Se ainda não tiver um Jenkins Master, começar com o [modelo de solução](install-jenkins-solution-template.md), que inclui o Java Development Kit (JDK) versão 8 e os seguintes necessários plug-ins do Jenkins:

* [Plug-in do Jenkins Git cliente](https://plugins.jenkins.io/git-client) versão 2.4.6 
* [Plug-in do docker Commons](https://plugins.jenkins.io/docker-commons) versão 1.4.0
* [Credenciais do Azure](https://plugins.jenkins.io/azure-credentials) versão 1.2
* [Serviço de aplicações do Azure](https://plugins.jenkins.io/azure-app-service) versão 0.1

Pode utilizar o plug-in do Jenkins para implementar uma aplicação web em qualquer idioma que é suportado pelas aplicações Web, como c#, PHP, Java e node. js. Neste tutorial, utilizamos um [aplicação web Java simples para o Azure](https://github.com/azure-devops/javawebappsample). Para bifurcar o repositório para sua própria conta GitHub, selecione o **Fork** botão no canto superior direito da interface do GitHub.  
> [!NOTE]
> É necessário o JDK do Java e o Maven para compilar o projeto de Java. Instale esses componentes no Jenkins Master, ou o agente da VM, se utilizar o agente para integração contínua. 

Para instalar os componentes, inicie sessão para a instância do Jenkins com o SSH e execute os seguintes comandos:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Para implementar a aplicação Web para contentores, instale o Docker no Jenkins Master ou o agente da VM que é utilizado para a compilação. Para obter instruções, consulte [instalar o Docker no Ubuntu](https://docs.docker.com/engine/installation/linux/ubuntu/).

##<a name="service-principal"></a> Adicionar um principal de serviço do Azure para as credenciais do Jenkins

Precisa de um principal de serviço do Azure para implementar no Azure. 


1. Para criar um Azure principal de serviço, utilize o [CLI do Azure](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) ou o [portal do Azure](/azure/azure-resource-manager/resource-group-create-service-principal-portal).
2. No dashboard do Jenkins, selecione **credenciais** > **sistema**. Em seguida, selecione **Global credentials(unrestricted)**.
3. Para adicionar um principal de serviço do Microsoft Azure, selecione **adicionar credenciais**. Fornecer valores para o **ID de subscrição**, **ID de cliente**, **segredo do cliente**, e **ponto final de Token do OAuth 2.0** campos. Definir o **ID** campo **mySp**. Podemos utilizar este ID nos passos subsequentes neste artigo.


## <a name="configure-jenkins-to-deploy-web-apps-by-uploading-files"></a>Configurar o Jenkins para implementar aplicações Web através do carregamento de ficheiros

Para implementar o seu projeto para aplicações Web, pode carregar seus artefactos de compilação (por exemplo, um ficheiro WAR no Java) com o Git ou FTP.

Antes de configurar a tarefa no Jenkins, precisará de um plano do serviço de aplicações do Azure e uma aplicação web para executar a aplicação de Java.


1. Crie um plano de serviço de aplicações do Azure com o **gratuito** escalão de preço, utilizando o `az appservice plan create` [comando da CLI do Azure](/cli/azure/appservice/plan#az_appservice_plan_create). O plano do serviço de aplicações define os recursos físicos que são utilizados para alojar as suas aplicações. Todas as aplicações que estão atribuídas a um plano do serviço de aplicações partilham estes recursos. Recursos partilhados ajudá-lo a poupar nos custos quando alojar várias aplicações.
2. Crie uma aplicação Web. Pode utilizar o [portal do Azure](/azure/app-service-web/web-sites-configure) ou o seguinte `az` comando da CLI do Azure:
    ```azurecli-interactive 
    az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
    ```
    
3. Defina a configuração de tempo de execução do Java que a sua aplicação precisa. O seguinte comando da CLI do Azure configura a aplicação web para executar numa recente JDK 8 e [Apache Tomcat](http://tomcat.apache.org/) versão 8.0:
    ```azurecli-interactive
    az webapp config set \
    --name <myAppName> \
    --resource-group <myResourceGroup> \
    --java-version 1.8 \
    --java-container Tomcat \
    --java-container-version 8.0
    ```

### <a name="set-up-the-jenkins-job"></a>Configurar a tarefa do Jenkins

1. Criar uma nova **freestyle** project no Dashboard do Jenkins.
2. Configurar o **gestão de código fonte** campo a utilizar o seu fork local da [simple aplicação de web de Java para o Azure](https://github.com/azure-devops/javawebappsample). Forneça o **URL do repositório** valor. Por exemplo: http://github.com/&lt; your_ID > / javawebappsample.
3. Adicione um passo para criar o projeto com o Maven, adicionando a **executar shell** comando. Neste exemplo, precisamos de um comando adicional para mudar o nome da \*ficheiro. War na pasta de destino para **War**:   
    ```bash
    mvn clean package
    mv target/*.war target/ROOT.war
    ```

4. Adicionar uma ação de pós-compilação selecionando **publicar uma aplicação Web do Azure**.
5. Fornecer **mySp** como o principal de serviço do Azure. Este principal foi armazenado como o [credenciais do Azure](#service-principal) num passo anterior.
6. Na **configuração da aplicação** secção, selecione a aplicação de web e de grupo de recursos na sua subscrição. O plug-in do Jenkins Deteta automaticamente se a aplicação web baseada em Windows ou Linux. Para uma aplicação web do Windows, o **publicar ficheiros** opção é apresentada.
7. Preencha os ficheiros que pretende implementar. Por exemplo, especifique o pacote WAR se estiver a utilizar o Java. Utilizar o opcional **diretório de origem** e **diretório de destino** parâmetros para especificar as pastas de origem e destino a utilizar para o carregamento de ficheiros. A aplicação web em Java no Azure é executada num servidor Tomcat. Portanto, para Java, carregue seu pacote WAR para a pasta webapps. Neste exemplo, definir o **diretório de origem** valor **destino** e o **diretório de destino** valor para **webapps**.
8. Se pretender implementar a um bloco que não seja de produção, também pode definir o **ranhura** nome.
9. Guardar o projeto e compilá-lo. A aplicação web é implementada para o Azure quando a compilação estiver concluída.

### <a name="deploy-web-apps-by-uploading-files-using-jenkins-pipeline"></a>Implementar aplicações Web através do carregamento de ficheiros com o Pipeline do Jenkins

O plug-in do Jenkins de serviço de aplicações do Azure está preparado para o pipeline. Pode consultar o exemplo a seguir no repositório do GitHub.

1. Na interface do GitHub, abra a **Jenkinsfile_ftp_plugin** ficheiro. Para editar o ficheiro, selecione o ícone de lápis. Atualização do **resourceGroup** e **webAppName** definições para a aplicação web em linhas 11 e 12, respectivamente:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Definir o **withCredentials** definição na linha 14 para o ID de credencial na instância do Jenkins:
    ```java
    withCredentials([azureServicePrincipal('<mySp>')]) {
    ```

### <a name="create-a-jenkins-pipeline"></a>Criar um pipeline do Jenkins

1. Abra o Jenkins num browser. Selecione **Novo Item**.
2. Forneça um nome para a tarefa e selecione **Pipeline**. Selecione **OK**.
3. Selecione o **Pipeline** separador.
4. Para o **definição** valor, selecione **Pipeline de script do SCM**.
5. Para o **SCM** valor, selecione **Git**. Introduza o URL do GitHub para o seu repositório bifurcado. Por exemplo: https://&lt;your_forked_repo > projeto.
6. Atualização do **caminho do Script** valor **Jenkinsfile_ftp_plugin**.
7. Selecione **guardar** e executar a tarefa.

## <a name="configure-jenkins-to-deploy-web-app-for-containers"></a>Configurar o Jenkins para implementar a aplicação Web para contentores

Aplicações Web no Linux suporta implementação com o Docker. Para implementar a aplicação web com o Docker, terá de fornecer um Dockerfile que empacota a sua aplicação web com um tempo de execução do serviço numa imagem do Docker. O plug-in do Jenkins, em seguida, cria a imagem, envia-o para um registo do Docker e implementa a imagem à sua aplicação web.

Aplicações Web no Linux também oferece suporte a métodos de implantação tradicionais, como o Git e FTP, mas apenas para idiomas incorporados (.NET Core, node. js, PHP e Ruby). Para outros idiomas, terá de empacotar seu tempo de execução do código e o serviço de aplicações em conjunto numa imagem do Docker e utilizar o Docker para implementar.

Antes de configurar a tarefa no Jenkins, terá uma aplicação web no Linux. Também precisa de um registo de contentores para armazenar e gerir as imagens de contentor privadas do Docker. Pode usar o DockerHub para criar o registo de contentor. Neste exemplo, utilizamos o Azure Container Registry.

* [Criar a sua aplicação web no Linux](../app-service/containers/quickstart-nodejs.md).
* O Azure Container Registry é um gerido [registo de Docker](https://docs.docker.com/registry/) serviço que se baseia na versão 2.0 do registo do Docker de código aberto. [Criar um Azure container registry](/azure/container-registry/container-registry-get-started-azure-cli). Também pode usar o DockerHub.

### <a name="set-up-the-jenkins-job-for-docker"></a>Configurar a tarefa do Jenkins para Docker

1. Criar uma nova **freestyle** project no Dashboard do Jenkins.
2. Configurar o **gestão de código fonte** campo a utilizar o seu fork local da [simple aplicação de web de Java para o Azure](https://github.com/azure-devops/javawebappsample). Forneça o **URL do repositório** valor. Por exemplo: http://github.com/&lt; your_ID > / javawebappsample.
3. Adicione um passo para criar o projeto com o Maven, adicionando um **executar shell** comando. Inclua a seguinte linha no comando:
    ```bash
    mvn clean package
    ```

4. Adicionar uma ação de pós-compilação selecionando **publicar uma aplicação Web do Azure**.
5. Fornecer **mySp** como o principal de serviço do Azure. Este principal foi armazenado como o [credenciais do Azure](#service-principal) num passo anterior.
6. Na **configuração da aplicação** secção, selecione o grupo de recursos e uma aplicação web do Linux na sua subscrição.
7. Escolher **publicar através do Docker**.
8. Preencha os **Dockerfile** o valor do caminho. Pode manter o /Dockerfile de valor predefinido.
Para o **URL de registo do Docker** valor, forneça o URL utilizando o formato https://&lt;yourRegistry >. azurecr.io se utilizar o Azure Container Registry. Se usar DockerHub, deixe o valor em branco.
9. Para o **as credenciais do registo** valor, adicionar a credencial para o registo de contentor. Pode obter o ID de utilizador e palavra-passe ao executar os seguintes comandos na CLI do Azure. O primeiro comando permite que a conta de administrador:
    ```azurecli-interactive
    az acr update -n <yourRegistry> --admin-enabled true
    az acr credential show -n <yourRegistry>
    ```

10. O valor de nome e a etiqueta de imagem de Docker no **avançadas** separador são opcionais. Por predefinição, o valor para o nome da imagem é obtido a partir do nome da imagem que configurou no portal do Azure no **contentor do Docker** definição. A etiqueta é gerada a partir de $BUILD_NUMBER.
    > [!NOTE]
    > Certifique-se de que especifique o nome de imagem no portal do Azure ou fornecer um **imagem do Docker** valor no **avançadas** separador. Neste exemplo, definir o **imagem do Docker** valor &lt;your_Registry >.azurecr.io/calculator e deixe o **Tag de imagem do Docker** valor em branco.

11. A implementar falha se usar uma definição de imagem do Docker incorporada. Alterar a configuração de Docker para utilizar uma imagem personalizada no **contentor do Docker** definição no portal do Azure. Para uma imagem interna, utilize a abordagem de carregamento de ficheiros para implementar.
12. Assim como a abordagem de carregamento de arquivo, pode escolher outra **ranhura** atribua um nome diferente de **produção**.
13. Guardar e compile o projeto. A imagem de contentor é emitidos via push para o seu registo e a aplicação web é implementada.

### <a name="deploy-web-app-for-containers-by-using-jenkins-pipeline"></a>Implementar aplicação Web para contentores com o Pipeline do Jenkins

1. Na interface do GitHub, abra a **Jenkinsfile_container_plugin** ficheiro. Para editar o ficheiro, selecione o ícone de lápis. Atualização do **resourceGroup** e **webAppName** definições para a aplicação web em linhas 11 e 12, respectivamente:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Altere a linha 13 para o servidor de registo de contentor:
    ```java
    def registryServer = '<registryURL>'
    ```

3. Altere a linha 16 para utilizar o ID de credencial na instância do Jenkins:
    ```java
    azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
    ```

### <a name="create-a-jenkins-pipeline"></a>Criar um pipeline do Jenkins    

1. Abra o Jenkins num browser. Selecione **Novo Item**.
2. Forneça um nome para a tarefa e selecione **Pipeline**. Selecione **OK**.
3. Selecione o **Pipeline** separador.
4. Para o **definição** valor, selecione **Pipeline de script do SCM**.
5. Para o **SCM** valor, selecione **Git**. Introduza o URL do GitHub para o seu repositório bifurcado. Por exemplo: https://&lt;your_forked_repo > projeto.
7. Atualização do **caminho do Script** valor **Jenkinsfile_container_plugin**.
8. Selecione **guardar** e executar a tarefa.

## <a name="verify-your-web-app"></a>Certifique-se a sua aplicação web

1. Para verificar que o ficheiro WAR implementar com êxito à sua aplicação web, abra um navegador da web.
2. Aceda a http://&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Substitua &lt;your_app_name > com o nome da sua aplicação web. Verá a mensagem:
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jun 17 16:39:10 UTC 2017
    ```

3. Aceda a http://&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Substitua &lt;x > e &lt;y > com quaisquer números para obter a soma de x + y. A Calculadora mostra a soma: ![Calculadora: adicionar](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-azure-app-service-on-linux"></a>Para o serviço de aplicações do Azure no Linux

1. Para verificar a sua aplicação web, execute o seguinte comando na CLI do Azure:
    ```CLI
    az acr repository list -n <myRegistry> -o json
    ```
    É apresentada a mensagem seguinte:
    ```CLI
    ["calculator"]
    ```
    
2. Aceda a http://&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Substitua &lt;your_app_name > com o nome da sua aplicação web. Verá a mensagem: 
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jul 09 16:39:10 UTC 2017
    ```

3. Aceda a http://&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Substitua &lt;x > e &lt;y > com quaisquer números para obter a soma de x + y.
    
## <a name="troubleshooting"></a>Resolução de problemas

Se tiver quaisquer erros com os plug-ins do Jenkins, comunique o assunto na [Jenkins JIRA](https://issues.jenkins-ci.org/) para o componente específico.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, utilizou o plug-in do Jenkins de serviço de aplicações do Azure para implementar no Azure.

Aprendeu a:

> [!div class="checklist"]
> * Configurar o Jenkins para implementar o serviço de aplicações do Azure através de FTP 
> * Configurar o Jenkins para implementar a aplicação Web para contentores 
