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
ms.openlocfilehash: 41e6f64ada7c95674cc2573048eef8afc83e4385
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604357"
---
# <a name="tutorial-deploy-apps-to-azure-and-azure-stack"></a>Tutorial: implementar aplicações do Azure e pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Saiba como implementar uma aplicação do Azure e pilha do Azure através de um pipeline de entrega (CI/CD) integração contínua/contínua híbrida.

Neste tutorial, irá criar um ambiente de exemplo para:

> [!div class="checklist"]
> * Inicie uma nova compilação com base no código consolidações para o seu repositório de serviços de equipa do Visual Studio (VSTS).
> * Implemente automaticamente a aplicação no Azure global para testes de aceitação de utilizadores.
> * Quando o seu código transmite testar, implemente automaticamente a aplicação Azure pilha.

## <a name="benefits-of-the-hybrid-delivery-build-pipe"></a>Vantagens de entrega híbrida criar o pipe

Continuidade, segurança e fiabilidade são elementos essenciais da implementação de aplicação. Estes elementos são essenciais para a sua organização e críticas para a sua equipa de desenvolvimento. Um pipeline de CI/CD híbrida permite-lhe consolidar os pipes compilação em ambiente no local e nuvem pública. Um modelo de entrega híbrida também lhe permite alterar as localizações de implementação sem alterar a sua aplicação.

Outros benefícios para utilizar a abordagem de híbridas são:

* Pode manter um conjunto de ferramentas de desenvolvimento consistente entre o seu ambiente de pilha do Azure no local e nuvem pública do Azure.  Um conjunto de ferramentas comuns torna mais fácil de implementar CI/CD padrões e práticas.
* As aplicações e serviços implementados no Azure ou a pilha do Azure são permutáveis e o mesmo código pode ser executado em qualquer localização. Pode tirar partido das capacidades e funcionalidades de nuvem pública e no local.

Para saber mais sobre CI e CD:

* [O que é a integração contínua?](https://www.visualstudio.com/learn/what-is-continuous-integration/)
* [O que é a distribuição contínua?](https://www.visualstudio.com/learn/what-is-continuous-delivery/)

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter os componentes no local para criar um pipeline de CI/CD híbrida. Os seguintes componentes irão demorar tempo para preparar:

* Um parceiro Azure OEM/hardware pode implementar uma pilha de Azure de produção. Todos os utilizadores podem implementar o Kit de desenvolvimento de pilha do Azure (ASDK).
* Também tem um operador de pilha do Azure: implementar o serviço de aplicações, criar planos e ofertas, criar uma subscrição de inquilino e adicionar a imagem do Windows Server 2016.

>[!NOTE]
>Se já tiver alguns destes componentes implementado, certifique-se de que as mesmas cumprem os todos os requisitos antes de começar este tutorial.

Este tutorial parte do princípio de que tiver algum conhecimento básico do Azure e pilha do Azure. Para obter mais informações antes de iniciar o tutorial, leia os artigos seguintes:

* [Introdução ao Azure](https://azure.microsoft.com/overview/what-is-azure/)
* [Conceitos chave de pilha do Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure-requirements"></a>Requisitos do Azure

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
* Criar um [aplicação Web](https://docs.microsoft.com/azure/app-service/app-service-web-overview) no Azure. Certifique-tenha em atenção de que o URL da aplicação Web, terá de utilizá-lo no tutorial.

### <a name="azure-stack-requirements"></a>Requisitos de pilha do Azure

* Utilizar um sistema de pilha do Azure integrado ou implementar o Kit de desenvolvimento de pilha do Azure (ASDK). Para implementar o ASDK:
    * O [Tutorial: implementar o ASDK utilizando o instalador](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy) dá instruções de implementação detalhados.
    * Utilize o [ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 ) script do PowerShell para automatizar passos de pós-implementação ASDK.

    > [!Note]
    > A instalação de ASDK demora cerca de sete horas a concluir, por isso planeie em conformidade.

 * Implementar [do serviço de aplicações](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS serviços à pilha do Azure.
 * Criar [plano/ofertas](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) na pilha do Azure.
 * Criar um [subscrição de inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) na pilha do Azure.
 * Crie uma aplicação Web na subscrição do inquilino. Anote o URL da aplicação Web novo para utilizar mais tarde.
 * Implemente Máquina Virtual de VSTS na subscrição do inquilino.
* Forneça uma imagem do Windows Server 2016 com o .NET 3.5 para uma máquina virtual (VM). Esta VM vai ser criada na pilha do Azure como um agente de compilação privada.

### <a name="developer-tool-requirements"></a>Requisitos da ferramenta de programador

* Criar um [área de trabalho VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services). O processo de inscrição cria um projeto com o nome **MyFirstProject**.
* [Instalar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) e [início de sessão para VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).
* Ligar ao seu projeto e [clone-o localmente](https://www.visualstudio.com/docs/git/gitquickstart).

 > [!Note]
 > O ambiente de pilha do Azure tem das imagens corretas syndicated para executar o Windows Server e SQL Server. Também tem de ter implementado do serviço de aplicações.

## <a name="prepare-the-private-build-and-release-agent-for-visual-studio-team-services-integration"></a>Preparar a compilação privada e o agente de versão para a integração do Visual Studio Team Services

### <a name="prerequisites"></a>Pré-requisitos

Serviços de equipa do Visual Studio (VSTS) autentica contra o Azure Resource Manager utilizando um Principal de serviço. VSTS tem de ter o **contribuinte** função de recursos de aprovisionar uma subscrição de pilha do Azure.

Os passos seguintes descrevem o que necessárias para configurar a autenticação:

1. Criar um Principal de serviço, ou utilize um Principal de serviço existente.
2. Crie chaves de autenticação para o Principal de serviço.
3. Valide a subscrição de pilha do Azure através do controlo de acesso baseado em funções para permitir que o serviço de nome Principal (SPN) como parte da função de contribuinte.
4. Crie uma nova definição de serviço no VSTS utilizando as informações de SPN e os pontos finais de pilha do Azure.

### <a name="create-a-service-principal"></a>Criar um Principal de serviço

Consulte o [criação de principais de serviço](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) instruções para criar um principal de serviço e, em seguida, escolha **aplicação/API Web** para o tipo de aplicação.

### <a name="create-an-access-key"></a>Criar uma chave de acesso

Um Principal de serviço requer uma chave de autenticação. Utilize os seguintes passos para gerar uma chave.

1. Em **Registos das aplicações** no Azure Active Directory, selecione a aplicação.

    ![Selecione a aplicação](media\azure-stack-solution-hybrid-pipeline\000_01.png)

2. Tome nota do valor de **ID da aplicação**. Irá utilizar esse valor quando configurar o ponto final do serviço no VSTS.

    ![ID da aplicação](media\azure-stack-solution-hybrid-pipeline\000_02.png)

3. Para gerar uma chave de autenticação, selecione **Definições**.

    ![Editar definições de aplicação](media\azure-stack-solution-hybrid-pipeline\000_03.png)

4. Para gerar uma chave de autenticação, selecione **Chaves**.

    ![Configurar definições de chave](media\azure-stack-solution-hybrid-pipeline\000_04.png)

5. Forneça uma descrição para a chave e definir a duração da chave. Quando terminar, selecione **Guardar**.

    ![Descrição de chave e duração](media\azure-stack-solution-hybrid-pipeline\000_05.png)

    Depois de guardar a chave, a chave **valor** é apresentado. Copie este valor, porque não é possível obter este valor mais tarde. Terá de fornecer o **valor chave** com o ID de aplicação para iniciar sessão como a aplicação. Armazene o valor da chave num local onde a aplicação o possa obter.

    ![VALOR de chave](media\azure-stack-solution-hybrid-pipeline\000_06.png)

### <a name="get-the-tenant-id"></a>Obter o ID de inquilino

Como parte da configuração de ponto final do serviço, VSTS requer o **ID do inquilino** que corresponde ao diretório do AAD que está implementado o carimbo de pilha do Azure. Utilize os seguintes passos para obter o ID de inquilino.

1. Selecione **Azure Active Directory**.

    ![Azure Active Directory para o tenant](media\azure-stack-solution-hybrid-pipeline\000_07.png)

2. Para obter o ID de inquilino, selecione as **Propriedades** do seu inquilino do Azure AD.

    ![Ver propriedades de inquilino](media\azure-stack-solution-hybrid-pipeline\000_08.png)

3. Copie o **ID do Diretório**. Este valor é o ID do inquilino.

    ![ID do diretório](media\azure-stack-solution-hybrid-pipeline\000_09.png)

### <a name="grant-the-service-principal-rights-to-deploy-resources-in-the-azure-stack-subscription"></a>Conceder os direitos de principal de serviço para implementar os recursos na subscrição do Azure pilha

Para aceder a recursos na sua subscrição, tem de atribuir a aplicação a uma função. Decida qual a função representa as permissões melhor para a aplicação. Para saber mais sobre as funções disponíveis, consulte [RBAC: funções incorporadas](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Pode definir o âmbito ao nível da subscrição, do grupo de recursos ou do recurso. As permissões são herdadas a níveis inferiores de âmbito. Por exemplo, adicionar uma aplicação para a função de leitor para um grupo de recursos significa podem ler o grupo de recursos e algum dos respetivos recursos.

1. Navegue para o nível de âmbito que pretende atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito de subscrição, selecione **subscrições**.

    ![Selecione as subscrições](media\azure-stack-solution-hybrid-pipeline\000_10.png)

2. No **subscrição**, selecione o Visual Studio Enterprise.

    ![Visual Studio Enterprise](media\azure-stack-solution-hybrid-pipeline\000_11.png)

3. No Visual Studio Enterprise, selecione **controlo de acesso (IAM)**.

    ![Controlo de acesso (IAM)](media\azure-stack-solution-hybrid-pipeline\000_12.png)

4. Selecione **Adicionar**.

    ![Adicionar](media\azure-stack-solution-hybrid-pipeline\000_13.png)

5. No **adicionar permissões**, selecione a função que pretende atribuir à aplicação. Neste exemplo, o **proprietário** função.

    ![Função de proprietário](media\azure-stack-solution-hybrid-pipeline\000_14.png)

6. Por predefinição, as aplicações do Azure Active Directory não são apresentadas nas opções disponíveis. Para localizar a aplicação, tem de fornecer o respetivo nome no **selecione** campo para procurá-lo. Selecione a aplicação.

    ![Resultado da pesquisa de aplicação](media\azure-stack-solution-hybrid-pipeline\000_16.png)

7. Selecione **guardar** para concluir a atribuição de função. Verá a aplicação na lista de utilizadores atribuídos a uma função para esse âmbito.

### <a name="role-based-access-control"></a>Controlo de Acesso Baseado em Funções

Azure baseada em funções controlo de acesso (RBAC) permite a gestão de acesso detalhada para o Azure. Ao utilizar o RBAC, pode controlar o nível de acesso que os utilizadores precisam para fazer as respetivas tarefas. Para obter mais informações sobre o controlo de acesso baseado em funções, consulte [gerir o acesso a recursos de subscrição do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

### <a name="vsts-agent-pools"></a>VSTS conjuntos de agente

Em vez de gerir cada agente separadamente, pode organizar os agentes em agrupamentos de agente. Um agrupamento de agentes define o limite de partilha para todos os agentes no conjunto. No VSTS, conjuntos de agente passam para a conta VSTS, o que significa que pode partilhar um agrupamento de agentes em projetos de equipa. Para obter mais informações sobre agrupamentos de agente, consulte [criar conjuntos de agente e filas](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts).

### <a name="add-a-personal-access-token-pat-for-azure-stack"></a>Adicionar um Token de acesso pessoal (TERESA) para a pilha do Azure

Crie um Token acesso pessoais para aceder à VSTS.

1. Inicie sessão na sua conta VSTS e selecione o nome do perfil de conta.
2. Selecione **Gerir segurança** à página de criação de token de acesso.

    ![Início de sessão de utilizador](media\azure-stack-solution-hybrid-pipeline\000_17.png)

    ![Selecione o projeto de equipa](media\azure-stack-solution-hybrid-pipeline\000_18.png)

    ![Adicionar o token de acesso pessoal](media\azure-stack-solution-hybrid-pipeline\000_18a.png)

    ![Criar token](media\azure-stack-solution-hybrid-pipeline\000_18b.png)

3. Copie o token.

    > [!Note]
    > Guarde as informações do token. Estas informações não se encontra armazenadas e não serão apresentadas novamente quando deixar a página web.

    ![Token de acesso pessoal](media\azure-stack-solution-hybrid-pipeline\000_19.png)

### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Instalar que os VSTS Criar agente na pilha do Azure alojada criar servidor

1. Ligar ao seu servidor de compilação que implementou no anfitrião de pilha do Azure.
2. Transferir e implementar o agente de compilação como um serviço com o seu pessoal aceder token (TERESA) e executam como a conta de administrador de VM.

    ![Transferir o agente de compilação](media\azure-stack-solution-hybrid-pipeline\010_downloadagent.png)

3. Navegue para a pasta para o agente de compilação extraídos. Execute o **run.cmd** ficheiro a partir da linha de comandos elevada.

    ![Agente de compilação extraídos](media\azure-stack-solution-hybrid-pipeline\000_20.png)

    ![Registar o agente de compilação](media\azure-stack-solution-hybrid-pipeline\000_21.png)

4. Quando o run.cmd estiver concluída, na pasta de agentes de compilação é atualizada com ficheiros adicionais. A pasta com o conteúdo extraído deve ter o seguinte aspeto:

    ![Criar agente de atualização de pasta](media\azure-stack-solution-hybrid-pipeline\009_token_file.png)

    Pode ver o agente na pasta VSTS.

## <a name="endpoint-creation-permissions"></a>Permissões de criação de ponto final

Através da criação de pontos finais, uma compilação do Visual Studio Online (VSTO) pode implementar aplicações de serviço do Azure com a pilha do Azure. VSTS liga-se para o agente de compilação, o que se liga a pilha do Azure.

![Aplicação de exemplo NorthwindCloud no VSTO](media\azure-stack-solution-hybrid-pipeline\012_securityendpoints.png)

1. Inicie sessão no VSTO e navegue para a página de definições de aplicação.
2. No **definições**, selecione **segurança**.
3. No **VSTS grupos**, selecione **criadores de ponto final**.

    ![Criadores de ponto final de NorthwindCloud](media\azure-stack-solution-hybrid-pipeline\013_endpoint_creators.png)

4. No **membros** separador, selecione **adicionar**.

    ![Adicionar um membro](media\azure-stack-solution-hybrid-pipeline\014_members_tab.png)

5. No **adicionar utilizadores e grupos**, introduza um nome de utilizador, selecione esse utilizador na lista de utilizadores.
6. Selecione **guardar alterações**.
7. No **VSTS grupos** lista, selecione **administradores de ponto final**.

    ![Administradores de ponto final de NorthwindCloud](media\azure-stack-solution-hybrid-pipeline\015_save_endpoint.png)

8. No **membros** separador, selecione **adicionar**.
9. No **adicionar utilizadores e grupos**, introduza um nome de utilizador, selecione esse utilizador na lista de utilizadores.
10. Selecione **guardar alterações**.

Agora que as informações de ponto final existe, VSTS a ligação de pilha do Azure está pronto a utilizar. O agente de compilação na pilha do Azure obtém instruções de VSTS e, em seguida, o agente transmite informações de ponto final para a comunicação com a pilha do Azure.

![Criar agente](media\azure-stack-solution-hybrid-pipeline\016_save_changes.png)

## <a name="develop-your-application-build"></a>Desenvolver a compilação de aplicações

Nesta parte do tutorial, vai:

* Adicione o código para um projeto VSTS.
* Crie implementação de aplicações web autónomo.
* Configurar o processo de implementação contínua

> [!Note]
 > O ambiente de pilha do Azure tem das imagens corretas syndicated para executar o Windows Server e SQL Server. Também tem de ter implementado do serviço de aplicações. Reveja a documentação do App Service secção "Pré-requisitos" requisitos de operador de pilha do Azure.

Híbrida CI/CD pode aplicar a código da aplicação e o código de infraestrutura. Utilize [modelos Azure Resource Manager, como web ](https://azure.microsoft.com/resources/templates/) código da aplicação de VSTS para implementar em ambas as nuvens.

### <a name="add-code-to-a-vsts-project"></a>Adicionar código para um projeto VSTS

1. Inicie sessão no VSTS com uma conta que tenha direitos de criação do projeto na pilha do Azure. A seguinte captura de ecrã mostra como ligar ao projeto HybridCICD.

    ![Ligar a um projeto](media\azure-stack-solution-hybrid-pipeline\017_connect_to_project.png)

2. **Clone o repositório** através da criação e abrir a aplicação de web predefinida.

    ![Repositório de clone](media\azure-stack-solution-hybrid-pipeline\018_link_arm.png)

### <a name="create-self-contained-web-app-deployment-for-app-services-in-both-clouds"></a>Criar implementação de aplicações web autónomo para serviços de aplicações em ambas as nuvens

1. Editar o **WebApplication.csproj** ficheiro: selecione **Runtimeidentifier** e, em seguida, adicione `win10-x64.` para obter mais informações, consulte [implementação autónomo](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) documentação.

    ![Configurar Runtimeidentifier](media\azure-stack-solution-hybrid-pipeline\019_runtimeidentifer.png)

2. Utilizar o Explorador de equipa para verificar o código no VSTS.

3. Certifique-se de que o código da aplicação foi marcado para o Visual Studio Team Services.

### <a name="create-the-build-definition"></a>Criar a definição de compilação

1. Inicie sessão no VSTS com uma conta que pode criar uma definição de compilação.
2. Navegue para o **Applicaiton de compilação de Web** página para o projeto.

3. No **argumentos**, adicionar **- r win10-x64** código. Isto é necessário para acionar uma implementação autónomo com o .net Core.

    ![Adicionar definição de compilação do argumento](media\azure-stack-solution-hybrid-pipeline\020_publish_additions.png)

4. Execute a compilação. O [implementação autónomo compilação](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) processo irá publicar artefactos que podem ser executados no Azure e pilha do Azure.

### <a name="use-an-azure-hosted-build-agent"></a>Utilize do Azure alojada agente de compilação

Utilizar um agente de compilação alojada no VSTS é uma opção conveniente para criar e implementar aplicações web. Manutenção de agente e as atualizações são executadas automaticamente pelo Microsoft Azure, que permite que um ciclo de desenvolvimento ininterrupta e contínua.

### <a name="configure-the-continuous-deployment-cd-process"></a>Configurar o processo de implementação contínua (CD)

Visual Studio Team Services (VSTS) e o Team Foundation Server (TFS) fornecem um pipeline altamente configurável e gerível para versões para vários ambientes, como o desenvolvimento, teste, garantia de qualidade (pergunta e resposta) e de produção. Este processo pode incluir a necessidade de aprovações em específicos fases do ciclo de vida de aplicação.

### <a name="create-release-definition"></a>Criar a definição da versão

Criar uma definição de versão é o último passo na sua aplicação criar o processo. Esta definição de versão é utilizada para criar uma versão e implementar uma compilação.

1. Inicie sessão no VSTS e navegue para **compilar e versão** para o seu projeto.
2. No **versões** separador, selecione  **\[ +]** e, em seguida, escolha **criar versão definição**.

   ![Criar a definição da versão](media\azure-stack-solution-hybrid-pipeline\021a_releasedef.png)

3. No **selecionar um modelo**, escolha **implementação de serviço de aplicações do Azure**e, em seguida, selecione **aplicar**.

    ![Aplicar modelo](media\azure-stack-solution-hybrid-pipeline\102.png)

4. No **adicionar artefactos**, do **origem (definição de compilação)** menu pendente, selecione a aplicação de compilação de nuvem do Azure.

    ![Adicionar artefacto](media\azure-stack-solution-hybrid-pipeline\103.png)

5. No **Pipeline** separador, selecione o **1 fase**, **1 tarefas** associar ao **ver tarefas do ambiente**.

    ![Tarefas da vista de pipeline](media\azure-stack-solution-hybrid-pipeline\104.png)

6. No **tarefas** separador, introduza Azure como o **o nome do ambiente** e selecione EP de Traders Web AzureCloud do **subscrição do Azure** na lista pendente.

    ![Definir variáveis de ambiente](media\azure-stack-solution-hybrid-pipeline\105.png)

7. Introduza o **nome do serviço de aplicações do Azure**, que é "northwindtraders" na captura de ecrã seguinte.

    ![Nome do serviço de aplicações](media\azure-stack-solution-hybrid-pipeline\106.png)

8. Para a fase de agente, selecione **alojado VS2017** do **fila agente** na lista pendente.

    ![Agentes alojados](media\azure-stack-solution-hybrid-pipeline\107.png)

9. No **App Service do Azure implementar**, selecione válido **pacote ou de pasta** para o ambiente.

    ![Selecione o pacote ou pasta](media\azure-stack-solution-hybrid-pipeline\108.png)

10. No **selecionar ficheiro ou pasta**, selecione **OK** para **localização**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\109.png)

11. Guardar todas as alterações e regressar ao **Pipeline**.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\110.png)

12. No **Pipeline** separador, selecione **adicionar artefactos**e escolha o **NorthwindCloud Traders-Vessel** do **origem (Criar definição)** na lista pendente.

    ![Adicionar novo artefactos](media\azure-stack-solution-hybrid-pipeline\111.png)

13. No **selecionar um modelo**, adicione outro ambiente. Escolha **implementação de serviço de aplicações do Azure** e, em seguida, selecione **aplicar**.

    ![Selecionar modelo](media\azure-stack-solution-hybrid-pipeline\112.png)

14. Introduza "Azure pilha" como o **o nome do ambiente**.

    ![Nome do ambiente](media\azure-stack-solution-hybrid-pipeline\113.png)

15. No **tarefas** separador, localize e selecione a pilha do Azure.

    ![Ambiente de pilha do Azure](media\azure-stack-solution-hybrid-pipeline\114.png)

16. Do **subscrição do Azure** pendente lista, selecione "AzureStack Traders Vessel EP" para o ponto final de pilha do Azure.

    ![Texto alternativo](media\azure-stack-solution-hybrid-pipeline\115.png)

17. Introduza o nome de aplicação web de pilha do Azure como o **nome do serviço de aplicações**.

    ![Nome do serviço de aplicações](media\azure-stack-solution-hybrid-pipeline\116.png)

18. Em **seleção de agente**, escolha "AzureStack - bDouglas Fir" o **fila agente** na lista pendente.

    ![Escolha o agente](media\azure-stack-solution-hybrid-pipeline\117.png)

19. Para **App Service do Azure implementar**, selecione válido **pacote ou de pasta** para o ambiente. No **selecionar ficheiro ou pasta**, selecione **OK** para a pasta **localização**.

    ![Escolha o pacote ou pasta](media\azure-stack-solution-hybrid-pipeline\118.png)

    ![Aprovar localização](media\azure-stack-solution-hybrid-pipeline\119.png)

20. No **variável** separador, localizar a variável com o nome **VSTS_ARM_REST_IGNORE_SSL_ERRORS**. Definir o valor da variável **verdadeiro**e definir o âmbito **Azure pilha**.

    ![Configurar a variável](media\azure-stack-solution-hybrid-pipeline\120.png)

21. No **Pipeline** separador, selecione o **acionador de implementação contínua** ícone para o artefacto NorthwindCloud Traders-Web e o conjunto de **acionador de implementação contínua** para **Ativada**.  Fazer a mesma coisa para o artefacto "NorthwindCloud Traders-Vessel".

    ![Conjunto de Acionador de implementação contínua](media\azure-stack-solution-hybrid-pipeline\121.png)

22. Para o ambiente de pilha do Azure, selecione o **condições de pré-implementação** ícone definir o acionador **após lançamento**.

    ![Conjunto de condições de pré-implementação acionador](media\azure-stack-solution-hybrid-pipeline\122.png)

23. Guarde todas as alterações.

> [!Note]
> Algumas definições para as tarefas de libertação podem ter sido definidas automaticamente como [variáveis de ambiente](https://docs.microsoft.com/vsts/build-release/concepts/definitions/release/variables?view=vsts#custom-variables) quando criou uma definição de versão de um modelo. Estas definições não podem ser modificadas nas definições de tarefas. No entanto, pode editar estas definições nos itens de ambiente de principal.

## <a name="create-a-release"></a>Criar uma versão

Agora que concluiu as modificações à definição de versão, está na altura de iniciar a implementação. Para tal, crie uma versão da definição de versão. Uma versão pode ser criada automaticamente; Por exemplo, o acionador de implementação contínua está definido na definição da versão. Isto significa que a modificar o código fonte iniciará uma nova compilação e, de que uma nova versão. No entanto, esta secção, irá criar uma nova versão manualmente.

1. No **Pipeline** separador, abra o **versão** pendente lista e escolha **criar versão**.

    ![Criar uma versão](media\azure-stack-solution-hybrid-pipeline\200.png)

2. Introduza uma descrição para a versão, certifique-se que os artefactos corretos estão selecionados e, em seguida, escolha **criar**. Após alguns instantes, é apresentada uma faixa que indica que foi criada a nova versão e o nome de versão é apresentado como uma ligação. Escolha a ligação para ver a página de resumo de versão.

    ![Faixa de criação de versão](media\azure-stack-solution-hybrid-pipeline\201.png)

3. A versão página de resumo mostra detalhes sobre a versão. Na captura de ecrã seguinte para "Versão 2", o **ambientes** secção mostra o **estado da implementação** para o Azure como "Em curso" e o estado de pilha do Azure é "foi concluída com êxito". Quando o estado de implementação para o ambiente do Azure é alterado para "Com êxito", é apresentada uma faixa que indica que a versão é pronta para aprovação. Quando uma implementação está pendente ou falhado, um blue **(i)** é apresentado o ícone de informações. Coloque o cursor sobre o ícone para ver um pop-up que contém o motivo para atrasos ou falha.

    ![Página de resumo de versão](media\azure-stack-solution-hybrid-pipeline\202.png)

Outras vistas, tais como a lista de versões, também irá apresentar um ícone que indica a aprovação está pendente. O pop-up para este ícone mostra o nome do ambiente e obter mais detalhes relacionados com a implementação. É fácil para um administrador, consulte o progresso global de versões e ver que versões estão a aguardar aprovação.

### <a name="monitor-and-track-deployments"></a>Monitorizar e controlar implementações

Esta secção mostra como pode monitorizar e controlar as suas implementações. A versão para a implementação de dois sites de serviços de aplicações do Azure fornece um bom exemplo.

1. Na página de resumo "Versão 2", selecione **registos**. Durante uma implementação, esta página mostra o registo em direto do agente. O painel esquerdo apresenta o estado de cada operação na implementação de cada ambiente.

    Pode escolher um ícone de pessoa no **ação** coluna de uma aprovação de pré-implementação ou pós-implementação para ver quem aprovado (ou rejeitado) a implementação e a mensagem que fornecido.

2. Após a conclusão da implementação, todo o ficheiro de registo é apresentado no painel direito. Pode selecionar qualquer **passo** no painel da esquerda para ver o ficheiro de registo para um único passo, tais como "Tarefa de inicializar". A capacidade para ver registos individuais torna mais fácil de rastreio e depuração partes da implementação geral. Também pode **guardar** o ficheiro de registo de um passo ou **transferir todos os registos como zip**.

    ![Registos de versão](media\azure-stack-solution-hybrid-pipeline\203.png)

3. Abra o **resumo** separador para ver informações gerais sobre a versão. Esta vista apresenta os detalhes sobre a compilação, os ambientes que foi implementado, o estado de implementação e outras informações sobre a versão.

4. Selecione uma ligação de ambiente (**Azure** ou **Azure pilha**) para ver informações sobre existente e implementações para um ambiente específico pendentes. Pode utilizar estas vistas como uma forma rápida de verificar que a mesma compilação foi implementada para ambos os ambientes.

5. Abra o **implementar a aplicação de produção** no seu browser. Por exemplo, para o Web site dos serviços de aplicações do Azure, abra o URL `http://[your-app-name].azurewebsites.net`.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os padrões de nuvem do Azure, consulte [padrões de conceção de nuvem](https://docs.microsoft.com/azure/architecture/patterns).
