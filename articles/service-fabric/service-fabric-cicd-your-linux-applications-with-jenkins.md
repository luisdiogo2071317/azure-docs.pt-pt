---
title: Compilação contínua e a integração para as suas aplicações do Azure Service Fabric Linux utilizando Jenkins | Microsoft Docs
description: Compilação contínua e a integração para a sua aplicação de Service Fabric Linux utilizando Jenkins
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: ''
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: saysa
ms.openlocfilehash: 3ee4041390c9452153a7a276b3d5da66249dbde6
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Utilize Jenkins para criar e implementar as aplicações do Linux
O Jenkins é uma ferramenta popular para integração e implementação contínuas de aplicações. Eis como pode utilizar o Jenkins para compilar e implementar a sua aplicação do Azure Service Fabric.

## <a name="topic-overview"></a>Descrição geral de tópico
Este artigo abrange várias formas possíveis de configuração de ambiente Jenkins, bem como de diferentes formas de implementar a aplicação para um cluster do Service Fabric após ter sido concebido. Siga estes passos gerais com êxito, Jenkins o programa de configuração, solicitar as alterações a partir do GitHub, criar a sua aplicação e implementá-la para o cluster:

1. Certifique-se de que instala o [pré-requisitos](#prerequisites).
2. Em seguida, siga os passos nestas secções para configurar Jenkins:
   * [Configurar Jenkins dentro de um cluster do Service Fabric](#set-up-jenkins-inside-a-service-fabric-cluster), 
   * [Configurar Jenkins fora de um cluster do Service Fabric](#set-up-jenkins-outside-a-service-fabric-cluster), ou
   * [Instalar o plug-in do Service Fabric num ambiente existente Jenkins](#install-service-fabric-plugin-in-an-existing-jenkins-environment).
3. Após ter configurado Jenkins, siga os passos no [criar e configurar uma tarefa de Jenkins](#create-and-configure-a-jenkins-job) definir GitHub para o acionador Jenkins quando são efetuadas alterações à sua aplicação e para configurar o pipeline de tarefa Jenkins através do passo de compilação para solicitar o altera a partir do GitHub e criar a sua aplicação. 
4. Por fim, configure o passo de pós-compilação de Jenkins tarefas para implementar a aplicação no seu cluster do Service Fabric. Existem duas formas de configurar Jenkins para implementar a aplicação para um cluster:    
   * Para ambientes de desenvolvimento e teste, utilize [configurar implementação utilizando o ponto final de gestão do cluster](#configure-deployment-using-cluster-management-endpoint). Este é o método mais simples de implementação para configurar.
   * Para ambientes de produção, utilize [configurar a implementação com as credenciais do Azure](#configure-deployment-using-azure-credentials). A Microsoft recomenda este método para ambientes de produção porque com credenciais do Azure pode limitar o acesso que tem uma tarefa de Jenkins aos seus recursos do Azure. 

## <a name="prerequisites"></a>Pré-requisitos

- Certifique-se de que Git é instalado localmente. Pode instalar a versão adequada do Git do [o Git transfere página](https://git-scm.com/downloads) com base no seu sistema operativo. Se estiver familiarizado com o Git, saiba mais sobre ao [Git documentação](https://git-scm.com/docs).
- Este artigo utiliza o *recursos de infraestrutura de serviço-introdução ao exemplo iniciado* no GitHub: [ https://github.com/Azure-Samples/service-fabric-java-getting-started ](https://github.com/Azure-Samples/service-fabric-java-getting-started) para a aplicação para criar e implementar. Pode copiar este repositório para acompanhar, ou, com algumas modificações para as instruções, utilize o suas próprias projeto do GitHub.


## <a name="install-service-fabric-plugin-in-an-existing-jenkins-environment"></a>Instalar o plug-in do Service Fabric num ambiente Jenkins existente
Se estiver a adicionar o plug-in do Service Fabric para um ambiente Jenkins existente, precisa do seguinte:

- O [CLI de recursos de infraestrutura de serviço](service-fabric-cli.md) (sfctl).

   > [!NOTE]
   > Não se esqueça de instalar a CLI ao nível do sistema em vez de ao nível do utilizador, por isso, Jenkins pode executar comandos da CLI. 
   >

- Para implementar aplicações Java, instalar ambas [Gradle e abra JDK 8.0](service-fabric-get-started-linux.md#set-up-java-development). 
- Para implementar aplicações de .NET Core 2.0, instale o [SDK 2.0 do .NET Core](service-fabric-get-started-linux.md#set-up-net-core-20-development). 

Depois de instalar os pré-requisitos necessários para o seu ambiente, pode pesquisar o plug-in do Azure Service Fabric no Jenkins marketplace e instalá-lo.

Depois de instalar o plug-in, avançar diretamente para [criar e configurar uma tarefa de Jenkins](#create-and-configure-a-jenkins-job).


## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Configurar o Jenkins num cluster do Service Fabric

Pode configurar o Jenkins dentro ou fora de um cluster do Service Fabric. As secções seguintes mostram como configurá-lo dentro de um cluster ao utilizar uma conta de armazenamento do Azure para guardar o estado da instância do contentor.

### <a name="prerequisites"></a>Pré-requisitos
- Tem um cluster do Service Fabric Linux com o Docker instalado. Os clusters de Service Fabric em execução no Azure já têm Docker instalado. Se estiver a executar o cluster localmente (ambiente de desenvolvimento OneBox), verifique se o Docker está instalado no seu computador com o `docker info` comando. Se não estiver instalado, instale-o utilizando os seguintes comandos:

   ```sh
   sudo apt-get install wget
   wget -qO- https://get.docker.io/ | sh
   ``` 

   > [!NOTE]
   > Certifique-se de que a porta 8081 é especificada como um ponto final personalizado no cluster. Se estiver a utilizar um cluster local, certifique-se de que porta 8081 está aberta no computador anfitrião e que tem um endereço IP destinado ao público.
   >

### <a name="steps"></a>Passos
1. Clone a aplicação, utilizando os seguintes comandos:
   ```sh
   git clone https://github.com/suhuruli/jenkins-container-application.git
   cd jenkins-container-application
   ```

3. Manter o estado do contentor Jenkins numa partilha de ficheiros:
   1. Criar uma conta de armazenamento do Azure no **mesma região** do seu cluster com um nome, tal como `sfjenkinsstorage1`.
   2. Criar um **partilha de ficheiros** sob o armazenamento de conta com um nome, tal como `sfjenkins`.
   3. Clique em **Connect** para a partilha de ficheiros e tenha em atenção os valores apresenta em **ligar a partir de Linux**, o valor deve ter um aspeto semelhante ao abaixo:

      ```sh
      sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
      ```

   > [!NOTE]
   > Para partilhas de montagem cifs, tem de ter o pacote de cifs utils instalado em nós de cluster.      
   >

4. Atualize os valores de marcador de posição no `setupentrypoint.sh` script com os detalhes de armazenamento do azure a partir do passo 2.
   ```sh
   vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
   ```
   * Substitua `[REMOTE_FILE_SHARE_LOCATION]` com o valor `//sfjenkinsstorage1.file.core.windows.net/sfjenkins` da saída do connect no passo 2 acima.
   * Substitua `[FILE_SHARE_CONNECT_OPTIONS_STRING]` com o valor `vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777` do passo 2 acima.

5. **Proteger apenas o Cluster:** 
   
   Para configurar a implementação de aplicações num cluster de Jenkins seguro, o certificado de cluster tem de ser acessível no contentor Jenkins. No *ApplicationManifest.xml* ficheiro, sob o **ContainerHostPolicies** tag adicionar esta referência de certificado e atualize o valor de thumbprint com que o certificado de cluster.

   ```xml
   <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
   ```

   Além disso, adicione as seguintes linhas no **ApplicationManifest** etiqueta (root) no *ApplicationManifest.xml* de ficheiros e atualize o valor de thumbprint com que o certificado de cluster.

   ```xml
   <Certificates>
     <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="[Thumbprint]" />
   </Certificates> 
   ```

6. Ligar ao cluster e instalar a aplicação de contentor.

   **Proteger o Cluster**
   ```sh
   sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
   bash Scripts/install.sh
   ```
   O comando anterior demora o certificado no formato PEM. Se o certificado está no formato PFX, pode utilizar o seguinte comando para convertê-lo. Se o ficheiro PFX não estiver protegido de palavra-passe, especifique o **passin** parâmetro como `-passin pass:`.
   ```sh
   openssl pkcs12 -in cert.pfx -out cert.pem -nodes -passin pass:MyPassword1234!
   ``` 
   
   **Clusters não seguros**
   ```sh
   sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
   bash Scripts/install.sh
   ```

   Instala o contentor do Jenkins no cluster e pode ser monitorizado com o Service Fabric Explorer.

   > [!NOTE]
   > Pode demorar alguns minutos para a imagem de Jenkins a ser transferido no cluster.
   >

7. A partir do browser, aceda a `http://PublicIPorFQDN:8081`. Fornece o caminho da palavra-passe de administrador inicial necessária para iniciar sessão. 
2. Observe o Service Fabric Explorer para determinar o nó que o contentor de Jenkins está a ser executado. Secure Shell (SSH) de início de sessão para este nó.
   ```sh
   ssh user@PublicIPorFQDN -p [port]
   ``` 
3. Obtenha o ID de instância do contentor com `docker ps -a`.
4. Secure Shell (SSH) de início de sessão para o contentor e cole o caminho que foram apresentados no portal do Jenkins. Por exemplo, se no portal mostra o caminho `PATH_TO_INITIAL_ADMIN_PASSWORD`, execute os seguintes comandos:

   ```sh
   docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
   ```
   ```sh
   cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the password value
   ```
5. Na página Introdução Jenkins, escolha o plug-ins selecione a opção de instalar, selecione o **nenhum** caixa de verificação e clique em instalar.
6. Criar um utilizador ou optar por continuar como um administrador.

Depois de ter configurado Jenkins, avançar diretamente para [criar e configurar uma tarefa de Jenkins](#create-and-configure-a-jenkins-job).  

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Configurar o Jenkins fora de um cluster do Service Fabric

Pode configurar o Jenkins dentro ou fora de um cluster do Service Fabric. As secções seguintes mostram como configurá-lo fora de um cluster.

### <a name="prerequisites"></a>Pré-requisitos
- Certifique-se de que o Docker está instalado no seu computador. Os comandos seguintes podem ser utilizados para instalar o Docker a partir do terminal:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

  Quando executa `docker info` no terminal, o resultado deve mostrar que o serviço de Docker está em execução.

### <a name="steps"></a>Passos
1. Extraia a imagem do contentor do Jenkins do Service Fabric: `docker pull rapatchi/jenkins:latest`. Esta imagem inclui o plug-in do Jenkins do Service Fabric pré-instalado.
2. Execute a imagem do contentor:`docker run -itd -p 8080:8080 rapatchi/jenkins:latest`
3. Obtenha o ID da instância de imagem do contentor. Pode listar todos os contentores de Docker com o comando `docker ps –a`
4. Inicie sessão no portal do Jenkins com os seguintes passos:

   1. Iniciar sessão numa shell Jenkins a partir do anfitrião. Utilize os primeiro quatro dígitos do ID de contentor. Por exemplo, se o ID de contentor é `2d24a73b5964`, utilize `2d24`.

      ```sh
      docker exec -it [first-four-digits-of-container-ID] /bin/bash
      ```
   2. A partir da Jenkins shell, obtenha a palavra-passe de administrador para a instância do contentor:

      ```sh
      cat /var/jenkins_home/secrets/initialAdminPassword
      ```      
   3. Para iniciar sessão no Jenkins dashboard, abra o URL seguinte num browser: `http://<HOST-IP>:8080`. Utilize a palavra-passe do passo anterior para desbloquear Jenkins.
   4. (Opcional.) Depois de iniciarem sessão pela primeira vez, pode criar a sua própria conta de utilizador e utilizá-lo para os seguintes passos, ou pode continuar a utilizar a conta de administrador. Se criar um utilizador, terá de continuar com esse utilizador.
5. Configurar o GitHub para trabalhar com Jenkins utilizando os passos em [gerar uma nova chave SSH e adicioná-lo para o agente SSH](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
   * Utilize as instruções fornecidas pelo GitHub para gerar a chave SSH e adicioná-la à conta GitHub que está a alojar o repositório.
   * Execute os comandos mencionados na ligação anterior na shell Docker do Jenkins (e não no seu anfitrião).
   * Para iniciar sessão na shell do Jenkins a partir do seu anfitrião, utilize o seguinte comando:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Certifique-se de que a máquina onde está alojada a imagem de contentor Jenkins ou o cluster tem um endereço IP destinado ao público. Isto permite que a instância do Jenkins receba notificações do GitHub.

Depois de ter configurado Jenkins, avançar para a secção seguinte, [criar e configurar uma tarefa de Jenkins](#create-and-configure-a-jenkins-job).

## <a name="create-and-configure-a-jenkins-job"></a>Criar e configurar uma tarefa do Jenkins

Os passos nesta secção mostram como configurar uma tarefa de Jenkins para responder a alterações no repositório do GitHub, obtenha as alterações e criá-los. No final desta secção, está direcionado para os passos finais para configurar a tarefa de implementar a sua aplicação com base em se estiver a implementar num ambiente de desenvolvimento/teste, ou para um ambiente de produção. 

1. No dashboard do Jenkins, clique em **Novo Item**.
2. Introduza um nome para o item (por exemplo, **MyJob**). Selecione **free-style project** (projeto de estilo livre) e clique em **OK**.
3. Abre a página de configuração da tarefa. (Para obter a configuração a partir do Jenkins dashboard, clique na tarefa e, em seguida, clique em **configurar**).

4. No **geral** separador, marque a caixa de **GitHub projeto**, e especifique o URL de projeto do GitHub. Este URL aloja a aplicação Java do Service Fabric que pretende integrar no fluxo de integração contínua do Jenkins e implementação contínua (CI/CD) (por exemplo, `https://github.com/{your-github-account}/service-fabric-java-getting-started`).

5. No **gestão da origem de código** separador, selecione **Git**. Especifique o URL do repositório que aloja a aplicação Java do Service Fabric que quer integrar no fluxo CI/CD do Jenkins (por exemplo, `https://github.com/{your-github-account}/service-fabric-java-getting-started`). Também pode especificar que ramo para criar (por exemplo, `/master`).
6. Configurar o seu *GitHub* repositório para falar com Jenkins:

   a. Na página de repositório do GitHub, aceda a **definições** > **serviços e integrações**.

   b. Selecione **Add Service** (Adicionar Serviço), escreva **Jenkins** e selecione **Jenkins-GitHub plugin** (Plug-in Jenkins-GitHub).

   c. Introduza o URL do webhook do Jenkins (por predefinição, deve ser `http://<PublicIPorFQDN>:8081/github-webhook/`). Clique em **add/update service** (adicionar/atualizar serviço).

   d. É enviado um evento de teste para a instância do Jenkins. Deverá ver uma verificação a verde através do webhook do GitHub, e o seu projeto será compilado.

7. No **criar Acionadores** separador Jenkins, selecione de que a opção que pretende de compilação. Neste exemplo, pretende para acionar uma compilação sempre que um push para o repositório acontece, por isso, selecione **acionador de rotina do GitHub para consulta GITScm**. (Anteriormente, esta opção era denominada **Build when a change is pushed to GitHub** [Compilar quando é enviada uma alteração para o GitHub]).
8. No **criar** separador, efetue um dos seguintes, dependendo se está a criar uma aplicação de Java ou uma aplicação .NET Core:

   * **Para aplicações de Java:** do **Adicionar passo de compilação** pendente, selecione **invocar o Gradle Script**. Clique em **avançadas**. No menu avançado, especifique o caminho para **raiz compilar o script** para a sua aplicação. Este obtém o build.gradle a partir do caminho especificado e funciona em conformidade. Para o [ActorCounter aplicação](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/reliable-services-actor-sample/Actors/ActorCounter), isto é: `${WORKSPACE}/reliable-services-actor-sample/Actors/ActorCounter`.

     ![Ação de Compilação do Jenkins do Service Fabric][build-step]

   * **Para aplicações do .NET Core:** do **Adicionar passo de compilação** pendente, selecione **executar Shell**. Na caixa de comando que aparece, o diretório primeiro tem de ser alterada para o caminho onde o ficheiro de build.sh está localizado. Depois do diretório foi alterado, o script de build.sh pode ser executado e irá criar a aplicação.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

     A seguinte captura de ecrã mostra um exemplo dos comandos que são utilizados para construir o [contador serviço](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) exemplo com um nome de tarefa Jenkins de CounterServiceApplication.

      ![Ação de Compilação do Jenkins do Service Fabric][build-step-dotnet]

9. Para configurar Jenkins para implementar a sua aplicação para um cluster do Service Fabric nas ações de pós-compilação, terá a localização do certificado desse cluster no contentor de Jenkins. Escolha um dos seguintes, dependendo se o contentor de Jenkins está em execução dentro ou fora do seu cluster e tenha em atenção a localização do certificado de cluster:

   * **Para Jenkins em execução dentro do cluster:** o caminho para o certificado pode ser encontrado pelo echoing o valor da *Certificates_JenkinsOnSF_Code_MyCert_PEM* variável de ambiente de dentro do contentor.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
   * **Para Jenkins executado fora do seu cluster:** siga estes passos para copiar o certificado de cluster para o contentor:
      1. O certificado deve estar no formato PEM. Se não tiver um ficheiro PEM, pode criar um o ficheiro PFX de certificado. O ficheiro PFX não esteja protegido de palavra-passe, execute o seguinte comando a partir do seu anfitrião:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:
         ``` 

      Se o ficheiro PFX protegido de palavra-passe, incluir a palavra-passe a `-passin` parâmetro. Por exemplo:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:MyPassword1234!
         ``` 

      2. Para obter o ID de contentor do contentor de Jenkins, execute `docker ps` do seu anfitrião.
      3. Copie o ficheiro PEM para o contentor com o seguinte comando do Docker:
    
         ```sh
         docker cp clustercert.pem [first-four-digits-of-container-ID]:/var/jenkins_home
         ``` 

Está quase a terminar! A tarefa de Jenkins manter abertas. A tarefa de restantes apenas consiste em configurar os passos de pós-compilação para implementar a aplicação no seu cluster do Service Fabric:

* Para implementar um ambiente de desenvolvimento ou teste, siga os passos no [configurar implementação utilizando o ponto final de gestão do cluster](#configure-deployment-using-cluster-management-endpoint).
* Para implementar um ambiente de produção, siga os passos no [configurar a implementação com as credenciais do Azure](#configure-deployment-using-azure-credentials).

## <a name="configure-deployment-using-cluster-management-endpoint"></a>Configurar a implementação com o ponto final de gestão de cluster
Para ambientes de desenvolvimento e teste, pode utilizar o ponto final de gestão de cluster para implementar a sua aplicação. Configurar a ação de pós-compilação de com o ponto final de gestão de cluster para implementar a aplicação requer o mínimo de configuração. Se estiver a implementar num ambiente de produção, avançar diretamente para [configurar a implementação com as credenciais do Azure](#configure-deployment-using-azure-credentials) para configurar um principal de serviço do Azure Active Directory a utilizar durante a implementação.    

1. Esta tarefa Jenkins, clique em de **ações de pós-compilação de** separador. 
2. No menu pendente **Post-Build Actions** (Ações de Pós-compilação), selecione **Deploy Service Fabric Project** (Implementar Projeto do Service Fabric). 
3. Em **configuração de Cluster do Service Fabric**, selecione o **preencher o ponto final de gestão do Service Fabric** botão de opção.
4. Para **anfitrião gestão**, introduza o ponto final de ligação para o seu cluster; por exemplo `{your-cluster}.eastus.cloudapp.azure.com`.
5. Para **chave cliente** e **certificados de cliente**, introduza a localização do ficheiro PEM no contentor de Jenkins; por exemplo `/var/jenkins_home/clustercert.pem`. (Que copiou a localização do certificado, o último passo de [criar e configurar uma tarefa de Jenkins](#create-and-configure-a-jenkins-job).)
6. Em **configuração da aplicação**, configure o **nome da aplicação**, **tipo de aplicação**e (relativa) **caminho para o Application Manifest** campos.

   ![Ação do serviço de recursos de infraestrutura Jenkins pós-compilação configurar o ponto final de gestão](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-endpoint.png)

7. Clique em **verificar a configuração**. Verificação bem sucedida, clique em **guardar**. O pipeline de tarefa Jenkins totalmente está agora configurado. Avançar diretamente para [passos](#next-steps) para testar a implementação.

## <a name="configure-deployment-using-azure-credentials"></a>Configurar a implementação com as credenciais do Azure
Para ambientes de produção, configurar uma credencial do Azure para implementar a aplicação é vivamente recomendado. Esta secção mostra como configurar um principal de serviço do Azure Active Directory a utilizar para implementar a aplicação na ação de compilação pós-implementação. Pode atribuir principais de serviço para as funções no seu diretório para limitar as permissões da tarefa Jenkins. 

Para ambientes de desenvolvimento e teste, pode configurar credenciais do Azure ou o ponto final a gestão de cluster para implementar a sua aplicação. Para obter mais informações sobre como configurar um ponto final de gestão de cluster, consulte [configurar implementação utilizando o ponto final de gestão do cluster](#configure-deployment-using-cluster-management-endpoint).   

1. Para criar um Azure Active Directory principal de serviço e atribua-lhe as permissões da sua subscrição do Azure, siga os passos no [utilizar o portal para criar um Azure Active Directory principal de serviço e aplicação](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal). Tenha atenção ao seguinte:

   * Ao seguir os passos no tópico, é necessário copiar e guardar os seguintes valores: *ID da aplicação*, *chave da aplicação*, *ID de diretório (ID de inquilino)* e *ID de subscrição*. É necessário para configurar as credenciais do Azure no Jenkins.
   * Se não tiver o [as permissões necessárias](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) no seu diretório, terá de peça a um administrador para lhe conceder as permissões ou criar o principal de serviço para si ou terá de configurar o ponto final de gestão para o cluster no **ações de pós-compilação de** a tarefa no Jenkins.
   * No [criar uma aplicação do Azure Active Directory](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#create-an-azure-active-directory-application) secção, pode introduzir qualquer formado URL para o **URL de início de sessão**.
   * No [atribuir a aplicação a uma função](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) secção, pode atribuir a aplicação a *leitor* função no grupo de recursos para o cluster.

2. Novamente na tarefa Jenkins, clique em de **ações de pós-compilação de** separador.
3. No menu pendente **Post-Build Actions** (Ações de Pós-compilação), selecione **Deploy Service Fabric Project** (Implementar Projeto do Service Fabric). 
4. Em **configuração de Cluster do Service Fabric**, selecione o **selecione o Cluster do Service Fabric** botão de opção. Clique em **adicionar** junto a **credenciais do Azure**. Clique em **Jenkins** para selecionar o fornecedor de credenciais Jenkins.
5. No fornecedor de credenciais Jenkins, selecione **Principal de serviço do Microsoft Azure** do **tipo** pendente.
6. Utilize os valores guardados quando definir se o principal de serviço no passo 1 para definir os seguintes campos:

   * **ID de cliente**: *ID da aplicação*
   * **Segredo do cliente**: *chave da aplicação*
   * **ID de inquilino**: *ID de diretório*
   * **ID de subscrição**: *ID de subscrição*
6. Introduza um descritivo **ID** que utilizar para selecionar a credencial Jenkins e leia a breve **Descrição**. Em seguida, clique em **verificar Principal de serviço**. Se a verificação for bem sucedida, clique em **adicionar**.

   ![Serviço de recursos de infraestrutura Jenkins introduza credenciais do Azure](./media/service-fabric-cicd-your-linux-application-with-jenkins/enter-azure-credentials.png)
7. Novamente em **configuração de Cluster do Service Fabric**, certifique-se de que a sua nova credencial é selecionada para **credenciais do Azure**. 
8. Do **grupo de recursos** pendente, selecione o grupo de recursos do cluster que pretende implementar a aplicação.
9. Do **Service Fabric** pendente, selecione o cluster que pretende implementar a aplicação.
10. Para **chave cliente** e **certificados de cliente**, introduza a localização do ficheiro PEM no contentor de Jenkins. Por exemplo, `/var/jenkins_home/clustercert.pem`. 
11. Em **configuração da aplicação**, configure o **nome da aplicação**, **tipo de aplicação**e (relativa) **caminho para o Application Manifest** campos.
    ![Ação de pós-compilação de do serviço Fabric Jenkins configurar credenciais do Azure](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-credentials.png)
12. Clique em **verificar a configuração**. Verificação bem sucedida, clique em **guardar**. O pipeline de tarefa Jenkins totalmente está agora configurado. Avançar para [passos](#next-steps) para testar a implementação.

## <a name="next-steps"></a>Passos Seguintes
O GitHub e o Jenkins estão agora configurados. Considere efetuar algumas exemplo alterar o `reliable-services-actor-sample/Actors/ActorCounter` projeto no seu bifurcação do repositório, https://github.com/Azure-Samples/service-fabric-java-getting-started. Emita as alterações para remoto `master` ramo (ou qualquer ramo que tiver configurado para trabalhar com). Isto aciona a tarefa `MyJob` do Jenkins, que configurou. Obtém as alterações a partir do GitHub, baseia-se-los e implementa a aplicação para o cluster que especificou nas ações de compilação pós-implementação.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png

