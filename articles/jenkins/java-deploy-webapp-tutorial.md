---
title: Utilizar o Jenkins para implementar as suas aplicações Web no Azure
description: Utilize o Jenkins e o Docker para configurar a integração contínua a partir do GitHub no Serviço de Aplicações do Azure para as suas aplicações Web.
ms.service: jenkins
keywords: jenkins, azure, devops, app service, continuous integration, ci, continuous deployment, cd
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 07/31/2018
ms.openlocfilehash: 81959159d4860512c184ada25930da814b5ae044
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946583"
---
# <a name="set-up-continuous-integration-and-deployment-to-azure-app-service-with-jenkins"></a>Configurar a integração e a implementação contínua no Serviço de Aplicações do Azure com o Jenkins

Este tutorial configura a integração e a implementação contínua (CI/CD) de uma aplicação Web Java de exemplo desenvolvida com a arquitetura [Spring Boot](http://projects.spring.io/spring-boot/) nas [Aplicações Web do Serviço de Aplicações do Azure no Linux](/azure/app-service/containers/app-service-linux-intro) mediante a utilização do Jenkins.

Neste tutorial, vai realizar as seguintes tarefas:

> [!div class="checklist"]
> * Instalar os plug-ins do Jenkins necessários para implementar no Serviço de Aplicações do Azure.
> * Definir um trabalho do Jenkins para compilar imagens do Docker a partir de um repositório do GitHub quando é emitida uma consolidação nova.
> * Definir uma Aplicação Web do Azure nova para Linux e configurá-la para implementar imagens do Docker emitidas para o Azure Container Registry.
> * Configurar o plug-in do Jenkins para o Serviço de Aplicações do Azure.
> * Implementar a aplicação de exemplo no Serviço de Aplicações do Azure com uma compilação manual.
> * Acionar uma compilação do Jenkins e atualizar a aplicação Web ao emitir alterações para o GitHub.

## <a name="before-you-begin"></a>Antes de começar

Para concluir este tutorial, precisa de:

* [Jenkins](https://jenkins.io/) com ferramentas JDK e Maven configuradas. Se não tiver um sistema Jenkins, crie-o no Azure agora a partir do [modelo de solução do Jenkins](/azure/jenkins/install-jenkins-solution-template).
* Uma conta do [GitHub](https://github.com).
* [CLI do Azure](/cli/azure), a partir da linha de comandos local ou no [Azure Cloud Shell](/azure/cloud-shell/overview)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-jenkins-plug-ins"></a>Instalar os plug-ins do Jenkins

1. Abra um browser na consola Web do Jenkins e selecione **Manage Jenkins** (Gerir Jenkins), no menu do lado esquerdo, e selecione **Manage Plugins** (Gerir Plug-ins).
2. Selecione o separador **Available** (Disponíveis).
3. Procure e selecione a caixa de verificação junto aos seguintes plug-ins:   

    - [Azure App Service Plug-in](https://plugins.jenkins.io/azure-app-service)
    - [GitHub Branch Source Plug-in](https://plugins.jenkins.io/github-branch-source)

    Se os plug-ins não aparecerem, aceda ao separador **Installed** (Instalados) para verificar se já estão instalados.

1. Selecione**Download now and install after restart** (Transferir agora e instalar depois de reiniciar) para ativar os plug-ins na configuração do Jenkins.

## <a name="configure-github-and-jenkins"></a>Configurar o GitHub e o Jenkins

Configure o Jenkins para receber [webhooks do GitHub](https://developer.github.com/webhooks/) quando forem emitidas consolidações novas para um repositório na sua conta.

1. Selecione **Manage Jenkins** (Gerir Jenkins) e **Configure System** (Configurar Sistema). Na secção **GitHub**, confirme que **Manage hooks** (Gerir hooks) está selecionado, escolha **Manage additional GitHub actions** (Gerir ações do GitHub adicionais) e escolha **Convert login and password to token** (Converter início de sessão e palavra-passe em token).
2. Selecione o botão de opção **From login and password** (A partir de início de sessão e de palavra-passe) e introduza o seu nome de utilizador e a sua palavra-passe do GitHub. Selecione **Create token credentials** (Criar credenciais de token) para criar um [Token de Acesso Pessoal do GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).   
   ![Criar PAT do GitHub a partir de início de sessão e de palavra-passe](media/jenkins-java-quickstart/create_github_credentials.png)
3.  Selecione o token acabado de criar no menu pendente **Credentials** (Credenciais) na configuração dos Servidores do GitHub. Selecione **Test connection** (Testar ligação) para verificar se a autenticação está a funcionar.   
   ![Verificar a ligação ao GitHub após o PAT estar configurado](media/jenkins-java-quickstart/verify_github_connection.png)

> [!NOTE]
> Se a sua conta do GitHub tiver a autenticação de dois fatores ativada, crie o token no GitHub e configure o Jenkins para utilizá-lo. Reveja a documentação [do plug-in do GitHub do Jenkins](https://wiki.jenkins.io/display/JENKINS/Github+Plugin) para obter os detalhes completos.

## <a name="fork-the-sample-repo-and-create-a-jenkins-job"></a>Duplicar (fork) o repositório de exemplo e criar um trabalho do Jenkins 

1. Abra o [repositório da aplicação de exemplo Spring Boot](https://github.com/spring-guides/gs-spring-boot-docker) e duplique-o para a sua conta do GitHub ao selecionar **Fork**, no canto superior direito.   
    ![Fork no GitHub](media/jenkins-java-quickstart/fork_github_repo.png)
1. Na consola Web do Jenkins, selecione **New Item** (Novo Item), dê-lhe o nome **MyJavaApp**, selecione **Freestyle project** (Projeto para fins gerais) e selecione **OK**.   
    ![Projeto para fins gerais novo do Jenkins](media/jenkins-java-quickstart/jenkins_freestyle.png)
2. Na secção **General** (Geral), selecione o projeto do **GitHub** e introduza o URL do repositório duplicado, como https://github.com/raisa/gs-spring-boot-docker
3. Na secção **Source code management**  (Gestão do código de origem), selecione **Git** e introduza o URL `.git` do repositório duplicado, como https://github.com/raisa/gs-spring-boot-docker.git
4. Na secção **Criar Acionadores**, selecione **Acionador de hook do GitHub para consulta GITScm**.
5. Na secção **Build** (Compilar), selecione **Add build step** (Adicionar passo de compilação) e escolha **Invoke top-level Maven targets** (Invocar destinos de Maven de nível superior). Introduza `package` no campo **Goals** (Objetivos).
6. Selecione **Guardar**. Pode selecionar **Build Now** (Testar Agora), na página do projeto, para testar o seu trabalho.

## <a name="configure-azure-app-service"></a>Configurar o Serviço de Aplicações 

1. Utilize a CLI do Azure ou o [Cloud Shell](/azure/cloud-shell/overview) para criar uma [Aplicação Web no Linux](/azure/app-service/containers/app-service-linux-intro) nova. O nome da aplicação Web neste tutorial é `myJavaApp`, mas tem de utilizar um nome exclusivo para a sua própria aplicação.
   
    ```azurecli-interactive
    az group create --name myResourceGroupJenkins --location westus
    az appservice plan create --is-linux --name myLinuxAppServicePlan --resource-group myResourceGroupJenkins 
    az webapp create --name myJavaApp --resource-group myResourceGroupJenkins --plan myLinuxAppServicePlan --runtime "java|1.8|Tomcat|8.5"
    ```

2. Crie um registo do [Azure Container Registry](/azure/container-registry/container-registry-intro) para armazenar as imagens do Docker que o Jenkins cria. O nome do registo de contentor utilizado neste tutorial é `jenkinsregistry`, mas tem de utilizar um nome exclusivo para o seu próprio registo. 

    ```azurecli-interactive
    az acr create --name jenkinsregistry --resource-group myResourceGroupJenkins --sku Basic --admin-enabled
    ```
3. Configure a aplicação Web para executar as imagens do Docker emitidas para o registo de contentor e especifique que a aplicação que está em execução no contentor escuta os pedidos na porta 8080.   

    ```azurecli-interactive
    az webapp config container set -c jenkinsregistry/webapp --resource-group myResourceGroupJenkins --name myJavaApp
    az webapp config appsettings set --resource-group myResourceGroupJenkins --name myJavaApp --settings PORT=8080
    ```

## <a name="configure-the-azure-app-service-jenkins-plug-in"></a>Configurar o plug-in Azure App Service Jenkins

1. Na consola Web do Jenkins, selecione o trabalho **MyJavaApp** que criou e selecione **Configure** (Configurar), no lado esquerdo da página.
2. Desloque-se para **Post-build Actions** (Ações pós-compilação), selecione **Add post-build action** (Adicionar ação pós-compilação) e escolha **Publish an Azure Web App** (Publicar uma Aplicação Web do Azure).
3. Em **Azure Profile Configuration** (Configuração de Perfil do Azure), selecione **Add** (Adicionar), junto a **Azure Credentials** (Credenciais do Azure) e escolha **Jenkins**.
4. Na caixa de diálogo **Add Credentials** (Adicionar Credenciais), selecione **Microsoft Azure Service Principal** (Principal de Serviço do Microsoft Azure), no menu pendente **Kind** (Tipo).
5. Crie um Principal de serviço do Active Directory na CLI do Azure ou no [Cloud Shell](/azure/cloud-shell/overview).
    
    ```azurecli-interactive
    az ad sp create-for-rbac --name jenkins_sp --password secure_password
    ```

    ```json
    {
        "appId": "BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBB",
        "displayName": "jenkins_sp",
        "name": "http://jenkins_sp",
        "password": "secure_password",
        "tenant": "CCCCCCCC-CCCC-CCCC-CCCCCCCCCCC"
    }
    ```
6. Introduza as credenciais do principal de serviço na caixa de diálogo **Add Credentials** (Adicionar credenciais). Se não souber o ID da sua subscrição do Azure, pode consultá-lo na CLI:
     
     ```azurecli-interactive
     az account list
     ```

     ```json
        {
            "cloudName": "AzureCloud",
            "id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA",
            "isDefault": true,
            "name": "Visual Studio Enterprise",
            "state": "Enabled",
            "tenantId": "CCCCCCCC-CCCC-CCCC-CCCC-CCCCCCCCCCC",
            "user": {
            "name": "raisa@fabrikam.com",
            "type": "user"
            }
     ```

    ![Configurar o Principal de Serviço do Azure](media/jenkins-java-quickstart/azure_service_principal.png)
6. Selecione **Verify Service Principal** para verificar se o principal de serviço se autentica no Azure. 
7. Selecione **Add** (Adicionar) para guardar as credenciais.
8. Selecione a credencial do principal de serviço que acabou de adicionar a partir do menu pendente **Azure Credentials** (Credenciais do Azure) quando regressar à configuração **Publish an Azure Web App** (Publicar uma Aplicação Web do Azure).
9. Em **App Configuration** (Configuração da Aplicação), escolha o seu grupo de recursos e o nome da aplicação Web no menu pendente.
10. Selecione o botão de opção **Publish via Docker** (Publicar através do Docker).
11. Introduza `complete/Dockerfile` em **Dockerfile path** (Caminho do Dockerfile).
12. Introduza `https://jenkinsregistry.azurecr.io` no campo **Docker registry URL** (URL do registo do Docker).
13. Selecione **Add** (Adicionar), junto a **Registry Credentials** (Credenciais do Registo). 
14. Introduza o nome de utilizador do administrador do registo do Azure Container Registry que criou em **Username** (Nome de utilizador).
15. Introduza a palavra-passe do registo do Azure Container Registry no campo **Password** (Palavra-passe). Pode obter o nome de utilizador e a palavra-passe no portal do Azure ou através do seguinte comando da CLI:

    ```azurecli-interactive
    az acr credential show -n jenkinsregistry
    ```
    ![Adicionar as credenciais do registo de contentor](media/jenkins-java-quickstart/enter_acr_credentials.png)
15. Selecione **Add** (Adicionar) para guardar a credencial.
16. Selecione a credencial acabada de criar no menu pendente **Registry credentials** (Credenciais do registo), no painel **App Configuration** (Configuração do painel) para **Publish an Azure Web App** (Publicar uma Aplicação Web do Azure). A ação pós-compilada concluída deve ser semelhante à imagem seguinte:   
    ![Configuração de ação pós-compilação para Implementação do Serviço de Aplicações do Azure](media/jenkins-java-quickstart/appservice_plugin_configuration.png)
17. Selecione **Save** (Guardar) para guardar a configuração do trabalho.

## <a name="deploy-the-app-from-github"></a>Implementar a aplicação a partir do GitHub

1. No projeto do Jenkins, selecione **Build Now** (Compilar Agora) para implementar a aplicação de exemplo no Azure.
2. Após a conclusão da compilação, a aplicação é ativada no respetivo URL de publicação, como, por exemplo, http://myjavaapp.azurewebsites.net.   
   ![Ver a aplicação implementada no Azure](media/jenkins-java-quickstart/hello_docker_world_unedited.png)

## <a name="push-changes-and-redeploy"></a>Emitir alterações e reimplementar

1. No fork do Github, navegue para `complete/src/main/java/Hello/Application.java`. Selecione a ligação **Edit this file** (Editar este ficheiro), no lado direito da interface do GitHub.
2. Faça a seguinte alteração ao método `home()` e consolide-a no ramo principal do repositório.
   
    ```java
    return "Hello Docker World on Azure";
    ```
3. É iniciada uma compilação nova no Jenkins, acionada pela consolidação nova no ramo `master` do repositório. Após a conclusão da compilação, recarregue a aplicação no Azure.     
      ![Ver a aplicação implementada no Azure](media/jenkins-java-quickstart/hello_docker_world.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Resolver problemas nos plug-ins do Jenkins

Se se deparar com erros nos plug-ins do Jenkins, comunique os problemas com os componentes específicos no [Jenkins JIRA](https://issues.jenkins-ci.org/).

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Use Azure VMs as build agents](/azure/jenkins/jenkins-azure-vm-agents) (Utilizar VMs do Azure como agentes de compilação)