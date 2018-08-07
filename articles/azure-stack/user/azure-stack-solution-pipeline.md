---
title: Implementar a aplicação no Azure e o Azure Stack | Documentos da Microsoft
description: Saiba como implementar aplicações no Azure e o Azure Stack com um pipeline de CI/CD híbrida.
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
ms.date: 06/08/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 3fcede7f813e97885d8fc3d7e0bc04776f2d0d12
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582145"
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Tutorial: implementar aplicações no Azure e o Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Saiba como implementar uma aplicação do Azure e o Azure Stack com um pipeline de entrega (CI/CD) de integração contínua/contínua de híbrida.

Neste tutorial, irá criar um ambiente de exemplo para:

> [!div class="checklist"]
> * Inicie uma nova compilação com base nas consolidações de código para o seu repositório do Visual Studio Team Services (VSTS).
> * Implemente automaticamente a sua aplicação para o Azure global para o teste de aceitação do usuário.
> * Quando seu código transmite o teste, implemente automaticamente a aplicação no Azure Stack.

## <a name="benefits-of-the-hybrid-delivery-build-pipe"></a>Benefícios da entrega híbrida criar pipe

Continuidade, segurança e confiabilidade são elementos fundamentais da implementação da aplicação. Estes elementos são essenciais para a sua organização e críticas para a sua equipa de desenvolvimento. Um pipeline de CI/CD híbrida permite-lhe consolidar os pipes de compilação em seu ambiente no local e a cloud pública. Um modelo de entrega híbrida também permite-lhe alterar as localizações de implantação sem alterar a sua aplicação.

Outros benefícios, para usar a abordagem híbrida são:

* Pode manter um conjunto consistente de ferramentas de desenvolvimento em seu ambiente do Azure Stack no local e nuvem pública do Azure.  Um conjunto de ferramentas comuns torna mais fácil de implementar o CI/CD padrões e práticas.
* As aplicações e serviços implementados no Azure ou no Azure Stack são intercambiáveis e o mesmo código pode ser executado em ambos os locais. Pode tirar partido das capacidades e recursos de nuvem pública e no local.

Para saber mais sobre CI e CD:

* [O que é a integração contínua?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [O que é a entrega contínua?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter os componentes no local para criar um pipeline de CI/CD híbrida. Os seguintes componentes levará tempo para preparar:

* Um parceiro OEM/hardware do Azure, pode implementar um Azure Stack de produção. Todos os utilizadores podem implementar o Azure Stack Development Kit (ASDK).
* Também tem um operador do Azure Stack: implementar o serviço de aplicações, criar planos e ofertas, criar uma subscrição de inquilino e adicionar a imagem do Windows Server 2016.

>[!NOTE]
>Se já tiver alguns desses componentes implementado, certifique-se de que cumprem todos os requisitos antes de começar este tutorial.

Este tutorial parte do princípio de que tenha algum conhecimento básico do Azure e o Azure Stack. Para saber mais antes de iniciar o tutorial, leia os artigos seguintes:

* [Introdução ao Azure](https://azure.microsoft.com/overview/what-is-azure/)
* [Conceitos-chave do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure-requirements"></a>Requisitos do Azure

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
* Criar uma [aplicação Web](https://docs.microsoft.com/azure/app-service/app-service-web-overview) no Azure. Tenha em atenção de URL da aplicação Web de fazer, terá de utilizá-lo no tutorial.

### <a name="azure-stack-requirements"></a>Requisitos do Azure Stack

* Utilizar um sistema integrado do Azure Stack ou implementar o Azure Stack Development Kit (ASDK). Para implementar o ASDK:
    * O [Tutorial: implementar o ASDK através do instalador](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy) dá instruções de implementação detalhados.
    * Utilize o [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) script do PowerShell para automatizar passos de pós-implementação ASDK.

    > [!Note]
    > A instalação de ASDK demora cerca de sete horas a concluir, então, planeie em conformidade.

 * Implementar [serviço de aplicações](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) serviços de PaaS para o Azure Stack.
 * Crie [plano/ofertas](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) no Azure Stack.
 * Criar uma [subscrição do inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) no Azure Stack.
 * Crie uma aplicação Web na subscrição do inquilino. Tome nota do novo URL de aplicação Web para utilizar mais tarde.
 * Implemente a máquina de Virtual de VSTS na subscrição do inquilino.
* Forneça uma imagem do Windows Server 2016 com o .NET 3.5 para uma máquina virtual (VM). Esta VM será criado no seu Azure Stack como um agente de compilação privada.

### <a name="developer-tool-requirements"></a>Requisitos da ferramenta de desenvolvedor

* Criar uma [área de trabalho do VSTS](https://docs.microsoft.com/vsts/repos/tfvc/create-work-workspaces). O processo de inscrição cria um projeto chamado **MyFirstProject**.
* [Instale o Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) e [iniciar sessão no VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
* Ligar ao seu projeto e [cloná-lo localmente](https://www.visualstudio.com/docs/git/gitquickstart).

 > [!Note]
 > O seu ambiente do Azure Stack tem as imagens corretas distribuídas para executar o Windows Server e SQL Server. Também tem de ter implementado o serviço de aplicações.

## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Preparar o compilação privada e o agente de lançamento para a integração do Visual Studio Team Services

### <a name="prerequisites"></a>Pré-requisitos

Visual Studio Team Services (VSTS) autentica contra o Azure Resource Manager com um Principal de serviço. VSTS tem de ter o **contribuinte** função para aprovisionar os recursos numa subscrição do Azure Stack.

Os passos seguintes descrevem o que é necessário para configurar a autenticação:

1. Criar um Principal de serviço ou utilizar um Principal de serviço existente.
2. Crie chaves de autenticação para o Principal de serviço.
3. Valide a subscrição do Azure Stack através do controlo de acesso baseado em funções para permitir que o serviço Principal Name (SPN) para fazer parte da função de contribuinte.
4. Crie uma nova definição de serviço no VSTS com os pontos finais do Azure Stack e as informações de SPN.

### <a name="create-a-service-principal"></a>Criar um Principal de serviço

Consulte a [criação do Principal de serviço](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) instruções para criar um principal de serviço e, em seguida, escolha **aplicação/API Web** para o tipo de aplicação.

### <a name="create-an-access-key"></a>Criar uma chave de acesso

Um Principal de serviço requer uma chave para autenticação. Utilize os seguintes passos para gerar uma chave.

1. Em **Registos das aplicações** no Azure Active Directory, selecione a aplicação.

    ![Selecione a aplicação](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2. Tome nota do valor de **ID da aplicação**. Irá utilizar esse valor quando configurar o ponto final de serviço no VSTS.

    ![ID da aplicação](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Para gerar uma chave de autenticação, selecione **Definições**.

    ![Editar definições de aplicação](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Para gerar uma chave de autenticação, selecione **Chaves**.

    ![Configurar definições de chave](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Forneça uma descrição para a chave e definir a duração da chave. Quando terminar, selecione **Guardar**.

    ![Descrição da chave e a duração](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Depois de guardar a chave, a chave **valor** é apresentado. Copie este valor, porque não é possível obter este valor mais tarde. Forneça o **valor da chave** com o ID da aplicação para iniciar sessão como o aplicativo. Armazene o valor da chave num local onde a aplicação o possa obter.

    ![VALOR de chave](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-the-tenant-id"></a>Obter o ID de inquilino

Como parte da configuração do ponto final de serviço, o VSTS requer o **ID de inquilino** que corresponde ao diretório do AAD que o carimbo de data / Azure Stack é implementado. Utilize os seguintes passos para obter o ID de inquilino.

1. Selecione **Azure Active Directory**.

    ![O Azure Active Directory para o inquilino](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Para obter o ID de inquilino, selecione as **Propriedades** do seu inquilino do Azure AD.

    ![Ver as propriedades do inquilino](media\azure-stack-solution-hybrid-pipeline\000_08.png)

3. Copie o **ID do Diretório**. Este valor é o ID do inquilino.

    ![ID do diretório](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>Conceder os direitos de principal de serviço para implementar recursos na subscrição do Azure Stack

Para acessar recursos na sua subscrição, tem de atribuir a aplicação a uma função. Decida que função representa as permissões recomendadas para a aplicação. Para saber mais sobre as funções disponíveis, veja [RBAC: funções incorporadas](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Pode definir o âmbito no nível da subscrição, no grupo de recursos ou ao recurso. As permissões são herdadas para níveis inferiores de âmbito. Por exemplo, adicionar uma aplicação à função do leitor para um grupo de recursos significa pode ler o grupo de recursos e qualquer um dos seus recursos.

1. Navegue para o nível de âmbito que pretende atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito da subscrição, selecione **subscrições**.

    ![Selecione as subscrições](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. Na **subscrição**, selecione o Visual Studio Enterprise.

    ![Visual Studio Enterprise](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. No Visual Studio Enterprise, selecione **controlo de acesso (IAM)**.

    ![Controlo de acesso (IAM)](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. Selecione **Adicionar**.

    ![Adicionar](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. Na **adicionar permissões**, selecione a função que pretende atribuir à aplicação. Neste exemplo, o **proprietário** função.

    ![Função de proprietário](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Por predefinição, as aplicações do Azure Active Directory não são apresentadas nas opções disponíveis. Para localizar a sua aplicação, tem de fornecer o respetivo nome no **selecione** campo para procurá-lo. Selecione a aplicação.

    ![Resultado da pesquisa de aplicação](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Selecione **guardar** para concluir a atribuir a função. Pode ver a sua aplicação na lista de utilizadores atribuídos a uma função para esse âmbito.

### <a name="role-based-access-control"></a>Controlo de Acesso Baseado em Funções

Do Azure com base em função de controlo de acesso (RBAC) proporciona gestão de acessos detalhada para o Azure. Ao utilizar o RBAC, pode controlar o nível de acesso que os utilizadores precisam para realizarem seus trabalhos. Para obter mais informações sobre o controlo de acesso baseado em funções, consulte [gerir o acesso aos recursos de subscrição do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>Conjuntos de agentes do VSTS

Em vez de gerir cada agente separadamente, pode organizar agentes em conjuntos de agentes. Um conjunto de agentes define o limite de partilha para todos os agentes nesse agrupamento. Conjuntos de agentes do VSTS, passam a ter para a conta do VSTS, o que significa que pode partilhar um conjunto de agentes entre projetos de equipe. Para saber mais sobre conjuntos de agentes, consulte [criar conjuntos de agentes e filas](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Adicionar um Token de acesso pessoal (PAT) para o Azure Stack

Crie um Token de acesso pessoal para aceder ao VSTS.

1. Inicie sessão na sua conta do VSTS e selecione o nome do perfil de conta.
2. Selecione **gerir a segurança** à página de criação de token de acesso.

    ![Início de sessão de utilizador](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Selecione o projeto de equipe](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Adicionar token de acesso pessoal](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Criar token](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Copie o token.

    > [!Note]
    > Guarde as informações do token. Estas informações não estão armazenadas e não serão apresentadas novamente quando deixar a página da web.

    ![Token de acesso pessoal](media\azure-stack-solution-hybrid-pipeline\000_19.png)

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Instalar que agente de compilação VSTS na pilha do Azure alojado o servidor de compilação do

1. Ligar ao seu servidor de compilação de mensagens em fila que implementou no anfitrião do Azure Stack.
2. Baixar e implantar o agente de compilação como um serviço com o seu pessoal (PAT) de token de acesso e executam como a conta de administrador de VM.

    ![Transferir o agente de compilação](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Navegue para a pasta para o agente de compilação extraídos. Executar o **config.cmd** ficheiro a partir da linha de comandos elevada.

    ![Agente de compilação extraídos](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Registar o agente de compilação](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4. Quando o config.cmd estiver concluída, na pasta de agentes de compilação é atualizada com ficheiros adicionais. A pasta com o conteúdo extraído deve ter um aspeto semelhante ao seguinte:

    ![Criar agente de atualização de pasta](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    Pode ver o agente na pasta do VSTS.

## <a name="endpoint-creation-permissions"></a>Permissões de criação do ponto final

Através da criação de pontos de extremidade, uma compilação do Visual Studio Online (VSTO) pode implementar aplicações de serviço do Azure para o Azure Stack. VSTS liga-se para o agente de compilação, que liga ao Azure Stack.

![Aplicação de exemplo NorthwindCloud no VSTO](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. Inicie sessão para o VSTO e navegue para a página de definições.
2. No **configurações**, selecione **segurança**.
3. Na **grupos de VSTS**, selecione **criadores de ponto final**.

    ![Criadores de ponto final de NorthwindCloud](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

4. Sobre o **membros** separador, selecione **Add**.

    ![Adicionar um membro](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

5. Na **adicionar utilizadores e grupos**, introduza um nome de utilizador e selecione o que o utilizador da lista de utilizadores.
6. Selecione **guardar alterações**.
7. Na **grupos de VSTS** lista, selecione **administradores do ponto final**.

    ![Administradores de ponto final de NorthwindCloud](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

8. Sobre o **membros** separador, selecione **Add**.
9. Na **adicionar utilizadores e grupos**, introduza um nome de utilizador e selecione o que o utilizador da lista de utilizadores.
10. Selecione **guardar alterações**.

Agora que as informações de ponto final existem, o VSTS para ligação do Azure Stack está pronta a utilizar. O agente de compilação no Azure Stack obtém as instruções do VSTS e, em seguida, o agente transmite informações de ponto final para comunicação com o Azure Stack.

![Agente de compilação](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

## <a name="develop-your-application-build"></a>Desenvolver a sua compilação de aplicação

Nesta parte do tutorial, irá:

* Adicione o código para um projeto VSTS.
* Crie implementação da aplicação web autossuficiente.
* Configurar o processo de implementação contínua

> [!Note]
 > O seu ambiente do Azure Stack tem as imagens corretas distribuídas para executar o Windows Server e SQL Server. Também tem de ter implementado o serviço de aplicações. Reveja a documentação do serviço de aplicações secção "Pré-requisitos" para os requisitos de operador do Azure Stack.

Pode aplicar híbrida CI/CD de código do aplicativo e o código de infraestrutura. Uso [modelos do Azure Resource Manager, como web ](https://azure.microsoft.com/resources/templates/) código da aplicação do VSTS para implementar em ambas as nuvens.

### <a name="add-code-to-a-vsts-project"></a>Adicione o código para um projeto do VSTS

1. Inicie sessão no VSTS com uma conta que tenha direitos de criação do projeto no Azure Stack. Captura de ecrã seguinte mostra como ligar ao projeto HybridCICD.

    ![Ligar a um projeto](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **Clonar o repositório** ao criar e abrir a aplicação de web padrão.

    ![Repositório de clone](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Criar implementação da aplicação web autônomo para serviços de aplicações em ambas as nuvens

1. Editar a **WebApplication.csproj** ficheiro: selecionar **Runtimeidentifier** e, em seguida, adicione `win10-x64.` para obter mais informações, consulte [implementação autónoma e contém](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentação.

    ![Configurar Runtimeidentifier](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Utilize o Team Explorer para verificar o código no VSTS.

3. Confirme que o código da aplicação foi verificado no Visual Studio Team Services.

### <a name="create-the-build-definition"></a>Criar a definição de compilação

1. Inicie sessão no VSTS com uma conta que pode criar uma definição de compilação.
2. Navegue para o **criar o Web Applicaiton** página para o projeto.

3. Na **argumentos**, adicione **win10-x64 - r** código. Isto é necessário para acionar uma implementação independente com .net Core.

    ![Adicionar definição de compilação do argumento](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

4. Execute a compilação. O [compilação de implementação autónoma e contém](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) processo irá publicar artefactos que podem ser executadas no Azure e no Azure Stack.

### <a name="use-an-azure-hosted-build-agent"></a>Utilização do Azure alojada agente de compilação

Utilizar um agente de compilação hospedado no VSTS é uma opção conveniente para a criação e implementação de aplicações web. Atualizações e manutenção de agente seja executadas automaticamente pelo Microsoft Azure, que permite que um ciclo de desenvolvimento contínuo e sem interrupções.

### <a name="configure-the-continuous-deployment-cd-process"></a>Configurar o processo de implementação contínua (CD)

O Visual Studio Team Services (VSTS) e o Team Foundation Server (TFS) fornecem um pipeline totalmente configurável e gerenciável para as versões em vários ambientes, tais como o desenvolvimento, teste, controle de qualidade (QA) e a produção. Esse processo pode incluir a necessidade de aprovações, específicos estágios do ciclo de vida do aplicativo.

### <a name="create-release-definition"></a>Criar definição de versão

A criação de uma definição de versão é o processo de compilação a etapa final na sua aplicação. Esta definição de versão é utilizada para criar uma versão e implementar uma compilação.

1. Inicie sessão no VSTS e navegue para **criar e lançar** para o seu projeto.
2. Sobre o **versões** separador, selecione  **\[ +]** e, em seguida, escolha **Criar definição de versão**.

   ![Criar definição de versão](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

3. No **selecionar um modelo**, escolha **implementação de serviço de aplicações do Azure**e, em seguida, selecione **aplicar**.

    ![Aplicar modelo](media\azure-stack-solution-hybrid-pipeline\102.png)

4. No **adicionar artefacto**, da **origem (definição de compilação)** menu pendente, selecione a aplicação de compilação na Cloud do Azure.

    ![Adicionar artefacto](media\azure-stack-solution-hybrid-pipeline\103.png)

5. Sobre o **Pipeline** separador, selecione a **1 fase**, **1 tarefa** ligar a **ver tarefas do ambiente**.

    ![Tarefas da vista de pipeline](media\azure-stack-solution-hybrid-pipeline\104.png)

6. Na **tarefas** separador, introduza Azure como o **nome do ambiente** e selecione EP de Traders Web AzureCloud do **subscrição do Azure** na lista pendente.

    ![Definir variáveis de ambiente](media\azure-stack-solution-hybrid-pipeline\105.png)

7. Introduza o **nome do serviço de aplicações do Azure**, que é "northwindtraders" na captura de ecrã seguinte.

    ![Nome do serviço de aplicações](media\azure-stack-solution-hybrid-pipeline\106.png)

8. Para a fase de agente, selecione **Hosted VS2017** partir do **fila de agente** na lista pendente.

    ![Agentes alojados](media\azure-stack-solution-hybrid-pipeline\107.png)

9. Na **implementar serviço de aplicações do Azure**, selecione o válido **pacote ou de pasta** para o ambiente.

    ![Selecione o pacote ou de pasta](media\azure-stack-solution-hybrid-pipeline\108.png)

10. Na **selecione o ficheiro ou pasta**, selecione **OK** para **localização**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\109.png)

11. Guarde todas as alterações e voltar à **Pipeline**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\110.png)

12. Na **Pipeline** separador, selecione **adicionar artefacto**e selecione o **NorthwindCloud Traders-Vessel** do **origem (definição de compilação)** na lista pendente.

    ![Adicionar artefacto novo](media\azure-stack-solution-hybrid-pipeline\111.png)

13. No **selecionar um modelo**, adicionar outro ambiente. Escolher **implementação de serviço de aplicações do Azure** e, em seguida, selecione **aplicar**.

    ![Selecionar modelo](media\azure-stack-solution-hybrid-pipeline\112.png)

14. Introduza "Azure Stack" como o **nome do ambiente**.

    ![Nome do ambiente](media\azure-stack-solution-hybrid-pipeline\113.png)

15. Sobre o **tarefas** separador, localize e selecione o Azure Stack.

    ![Ambiente do Azure Stack](media\azure-stack-solution-hybrid-pipeline\114.png)

16. Partir do **subscrição do Azure** pendente, selecione "AzureStack Traders Vessel EP" para o ponto de final do Azure Stack.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\115.png)

17. Introduza o nome da aplicação web do Azure Stack como o **nome do serviço de aplicações**.

    ![Nome do serviço de aplicações](media\azure-stack-solution-hybrid-pipeline\116.png)

18. Sob **seleção do agente**, escolha "AzureStack - bDouglas preencha" entre a **fila de agente** na lista pendente.

    ![Escolher agente](media\azure-stack-solution-hybrid-pipeline\117.png)

19. Para **implementar serviço de aplicações do Azure**, selecione o válido **pacote ou de pasta** para o ambiente. No **selecionar ficheiro ou pasta**, selecione **OK** para a pasta **localização**.

    ![Escolha o pacote ou de pasta](media\azure-stack-solution-hybrid-pipeline\118.png)

    ![Aprovar localização](media\azure-stack-solution-hybrid-pipeline\119.png)

20. Sobre o **variável** separador, encontre a variável chamada **VSTS_ARM_REST_IGNORE_SSL_ERRORS**. Defina o valor da variável para **true**e defina seu escopo para **do Azure Stack**.

    ![Configurar a variável](media\azure-stack-solution-hybrid-pipeline\120.png)

21. Sobre o **Pipeline** separador, selecione a **acionador de implementação contínua** ícone para o artefacto de NorthwindCloud Traders-Web e o conjunto a **acionador de implementação contínua** para **Ativada**.  Fazer a mesma coisa para o artefacto "NorthwindCloud Traders-Vessel".

    ![Acionador de implementação contínua de conjunto](media\azure-stack-solution-hybrid-pipeline\121.png)

22. Para o ambiente do Azure Stack, selecione o **condições de pré-implantação** ícone defina o gatilho **após o lançamento**.

    ![Acionador de condições de pré-implantação do conjunto](media\azure-stack-solution-hybrid-pipeline\122.png)

23. Guarde todas as alterações.

> [!Note]
> Algumas definições para tarefas de lançamento podem ter sido automaticamente definidas como [variáveis de ambiente](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) ao criar uma definição de versão a partir de um modelo. Estas definições não podem ser modificadas nas definições de tarefa. No entanto, pode editar estas definições nos itens de ambiente de principal.

## <a name="create-a-release"></a>Criar uma versão

Agora que concluiu as modificações para a definição de versão, chegou a hora para iniciar a implementação. Para tal, crie uma versão da definição de versão. Uma versão pode ser criada automaticamente; Por exemplo, o acionador de implementação contínua está definido na definição de versão. Isso significa que a modificar o código-fonte iniciará uma nova compilação e, a partir disso, uma nova versão. No entanto, nesta secção, irá criar uma nova versão manualmente.

1. Na **Pipeline** separador, abra o **versão** pendente lista e escolha **criar versão**.

    ![Criar uma versão](media\azure-stack-solution-hybrid-pipeline\200.png)

2. Introduza uma descrição para a versão, verifique que os artefactos corretos estão selecionados e, em seguida, escolha **criar**. Após alguns instantes, é apresentada uma faixa que indica que foi criada a nova versão e o nome de versão é apresentado como um link. Escolha a ligação para ver a página de resumo de lançamento.

    ![Faixa de criação de versão](media\azure-stack-solution-hybrid-pipeline\201.png)

3. A página de resumo de lançamento para mostra detalhes sobre a versão. Na captura de ecrã seguinte para "Release-2", o **ambientes** secção mostra o **estado de implementação** para o Azure como "Em curso" e o estado para o Azure Stack é "concluído com êxito". Quando o estado de implementação para o ambiente do Azure é alterado para "Êxito", é apresentada uma faixa que indica que a versão está pronta para aprovação. Quando uma implementação está pendente ou falhou, uma azul **(i)** é mostrado o ícone de informações. Coloque o cursor sobre o ícone para ver um pop-up que contém o motivo do atraso ou falha.

    ![Página de resumo de versão](media\azure-stack-solution-hybrid-pipeline\202.png)

Outras exibições, como a lista de versões, também irá apresentar um ícone que indica a aprovação está pendente. O pop-up para este ícone mostra o nome do ambiente e obter mais detalhes relacionados com a implementação. É fácil para um administrador, consulte o progresso global de versões e veja que versões estão a aguardar aprovação.

### <a name="monitor-and-track-deployments"></a>Monitorar e controlar Implantações

Esta secção mostra como pode monitorizar e controlar todas as suas implementações. A versão para implementar os dois sites de serviços aplicacionais do Azure fornece um bom exemplo.

1. Na página de resumida de "Lançamento-2", selecione **registos**. Durante uma implantação, esta página mostra o registo em direto do agente. O painel esquerdo apresenta o estado de cada operação na implementação de cada ambiente.

    Pode escolher um ícone de pessoa no **ação** coluna para uma aprovação de pré-implantação ou pós-implementação para ver quem aprovado (ou rejeitado) a implementação e a mensagem fornecidos por eles.

2. Após a conclusão da implementação, todo o arquivo de log é apresentado no painel da direita. Pode selecionar qualquer **passo** no painel da esquerda para ver o ficheiro de registo para um passo único, por exemplo, "A inicializar o trabalho". A capacidade de ver os registos individuais torna mais fácil rastrear e depurar as partes da implementação geral. Também pode **salvar** o ficheiro de registo de um passo ou **transferir os registos de todas as jako zip**.

    ![Registos de versão](media\azure-stack-solution-hybrid-pipeline\203.png)

3. Abra o **resumo** separador para ver informações gerais sobre o lançamento. Esta vista mostra detalhes sobre a compilação, os ambientes que foi implantado, estado de implementação e outras informações sobre a versão.

4. Selecione uma ligação de ambiente (**Azure** ou **do Azure Stack**) para ver informações sobre existente e implementações para um ambiente específico pendentes. Pode usar esses modos de exibição como uma forma rápida de verificar que a mesma compilação foi implementada para os dois ambientes.

5. Abra o **implementado a aplicação de produção** no seu browser. Por exemplo, para o Web site dos serviços de aplicações do Azure, abra o URL `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os padrões de Cloud do Azure, veja [padrões de conceção de Cloud](https://docs.microsoft.com/azure/architecture/patterns).
