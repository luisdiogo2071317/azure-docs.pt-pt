---
title: CI/CD com o Azure Container Service e do Swarm
description: Utilizar o Azure Container Service com Docker Swarm, um Azure Container Registry e Visual Studio Team Services para fornecer continuamente uma aplicação .NET Core de vários contentor
services: container-service
author: jcorioland
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: ac3133ac093d578c89d24bddd1cc0a7c9588c2fd
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715003"
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Pipeline de CI/CD completo para implementar uma aplicação de vários contentor no Azure Container Service com o Docker Swarm com o Visual Studio Team Services

Um dos maiores desafios ao desenvolver aplicações modernas para a cloud é a capacidade de fornecer esses aplicativos continuamente. Neste artigo, saiba como implementar um pipeline de implementação (CI/CD) com o Azure Container Service com Docker Swarm, o Azure Container Registry e o Visual Studio Team Services build de integração completa e e gestão de versões.

Este artigo se baseia num aplicativo simples, disponível no [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), desenvolvidos com o ASP.NET Core. A aplicação é composta por quatro serviços diferentes: APIs e front-end da web de uma de três web:

![Aplicação de exemplo MyShop](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

O objetivo é fornecer esse aplicativo continuamente num cluster Docker Swarm, com o Visual Studio Team Services. A figura a seguir detalha este pipeline de entrega contínua:

![Aplicação de exemplo MyShop](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Eis uma breve explicação sobre os passos:

1. Alterações de código são confirmadas para o repositório de código de origem (aqui, GitHub) 
1. GitHub aciona uma compilação no Visual Studio Team Services 
1. Visual Studio Team Services obtém a versão mais recente das fontes e baseia-se todas as imagens que compõem a aplicação 
1. Visual Studio Team Services envia por push a cada imagem para um registo de Docker criado com o serviço do Azure Container Registry 
1. Visual Studio Team Services aciona uma nova versão 
1. A versão executa alguns comandos através de SSH no nó de principal do cluster de serviço de contentor do Azure 
1. O docker Swarm no cluster obtém a versão mais recente das imagens 
1. A nova versão da aplicação é implementada com o Docker Compose 

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar este tutorial, terá de concluir as seguintes tarefas:

- [Criar um cluster Swarm no Azure Container Service](container-service-deployment.md)
- [Ligar ao cluster Swarm no Azure Container Service](../container-service-connect.md)
- [Criar um Azure container registry](../../container-registry/container-registry-get-started-portal.md)
- [Ter um projeto de conta e a equipe de Visual Studio Team Services criado](https://docs.microsoft.com/vsts/organizations/accounts/create-organization-msa-or-work-student)
- [Bifurcar o repositório do GitHub para a sua conta do GitHub](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Também precisa de uma máquina do Ubuntu (14.04 ou 16.04) com o Docker instalado. Esta máquina é utilizada pelo Visual Studio Team Services durante os processos de criação e versão. Uma forma de criar esta máquina é utilizar a imagem disponível na [do Azure Marketplace](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>Passo 1: Configurar a sua conta do Visual Studio Team Services 

Nesta secção, configura a sua conta do Visual Studio Team Services.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Configurar um agente de compilação do Linux de serviços de equipe do Visual Studio

Para criar imagens do Docker e emitir essas imagens para um registo de contentor do Azure a partir de uma compilação do Visual Studio Team Services, terá de registar um agente do Linux. Tem estas opções de instalação:

* [Implementar um agente no Linux](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Utilizar o Docker para executar o agente do VSTS](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a>Instalar a extensão do VSTS de integração de Docker

A Microsoft fornece uma extensão do VSTS para trabalhar com o Docker são criados e processos de versão. Esta extensão está disponível na [VSTS Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Clique em **instalar** para adicionar esta extensão para a sua conta do VSTS:

![Instalar a integração de Docker](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

É-lhe perguntado para ligar à sua conta do VSTS com as suas credenciais. 

### <a name="connect-visual-studio-team-services-and-github"></a>Ligar o Visual Studio Team Services e do GitHub

Configure uma ligação entre o seu projeto VSTS e a sua conta do GitHub.

1. No seu projeto do Visual Studio Team Services, clique nas **configurações** ícone na barra de ferramentas e selecione **serviços**.

    ![Visual Studio Team Services - ligação externa](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

1. No lado esquerdo, clique em **novo ponto final de serviço** > **GitHub**.

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

1. Para autorizar o VSTS para trabalhar com a sua conta do GitHub, clique em **autorizar** e siga o procedimento de janela que é aberta.

    ![Autorizar o Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a>Ligar o VSTS ao seu registo de contentor do Azure e o cluster do Azure Container Service

São os últimos passos antes de entrar no pipeline de CI/CD para configurar ligações externas ao seu registo de contentor e o seu cluster Docker Swarm no Azure. 

1. Na **serviços** definições do seu projeto do Visual Studio Team Services, adicionar um ponto final de serviço do tipo **registo Docker**. 

1. No pop-up que se abre, introduza o URL e as credenciais do seu registo de contentor do Azure.

    ![Visual Studio Team Services - registo Docker](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

1. Para o cluster Docker Swarm, adicionar um ponto final do tipo **SSH**. Em seguida, introduza as informações de ligação de SSH do seu cluster do Swarm.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Toda a configuração é feita agora. Os passos seguintes, vai criar o pipeline de CI/CD que cria e implementa a aplicação no cluster Docker Swarm. 

## <a name="step-2-create-the-build-definition"></a>Passo 2: Criar a definição de compilação

Neste passo, configure uma compilação definitionfor seu projeto VSTS e definir o fluxo de trabalho de compilação para as imagens de contentor

### <a name="initial-definition-setup"></a>Configuração inicial de definições

1. Para criar uma definição de compilação, ligar ao seu projeto do Visual Studio Team Services e clique em **compilação e versão**. 

1. Na **definições de compilação** secção, clique em **+ novo**. Selecione o **vazio** modelo.

    ![Definição de compilação do Visual Studio Team Services - novo](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

1. Configurar a nova compilação com uma origem de repositório do GitHub, verificação **integração contínua**e selecione a fila de agente em que registrou o agente do Linux. Clique em **criar** para criar a definição de compilação.

    ![Visual Studio Team Services - Criar definição de compilação](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

1. Na **definições de compilação** página, primeiro abra o **repositório** separador e configurar a compilação para usar o fork do projeto MyShop que criou nos pré-requisitos. Certifique-se de que seleciona *acs-docs* como o **ramo predefinido**.

    ![Visual Studio Team Services - configuração do repositório de compilação](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

1. Sobre o **Acionadores** separador, configure a compilação para ser acionado após cada consolidação. Selecione **integração contínua** e **alterações do Batch**.

    ![Visual Studio Team Services - configuração de Acionador de compilação](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Definir o fluxo de trabalho de compilação
Os passos seguintes definem o fluxo de trabalho de compilação. Existem cinco imagens de contentor para criar aplicativos para o *MyShop* aplicação. Cada imagem é criada usando o Dockerfile localizado nas pastas de projeto:

* ProductsApi
* Proxy
* RatingsApi
* RecommandationsApi
* ShopFront

Tem de adicionar duas etapas de Docker para cada imagem, um para compilar a imagem e um para enviar a imagem no Azure container registry. 

1. Para adicionar um passo no fluxo de trabalho de compilação, clique em **+ Adicionar passo de compilação** e selecione **Docker**.

    ![Visual Studio Team Services - adicionar passos de compilação](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

1. Para cada imagem, configure uma etapa que utiliza o `docker build` comando.

    ![Compilação do Visual Studio Team Services - Docker](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Para a operação de compilação, selecione o seu registo de contentor do Azure, o **crie uma imagem** ação e o Dockerfile que define cada imagem. Definir o **contexto de compilação** como o Dockerfile diretório de raiz e definir o **nome da imagem**. 
    
    Conforme mostrado no ecrã anterior, inicie o nome da imagem com o URI do seu registo de contentor do Azure. (Também pode utilizar uma variável de compilação para parametrizar a etiqueta da imagem, como o identificador de compilação neste exemplo.)

1. Para cada imagem, configure uma segunda etapa, que utiliza o `docker push` comando.

    ![Visual Studio Team Services - Push do Docker](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Para a operação de push, selecione o seu registo de contentor do Azure, o **enviar uma imagem** ação e introduza o **nome da imagem** criada no passo anterior.

1. Depois de configurar os passos de compilação e emissão para cada uma das cinco imagens, adicione mais duas etapas do fluxo de trabalho de compilação.

    a. Uma tarefa da linha de comandos que utiliza um script de bash para substituir a *BuildNumber* ocorrência no ficheiro docker-Compose com o atual criar ID. Veja o ecrã seguinte para obter detalhes.

    ![Visual Studio Team Services - ficheiro de composição de atualização](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Uma tarefa que descarta o ficheiro de composição atualizado como um artefacto de compilação para que possa ser utilizada na versão. Veja o ecrã seguinte para obter detalhes.

    ![Visual Studio Team Services - publicar Compose ficheiro](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

1. Clique em **guardar** e nomeie a sua definição de compilação.

## <a name="step-3-create-the-release-definition"></a>Passo 3: Criar a definição de versão

Visual Studio Team Services permite-lhe [gerir versões em ambientes](https://www.visualstudio.com/team-services/release-management/). Pode ativar a implementação contínua para se certificar de que a aplicação é implementada nos seus ambientes diferentes (por exemplo, desenvolvimento, teste, pré-produção e produção), de forma uniforme. Pode criar um novo ambiente que representa o seu cluster do Azure Container Service Docker Swarm.

![Visual Studio Team Services - versão para ACS](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Versão inicial do programa de configuração

1. Para criar uma definição de versão, clique em **versões** > **+ versão**

1. Para configurar a origem de artefacto, clique em **artefactos** > **ligar uma origem de artefacto**. Ligação aqui, esta nova definição de versão para a compilação que definiu no passo anterior. Ao fazer isso, o ficheiro docker-Compose yml está disponível no processo de liberação.

    ![Visual Studio Team Services - artefactos de versão](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

1. Para configurar o acionador de versão, clique em **Acionadores** e selecione **implementação contínua**. Defina o gatilho na mesma origem de artefacto. Esta definição garante que uma nova versão é iniciada assim que a compilação for concluída com êxito.

    ![Visual Studio Team Services - Acionadores de versão](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Definir o fluxo de trabalho de versão

O fluxo de trabalho de versão é composto por duas tarefas que adicionar.

1. Configurar uma tarefa para copiar de forma segura o ficheiro de composição para um *implementar* pasta do Docker Swarm nó principal, usando a ligação de SSH que configurou anteriormente. Veja o ecrã seguinte para obter detalhes.

    ![Visual Studio Team Services - versão SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

1. Configurar uma segunda tarefa para executar um comando bash para ser executado `docker` e `docker-compose` comandos no nó principal. Veja o ecrã seguinte para obter detalhes.

    ![Visual Studio Team Services - versão Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    O comando é executado no mestre, utilize a CLI do Docker e a CLI do Docker-Compose para efetuar as seguintes tarefas:

    - Início de sessão para o Azure container registry (utiliza três variab'les de compilação que são definidos na **variáveis** separador)
    - Definir o **DOCKER_HOST** variável para trabalhar com o ponto final do Swarm (: 2375)
    - Navegue para o *implementar* pasta que foi criada pela tarefa de cópia de segurança anterior e que contém o ficheiro docker-Compose. 
    - Executar `docker-compose` comandos que extraia as novas imagens, pare os serviços, remova os serviços e criar os contentores.

    >[!IMPORTANT]
    > Conforme mostrado no ecrã anterior, deixe a **falhar em STDERR** caixa de verificação desmarcada. Isso é uma definição importante, porque `docker-compose` imprime várias mensagens de diagnóstico, como contentores estão a parar ou a ser eliminado, a saída de erro padrão. Se selecionar a caixa de verificação, Visual Studio Team Services relatórios que ocorreram erros durante o lançamento, mesmo se tudo correr bem.
    >
1. Guarde esta nova definição de versão.


>[!NOTE]
>Esta implementação inclui algum período de indisponibilidade porque estamos a parar os serviços antigos e novo a executar. É possível evitar isto, fazendo uma implementação de "blue-Green".
>

## <a name="step-4-test-the-cicd-pipeline"></a>Passo 4. Testar o pipeline de CI/CD

Agora que já com a configuração, é altura de o testar este novo pipeline de CI/CD. É a maneira mais fácil para testá-lo atualizar o código-fonte e consolidar as alterações no seu repositório do GitHub. Alguns segundos depois de enviar o código, verá uma nova compilação em execução no Visual Studio Team Services. Depois de concluída com êxito, uma nova versão será acionada e irá implementar a nova versão da aplicação no cluster do Azure Container Service.

## <a name="next-steps"></a>Próximos Passos

* Para obter mais informações sobre a CI/CD com o Visual Studio Team Services, consulte a [descrição geral de compilação VSTS](https://www.visualstudio.com/docs/build/overview).
