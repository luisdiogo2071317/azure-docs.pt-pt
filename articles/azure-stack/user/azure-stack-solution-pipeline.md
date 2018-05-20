---
title: Implementar a aplicação no Azure e Azure pilha | Microsoft Docs
description: Saiba como implementar aplicações do Azure e pilha do Azure com um pipeline de CI/CD híbrida.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/15/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 483122f5d2e39c8595d0f28a6b937772c4ea2e50
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Tutorial: Implementar aplicações do Azure e Azure pilha

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Um pipeline de entrega (CI/CD) integração contínua/contínua híbrida permite-lhe criar, testar e implementar a sua aplicação para várias nuvens.  Neste tutorial, irá criar um ambiente de exemplo para:
 
> [!div class="checklist"]
> * Inicie uma nova compilação com base no código consolidações para o seu repositório de serviços de equipa do Visual Studio (VSTS).
> * Implemente automaticamente o código incorporado recentemente global do Azure para testes de aceitação de utilizadores.
> * Assim que o seu código passou a testar, implemente automaticamente a pilha do Azure.

### <a name="about-the-hybrid-delivery-build-pipe"></a>Sobre a entrega híbrida criar o pipe

Continuidade de implementação de aplicação, segurança e fiabilidade é essencial para a sua organização e críticas para a sua equipa de desenvolvimento. Com um pipeline de CI/CD híbrida, pode consolidar os seus pipelines de entre o seu ambiente no local e nuvem pública. Pode alterar a localização sem comutar a sua aplicação.

Esta abordagem também permite-lhe manter um conjunto de ferramentas de desenvolvimento consistente. Ferramentas consistentes em nuvem pública do Azure e o seu ambiente de pilha do Azure no local significa que é muito mais fácil de implementar CI/CD dev prática. As aplicações e serviços implementados no Azure ou a pilha do Azure são permutáveis e o mesmo código pode ser executado em ambos, tirando partido de capacidades e funcionalidades de nuvem pública e no local.

Saiba mais sobre:
 - [O que é a integração contínua?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
 - [O que é a distribuição contínua?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)


## <a name="prerequisites"></a>Pré-requisitos

Terá de ter alguns componentes no local para criar um pipeline de CI/CD híbrida. Estes podem demorar algum tempo para preparar.
 
 - Um parceiro Azure OEM/hardware pode implementar uma pilha de Azure de produção e todos os utilizadores podem implementar um Kit de desenvolvimento de pilha do Azure (ASDK). 
 - Um operador de pilha do Azure deve também implementar o serviço de aplicações, criar planos e ofertas, criar uma subscrição de inquilino e adicionar a imagem do Windows Server 2016.

Se já tiver alguns destes componentes, certifique-se de que cumprem os requisitos antes de começar.

Este tópico também parte do princípio que tiver algum conhecimento do Azure e a pilha do Azure. Se pretender obter mais informações antes de continuar, é necessário começar com estes tópicos:


Este tutorial também parte do princípio que tiver algum conhecimento do Azure e a pilha do Azure. 

Se pretender obter mais informações antes de continuar, pode começar com estes tópicos:
 - [Introdução ao Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Conceitos chave de pilha do Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

 - Criar um [aplicação Web](https://docs.microsoft.com/azure/app-service/app-service-web-overview) no Azure. Anote o URL da aplicação Web novas, porque é utilizado mais tarde.

Azure Stack
 - Utilizar um sistema de pilha do Azure integrado ou implementar o Azure pilha Development Kit (ASDK) hiperligações em abaixo:
    - Pode encontrar instruções detalhadas sobre a implementação ASDK em "[Tutorial: implementar o ASDK utilizando o instalador](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy)"
    - Pode automatizar muitas dos passos de pós-implementação ASDK com o seguinte script do PowerShell, [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ).

    > [!Note]  
    > A instalação de ASDK demora um sete horas a concluir, por isso planeie em conformidade.

 - Implementar [do serviço de aplicações](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS serviços à pilha do Azure. 
 - Criar [plano/ofertas](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) no ambiente de pilha do Azure. 
 - Criar [subscrição de inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) no ambiente de pilha do Azure. 
 - Crie uma aplicação Web dentro da subscrição de inquilino. Anote o URL da aplicação Web novo para utilizar mais tarde.
 - Implemente a Máquina Virtual de VSTS, ainda dentro da subscrição de inquilino.
 - VM Windows Server 2016 com o .NET 3.5 necessário. Esta VM será criada na pilha do Azure como o agente de compilação privada. 

### <a name="developer-tools"></a>Ferramentas de programação

 - Criar um [área de trabalho VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services). O processo de inscrição cria um projeto com o nome **MyFirstProject**.
 - [Instalar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) e [início de sessão para VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
 - Ligar para o projeto e [clonar localmente](https://www.visualstudio.com/docs/git/gitquickstart).
 
 > [!Note]  
 > Precisa de pilha do Azure com imagens adequadas syndicated para executar (Windows Server e SQL) e tem o serviço de aplicações implementado.
 
## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Preparar a compilação privada e o agente de versão para a integração do Visual Studio Team Services

### <a name="prerequisites"></a>Pré-requisitos

Serviços de equipa do Visual Studio (VSTS) autentica contra o Azure Resource Manager utilizando um Principal de serviço. Para VSTS conseguir recursos aprovisionar uma subscrição de pilha do Azure, requer o estado de contribuinte.

Seguem-se os passos de alto nível que precisam de ser configuradas para permitir essa autenticação:

1. Deverá ser criado um Principal de serviço ou pode ser utilizado um existente.
2. Chaves de autenticação tem de ser criada para o Principal de serviço.
3. Subscrição de pilha do Azure tem de ser validada através de controlo de acesso baseado em funções para permitir o SPN ser parte da função de contribuinte.
4. Uma nova definição de serviço no VSTS deve ser criada utilizando os pontos finais de pilha do Azure, bem como informações de SPN.

### <a name="service-principal-creation"></a>Criação de principal de serviço

Consulte o [criação de principais de serviço](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) instruções para criar um principal de serviço e escolher aplicação/API Web para o tipo de aplicação.

### <a name="access-key-creation"></a>Criação de chave de acesso

Um Principal de serviço requer uma chave de autenticação, siga os passos nesta secção para gerar uma chave.


1. Em **Registos das aplicações** no Azure Active Directory, selecione a aplicação.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2.  Tome nota do valor de **ID da aplicação**. Irá utilizar esse valor quando configurar o ponto final do serviço no VSTS.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Para gerar uma chave de autenticação, selecione **Definições**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Para gerar uma chave de autenticação, selecione **Chaves**.
 
    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Indique uma descrição e uma duração para a chave. Quando terminar, selecione **Guardar**.
 
    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Depois de guardar a chave, o valor da mesma é apresentado. Copie este valor, porque não vai conseguir obter a chave mais tarde. Terá de fornecer o **valor chave** com o ID de aplicação para iniciar sessão como a aplicação. Armazene o valor da chave num local onde a aplicação o possa obter.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-tenant-id"></a>Obter o ID de inquilino

Como parte da configuração de ponto final do serviço, VSTS requer o **ID do inquilino** que corresponde do diretório do AAD que o carimbo de pilha do Azure foi implementado. Siga os passos nesta secção para recolher o ID do inquilino.

1. Selecione **Azure Active Directory**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Para obter o ID de inquilino, selecione as **Propriedades** do seu inquilino do Azure AD.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_08.png)
 
3. Copie o **ID do Diretório**. Este valor é o ID do inquilino.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>Conceder os direitos de principal de serviço para implementar os recursos na subscrição do Azure pilha 

Para aceder a recursos na sua subscrição, tem de atribuir a aplicação a uma função. Decida qual a função representa as permissões corretas para a aplicação. Para saber mais sobre as funções disponíveis, consulte [RBAC: funções incorporadas](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Pode definir o âmbito ao nível da subscrição, do grupo de recursos ou do recurso. As permissões são herdadas a níveis inferiores de âmbito. Por exemplo, adicionar uma aplicação para a função de leitor para um grupo de recursos significa podem ler o grupo de recursos e todos os recursos que nele contidos.

1. Navegue para o nível de âmbito que pretende atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito de subscrição, selecione **subscrições**. Em vez disso, pode selecionar um grupo de recursos ou um recurso.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. Selecione o **subscrição** (grupo de recursos ou recursos) para atribuir a aplicação.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. Selecione **(IAM) do controlo de acesso**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. Selecione **Adicionar**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. Selecione a função que pretende atribuir à aplicação. A imagem seguinte mostra o **proprietário** função.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Por predefinição, as aplicações do Azure Active Directory não são apresentadas nas opções disponíveis. Para localizar a aplicação, terá **forneça o nome** no campo de pesquisa. Selecione-o.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Selecione **guardar** para concluir a atribuição de função. Verá a aplicação na lista de utilizadores atribuídos a uma função para esse âmbito.

### <a name="role-based-access-control"></a>Controlo de Acesso Baseado em Funções

Azure baseada em funções controlo de acesso (RBAC) permite a gestão de acesso detalhada para o Azure. Utilizando o RBAC, pode conceder apenas a quantidade de acesso precisa aos utilizadores para realizar os trabalhos. Para obter mais informações sobre o controlo de acesso baseado em funções, consulte [gerir o acesso a recursos de subscrição do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>VSTS conjuntos de agente

Em vez de gerir individualmente cada agente, organizar os agentes em conjuntos de agente. Um agrupamento de agentes define o limite de partilha para todos os agentes no conjunto. No VSTS, conjuntos de agente estão no âmbito da conta VSTS por isso, pode partilhar um agrupamento de agentes em projetos de equipa. Para obter mais informações e um tutorial sobre como criar conjuntos de agente VSTS, consulte [criar conjuntos de agente e filas](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Adicionar um token de acesso pessoal (TERESA) para a pilha do Azure

1. Inicie sessão na sua conta VSTS e selecione o nome do perfil de conta.
2. Selecione **Gerir segurança** à página de criação de token de acesso.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Copie o token.
    
    > [!Note]  
    > Obter as informações do token. Não será apresentado depois de abandonar o fileparser neste ecrã. 
    
    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_19.png)
    

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Instalar que os VSTS Criar agente na pilha do Azure alojada criar servidor

1.  Ligar ao seu servidor de compilação que implementou no anfitrião de pilha do Azure.

2.  Transferir e implementar o agente de compilação como um serviço com o seu pessoal aceder token (TERESA) e executam como a conta de administrador de VM.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Aceda à pasta de agentes de compilação extraídos. Execute o **run.cmd** ficheiro a partir da linha de comandos elevada. 

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4.  Depois de concluída a run.cmd a pasta com o conteúdo extraído deve o seguinte aspeto:

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    Agora, pode ver o agente na pasta VSTS.

## <a name="endpoint-creation-permissions"></a>Permissões de criação de ponto final

Os utilizadores podem criar pontos finais para que VSTO compilações podem implementar aplicações de serviço do Azure para a pilha. VSTS estabelece ligação ao agente de compilação, em seguida, estabelece ligação com a pilha do Azure. 

![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. No **definições** menu, selecione **segurança**.
2. No **VSTS grupos** lista à esquerda, selecione **criadores de ponto final**. 

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

3. No **membros** separador, selecione **+ adicionar**. 

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

4. Escreva um nome de utilizador e selecione esse utilizador na lista.
5. Clique em **guardar alterações**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

6. No **VSTS grupos** lista à esquerda, selecione **administradores de ponto final**.
7. No **membros** separador, selecione **+ adicionar**.
8. Escreva um nome de utilizador e selecione esse utilizador na lista.
9. Clique em **guardar alterações**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

    O agente de compilação na pilha do Azure obtém as instruções do VSTS, que, em seguida, transmite informações de ponto final para a comunicação com a pilha do Azure. 
    
    VSTS a ligação de pilha do Azure está agora pronto.

## <a name="develop-your-application"></a>Desenvolver a sua aplicação

Configure híbrida CI/CD para implementar a aplicação Web do Azure e Azure pilha e automática envie as alterações para ambas as nuvens.

> [!Note]  
> Precisa de pilha do Azure com imagens adequadas syndicated para executar (Windows Server e SQL) e tem o serviço de aplicações implementado. Reveja a documentação do App Service secção "Pré-requisitos" requisitos de operador de pilha do Azure.

### <a name="add-code-to-vsts-project"></a>Adicionar código ao projeto VSTS

1. Inicie sessão no Visual Studio com uma conta que tenha direitos de criação do projeto na pilha do Azure.

    Híbrida CI/CD pode aplicar a código da aplicação e o código de infraestrutura. Utilize [modelos Azure Resource Manager, como web ](https://azure.microsoft.com/resources/templates/) código da aplicação de VSTS em ambas as nuvens.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **Clone o repositório** através da criação e abrir a aplicação de web predefinida.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Criar implementação de aplicações web autónomo para serviços de aplicações em ambas as nuvens

1. Editar o **WebApplication.csproj** ficheiro: selecione **Runtimeidentifier** e adicione `win10-x64.` para obter mais informações, consulte [implementação autónomo](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentação .

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Consulte o código no VSTS utilizando o Explorador de equipa.

3. Certifique-se de que o código de aplicação tiver verificado no Visual Studio Team Services. 

### <a name="create-the-build-definition"></a>Criar a definição de compilação

1. Inicie sessão no VSTS para confirmar a capacidade de criar definições de compilação.

2. Adicionar **- r win10-x64** código. Isto é necessário para acionar uma implementação autónomo com o .net Core. 

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

3. Execute a compilação. O [implementação autónomo compilação](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) processo irá publicar artefactos que podem ser executados no Azure e pilha do Azure.

### <a name="using-an-azure-hosted-agent"></a>Através de um agente alojado do Azure

Utilizar um agente alojado no VSTS é uma opção conveniente para criar e implementar aplicações web. Manutenção e as atualizações são executadas automaticamente pelo Microsoft Azure, permitindo expedição, ininterrupto desenvolvimento, teste e implementação.

### <a name="manage-and-configure-the-continuous-deployment-cd-process"></a>Gerir e configurar o processo de implementação contínua (CD)

Visual Studio Team Services (VSTS) e o Team Foundation Server (TFS) fornecem um pipeline altamente configurável e gerível para versões para vários ambientes, como o desenvolvimento, teste, pergunta e resposta e ambientes de produção incluindo a necessidade de aprovações em fases específicos.

### <a name="create-release-definition"></a>Criar a definição da versão

![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

1. Selecione o  **\[ +]** para adicionar uma nova versão sob o **separador versões** na página de compilação e a versão de VSO.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\102.png)

2. Aplicar o **implementação de serviço de aplicações do Azure** modelo.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\103.png)

3. No menu de pendente de artefacto de adicionar, **adicionar o artefacto** para a aplicação de compilação de nuvem do Azure.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\104.png)

4. No separador de Pipeline, selecione o **fase**, **tarefas** ligação do ambiente e definir valores de ambiente de nuvem do Azure.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\105.png)

5. Definir o **o nome do ambiente** e selecione Azure **subscrição** para o ponto final de nuvem do Azure.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\106.png)

6. Sob o nome do ambiente, defina necessários **nome do serviço de aplicações do Azure**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\107.png)

7. Introduza **alojado VS2017** em fila de agente para o ambiente do Azure na nuvem alojado.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\108.png)

8. No menu de implementar o serviço de aplicações do Azure, selecione válido **pacote ou de pasta** para o ambiente. Selecione **OK** para **localização da pasta**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\109.png)

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\110.png)

9. Guardar todas as alterações e regressar ao **versão pipeline**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\111.png)

10. Adicionar um **artefactos novo** selecionando a compilação da aplicação de pilha do Azure.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\112.png)

11. Adicionar uma aplicação de ambiente de mais de **implementação de serviço de aplicações do Azure**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\113.png)

12. Nome do novo ambiente **Azure pilha**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\114.png)

13. Localizar o ambiente de pilha do Azure em **tarefas** separador.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\115.png)

14. Selecione o **subscrição** para o ponto final de pilha do Azure.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\116.png)

15. Definir o nome de aplicação web de pilha do Azure como o **nome do serviço de aplicações**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\117.png)

16. Selecione o **agente Azure pilha**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\118.png)

17. Sob o serviço de aplicações do Azure implementar secção selecione válido **pacote ou de pasta** para o ambiente. Selecione OK para **localização da pasta**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\119.png)

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\120.png)

18. No separador variável adicionar uma variável com o nome **VSTS_ARM_REST_IGNORE_SSL_ERRORS**, defina o respetivo valor como **verdadeiro**e definir o âmbito para **Azure pilha**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\121.png)

19. Selecione o **Continuous** acionar ícone em ambos os artefactos de implementação e ativar o acionador de implementação Continues.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\122.png)

20. Selecione o **pré-implementação** ícone de condições do azure da pilha ambiente e definir o acionador para **após lançamento**.

21. Guarde todas as alterações.

> [!Note]  
> Algumas definições para as tarefas podem ter sido definidas automaticamente como [variáveis de ambiente](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) quando criou uma definição de versão de um modelo. Estas definições não podem ser modificadas nas definições de tarefas; em vez disso, tem de selecionar o item de ambiente de principal para editar estas definições.

## <a name="create-a-release"></a>Criar uma versão

Agora que concluiu as modificações à definição de versão, está na altura de iniciar a implementação. Para tal, crie uma versão da definição de versão. Uma versão pode ser criada automaticamente; Por exemplo, o acionador de implementação contínua está definido na definição da versão. Isto significa que a modificar o código fonte iniciará uma nova compilação e, de que uma nova versão. No entanto, esta secção, irá criar uma nova versão manualmente.

1. Abra o **versão** pendente lista e escolha **criar versão**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\200.png)
 
2. Introduza uma descrição para a versão, verifique os artefactos corretos são selecionados e, em seguida, escolha **criar**. Após alguns instantes, é apresentada uma faixa que indica que foi criada a nova versão. Escolha a ligação (o nome da versão).

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\201.png)
 
3. A página de resumo de versão abre-se que mostra detalhes da versão. No **ambientes** secção, irá ver o estado de implementação para o ambiente de "QA" alterar "Em curso" para "Com êxito" e, nesse momento, uma faixa é apresentada com a indicação de que a versão agora está a aguardar aprovação. Quando uma implementação num ambiente está pendente ou falhou, um blue (i) é apresentado o ícone de informações. Aponte para esta opção para ver um pop-up que contém o motivo.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\202.png)

Outras vistas, tais como a lista de versões, também apresentar um ícone que indica a aprovação está pendente. O ícone mostra um pop-up que contém o nome do ambiente e obter mais detalhes, quando aponta para a mesma. Isto torna mais fácil para um administrador ver as versões estão a aguardar aprovação, bem como o progresso global de todas as versões.

### <a name="monitor-and-track-deployments"></a>Monitorizar e controlar implementações

Nesta secção, irá ver como pode monitorizar e controlar implementações - neste exemplo dois sites de serviços de aplicações do Azure - a partir da versão que criou na secção anterior.

1. Na página de resumo de versão, escolha o **registos** ligação. Durante a implementação está a decorrer, esta página mostra o registo em direto do agente e, no painel esquerdo, uma indicação de estado de cada operação no processo de implementação para cada ambiente.

    Escolha o ícone no **ação** coluna de uma aprovação de pré-implementação ou pós-implementação para ver os detalhes da pessoa que aprovado (ou rejeitado) a implementação e a mensagem que é fornecido pelo utilizador.

2. Depois de concluída a implementação, todo o ficheiro de registo é apresentado no painel direito. Selecione qualquer uma do **processar passos** no painel da esquerda para mostrar apenas o registo de conteúdos do ficheiro para esse passo. Isto torna mais fácil de rastreio e depuração partes individuais da implementação geral. Em alternativa, transferir os ficheiros de registo individuais, ou um zip de todos os ficheiros de registo, dos ícones e hiperligações na página.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\203.png)
 
3. Abra o **resumo** separador para ver os detalhes global da versão. Mostra detalhes sobre a compilação e os ambientes que foi implementada, juntamente com o estado de implementação e outras informações sobre a versão.

4. Selecione cada um do **ligações de ambiente** para ver mais detalhes sobre existente e implementações para esse ambiente específico pendentes. Pode utilizar estas páginas para verificar que a mesma compilação foi implementada para ambos os ambientes.

5. Abra o **implementar a aplicação de produção** na sua procura. Por exemplo, para um site de serviços de aplicações do Azure, do URL `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre os padrões de nuvem do Azure, consulte [padrões de conceção de nuvem](https://docs.microsoft.com/azure/architecture/patterns).
