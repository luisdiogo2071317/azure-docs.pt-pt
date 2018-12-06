---
title: Criar uma edge solução de machine learning com o Azure e o Azure Stack | Documentos da Microsoft
description: Saiba como criar uma solução de aprendizado de máquina de borda com o Python com o Azure e o Azure Stack.
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
ms.date: 09/26/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 7c92b2a060e73484b73a60f9215425cba40ea3f1
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966726"
---
# <a name="tutorial-create-an-edge-machine-learning-solution-with-azure-and-azure-stack"></a>Tutorial: Criar uma edge solução de machine learning com o Azure e o Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Saiba como criar uma edge solução de machine learning com o Azure e o Azure Stack.

Neste tutorial, irá criar um ambiente de exemplo para:

> [!div class="checklist"]
> - Crie uma conta de armazenamento e um contentor para limpar dados a residir.
> - Crie uma Máquina Virtual de ciência de dados de Ubuntu (DSVM) no portal do Azure.
> - Implemente serviços do Azure Machine Learning no Azure para criar e formar modelos.
> - Crie contas de serviços do Azure Machine Learning.
> - Implementar e utilizar o Azure Container Registry.
> - Implemente um cluster de Kubernetes no Azure Stack.
> - Crie uma conta de armazenamento do Azure Stack e a fila de armazenamento para dados.
> - Crie uma função da nova pilha do Azure para mover a apagar dados do Azure Stack para o Azure.

**Quando utilizar esta solução**

 -  Sua organização está a utilizar uma abordagem de DevOps ou tem um planeadas para um futuro próximo.
 -  Queira implementar práticas de CI/CD em sua implementação do Azure Stack e a cloud pública.
 -  Pretende consolidar o pipeline de CI/CD em ambientes de cloud e no local.
 -  Pretende que a capacidade de desenvolver aplicações com os serviços cloud ou no local.
 -  Deseja aproveitar habilidades de desenvolvedor consistente em todas as aplicações na cloud e no local.

> [!Tip]  
> ![pillars.png híbrida](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack é uma extensão do Azure. O Azure Stack coloca a agilidade e inovação da cloud de informática para o seu ambiente no local e ativar a única cloud híbrida que permite-lhe criar e implementar aplicações de híbridas em qualquer lugar.  
> 
> O White Paper [considerações de Design para aplicações híbridas](https://aka.ms/hybrid-cloud-applications-pillars) analisa pilares de qualidade de software (posicionamento, escalabilidade, disponibilidade, resiliência, capacidade de gerenciamento e segurança) para estruturar, implementar e utilizar aplicações híbridas. As considerações de design ajudam a otimizar o design de aplicações híbridas, minimizando desafios em ambientes de produção.

## <a name="prerequisites"></a>Pré-requisitos

Alguns componentes necessários para criar este caso de utilização e podem demorar algum tempo para preparar:

 -  Um parceiro OEM/Hardware do Azure podem implementar uma produção do Azure Stack e todos os utilizadores podem implementar uma ASDK

 -  Um operador de pilha do Azure deve também implementar o serviço de aplicações, criar planos e ofertas, criar uma subscrição de inquilino e adicionar a imagem do Windows Server 2016

 -  Um serviço de aplicações de rede híbrida e a configuração é necessária (Saiba mais sobre [integração de aplicações com VNets.](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet))

 -  A privada [compilação e versão de agente](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts) para integração de VSTS tem de ser definida antes da implantação (Certifique-se de quaisquer componentes existentes usados cumpre os requisitos antes de começar.)

É necessário o conhecimento prévio do Azure e o Azure Stack. Para obter mais informações antes de continuar, comece com estes tópicos:

 -  [Introdução ao Azure](https://azure.microsoft.com/overview/what-is-azure/)

 -  [Conceitos-chave do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

 -  [Guia de soluções do Azure Stack híbrida CI/CD](/azure/azure-stack/user/azure-stack-solution-pipeline)

**Azure**

 -  Uma subscrição do Azure (criar um [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))

 -  Um novo URL da aplicação Web criada pelo [aplicação Web](https://docs.microsoft.com/azure/app-service/app-service-web-overview) no Azure

 -  Implementação de [dos serviços de contentor do Azure (ACS) do Kubernetes no Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

 -  Implementação do serviço do Azure Machine Learning (pré-visualização) [4-part tutorial](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/tutorial-classifying-iris-part-1)

 -  Experimentação do Azure Machine Learning [conta](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/experimentation-service-template)

**Azure Stack**

 -  Um sistema integrado do Azure Stack ou a implementação do Development Kit do Azure Stack.

    - Encontrará instruções para instalar o Azure Stack na [instalar o Kit de desenvolvimento do Azure Stack](../asdk/asdk-install.md).
     - [https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1](https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1) Esta instalação pode exigir algumas horas a concluir.

 -  Implementação do [serviço de aplicações](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) serviços de PaaS para o Azure Stack

 -  R [plano/ofertas](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) dentro do ambiente do Azure Stack

 -  R [subscrição do inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) dentro do ambiente do Azure Stack

 -  Uma imagem de VM do Ubuntu Server (disponível no [do Azure Stack Marketplace](https://buildazure.com/2016/05/04/azure-marketplace-ubuntu-server-16-04-lts/)) esta VM será incorporado na subscrição de inquilino na pilha do Azure como o agente de compilação privada, bem como as VMs de Kubernetes. Se esta imagem não estiver disponível, podem ajudar o operador de pilha do Azure, para garantir que este valor é adicionado ao ambiente. (Não utilize a compilação 18.04 do ubuntu, porque atualmente não suportado.)

 -  Uma aplicação Web dentro da subscrição de inquilino (Observe o novo URL da aplicação Web para utilização posterior.)

 -  Implementação de baseado em VSTS Linux privada Criar agente de Máquina Virtual, dentro da subscrição de inquilino

 -  Implementação de um [dos serviços de contentor do Azure (ACS) do Kubernetes no Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

**Ferramentas de programador**

 -  [Área de trabalho do VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) (o processo de inscrição cria um projeto com o nome "MyFirstProject")

 -  [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) instalação e [VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services) início de sessão

 -  [Local clone](https://www.visualstudio.com/docs/git/gitquickstart) do projeto

 -  [Subsistema de Linux para o Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) instalação (para o BASH e SSH)

 -  [Docker para Windows](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) instalação

 -  [O Azure Machine Learning Workbench (pré-visualização)](https://aka.ms/azureml-wb-msi) instalação

 -  [Python](https://www.python.org/ftp/python/3.7.0/python-3.7.0rc1-amd64.exe) instalação de ambiente

**VSTS**

 -  **Conta do VSTS.** Configure rapidamente a integração contínua para compilação, teste e implementação. Para obter mais informações sobre as contas VSTS, veja [criar a conta VSTS](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student?view=vsts).

 -  **Repositório de código.** Com os repositórios de código existente no GitHub, BitBucket, DropBox, Onedrive e o TFS, a plataforma do VSTS pode tirar partido vários repositórios de código para simplificar o pipeline de desenvolvimento. Para obter mais informações sobre código ver repositórios [começar com o Git e do VSTS](https://docs.microsoft.com/vsts/git/gitquickstart?view=vsts&tabs=visual-studio) tutorial.

 -  **Ligação de serviço.** Ligar a serviços externos e remotos para executar tarefas de teste, a compilação ou a implementação. Para obter mais informações sobre o VSTS ver ligações de serviço [pontos finais de serviço para criação e versão](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints?view=vsts).

 -  **Definições de compilação.** Definir processos de criação automatizados e um conjunto de tarefas através do catálogo de tarefas de composição. Para obter mais informações sobre a compilação ver definições [criar uma definição de compilação](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts) documentação.

 -  **Definições de versão.** Defina o processo de implementação para uma coleção de ambientes onde os artefactos de aplicação estão implementados. Para obter mais informações sobre versão ver definições [criar uma definição de versão](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1?view=vsts) documentação.

 -  **Conjunto de agentes de compilação de Linux do VSTS alojado.** Criar, testar e implementar rapidamente aplicações através de um Microsoft gerido e mantido alojado o agente. Para obter mais informações sobre a compilação hospedado do VSTS agentes ver [agentes alojados](https://docs.microsoft.com/vsts/build-release/concepts/agents/hosted?view=vsts) documentação.

## <a name="step-1-create-a-storage-account"></a>Passo 1: Criar uma conta de armazenamento

Crie uma conta de armazenamento e um contentor para limpar dados a residir.

1.  Inicie sessão para o [ *portal do Azure*](https://portal.azure.com/).

2.  No portal do Azure, expanda o menu no lado esquerdo para abrir o menu de serviços e escolha **todos os serviços**. Desloque para baixo até **armazenamento** e escolha **contas de armazenamento**. Na * * as contas de armazenamento * * janela escolher **adicionar**.

3.  Introduza um nome para a conta de armazenamento.

    > [!Note]  
    > Os nomes das contas do Storage devem ter entre 3 e 24 carateres de comprimento e apenas podem conter números e letras minúsculas. O nome da conta de armazenamento tem de ser exclusivo no Azure. O portal do Azure indica se o nome da conta de armazenamento selecionado já está em utilização.

4.  Especifique o modelo de implementação a ser utilizado: **Resource Manager**.

5.  Selecione o tipo de conta de armazenamento: **V1 de fins gerais**, em seguida, especifique a camada de desempenho: **padrão**.

6.  Selecione a opção de replicação para a conta de armazenamento: **GRS**.

7.  Selecione a nova subscrição de conta de armazenamento.

8.  Especifique um novo grupo de recursos ou selecione um grupo de recursos existente.

9.  Selecione a localização geográfica para a conta de armazenamento.

10. Selecione **Criar** para criar a conta de armazenamento.

    ![Texto alternativo](/media/azure-stack-solution-machine-learning/image1.png)

11.  Selecione a conta de armazenamento recentemente criada.

12.  Selecione no **Blobs**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image2.png)

13.  Selecione no **+ contentor** e selecione no **contentor**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image3.png)

14.  Atribua o nome de contentor **uploadeddata** e escolha o tipo de acesso **contentor**.

15.  Selecione no **criar**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image4.png)

## <a name="step-2-create-a-data-science-virtual-machine"></a>Passo 2: Criar uma máquina de Virtual de ciência de dados

Crie uma Máquina Virtual de ciência de dados de Ubuntu (DSVM) no portal do Azure.

1.  Inicie sessão no portal do Azure [https://portal.azure.com](https://portal.azure.com/)

2.  Selecione sobre o **+ novo** link e procure "Data Science Virtual Machine para Linux Ubuntu CSP

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image5.png)

1.  Escolher **Máquina Virtual de ciência de dados para Linux (Ubuntu)** na lista e siga na tela instruções para criar a DSVM.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image6.png)

> ! [Importante]  
> **Escolher** palavra-passe * * como o*tipo de autenticação*.

Coloque a DSVM nova no mesmo grupo de recursos como a conta de armazenamento recentemente criada. Todos os objetos de ML de borda são implementados no Azure dentro do grupo de recursos.

1.  Nas funcionalidades opcionais de configurar as definições

    a.  Selecione o **conta de armazenamento** criado anteriormente.

    b.  Criar uma nova **rede Virtual**, **sub-rede**, e **IP público** é necessário por predefinição criar um nome baseado no nome do grupo de recursos.

    c.  Criar uma nova **NSG** é necessário criar isso com as regras corretas já aplicadas automaticamente.

    d.  Para o **conta de armazenamento de diagnóstico**, selecione a conta de armazenamento que criou anteriormente.

    e.  Nota: Com o AAD ativada e configurada para a subscrição do Azure, identidades geridas para recursos do Azure podem ser ativadas também.

2.  Selecione **OK**.

### <a name="connect-to-the-dsvm"></a>Ligar a DSVM

Quando a DSVM tiver sido criada, ligar à VM do subsistema Windows para Linux.

```Bash  
    ssh <user>@<DSVM Public IP>
```

1.  Escreva Sim, quando lhe for pedido para confirmar a ligação de VM.

2.  Introduza a palavra-passe criada para a DSVM.

### <a name="update-the-dsvm"></a>Atualizar a DSVM 

```Bash  
sudo su 
apt-get update 
apt-get -y upgrade 
apt-get -y dist-upgrade 
apt-get -y autoremove
```

## <a name="step-3-deploy-azure-machine-learning-services"></a>Passo 3: Implementar o Azure Machine Learning Services

Implemente o Azure Machine Learning Services no Azure.

Os serviços do Azure Machine Learning (pré-visualização) são uma solução de análise avançada e de ciência de dados integrada e completa. Ajudam os cientistas de dados profissionais a preparar dados, desenvolver experimentações e implementar modelos à escala da cloud.

Esta secção explica:

> [!div class="checklist"]
> - Criar contas de serviço para os serviços do Azure Machine Learning
> - Instalar e iniciar sessão no Azure Machine Learning Workbench
> - Criar um projeto no Workbench
> - Executar um script nesse projeto
> - Aceder à interface de linha de comandos (CLI)

Enquanto parte do portefólio do Microsoft Azure, os serviços do Azure Machine Learning requerem uma subscrição do Azure. Para obter uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

São necessárias permissões adequadas para criar recursos, tais como grupos de recursos, máquinas virtuais e outros ativos.

A aplicação Azure Machine Learning Workbench pode ser instalada nos seguintes sistemas operativos:

 -  Windows 10 ou Windows Server 2016
 -  macOS Sierra ou High Sierra

## <a name="step-4-create-azure-machine-learning-services"></a>Passo 4: Criar serviços do Azure Machine Learning

Crie contas de serviços do Azure Machine Learning.

Utilize o portal do Azure para aprovisionar as contas do Azure Machine Learning:

1.  Inicie sessão para o [portal do Azure](https://portal.azure.com/) com as credenciais para a subscrição do Azure a ser utilizado. Para obter uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image7.png)

1.  Selecione o botão (+) **Criar um recurso**, no canto superior esquerdo do portal.

    ![Criar um recurso no portal do Azure](media/azure-stack-solution-machine-learning/image8.png)

1.  Introduza **Machine Learning** na barra de pesquisa. Selecione o resultado da pesquisa com o nome **Experimentação do Machine Learning (pré-visualização)**.

    ![Pesquisa por Azure Machine Learning](media/azure-stack-solution-machine-learning/image9.png)

1.  Na **experimentação do Machine Learning** painel, desloque-se para baixo e selecione **criar** para começar a definir a conta de experimentação.

    ![Azure Machine Learning - criar conta de experimentação](media/azure-stack-solution-machine-learning/image10.png)

1.  Na **experimentação do ML** painel Configurar a conta de experimentação do Machine Learning.

    | Definição | Valor sugerido para o tutorial | Descrição |
    |---------------------------------------|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Nome da conta de Experimentação | Nome exclusivo | Introduza um nome exclusivo que identifica a conta. Utilize um nome de departamento ou projeto que melhor identifique a experimentação. O nome deve ter entre 2 e 32 carateres. Deve incluir apenas carateres alfanuméricos e o caráter de travessão (–). |
    | Subscrição | A subscrição | Escolha a subscrição do Azure a utilizar para a experimentação. Se existirem várias subscrições, escolha a subscrição adequada na qual o recurso é cobrado. |
    | Grupo de recursos | O grupo de recursos | Utilize um grupo de recursos existente na subscrição ou introduza um nome para criar um novo grupo de recursos para esta conta de experimentação. |
    | Localização | A região mais próxima dos utilizadores | Escolha a localização mais próxima dos utilizadores e os recursos de dados. |
    | Número de utilizadores | 2 | Introduza o número de utilizadores. Saiba como [o número de utilizadores influencia os preços](https://azure.microsoft.com/pricing/details/machine-learning/).<br><br>Neste início rápido, são necessárias apenas dois utilizadores. É possível adicionar ou remover utilizadores no portal do Azure, conforme necessário. |
    | Conta de armazenamento | Nome exclusivo | Selecione **Criar nova** e indique um nome para criar uma [conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal). O nome deve incluir 3 a 24 carateres alfanuméricos. Em alternativa, selecione **utilizar existente** e selecione a conta de armazenamento existente na lista. A conta de armazenamento é necessária e é utilizada para armazenar artefactos de projetos e dados do histórico de execuções. |
    | Área de Trabalho para a conta de Experimentação | IrisGarden<br>(nome utilizado nos tutoriais) | Indique um nome para uma área de trabalho nesta conta. O nome deve ter entre 2 e 32 carateres. Deve incluir apenas carateres alfanuméricos e o caráter de travessão (–). Esta área de trabalho contém as ferramentas necessárias para criar, gerir e publicar experimentações. |
    | Atribuir proprietário à área de trabalho | A conta | Selecione a conta do própria como proprietário da área de trabalho. |
    | Criar conta de Gestão de Modelos | **selecionar** | Crie uma conta de Gestão de Modelos. Isto irá ser utilizado para implementar e gerir os modelos como serviços da web em tempo real. <br><br>Embora seja opcional, crie a conta de gestão de modelos ao mesmo tempo que a conta de experimentação é recomendado. |
    | Nome da conta | Nome exclusivo | Escolha um nome exclusivo que identifica a conta de gestão de modelos. Utilize o departamentais ou nome do projeto que melhor identifique a experimentação. O nome deve ter entre 2 e 32 carateres. Deve incluir apenas carateres alfanuméricos e o caráter de travessão (–). |
    | Escalão de preços da Gestão de Modelos | **DEV/TEST** | Selecione **nenhum escalão de preço selecionado** para especificar o escalão de preço para a nova conta de gestão de modelos. Para poupar nos custos, selecione o escalão de preço Dev/Test, se estiver disponível na subscrição (disponibilidade limitada). Caso contrário, selecione o escalão de preço S1. Escolha a selecionar a opção para guardar a seleção do escalão de preço. |
    | Afixar ao dashboard | Verificação | Selecione o **afixar ao dashboard** opção para permitir uma fácil monitorização da conta de experimentação do Machine Learning na página de rosto do dashboard do portal do Azure. |

    ![Configuração da conta de Experimentação do Machine Learning](media/azure-stack-solution-machine-learning/image11.png)

1.  Selecione **Criar** para começar o processo de criação da conta de Experimentação, juntamente com a conta de Gestão de Modelos.

    ![Configuração da conta de Experimentação do Machine Learning](media/azure-stack-solution-machine-learning/image12.png)

    Pode demorar alguns minutos para criar uma conta. Verificar o estado do processo de implantação, selecionando o ícone de notificações (campainha) na barra de ferramentas de portal do Azure.

    ![Notificações do portal do Azure](media/azure-stack-solution-machine-learning/image13.png)

### <a name="install-and-log-in-to-workbench"></a>Instalar e iniciar sessão no workbench 

Azure Machine Learning Workbench está disponível para Windows ou macOS. Veja a lista de [plataformas suportadas](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation).

**Aviso:** a instalação pode demorar cerca de uma hora para concluir.

1.  Transfira e inicie o programa de instalação mais recente do Workbench.

    > [!Important]  
    > Transfira o instalador completamente para o disco e execute-o aí. Não o execute diretamente a partir do widget de transferência do browser.<br>**No Windows:<br>**  um. Transfira [AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi).<br>b. Faça duplo clique no programa de instalação que transferiu no Explorador de Ficheiros.

1.  Siga na tela instruções no instalador do até à conclusão.

    * * A instalação poderá demorar até 30 minutos a concluir. **
    
    `Windows: C:\\Users\\<user>\\AppData\\Local\\AmlWorkbench`
    
    O instalador irá transferir e configurar todas as dependências necessárias, tais como Python, o Miniconda e outras bibliotecas associadas. Esta instalação também inclui a ferramenta de linha de comandos para várias plataformas do Azure ou a CLI do Azure.

1.  Inicie o Workbench ao selecionar o botão **Iniciar o Workbench** no último ecrã do programa de instalação.

    Se estiver fechado o instalador, inicie com o **Machine Learning Workbench** atalho de desktop.

1.  Selecione **iniciar sessão com a Microsoft** para autenticar com o Azure Machine Learning Workbench. Utilize as mesmas credenciais utilizadas no portal do Azure para criar as contas de experimentação e gestão de modelos.

    Depois de iniciar sessão, o Workbench utiliza a primeira conta de experimentação que encontra nas subscrições do Azure e mostra todas as áreas de trabalho e os projetos associados à mesma.

    > [!Tip]  
    > Mudar para outra conta de experimentação com o ícone no canto inferior esquerdo da janela de aplicativo do Workbench.

### <a name="create-a-new-project-in-workbench"></a>Criar um novo projeto no workbench

1.  Abra a aplicação Azure Machine Learning Workbench e inicie sessão, se necessário.

    - No Windows, inicie com o **Machine Learning Workbench** atalho de desktop.

    - No macOS, selecione **Azure ML Workbench** no Launchpad.

1.  Selecione o sinal de adição (+) no painel **PROJETOS** e escolha **Novo Projeto**.

    ![Nova área de trabalho](media/azure-stack-solution-machine-learning/image14.png)

1.  Preencha os campos do formulário e selecione o botão **Criar** para criar um novo projeto no Workbench.

    | Campo | Valor sugerido para o tutorial | Descrição |
    |-------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Nome do projeto | myIris | Introduza um nome exclusivo que identifica a conta. Utilize o departamentais ou nome do projeto que melhor identifique a experimentação. O nome deve ter entre 2 e 32 carateres. Deve incluir apenas carateres alfanuméricos e o caráter de travessão (–). |
    | Diretório do projeto | c:\Temp\ | Especifique o diretório no qual é criado o projeto. |
    | Descrição do projeto | Deixe em branco | Campo opcional útil para descrever os projetos. |
    | URL do Repositório GIT Visualstudio.com | Deixe em branco | Campo opcional. Associe um projeto com um repositório de Git no Visual Studio Team Services para controlo de código fonte e colaboração. [Saiba como configurar um repositório](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/using-git-ml-project). |
    | Área de trabalho selecionada | IrisGarden (se existir) | Escolha uma área de trabalho criada para a conta de experimentação no portal do Azure. <br>Utilizar o início rápido, área de trabalho com o nome IrisGarden está listada. Caso contrário, utilize a área de trabalho com o nome da conta de experimentação ou um nome de conta preferida. |
    | Modelo de projeto | Classifying Iris | Os modelos contêm scripts e os dados utilizados para explorar o produto. Este modelo contém os scripts e os dados necessários para este início rápido e de outros tutoriais neste site de documentação. |

    ![Novo projeto](media/azure-stack-solution-machine-learning/image15.png)

1.  É criado um novo projeto e o dashboard do projeto abre com esse projeto. Explore a home page do projeto, origens de dados, blocos de notas e arquivos de código de origem.

    ![Abrir projeto](media/azure-stack-solution-machine-learning/image16.png)

### <a name="attach-a-dsvm-compute-target"></a>Anexar um destino de computação DSVM

Quando a DSVM estiver criada, anexá-lo ao projeto do Azure ML.

1.  A partir da aplicação do Azure ML Workbench, inicie o CLI do Azure ML Workbench selecionando **arquivo**->**PowerShell aberto**

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image17.png)

1.  Assim que o PowerShell abriu a linha de comandos, utilize o seguinte comando:

    ```PowerShell  
        az login
    ```

1.  Receba o seguinte pedido:

     ![Texto alternativo](media/azure-stack-solution-machine-learning/image18.png)

1.  Navegue para o site conforme detalhado na linha de comandos e introduza o código fornecido.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image19.png)

1.  Selecionar continuar quando lhe for pedido, em seguida, selecione a conta do Azure que se encontra associada a conta Experimental do Azure ML.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image20.png)

1.  A CLI do Azure ML Workbench, em seguida, irá enviar o seguinte pedido:

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image21.png)

1.  Quando o início de sessão de conta de ML e área de trabalho é mostrado a conclusão com êxito, anexe a DSVM.

    ```PowerShell  
        az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password>
    ```

    Será apresentada a seguinte notificação:

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image22.png)

    ```PowerShell  
        # prepare the Docker image on the DSVM 
        az ml experiment prepare -c <compute target name>
    ```

Este processo irá demorar algum tempo e irá gerar uma quantidade significativa de texto que extrai diversas imagens do docker, regista-os e, em seguida, aplica-se as aplicações aos mesmos e o código necessário.

Experimentações agora podem ser executadas neste DSVM.

### <a name="create-a-data-preparation-package"></a>Criar um pacote de preparação de dados

Em seguida, começar a preparar os dados no Azure Machine Learning Workbench. Cada transformação executada na bancada de trabalho é armazenada no formato JSON num pacote de preparação de dados local (\*.dprep ficheiro). Este pacote de preparação de dados é o contentor principal para o trabalho de preparação de dados da bancada de trabalho.

Este pacote de preparação de dados pode ser posteriormente entregue a um tempo de execução, como o local C\#/CoreCLR, Scala/Spark ou Scala/HDI.

1.  Selecione o ícone de pasta para abrir a vista Ficheiros e, em seguida, selecione **iris.csv** para abrir esse ficheiro.

    Este ficheiro contém uma tabela com 5 colunas e 50 linhas. Quatro das colunas são colunas de funcionalidades numéricas. A quinta coluna é uma coluna de cadeia de caracteres de destino. Nenhuma das colunas tem nomes de cabeçalho.

    ![iris.csv](media/azure-stack-solution-machine-learning/image23.png)

1.  Na **Vista de dados**, selecione o sinal de adição (**+**) para adicionar uma origem de dados nova. É aberta a página **Adicionar Origem de Dados**.

    ![Vista de dados no Azure Machine Learning Workbench](media/azure-stack-solution-machine-learning/image24.png)

1.  Selecione **arquivos de texto (\*. csv, \*. JSON, \*. txt.,...)** .

    ![Origem de dados no Azure Machine Learning Workbench](media/azure-stack-solution-machine-learning/image25.png)

1.  Selecione **Seguinte**.

2.  Procure o ficheiro **IRIS**e selecione **concluir**. Este procedimento irá utilizar os valores predefinidos para os parâmetros, como o separador e os tipos de dados.

    > [!Important]  
    > Selecione o **IRIS** partir de ficheiros no diretório do projeto atual para este exercício. Caso contrário, os passos posteriores poderão falhar.

    ![Selecionar iris](media/azure-stack-solution-machine-learning/image26.png)

1.  Um novo ficheiro designado `*iris-1.dsource` é criado. O ficheiro é denominado exclusivamente com `-1` porque o projeto de exemplo já vem com um atribuído **dsource** ficheiro.

    O ficheiro abre-se e os dados são apresentados. Uma série de cabeçalhos de coluna, partir **Column1** ao **Column5**, é automaticamente adicionada a este conjunto de dados. Desloque para baixo e tenha em atenção que a última linha do conjunto de dados está vazia. A linha está vazia por causa da quebra de linha extra no ficheiro CSV.

    ![Vista de dados de iris](media/azure-stack-solution-machine-learning/image27.png)

1.  Selecione o botão **Métricas**. Os histogramas são gerados e apresentados.

    Mude novamente para a vista de dados ao selecionar o **dados** botão.

    ![Vista de dados de iris](media/azure-stack-solution-machine-learning/image28.png)

1.  Observe os histogramas. Foi calculado para cada coluna um conjunto completo de estatísticas.

    ![Vista de dados de iris](media/azure-stack-solution-machine-learning/image29.png)

1.  Comece a criar um pacote de preparação de dados ao selecionar o botão **Preparar**. É aberta a caixa de diálogo **Preparar**.

    O projeto de exemplo contém um **IRIS. dprep** ficheiro de preparação de dados que está selecionado por predefinição.

    ![Vista de dados de iris](media/azure-stack-solution-machine-learning/image30.png)

1.  Criar um novo pacote de preparação de dados, selecionando **+ novo pacote de preparação de dados** no menu.

    ![Vista de dados de iris](media/azure-stack-solution-machine-learning/image31.png)

1.  Introduza um novo valor para o nome do pacote (utilize **iris-1**) e, em seguida, selecione **OK**.

    Um novo pacote de preparação de dados com o nome **iris-1.dprep** é criado e aberto no editor de preparação de dados.

    ![Vista de dados de iris](media/azure-stack-solution-machine-learning/image32.png)

    Em seguida, é necessária a preparação de dados.

1.  Selecione cada cabeçalho de coluna para tornar o texto do cabeçalho editável. Em seguida, mude o nome de cada coluna da seguinte forma:

    Por ordem, introduza **Sepal Length**, **Sepal Width**, **Petal Length**, **largura da Pétala**, e **Species** nas cinco colunas, respetivamente.

    ![Mudar o nome das colunas](media/azure-stack-solution-machine-learning/image33.png)

1.  Faça a contagem dos valores distintos:

    1.  Selecione a coluna **Species**.

    2.  Clique com o botão direito do rato para a selecionar.

    3.  Selecione **contagens de valores** no menu.

        O painel **Inspectors** é aberto por baixo dos dados. É apresentado um histograma com quatro barras. A coluna de destino tem quatro valores distintos: **iris_virginica**, **iris_versicolor**,**Iris-setosa**e um **(null)** valor.

    ![Selecione Contagens de Valores](media/azure-stack-solution-machine-learning/image34.png)

    ![Histograma da contagem de valores](media/azure-stack-solution-machine-learning/image35.png)

1.  Para filtrar os valores nulos, selecione a barra "(null)" e, em seguida, selecione o sinal de subtração (**-**).

    Em seguida, a linha (null) aparece a cinzento para indicar que foi filtrada.

    ![Filtrar valores nulos](media/azure-stack-solution-machine-learning/image36.png)

1.  Repare nos passos de preparação de dados individuais descritos no painel **STEPS**. À medida que forem mudadas colunas e linhas com valores nulos são filtradas, cada ação é registada como um passo de preparação de dados. Edite os passos individuais para ajustar as definições, reordenar os passos e remover passos.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image37.png)

1.  Feche o editor de preparação de dados. Selecione o ícone **x** no separador **iris-1** com o ícone de gráfico para fechar o separador. O trabalho é guardado automaticamente no **iris-1.dprep** arquivo mostrado sob a **preparações de dados** cabeçalho.

    ![Fechar](media/azure-stack-solution-machine-learning/image38.png)

### <a name="generate-python-code-to-invoke-a-data-preparation-package"></a>Gerar código de Python para invocar um pacote de preparação de dados

O resultado de um pacote de preparação de dados pode ser explorado diretamente em Python ou num Bloco de Notas do Jupyter. Os pacotes podem ser executados em vários runtimes, incluindo o Python local, o Spark (inclusive no Docker) e o HDInsight.

1.  Localize o ficheiro **iris-1.dprep** no separador Preparações de Dados.

2.  Clique com o botão direito do rato no ficheiro **iris-1.dprep** e selecione **Gerar Ficheiro de Código de Acesso a Dados** no menu de contexto.

    ![Gerar código](media/azure-stack-solution-machine-learning/image39.png)

    Um novo ficheiro designado **IRIS-1.PY** abre-se com as seguintes linhas de código para invocar a lógica que criou como um pacote de preparação de dados:

    ```Python
    # Use the Azure Machine Learning data preparation package
    from azureml.dataprep import package

    # Use the Azure Machine Learning data collector to log various metrics
    from azureml.logging import get_azureml_logger
    logger = get_azureml_logger()

    # This call will load the referenced package and return a DataFrame.
    # If run in a PySpark environment, this call returns a
    # Spark DataFrame. If not, it will return a Pandas DataFrame.
    df = package.run('iris-1.dprep', dataflow_idx=0)
    # Remove this line and add code that uses the DataFrame
    df.head(10)
    ```

    Consoante o contexto em que esse código é executado, drep representa um tipo de DataFrame diferente:

    -  Quando a execução é realizada num runtime de Python, é utilizado um [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html).

    -  Quando a execução é realizada num contexto de Spark, é utilizado um [DataFrame de Spark](https://spark.apache.org/docs/latest/sql-programming-guide.html).

### <a name="review-irissklearnpy-and-the-configuration-files"></a>Reveja os ficheiros de configuração e iris_sklearn.py

1.  No projeto aberto, selecione o **ficheiros** botão (o ícone de pasta), no painel à extrema esquerda para abrir a lista de ficheiros na pasta de projeto.

    ![Abrir o projeto do Azure Machine Learning Workbench](media/azure-stack-solution-machine-learning/image40.png)

1.  Selecione o ficheiro de script de Python **iris_sklearn.py**.

    ![Escolher um script](media/azure-stack-solution-machine-learning/image41.png)

    O código é aberto num novo separador do editor de texto dentro do Workbench.

    > [!Note]  
    > O código apresentado pode não ser exatamente o mesmo que o código anterior porque este projeto de exemplo é atualizado frequentemente.

    ![Abrir um ficheiro](media/azure-stack-solution-machine-learning/image42.png)

1.  Examine o código do script de Python para se familiarizar com o estilo de programação.

    O script **iris_sklearn.py** executa as seguintes tarefas:  

    -  Carrega o pacote de preparação de dados predefinido chamado **iris.dprep** para criar um [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html).

    -   Adiciona funcionalidades aleatórias para tornar o problema mais difícil de resolver. A aleatoriedade é necessária porque o Íris é um conjunto de dados pequeno que pode ser facilmente classificado com uma precisão de quase 100%.

    -  Utiliza a biblioteca de aprendizagem thescikit learnmachine para criar um modelo de regressão logística. Esta biblioteca é fornecida com o Azure Machine Learning Workbench por predefinição.

    -  Serializa o modelo ao utilizar a biblioteca [pickle](https://docs.python.org/3/library/pickle.html) num ficheiro na pasta `outputs`.

    -  Carrega o modelo serializado e, em seguida, anula a serialização do mesmo, carregando-o novamente para a memória.

    -  Utiliza o modelo com a serialização anulada para fazer uma predição num registo novo.

    -  Desenha dois gráficos, uma matriz de confusão e um recetor de Roc curva característica (multiclasse mediante) a funcionar, utilizando o [matplotlib](https://matplotlib.org/) biblioteca e, em seguida, salva-os no theoutputsfolder. Instale esta biblioteca no ambiente, se não aparente.

    -  Desenha a precisão de modelo e a taxa de regularização no histórico de execuções automaticamente. O objeto `run_logger` é utilizado transversalmente para registar a taxa de regularização e a exatidão do modelo nos registos.

### <a name="run-irissklearnpy-in-the-local-environment"></a>Executar iris_sklearn. PY no ambiente local

1.  Abra a interface de linha de comandos (CLI) do Azure Machine Learning:

    1.  Inicie o Azure Machine Learning Workbench.

    2.  No menu Workbench, selecione **Ficheiro**> **Abrir Linha de Comandos**.

    A janela de interface de linha de comandos (CLI) do Azure Machine Learning é iniciado na pasta de projeto `C:\Temp\\myIris\` no Windows. Este projeto é o mesmo que criou na parte 1 do tutorial.

    > [!Important]  
    > Utilize esta janela da CLI para realizar os passos seguintes.

1.  Na janela da CLI, instale a biblioteca de desenho de Python **matplotlib**, se ainda não estiver instalado.

    O script **iris_sklearn.py** tem dependências em dois pacotes de Python: **scikit-learn** e **matplotlib**. O **scikit-Saiba** pacote está instalado pelo Azure Machine Learning Workbench para a conveniência. Ainda assim, a instalação do **matplotlib** poderá ser necessário.

    Se continuar sem instalar **matplotlib**, o código neste tutorial ainda pode executar com êxito. Contudo, o código não conseguirá produzir os gráficos de saída da matriz de confusão nem da curva ROC multiclasse, conforme mostrado nas visualizações do histórico.

    ```CLI
    pip install matplotlib
    python -m pip install --upgrade pip
    ```

    Esta instalação demora cerca de um minuto.

1.  Regresse à aplicação Workbench.

2.  Localize o separador chamado **iris_sklearn.py**.

    ![Localizar separador com o script](media/azure-stack-solution-machine-learning/image43.png)

1.  Na barra de ferramentas desse separador, selecione **local** como o ambiente de execução andiris_sklearn.pyas o script seja executado. Estes podem já estar selecionados.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image44.png)

1.  Mover para o lado direito da barra de ferramentas e enter0.01in a **argumentos** campo.

    Este valor corresponde à taxa de regularização do modelo de regressão logística.

    ![Escolha de local e script](media/azure-stack-solution-machine-learning/image45.png)

1.  Selecionar o botão **Executar**. É agendado imediatamente um trabalho. O trabalho é listado no painel **Trabalhos**, no lado direito da janela do Workbench.

    ![Escolha de local e script](media/azure-stack-solution-machine-learning/image46.png)

    Após alguns instantes, o estado do trabalho passa de **submeter**ao **em execução**e, finalmente, para **concluído**.

1.  Selecione **Concluído** no texto do estado do trabalho, no painel **Trabalhos**.

    ![Executar sklearn](media/azure-stack-solution-machine-learning/image47.png)

    Uma janela de pop-up abre e apresenta o texto de saída padrão (stdout) para a execução. Para fechar o texto stdout, selecione o **feche** (**x**) botão no canto superior direito da janela de pop-up.

    ![Saída padrão](media/azure-stack-solution-machine-learning/image48.png)

1.  No mesmo Estado de tarefa na **trabalhos** painel, selecione o texto azul **iris_sklearn.py \[n\] **(* n * é o número de execuções) imediatamente acima a  **Concluído** estado e a hora de início. É aberta a janela **Propriedades da Execução**, que mostra as informações abaixo relativas a essa execução:

    -  Informações das **Propriedades da Execução**

    -  **Saídas**

    -  **Métricas**

    -  **Visualizações**, se existentes

    -  **Registos**

    Quando a execução estiver concluída, a janela de pop-up mostra os resultados abaixo:

    > [!Note]  
    > Uma vez que o tutorial introduziu alguma aleatoriedade no conjunto de preparação anteriormente, os resultados exatos podem variar dos apresentados aqui.

    ```Python  
        Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]

        Iris dataset shape: (150, 5)
        Regularization rate is 0.01
        LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
        Accuracy is 0.6792452830188679

        ==========================================
        Serialize and deserialize using the outputs folder.

        Export the model to model.pkl
        Import the model from model.pkl
        New sample: [[3.0, 3.6, 1.3, 0.25]]
        Predicted class is ['Iris-setosa']
        Plotting confusion matrix...
        Confusion matrix in text:
        [[50  0  0]
        [ 1 37 12]
        [ 0  4 46]]
        Confusion matrix plotted.
        Plotting ROC curve....

        ROC curve plotted.
        Confusion matrix and ROC curve plotted. See them in Run History details pane.

    ```

1.  Feche o separador **Propriedades da Execução** e regresse ao separador **iris_sklearn.py**.

1.  Repita para execuções adicionais.

Introduza uma série de valores no campo **Argumentos**, entre `0.001` e `10`. Selecione **Executar** para executar o código mais algumas vezes. O valor do argumento alterado sempre que é fornecido ao modelo de regressão logística no código, resultando em resultados diferentes, cada vez.

### <a name="review-the-run-history-in-detail"></a>Rever o histórico de execuções em detalhe

No Azure Machine Learning Workbench, cada execução de script é capturada como um registo de histórico de execuções. Ver o histórico de execuções de scripts específicos, abrindo o **execuções** vista.

1.  Para abrir a lista de **Execuções**, selecione o botão **Execuções** (ícone de relógio), na barra de ferramentas do lado esquerdo. Em seguida, selecione **iris_sklearn. PY** para mostrar o **Dashboard da execução** ofiris_sklearn.py.

    ![Vista de execução](media/azure-stack-solution-machine-learning/image49.png)

1.  O separador **Dashboard da Execução** abre-se.

    Reveja as estatísticas capturadas nas várias execuções. Os gráficos são compostos na parte superior do separador. Cada execução tem um número consecutivo e os detalhes da execução estão listados na tabela na parte inferior do ecrã.

    ![Dashboard da execução](media/azure-stack-solution-machine-learning/image50.png)

1.  Filtre a tabela e selecione um dos gráficos para ver o estado, a duração, a precisão e a taxa de regularização de cada execução.

2.  Selecione as caixas de verificação junto de duas ou mais execuções na tabela **Execuções**. Selecione o botão **Comparar** para abrir um painel de comparação detalhado. Reveja a comparação lado a lado.

3.  Para regressar a **Dashboard da Execução**, selecione o botão de retrocesso **Lista de Execuções** no canto superior esquerdo do painel **Comparação**.

    ![Regressar a Lista de execuções](media/azure-stack-solution-machine-learning/image51.png)

1.  Selecione uma execução individual para ver a vista de detalhes da mesma. Repare que as estatísticas da execução selecionada são listadas na secção **Propriedades da Execução**. Os ficheiros escritos na pasta de saída são listados no **saídas** secção e transferir os ficheiros a partir daí.

    ![Detalhes da Execução](media/azure-stack-solution-machine-learning/image52.png)

Os dois gráficos -- matriz de confusão e curva COR multiclasse -- são compostos na secção **Visualizações**. Também estão disponíveis na secção **Registos** todos os ficheiros de registos.

### <a name="run-scripts-in-the-azure-machine-learning-ml-workbench-cli-window"></a>Executar scripts na janela da CLI do Azure Machine Learning (ML) Bancada de trabalho

1.  Abra a interface de linha de comandos (CLI) do Azure Machine Learning:

    1.  Inicie o Azure Machine Learning Workbench.

    2.  No menu Workbench, selecione **Ficheiro** > **Abrir Linha de Comandos**.

    Iniciada a linha de comandos da CLI na pasta de projeto `C:\\Temp\\myIris` no Windows. Este é o projeto que criou na parte 1 do tutorial.

    > [!Important]  
    > Utilize esta janela da CLI para realizar os passos seguintes.

1.  Na janela da CLI, inicie sessão no Azure. [Obter mais informações sobre o comando az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    Ignore este passo se já iniciou sessão.

1.  Na linha de comandos, escreva:

    ```CLI
        az login
    ```

    Este comando devolve um código para utilizar no browser em [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin).

1.  Aceda a [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) no browser. Introduza o código que recebeu da recebido na CLI.

    A aplicação Workbench e a CLI utilizam caches de credenciais independentes na autenticação em recursos do Azure. Autenticação não é necessária novamente até que o token em cache expire.

1.  Se a organização tiver várias subscrições do Azure (ambiente empresarial), defina a subscrição a ser utilizado. Localizar a subscrição, defini-lo com o ID de subscrição e, em seguida, testá-lo.

1.  Liste todas as subscrições do Azure acedidas utilizando este comando:

    ```CLI
        az account list -o table 
    ```

    O **lista de contas de az** comando devolve a lista de subscrições disponíveis para o início de sessão. Se houver mais do que uma, identifique o valor de ID de subscrição para a subscrição utilizada.

1.  Defina a subscrição do Azure utilizada como conta predefinida:

    ```CLI
        az account set -s <the-subscription-id
    ```

    Em que < a-subscription-id > é o valor de ID para a subscrição utilizada. Não inclua os parêntesis angulares.

1.  Confirme a nova definição de subscrição ao pedir os detalhes da subscrição atual.

    ```CLI
        az account show
    ```

1.  Na janela da CLI, submeta o script **iris_sklearn.py** como uma experimentação.

    A execução de iris_sklearn. PY é executada no contexto da computação local.

1.  No Windows:

    ```CLI
        az ml experiment submit -c local .\\iris_sklearn.py
    ```

1.  Em macOS:

    ```CLI
        az ml experiment submit -c local iris_sklearn.py
    ```

    O resultado deve ser semelhante a: 

    ```
        RunId: myIris_1521077190506

    Executing user inputs .....
    ===========================

    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]

    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
            intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
            penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
            verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679

    ==========================================
    Serialize and deserialize using the outputs folder.

    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.

    Execution Details
    =================
    RunId: myIris_1521077190506

    ```

6.  Reveja a saída. Todos eles devem estar na mesma saída e os resultados como quando o Workbench executa o script.

7.  Volte ao Workbench e:

    Selecione o ícone de pasta no painel da esquerda para listar os ficheiros do projeto.  Abra o script de Python com o nome **run.py**. Este script é útil ao loop através de vários taxas de regularização. 

    ![Regresse à lista de execuções](media/azure-stack-solution-machine-learning/image53.png)

1.  Execute a experimentação múltiplas vezes com estas taxas.

    Este script inicia` aniris_sklearn.pyjob` com um s da taxa de regularização `10.0` (um número ridiculamente grande). O script diminui, em seguida, a taxa para metade na execução seguinte e assim por diante, até que a taxa não seja inferior `0.005`. O script contém o seguinte código:

    ![Regressar a Lista de execuções](media/azure-stack-solution-machine-learning/image54.png)

1.  Execute o script **run.py** a partir da linha de comandos, do seguinte modo:

    ```CLI
        python run.py
    ```
Este comando submete `iris_sklearn.py` várias vezes com diferentes taxas de regularização

Quando `run.py` estiver concluída, gráficos de diferentes métricas é apresentada na vista de lista do histórico de execuções no workbench.

### <a name="run-scripts-in-an-ubuntu-based-data-science-virtual-machine-dsvm-on-azure"></a>Executar scripts num baseada em Ubuntu ciência de dados Máquina Virtual (DSVM) no Azure

Para executar o script num contentor do Docker num computador Linux remoto, o acesso SSH (nome de utilizador e palavra-passe) é necessária para executar nessa máquina remota. Além disso, o computador tem de ter um motor do Docker instalado e em execução.

1.  Editar o gerado<your DSVM Name>.runconfigfile underaml_configand altere a estrutura do valuePySparktoPython padrão:

    ```yaml  
    Framework: Python
    ```
1.  Emita o comando como anteriormente na janela da CLI, usando destino*<DSVM>* desta vez para executar iris_sklearn. PY num contentor do Docker remoto: (substitui o <DSVM> com o nome de VM de ciência de dados, sem os colchetes).

    ```CLI
        az ml experiment submit -c <DSVM> iris_sklearn.py
    ```

O comando é executado como se no adocker-pythonenvironment, exceto a execução ocorre na VM do Linux remota. A janela da CLI apresenta as mesmas informações de saída.

### <a name="download-the-model-pickle-file"></a>Transferir o ficheiro pickle do modelo

Na parte anterior do tutorial, o script **iris_sklearn.py** foi executado no Machine Learning Workbench localmente. Esta ação serializou o modelo de regressão logística mediante a utilização do popular pacote de serialização de objetos do Python, o [pickle](https://docs.python.org/3/library/pickle.html).

1.  Abra a aplicação Machine Learning Workbench. Em seguida, abra a **myIris** projeto criado nas partes anteriores da série do tutorial.

2.  Depois do projeto estiver aberto, selecione o **ficheiros** botão (ícone de pasta), no painel da esquerda para abrir a lista de ficheiros na pasta de projeto.

3.  Selecione o ficheiro **iris_sklearn.py**. O código de Python abre-se num novo separador do editor de texto dentro do Workbench.

4.  Reveja o ficheiro **iris_sklearn.py** para ver onde o ficheiro pickle foi gerado. Selecione Control+F para abrir a caixa de diálogo **Localizar** e localize a palavra **pickle** no código de Python.

Este fragmento de código mostra como o ficheiro de saída de pickle foi gerado. O ficheiro pickle de saída é denominado **model.pkl** no disco.

    ```Python
        print("Export the model to model.pkl")
        f = open('./outputs/model.pkl', 'wb')
        pickle.dump(clf1, f)
        f.close()

    ```

1.  Localize o ficheiro pickle do modelo nos ficheiros de saída de uma execução anterior.

    Quando o **iris_sklearn. PY** script é executado, o ficheiro de modelo é escrito para o **produz** pasta com o nome **pkl**. Esta pasta reside no ambiente de execução escolhido para executar o script e não na pasta do projeto local. 

    1. Para localizar o ficheiro, selecione o **execuções** botão (ícone de relógio), no painel da esquerda para abrir a lista de **todas as execuções**.  

    2. O separador **Todas as Execuções** abre-se. Na tabela de execuções, selecione uma das execuções recentes em que o destino era **local** e o nome do script **iris_sklearn. PY**.  

    3. Abre-se o painel **Propriedades da Execução**. Na secção de canto superior direito do painel, tenha em atenção a **saídas** secção. d\. Para transferir o ficheiro pickle, selecione a caixa de verificação junto aos **pkl** do ficheiro e, em seguida, selecione **transferir**. Guarde o ficheiro na raiz da pasta do projeto. O ficheiro é necessário nos passos.  

    ![Transferir o ficheiro pickle](media/azure-stack-solution-machine-learning/image55.png)

### <a name="get-scoring-script-and-schema-files"></a>Obter a classificação de ficheiros de script e de esquema

Para implementar o serviço web juntamente com o ficheiro de modelo, o script de classificação é necessária. Opcionalmente, é necessário um esquema para os dados de entrada do serviço web. O script de classificação carrega o ficheiro **model.pkl** a partir da pasta atual e utiliza-o para produzir novas predições.

1.  Abra a aplicação Machine Learning Workbench. Em seguida, abra a **myIris** projeto criado na parte anterior da série do tutorial.

2.  Depois do projeto estiver aberto, selecione o **ficheiros** botão (ícone de pasta), no painel da esquerda para abrir a lista de ficheiros na pasta de projeto.

3.  Selecione o ficheiro **score_iris.py**. É aberto o script de Python. Este ficheiro é utilizado como o ficheiro de classificação.

    ![Ficheiro de classificação](media/azure-stack-solution-machine-learning/image56.png)

1.  Para obter o ficheiro de esquema, execute o script. Selecione o ambiente **local** e o script **score_iris.py** na barra de comandos e selecione **Executar**.

    Este script cria um ficheiro JSON na **saídas** seção, que captura o esquema de dados de entrada, o modelo precisa.

1.  Repare no painel **Tarefas**, no lado direito do painel **Dashboard do Projeto**. Aguarde que a tarefa de score_iris.py** * * de mais recente apresentar o verde **concluído** estado. Em seguida, selecione a hiperligação **score_iris.py [1]** relativa à última execução de trabalho para ver os detalhes da execução.

2.  No painel **Propriedades da Execução**, na secção **Saídas**, selecione o ficheiro recém-criado **service_schema.json**. Selecione a caixa de verificação junto ao nome do ficheiro e selecione **Transferir**. Guarde o ficheiro na pasta de raiz do projeto.

3.  Regresse ao separador anterior e o **score_iris** script. Ao utilizar a recolha de dados, podem ser capturadas as entradas de modelo e as previsões do serviço web. Os seguintes passos são interessantes para a recolha de dados.

4.  Reveja o código na parte superior do ficheiro, o qual importa a classe **ModelDataCollector**. Contém a funcionalidade de recolha de dados do modelo:

    ```Python  
        from azureml.datacollector import ModelDataCollector
    ```

1.  Reveja as linhas de código seguintes na função **init()** que instancia **ModelDataCollector:**

    ```Python  
        global inputs_dc, prediction_dc
        inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
        prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1.  Reveja as linhas de código seguintes na função **run(input_df)**, que recolhe os dados de entrada e de predição:

    ```Python  
        inputs_dc.collect(input_df)
        prediction_dc.collect(pred)
    ```

Agora, prepare o ambiente para operacionalizar o modelo.

## <a name="step-5-deploy-and-use-azure-container-registry"></a>Passo 5: Implementar e utilizar o Azure Container Registry

Implementar e utilizar o Azure Container Registry.

Crie um registo de contentor do Azure com o comando **az acr create**. O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. O grupo de recursos é o mesmo.

    ```CLI
        az acr create --resource-group <ResourceGroup> --name  <acrName> --sku Basic
    ```

### <a name="container-registry-login"></a>Iniciar sessão no registo de contentor

Utilize o comando **az acr login** para iniciar sessão na instância do ACR. Forneça o nome exclusivo dado ao registo de contentor quando este foi criado.

    ```CLI
        az acr login --name <acrName>
    ```

O comando devolve uma "mensagem de início de sessão teve êxito depois de concluir.

### <a name="prepare-to-operationalize-locally-for-development-and-testing-the-service"></a>Preparar para operacionalizar localmente para desenvolvimento e teste do serviço

Uso *modo de local* implantação para executar em contentores do Docker no computador local e para desenvolvimento e teste.

O motor do Docker tem de estar em execução localmente para concluir os passos seguintes de modo a operacionalizar o modelo. Utilize o `-h` sinalizador no final de cada comando para mostrar a mensagem de ajuda correspondente.

    > [!Note]  
    > If Docker engine is not locally available, proceed by creating a cluster in Azure for deployment and keep the cluster for re-use, or delete it after the tutorial to avoid ongoing charges.

    > [!Note]  
    > Web services deployed locally do not appear in Azure Portal's list of services. They will be running in Docker on the local machine.

1.  Abra a interface de linha de comandos (CLI). Na aplicação Machine Learning Workbench, no menu **Ficheiro**, selecione **Abrir Linha de Comandos**.

    A linha de comandos abre-se na localização de pasta do projeto atual **c:\\temp\\myIris**.

1.  Certifique-se de que o fornecedor de recursos do Azure **Microsoft. containerregistry** está registado na subscrição. Registe este fornecedor de recursos antes de criar um ambiente no passo 3. Verifica se já está registado com o seguinte comando:

    ```CLI
        az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
    ```

    Ver esta saída:

    ```CLI
        Provider                                    Status 
        --------                                    --------
        Microsoft.Authorization                     Registered 
        Microsoft.ContainerRegistry                 Registered 
        microsoft.insights                          Registered 
        Microsoft.MachineLearningExperimentation    Registered 
    ```

    Se **Microsoft. containerregistry** é registado não utilize o seguinte comando:

    ```CLI
    az provider register --namespace Microsoft.ContainerRegistry
    ```

    O registo pode demorar alguns minutos. Verificar estado de registo com o anterior **lista de fornecedores de az** comando ou o seguinte comando:

    ```CLI
    az provider show -n Microsoft.ContainerRegistry
    ```

    A terceira linha da saída apresenta **"registrationState": "Registering"**. Aguarde alguns momentos e repita a **mostrar** comando até que a saída apresente **"registrationState": "Registered.**

1.  Crie o ambiente. Execute este passo uma vez por ambiente.

    O comando de configuração seguinte requer acesso de contribuinte à subscrição. É necessário o acesso de Contribuidor ao grupo de recursos a implementar. Especifique o nome do grupo de recursos como parte do comando de configuração com o gflag.

    ```CLI
    az ml env setup -n <new deployment environment name> --location <e.g. eastus2> -g <existing resource group name>
    ```

    Siga as instruções no ecrã para aprovisionar uma conta de armazenamento para armazenar as imagens do Docker, um registo do Azure Container Registry que lista as imagens do Docker e uma conta do Azure Application Insights que recolhe telemetria. **Se utilizar o cswitch, o comando criará adicionalmente um cluster do Container Service**.

    O nome do cluster é uma forma de identificar o ambiente. A localização deve ser a mesma localização da conta de gestão de modelos criada a partir do portal do Azure.

    Para certificar-se de que o ambiente foi configurado com êxito, utilize o comando seguinte para verificar o estado:

    ```CLI
    az ml env show -n <deployment environment name> -g <existing resource group name>
    ```

    Certifique-se de que o "Estado de aprovisionamento" tem o valor "Com êxito", conforme mostrado, antes de configurar o ambiente no passo 5:

    ![Estado do Aprovisionamento](media/azure-stack-solution-machine-learning/image57.png)

1.  Defina o ambiente.

    Depois de concluída a configuração, utilize o comando seguinte para definir as variáveis de ambiente necessárias para operacionalizar o ambiente. Utilize o mesmo nome de ambiente utilizado anteriormente no passo 3. Utilize o mesmo nome de grupo de recursos que foi produzido na janela do comando quando o processo de configuração foi concluído.

    ```CLI
        az ml env set -n <deployment environment name> -g <existing resource group name>
    ```

1.  Para verificar a configuração adequada do ambiente operacionalizado para a implementação do serviço local web, introduza o seguinte comando:

    ```CLI
    az ml env show
    ```

    Agora, crie o serviço web em tempo real.

    > [!Note]  
    > Reutilize a conta de gestão de modelos e o ambiente para implementações de serviços da web subsequentes. Não é necessário criá-los para cada serviço web. Uma conta ou um ambiente pode ter vários serviços Web associados.

### <a name="create-a-real-time-web-service-by-using-separate-commands"></a>Criar um serviço Web em tempo real com comandos separados

Como alternativa para o **em tempo real de criar serviço de ml az** comando mostrado anteriormente, estes passos podem também ser executados separadamente.

Em primeiro lugar, registe o modelo. Em seguida, gere o manifesto, crie a imagem do Docker e crie o serviço Web. Esta abordagem passo a passo dá mais flexibilidade a cada passo. Além disso, as entidades geradas nos passos anteriores podem ser reutilizadas.

1. Indique o nome do ficheiro pickle para registar o modelo.

    ```CLI
    az ml model register --model model.pkl --name model.pkl
    ```

    Este comando gera um ID de modelo.

2.  Crie um manifesto.

    Para criar um manifesto, utilize o seguinte comando e indique a saída de ID de modelo do passo anterior: 
    
    ```CLI
    az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json
    ```

    Este comando gera um ID de manifesto.

3.  Crie uma imagem do Docker.

    Para criar uma imagem do Docker, utilize o seguinte comando e indique a saída de valor de ID de manifesto do passo anterior. As dependências conda também podem ser utilizadas por meio do `-c` mudar. 
    
    ```CLI
    az ml image create -n irisimage --manifest-id <manifest ID> -c aml_config\conda_dependencies.yml
    ```
    
    Este comando gera um ID de imagem do Docker.

2.  Crie o serviço.

    Para criar um serviço, utilize o seguinte comando e indique a saída de ID de imagem do passo anterior: 
    
    ```CLI
    az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
    ```
    
    Este comando gera um ID de serviço Web.
    
    Em seguida, execute o serviço web.

## <a name="step-6-deploy-a-kubernetes-cluster-to-azure-stack"></a>Passo 6: Implementar um cluster de Kubernetes no Azure Stack

Implemente um cluster de Kubernetes no Azure Stack.

Kubernetes pode ser instalado através de modelos do Azure Resource Manager gerados pelo motor de serviços de contentor do Azure (ACS) no Azure Stack. [*Kubernetes* ](https://kubernetes.io/) é um sistema de código-fonte aberto para automatizar a implantação, dimensionamento e da gestão de aplicações em contentores. R [ *contentor* ](https://www.docker.com/what-container) está contida numa imagem, semelhante a uma VM. Ao contrário de uma VM, a imagem de contentor inclui recursos de que necessita para executar um aplicativo, como o código, o tempo de execução para executar o código, bibliotecas específicas e definições.

Utilize o Kubernetes para:

 -  Desenvolva aplicações escaláveis em massa, pode ser atualizadas, que podem ser implementadas em segundos.

 -  Simplificar o design do aplicativo e melhore a sua confiabilidade ao diferentes aplicativos de Helm. [*Helm* ](https://github.com/kubernetes/helm) é uma ferramenta de empacotamento de código-fonte aberto que ajuda a instalar e gerenciar o ciclo de vida de aplicações do Kubernetes.

 -  Facilmente monitorizar e diagnosticar o estado de funcionamento dos aplicativos com o dimensionamento e funcionalidade de atualização.

> [!Note]  
> Instalação do cluster irá demorar cerca de uma hora, plano adequadamente.

### <a name="prerequisites-for-kubernetes-cluster-deployment"></a>Pré-requisitos para implementação de cluster do Kubernetes

Para começar, confirme as permissões e preparação para o Azure Stack:

1.  Validar que o **criar Cluster do Kubernetes (pré-visualização)** Item está disponível no mercado de pilha do Azure. Se este item está em faltando, funcionam com um operador de pilha do Azure para adicionar este item para o ambiente do Azure Stack.

2.  Certifique-se a capacidade de criar aplicações no inquilino do Azure Active Directory (Azure AD). As permissões são necessárias para a implementação de Kubernetes.

    Para obter instruções sobre a verificar as permissões, consulte [ *permissões de verificação do Active Directory do Azure*](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

3.  Gere um público e privado par de chaves SSH para iniciar sessão na VM do Linux no Azure Stack. A chave pública é necessário quando criar o cluster. Para obter instruções, consulte [gerar uma chave de autenticação para SSH](#generate-an-authenticatio-key-for-ssh).

4.  Verificação de que a subscrição no portal de inquilino do Azure Stack é válida, e há suficiente IP público endereços disponíveis para adicionar novos aplicativos.

    O cluster não é possível implementar um Azure Stack **administrador** subscrição. Utilize um **utilizador** subscrição.

###  <a name="generate-an-authentication-key-for-ssh"></a>Gerar uma chave de autenticação para SSH

A partir do subsistema Windows para Linux sessão utilize os seguintes comandos para gerar uma chave de autenticação: 

1. Escreva:

    ```Bash  
    ssh-keygen -t rsa
    ```
    
2. Selecione `id_rsa` quando lhe for pedido. 
3. Crie uma frase de acesso quando lhe for pedido. É importante ter em conta esta palavra-passe para utilizar mais tarde. 
    O resultado será semelhante ao abaixo: 
    
    ```Bash  
    Your identification has been saved in `id_rsa`.
    Your public key has been saved in `id_rsa.pub`. 
    The key fingerprint is: SHA256:lUtUUjzaqWqGeolEPKeBmsnrhcNGM9Dn2OxYatt05SE  <user>@<machine-name>
    The key's randomart image is:  
    ```
    ![Texto alternativo](media/azure-stack-solution-machine-learning/image58.png)

4. Depois de gerar a chave, cole as informações de chave com os comandos seguintes: 
    ```Bash
    cat id_rsa.pub
    ```
    O resultado será semelhante ao abaixo: 
    ```Bash
    ssh-rsa  AAAAB3NzaC1yc2EAAAADAQABAAABAQDHaWrAktR3BlQ356T8Qvv8O2Q/U/hsXQwS9xJbuduuc9lkJwddzgmNCyM9PooZWYtGVXyHU6SC3YH1XkwqGtKhtPb03d24hmhX1euAaqIqHHSp4WgUS5s1gNsp37L8QCSGNCnF31FWavI8bnjOjccUkMowKl8iyGN++5UyQgNuynEVUbFJjrntoDL66HUu88xDxTcVB7rqDr2QKFwYJkg4HSoHYpezJd7W8kcmv33eh0xs8nlDA7Dzu7zXpyVc54bH9XtPR6EUXaQa+UqKaNlQNiJqEs+1X/zNuK9V6NLVNiO0h3jCHI3K8o4VnZyRKHCQProasiiPLUUJ6SF/RTLN  <user>@<machine-name>
    ```
    
5. Copie a chave gerada para o SSH Key campo público.

### <a name="create-a-service-principal-in-azure-ad"></a>Criar um principal de serviço no Azure AD

1.  Inicie sessão no global [ *portal do Azure*](http://portal.azure.com/).

2.  Inicie sessão com o inquilino do Azure AD associado à instância do Azure Stack.

3.  Crie uma aplicação do Azure AD.

    1. Selecione **do Azure Active Directory** > **+ registos de aplicações**> **novo registo de aplicação**.
    2. Introduza um **nome** do aplicativo. 
    3. Selecione **aplicação Web / API**. 
    4. Introduza `http://localhost` para o **URL de início de sessão**. 
    5. Selecione **Criar**.

1.  Aponte o **ID da Aplicação**. Este ID é necessária ao criar o cluster e é referenciada como **ID de cliente do Principal de serviço**.

2.  Selecione **configurações** > **chaves**.

    1. Introduza o **Descrição**. 
    2. Selecione **nunca expira** para **Expires**. 
    3. Selecione **Guardar**. Certifique-se de observar a cadeia de caracteres de chave. A cadeia de caracteres de chave é necessária ao criar o cluster e é referenciada como o **segredo do cliente de Principal de serviço**.

### <a name="give-the-service-principal-access"></a>Dar o acesso de principal de serviço

Atribua o principal de serviço acesso à subscrição, para que ele pode criar recursos.

1.  Inicie sessão para o [portal de administração](https://adminportal.local.azurestack.external/).

2.  Selecione **mais serviços** > * * as subscrições de utilizador * * > **+ adicionar**.

3.  Selecione a subscrição que criou.

4.  Selecione **controlo de acesso (IAM)** > selecione **+ adicionar**.

5.  Selecione o **proprietário** função.

6.  Selecione o nome da aplicação criado para o serviço principal. Escreva o nome na caixa de pesquisa, se for necessário.

7.  Selecione **Guardar**.

8.  Abra o [portal do Azure Stack](https://portal.local.azurestack.external).

9.  Selecione **+ nova** > **computação** > **Cluster do Kubernetes**. Selecione **Criar**.

    ![Implementar Modelo de Solução](media/azure-stack-solution-machine-learning/image59.png)

10\. Selecione **Noções básicas** na criar o Cluster de Kubernetes.

    ![Deploy Solution Template](media/azure-stack-solution-machine-learning/image60.png)

11. Introduza o **nome de utilizador de administrador VM de Linux**. Nome de utilizador para as máquinas de virtuais de Linux que fazem parte do cluster de Kubernetes e DVM.

12. Introduza o **chave pública SSH** utilizado na autorização de todas as máquinas de Linux criados como parte do cluster de Kubernetes e DVM.

13. Introduza o **prefixo de DNS de perfil do mestre** que seja exclusivo para a região. Tem de ser uma região exclusivo nome, como, por exemplo `ask8s-12345`. Tente escolhê-lo mesmo que o grupo de recursos dê um nome como melhor prática.

    > [!Note]  
    > Para cada cluster, utilize um prefixo DNS de perfil de mestre de novos e únicos.

14. Introduza o **contagem de perfil de agrupamento de agentes**. A contagem de contém o número de agentes no cluster. Podem existir de 1 a 4.

15. Introduza o **ID de cliente do Principal de serviço** é utilizada pelo fornecedor de cloud do Azure do Kubernetes.

16. Introduza o **segredo do cliente de Principal de serviço** criados durante a criação da aplicação do principal de serviço.

17. Introduza o **versão de fornecedor de Cloud do Azure do Kubernetes**. Esta é a versão para o fornecedor do Azure do Kubernetes. O Azure Stack lança uma compilação do Kubernetes personalizada para cada versão do Azure Stack.

18. Selecione o **subscrição** ID.

19. Introduza o nome de um novo grupo de recursos ou selecione um grupo de recursos existente. O nome do recurso tem de ser de alfanuméricos e minúsculas.

20. Selecione o **localização** do grupo de recursos. Esta é a região escolhido para a instalação do Azure Stack.

### <a name="specify-the-azure-stack-settings"></a>Especifique as definições do Azure Stack

1.  Selecione o **definições de carimbo de data / do Azure Stack**.

    ![Implementar Modelo de Solução](media/azure-stack-solution-machine-learning/image61.png)

2.  Introduza o **ponto final do Gestor de recursos do Azure do inquilino**. Este é o ponto de final do Azure Resource Manager para ligar ao criar o grupo de recursos para o cluster de Kubernetes. O ponto final do operador do Azure Stack é necessário para um sistema integrado. Para o Azure Stack Development Kit (ASDK), utilize `https://management.local.azurestack.external`.

3.  Introduza o **ID de inquilino** para o inquilino. Ver [ *obter ID de inquilino* ](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) para localizar o inquilino valor de ID.

### <a name="install-kubectl-on-windows-powershell-environment"></a>Instalar o kubectl no ambiente do Windows PowerShell

A partir de dentro do ambiente de WSL execute os seguintes comandos para instalar o kubectl no ambiente de WSL.

```PowerShell  
Install-script -name install-kubectl -scope CurrentUser -force
Install-kubectl.ps1 -downloadlocation “C:\Users\<Current User>\Documents\Kube
```

### <a name="install-kubectl-on-the-windows-subsystem-for-linux-environment"></a>Instalar o kubectl o subsistema Windows para o ambiente do Linux

A partir de dentro do ambiente de WSL, execute os seguintes comandos para instalar o kubectl no ambiente de WSL.

```Bash  
    apt-get update && apt-get install -y apt-transport-https
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
    sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
    apt-get update
    apt-get install -y kubectl
```

### <a name="configure-kubectl"></a>Configurar kubectl

Para que o kubectl localizar e aceder a um cluster de Kubernetes, um*kubeconfig ficheiro* é necessária. Isso é criado automaticamente quando é criado um cluster utilizando o kube up.sh ou implementar um cluster de Minikube. Consulte a [ *guias de introdução* ](https://kubernetes.io/docs/setup/) para obter mais informações sobre a criação de clusters. Para obter acesso a um cluster criado por outro utilizador, consulte a [ *documento de partilha de acesso de Cluster*](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/). Por predefinição, está localizada em configuração de kubectl **~.kube/config**.

### <a name="check-the-kubectl-configuration"></a>Verifique a configuração de kubectl

Verifique essa kubectl está corretamente configurado ao obter o estado do cluster:

```Bash  
kubectl cluster-info
```

kubectl está configurado corretamente para aceder ao cluster quando for apresentada a resposta de url.

kubectl não está corretamente configurado ou não é possível ligar a um cluster de Kubernetes, se for apresentada a mensagem seguinte:

```Bash  
The connection to the server <server-name:port> was refused -  did you specify the right host or port?
```

Por exemplo, ao executar um cluster de Kubernetes num portátil local, uma ferramenta pode ser necessária, (minikube ou semelhante) para voltar a executar os comandos indicados acima.

Se, mas o cluster ainda não é acessível, verificação de configuração adequada ao utilizar kubectl. o cluster-info retorna a resposta de url:

```Bash  
> kubectl cluster-info dump
```

### <a name="enable-shell-autocompletion"></a>Permitir Preenchimento automático de shell

kubectl inclui suporte de preenchimento automático, fazer a ativação de shell rápida e fácil.

O próprio script de conclusão é gerado pelo kubectl e está acessível a partir do perfil.

### <a name="connect-to-the-kubernetes-cluster"></a>Ligue ao Cluster do Kubernetes

Para ligar ao cluster, o cliente de linha de comandos do Kubernetes (**kubectl**) é necessária. Pode encontrar instruções para ligar e gerir o cluster na [documentação dos serviços de contentor do Azure.](https://docs.microsoft.com/azure/container-service/dcos-swarm/)

Qualquer cliente SSH pode ser utilizado para ligar o cluster de Kubernetes. Recomenda-se o subsistema do Windows para Linux (WSL). A máquina a ligar ao Cluster de Kubernetes estará no grupo de recursos criado para o cluster e serão iniciados com o prefixo de vmd edge-ml pilha.

```Bash  
ssh <user>@vmd-edge-ml-stack.<FQDN of Kubernetes Machine in  Azure Stack>
```

Quando estiverem ligados à máquina do Kubernetes, execute a máquina seguinte para obter o ficheiro de configuração de Kubernetes.

```Bash  
sudo find / -name \*kubeconfig\* -type f
```

A saída será terá um aspeto semelhante ao seguinte:

```Bash  
/var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

Copie estas informações de caminho de ficheiros e, em seguida, execute o seguinte comando, colar no caminho de ficheiro kubeconfig copiado acima:

```Bash  
sudo cat  /var/lib/waagent/custom-script/download/0/acs-engine/_output/edgemlstack/kubeconfig/kubeconfig.<regionname>.json
```

O resultado será um grande bloco de texto, que é o conteúdo JSON não processado. Copie este texto de saída e, em seguida, cole este código num arquivo do Visual Studio, guardar como um ficheiro JSON. Isso resulta num arquivo de kubeconfig.json armazenada localmente. (savepara/mnt/c/utilizadores/ <current user> /documentos/diretório Kube como kubeconfig.json)

### <a name="configure-the-kubernetes-cluster"></a>Configurar o Cluster de Kubernetes

Depois do ficheiro JSON local é obtido, numa nova sessão WSL, utilize os seguintes comandos para configurar o Cluster.

```Bash  
    cd /mnt/c/users/<current user>/documents/Kube
    kubectl proxy
    kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
    kubectl proxy
    set KUBECONFIG=”/mnt/c/users/<current user>/documents/Kube/kubeconfig.json”
    kubectl.exe config view
```

Definições de configuração Kubernetes serão definidas (ver um resultado abaixo).

![Texto alternativo](media/azure-stack-solution-machine-learning/image62.png)

Inicie o serviço de local proxy:

```Bash  
kubectl proxy
```

Navegue para o cluster de kubernetes da interface do Usuário no seguinte endereço: `https://localhost:8001`.

![Texto alternativo](media/azure-stack-solution-machine-learning/image63.png)

Agora tem um local para implementar o contentor e um contentor que reside na cloud que pode ver no local.

![Texto alternativo](media/azure-stack-solution-machine-learning/image64.png)

Personalizar o **iris_deployment.yaml** ficheiro (localizado na /*mnt/c/utilizadores/<current user>/documentos/Kube diretório*), de modo **webservicename** e contentores  **Imagem** e **nome** corresponder a implementação, utilizando qualquer editor de código à escolha.

![Texto alternativo](media/azure-stack-solution-machine-learning/image65.png)

Defina a porta do contentor para **5001.**

![Texto alternativo](media/azure-stack-solution-machine-learning/image66.png)

E, em seguida, crie o **imagePullSecret**:

### <a name="create-a-secret-in-the-cluster-that-holds-the-authorization-token"></a>Criar um segredo no cluster que contém o token de autorização

Um cluster de Kubernetes utiliza o segredo de **docker-registry** tipo para autenticar com um registo de contentor para solicitar uma imagem privada.

Criar este segredo, nomeando- **azuremlcr**:

```Bash  
kubectl create secret docker-registry azuremlcr --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password= "<your-pword>" --docker-email=<your-email>
```

Localize:

- **< your-registry-server >** é o Azure Container Registry **servidor de início de sessão**.
- **< nome da >** é o Azure Container Registry **nome de utilizador**.
- **< your pword >** é o Azure Container Registry **palavra-passe**. Certifique-se de que a palavra-passe é aspas.
- **< your e-mail >** é o utilizador que tenha acesso de R/W para o contentor.
- Encontrar estas informações sobre o **contentor do Azure** **Registro** sob **chaves de acesso**.
- Credenciais de docker agora são definidas no cluster como um segredo chamado **azuremlcr**.

Guardar a **iris_deployment.yaml** ficheiro (localizado na /*mnt/c/utilizadores/<current user>/documentos/Kube diretório*).

### <a name="create-the-kubernetes-container"></a>Criar o contentor do Kubernetes

```Bash  
kubectl.exe create -f /mnt/c/users/<current  user>/documents/Kube/iris_deployment.yaml
```

    ![Alt text](media/azure-stack-solution-machine-learning/image67.png)

Verifique o estado da implementação:

```Bash  
Kubectl get deployments
```

    ![Alt text](media/azure-stack-solution-machine-learning/image68.png)

A implementação pode demorar algum tempo.

### <a name="configure-visual-studio-team-services-to-deploy-automatically"></a>Configurar o Visual Studio Team Services para implementar automaticamente

#### <a name="create-a-team-project"></a>Criar um projeto de equipe

1.  Certifique-se de [associação de grupo de administradores de coleção do projeto.](https://docs.microsoft.com/vsts/organizations/security/set-project-collection-level-permissions?view=vsts) Para criar projetos de equipe **criar novos projetos** permissão deve ser definida como **permitir**.

2.  Na página de projetos, selecione **novo projeto**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image69.png)

1.  Nomeie o projeto **HybridMLIris**.

2.  Selecione o respetivo tipo de controlo de origem inicial como **Git**

3.  Selecione um processo e selecione **criar**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image70.png)

### <a name="import-some-code--create-repository"></a>Importar um criar repositório de código

É necessário um repositório Git para o código YAML.

#### <a name="add-user-to-the-git-repo"></a>Adicionar o utilizador para o repositório GIT

1.  A partir do painel de projeto padrão, selecione as credenciais do Git gerar.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image71.png)

1.  Introduza a palavra-passe sempre que necessário e guarde as credenciais do Git.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image72.png)

1.  Inicializar o repositório, selecionando o **inicializar** botão e a criação de um **Leiame** ficheiro.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image73.png)

#### <a name="clone-the-git-repository-locally-and-upload-the-code"></a>Clone o repositório de Git localmente e carregar o código. 

1.  Tornar um diretório na máquina em `c:\\users\\<User>\\source\\repos\\hybridMLIris`e clonar o repositório

    ```Bash  
    sudo mkdir /mnt/c/users/<User>/source sudo mkdir /mnt/c/users/<User>/source/repos sudo mkdir /mnt/c/users/<User>/source/repos/hybridMLIris cd /mnt/c/users/<User>/source/repos/hybridMLIris sudo git clone  https://<yourvstssite>.visualstudio.com/HybridMLIris/_git/HybridMLIris
    ```

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image74.png)

1.  Navegue para o repositório recentemente clonado:

    ```Bash  
    ls
    cd ./HybridMLIris
    ```
    
    ![Texto alternativo](media/azure-stack-solution-machine-learning/image75.png)

1.  Copiar o **iris_deployment.yaml** ficheiros no repositório.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_deployment.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_deployment.yaml
    ``` 

1.  Consolidar a alteração no GIT

    ```Bash  
    git add . git commit -m Added Deployment YAML git push
    ```

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image76.png)

### <a name="prepare-the-private-build-and-release-agent-for-vsts-integration"></a>Preparar o compilação privada e o agente de lançamento para a integração de VSTS

#### <a name="prerequisites"></a>Pré-requisitos

VSTS autentica contra o Azure Resource Manager com um Principal de serviço. Para o VSTS poder aprovisionar os recursos numa subscrição do Azure Stack requer contribuinte de estado. \ **seguem-se as etapas de alto nível que precisam de ser configurado para ativar a autenticação de tal:**

1.  Principal de serviço deve ser criado ou um já existente pode ser utilizado.

2.  Chaves de autenticação precisam ser criadas para o Principal de serviço.

3.  Tem de ser validado por meio de controlo de acesso baseado em funções para permitir que o SPN ser parte da função colaboradores de subscrição do Azure Stack.

4.  Uma nova definição de serviço no VSTS tem de ser criada usando os pontos finais de pilha do Azure, bem como informações de SPN.

#### <a name="service-principal-creation"></a>Criação de Principal de serviço

Consulte a [instruções de criação de Principal de serviço](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) para criar um principal de serviço e escolha a aplicação/API da Web para o tipo de aplicação.

**Criação de chave de acesso**

Um Principal de serviço requer uma chave para autenticação, siga os passos nesta secção para gerar uma chave.

1.  Partir **registos das aplicações** no Azure Active Directory, selecione a aplicação.

    ![selecionar aplicação](media/azure-stack-solution-machine-learning/image77.png)

1.  Tome nota do valor do **ID da aplicação. O valor é utilizado quando configurar o ponto final de serviço no VSTS.**

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image78.png)

1.  Para gerar uma chave de autenticação, selecione **Definições**.

    ![selecionar as definições](media/azure-stack-solution-machine-learning/image79.png)

1.  Selecione **Chaves**.

    ![selecionar chaves](media/azure-stack-solution-machine-learning/image80.png)

1.  Indique uma descrição e uma duração para a chave. Quando terminar, selecione **Guardar**.

    ![guardar chave](media/azure-stack-solution-machine-learning/image81.png)

Depois de guardar a chave, o valor da mesma é apresentado. Copie este valor, que é necessário mais tarde. O **valor da chave** com a aplicação é necessário um ID para iniciar sessão como o aplicativo. Store onde a aplicação o possa obter o valor da chave.

![Texto alternativo](media/azure-stack-solution-machine-learning/image82.png)

#### <a name="get-tenant-id"></a>Obter ID de inquilino

Como parte da configuração do ponto final de serviço, o VSTS requer o **ID de inquilino** que corresponde ao diretório do AAD o carimbo de data / Azure Stack foi implementada. Siga os passos nesta secção para recolher o ID do inquilino.

1.  Selecione **Azure Active Directory**.

    ![selecionar Azure Active Directory](media/azure-stack-solution-machine-learning/image83.png)

1.  Para obter o ID de inquilino, selecione **propriedades** para o inquilino do Azure AD.

    ![selecionar propriedades do Azure AD](media/azure-stack-solution-machine-learning/image84.png)

1.  Copie o **ID do Diretório**. Este valor é o ID do inquilino.

    ![ID do inquilino](media/azure-stack-solution-machine-learning/image85.png)

Conceder os direitos de Principal de serviço para implementar recursos na subscrição do Azure Stack

Para aceder aos recursos na subscrição, atribua a aplicação a uma função. Decida que função representa as permissões corretas para a aplicação. Para saber mais sobre as funções disponíveis, veja [RBAC: funções incorporadas](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Defina o âmbito no nível da subscrição, no grupo de recursos ou ao recurso. As permissões são herdadas para níveis inferiores de âmbito. Por exemplo, adicionar uma aplicação à função do leitor para um grupo de recursos permite que leia o grupo de recursos e todos os recursos que nele contidos.

1.  Navegue para o nível desejado de âmbito para atribuir a aplicação. Por exemplo, para atribuir uma função no âmbito da subscrição, selecione **subscrições**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image86.jpeg)

1.  Selecione o **subscrição** (grupo de recursos ou recurso) para atribuir a aplicação.

    ![Selecione a subscrição para atribuição](media/azure-stack-solution-machine-learning/image87.png)

1.  Selecione **controlo de acesso (IAM)**.

    ![Selecione o acesso](media/azure-stack-solution-machine-learning/image88.png)

1.  Selecione **Adicionar**.

    ![Selecione adicionar](media/azure-stack-solution-machine-learning/image89.png)

1.  Selecione a função para atribuir a aplicação. A imagem seguinte mostra os **proprietário** função.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image90.png)

1.  Por predefinição, as aplicações do Azure Active Directory não são apresentadas nas opções disponíveis. Para localizar a aplicação, **forneça o nome** na pesquisa campo e selecioná-lo.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image91.png)

1.  Selecione **guardar** para concluir a atribuir a função. A aplicação é apresentada na lista de utilizadores atribuídos a uma função para esse âmbito.

### <a name="role-based-access-control"></a>Controlo de Acesso Baseado em Funções

Do Azure com base em função de controlo de acesso (RBAC) permite a gestão de acessos detalhada para o Azure e o Azure Stack. Utilizando o RBAC, pode ser concedida apenas a quantidade de acesso que os utilizadores precisam para realizar seus trabalhos. Para obter mais informações sobre o controlo de acesso baseado em funções, consulte [gerir o acesso aos recursos de subscrição do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal?toc=%252fazure%252factive-directory%252ftoc.json).

**Conjuntos de agentes do VSTS**

Em vez de gerir individualmente cada agente, os agentes estão organizados nas **conjuntos de agentes**. Um conjunto de agentes define o limite de partilha para todos os agentes nesse agrupamento. Conjuntos de agentes do VSTS, passam a ter para a conta do VSTS, pelo que pode ser compartilhada entre projetos de equipe. Para obter mais informações e um tutorial sobre como criar VSTS conjuntos de agentes Consulte [criar conjuntos de agentes e filas.](https://docs.microsoft.com/vsts/build-release/concepts/agents/pools-queues?view=vsts)

**Adicionar o token de acesso de aPersonal (PAT) para o Azure Stack**

 -  Inicie sessão no conta VSTS e selecione ativado **perfil de conta** nome.

 -  Selecione **gerir a segurança** à página de criação de token de acesso.

![Texto alternativo](media/azure-stack-solution-machine-learning/image92.png)

![Texto alternativo](media/azure-stack-solution-machine-learning/image93.jpeg)

![Texto alternativo](media/azure-stack-solution-machine-learning/image94.jpeg)

> [!Note]  
> Obter as informações do token. Não será apresentado depois de deixar este ecrã.

1.  Copiar o **token**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image95.png)

#### <a name="install-the-vsts-build-agent-on-the-azure-stack-hosted-build-server"></a>Instalar que agente de compilação VSTS na pilha do Azure alojado o servidor de compilação do

1.  Ligar para o **servidor de compilação do** implementadas no anfitrião do Azure Stack.

    > [!Note]  
    > Verifique se o que servidor do Azure Stack alojado criar é acessível a partir da Internet pública. Caso contrário, trabalhar com um operador de pilha do Azure para obter acesso.

    ```Bash  
    ssh <user>@<buildserver.publicip>
    ```

2.  Atualize o servidor de compilação do Ubuntu para a versão mais recente (18.04):

    ```Bash  
    sudo su
    apt-get update
    apt-get upgrade
    apt-get dist-upgrade
    apt-get autoremove
    do-release-upgrade -d
    ```

    > [!Note]  
    > Esta operação irá demorar algum tempo.

2.  Instale aplicações de pré-requisitos para o servidor de compilação. Partir do bash shell do Ubuntu com base em execução de servidor de compilação os seguintes comandos:

    ```Bash  
    wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
    sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
    wget -q https://packages.microsoft.com/config/ubuntu/18.04/prod.list 
    sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
    sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
    sudo apt-get install apt-transport-https
    sudo apt-get update
    sudo apt-get install liblttng-ust0 libcurl3 libssl1.0.0 curl lsb-release libkrb5-3 zlib1g libicu60 aspnetcore-runtime-2.1 dotnet-sdk-2.1
    wget https://github.com/PowerShell/PowerShell/releases/download/v6.1.0-preview.3/powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo dpkg -i powershell-preview_6.1.0-preview.3-1.ubuntu.18.04_amd64.deb
    sudo apt-get install -f
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add –
    sudo apt-get update && sudo apt-get install azure-cli
    ```

2.  Baixe e implante o agente de compilação como um serviço a utilizar um **token de acesso pessoal (PAT)** e executar como a conta de administrador de VM.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image96.png)

    ```Bash  
        cd \home\<user>
        sudo mkdir myagent && cd myagent
        wget https://vstsagentpackage.azureedge.net/agent/2.134.2/vsts-agent-linux-x64-2.134.2.tar.gz
        sudo tar zxvf vsts-agent-linux-x64-2.134.2.tar.gz
    ```

2.  Aceda à pasta de agentes de compilação extraídos. Execute o seguinte código.

    ```Bash  
        cd ..
        sudo chmod -R 777 myagent
        cd myagent
        ./config.sh
    ```

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image97.png)

2.  Após **./config.sh**concluído, execute o seguinte código para ativar o serviço no arranque do servidor e iniciar o serviço:

    ```Bash  
    sudo ./svc.sh install
    sudo ./svc.sh start
    ```

O agente agora está visível na pasta do VSTS.

#### <a name="endpoint-creation-permissions"></a>Permissões de criação do ponto final

Os utilizadores podem criar pontos finais para que compilações VSTO podem implementar aplicações de serviço do Azure para a pilha. VSTS liga-se para o agente de compilação, que, em seguida, liga-se com o Azure Stack.

![Texto alternativo](media/azure-stack-solution-machine-learning/image98.png)

1.  Sobre o **configurações** menu, selecione **segurança**.

2.  Na **grupos de VSTS** lista à esquerda, selecione **criadores de ponto final**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image99.png)

3.  Sobre o **separador Membros,** selecione a **+ adicionar**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image100.png)

1.  Tipo **nome de utilizador** e selecione o nome de utilizador da lista.

2.  Selecione **guardar alterações**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image101.png)

3.  Na **grupos de VSTS** lista à esquerda, selecione **administradores do ponto final**.

4.  Sobre o **separador Membros,** selecione a **+ adicionar**.

5.  Introduza um **nome de utilizador** e selecione de que o utilizador da lista.

6.  Selecione **guardar as alterações.**

    ![buchatech](media/azure-stack-solution-machine-learning/image102.jpeg)

    O agente de compilação no Azure Stack ganhos de instruções do VSTS, que, em seguida, transmite informações de ponto final para comunicação com o Azure Stack.

    VSTS para ligação do Azure Stack agora está pronto.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image103.png)

### <a name="configure-build-and-release-definitions"></a>Configurar as definições de compilação e versão

Agora que as ligações são estabelecidas, manualmente será criada endpoint do Azure, AKS e registo de contentor do Azure para a compilação do mapa e definições de versão.

#### <a name="create-the-build-definition-for-the-yaml-code"></a>Criar a definição de compilação para o código YAML do

1.  Selecione a secção baseia-se sob o hub de compilação e lançamento e criar uma nova definição.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image104.png)

1.  Selecione o Git do VSTS e selecione o repositório que criou anteriormente.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image105.png)

1.  Selecione o Pipeline vazio como modelo

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image106.png)

1.  Nome da compilação **artefacto de cópia** e selecione o servidor de compilação do Azure Stack para a fila de agente.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image107.png)

1.  Selecione os processos de fase 1 e mude o nome para **artefacto de cópia**, em seguida, **adicionar uma tarefa** para a fase de:

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image108.png)

1.  Selecione **publicar artefactos de compilação** partir a **utilitário** lista e selecione **Add**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image109.png)

1.  Selecione o **caminho para publicar** e selecione o **iris_deployment.yaml** ficheiro.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image110.png)

1.  Nome do artefacto **iris_deployment** e selecione o local de publicação ser **Visual Studio Team Services/TFS**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image111.png)

1.  Selecione **guardar e colocar em fila**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image112.png)

1.  Verificar o estado da compilação ao selecionar o ID de compilação.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image113.png)

Êxito terá um aspeto semelhante a este:

![Texto alternativo](media/azure-stack-solution-machine-learning/image114.png)

#### <a name="create-the-release-definition-for-the-yaml-code"></a>Criar a definição de versão para o código YAML

1.  Selecione a secção de versões sob o hub de criação e versão, uma nova definição

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image115.png)

1.  Selecione o Pipeline vazio como um modelo.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image106.png)

1.  Nome do Azure de ambiente de pilha.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image116.png)

1.  Adicione um artefacto novo selecionando **artefactos** e **+ adicionar**

2.  Selecione a compilação como o tipo de origem e **HybridMLIris** como o projeto.

3.  Em seguida, selecione a definição de compilação que criou anteriormente para a origem.

4.  Em seguida, selecione **adicionar**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image117.png)

1.  Selecione o Azure Stack dos ambientes, em seguida, adicionar uma nova tarefa ao Azure Stack

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image118.png)


1.  Sobre a fase de agente, defina a fila de agente para o servidor do Azure Stack alojado criar.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image119.png)

1.  Adicionar uma nova tarefa para esta fase, selecione a implementar a tarefa de Kubernetes em implementar e selecione Adicionar.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image120.png)


1.  Atribua o nome **Kubectl aplicar** (nome predefinido) e selecione o comando apply.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image121.png)

    Agora, crie uma nova ligação de serviço do Kubernetes.

#### <a name="create-kubernetes-service-endpoint"></a>Criar o ponto final de serviço do Kubernetes

1.  Em ligação de serviço do Kubernetes, selecione o **+ novo** botão e selecione**Kubernetes**da lista. Pode utilizar este ponto final para ligar a**VSTS**e o**Azure Container Service (AKS)**.

2.  **Nome da ligação**: forneça o nome da ligação.

3.  **URL do servidor**: forneça o endereço do serviço de contentor no formathttp: / / {endereço do servidor de API}

4.  **Kubeconfig**: para obter o valor de Kubeconfig, execute os seguintes comandos do Azure numa linha de comandos iniciada com o privilégio de administrador.

    > [!Important]  
    > Utilize esta janela da CLI para realizar os passos seguintes.

6.  Na janela da CLI, inicie sessão no Azure. [Obter mais informações sobre o comando az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

7.  Na linha de comandos, escreva:

    ```CLI
        az login
    ```

10. Este comando devolve um código para utilizar no browser em <https://aka.ms/devicelogin>.

11. Aceda a <https://aka.ms/devicelogin> no browser. Quando lhe for pedido, introduza o código, recebido na CLI, no navegador.

    ![Ponto final de serviço do Kubernetes](media/azure-stack-solution-machine-learning/image122.png)

1.  Escreva o seguinte comando no prompt de comando para obter as credenciais de acesso para o cluster de Kubernetes.

### <a name="azure-ml-workbench-cli"></a>CLI do Azure ML Workbench

AZ aks get-credentials-grupo de recursos <yourResourceGroup> nome <yourazurecontainerservice>

![Ponto final de serviço do Kubernetes](media/azure-stack-solution-machine-learning/image123.png)

1.  Navegue para o **.kube**pasta sob o diretório de raiz (por exemplo: c:\\utilizadores\\<user>\\documentos\\Kube)

2.  Copie o conteúdo dos**config**de ficheiros e cole-a na janela de ligação de Kubernetes. Selecione o**OK**botão.

    ![Ponto final de serviço do Kubernetes](media/azure-stack-solution-machine-learning/image124.png)
    

3.  Depois do ponto de extremidade do Kubernetes é criado e selecionado, selecione a caixa de verificação da ficheiros de configuração utilizado para adicionar um ficheiro de configuração. Em seguida, procure o ficheiro de iris_deployment.yaml nos artefactos ligado.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image125.png)

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image126.png)

4.  Guarde a definição de versão.

#### <a name="check-the-status-of-the-release-definition"></a>Verificar o estado da definição de versão. 

Depois de guardado, a definição de versão do VSTS automaticamente deve iniciar uma versão.

Verifique o estado da implementação, a execução de um comando rápido no prompt de comando WSL, em seguida, verificar a IU do Kubernetes.

```Bash  
kubectl get deployments
```

O resultado deverá ter um aspeto semelhante a esta, enquanto estiver no processo de implantação.

![Texto alternativo](media/azure-stack-solution-machine-learning/image127.png)

```Bash  
kubectl proxy
```

Assim que o kubernetes interface do Usuário está em execução, navegue para a implantação na [ **https://localhost:8001/** ](https://localhost:8001/) , em seguida, navegue para **cargas de trabalho -> conjuntos de réplicas**.

![Texto alternativo](media/azure-stack-solution-machine-learning/image128.png)

### <a name="deploy-the-yaml-service"></a>Implementar o serviço YAML

#### <a name="upload-the-irisserviceyaml-to-the-repository-and-sync-changes"></a>Carregar o iris_service.yaml para as alterações do repositório e sincronização

1.  Navegue para o repositório recentemente clonado:

    ```Bash  
    cd /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/
    ```

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image75.png)

1.  Copiar o **iris_service.yaml** ficheiros no repositório.

    ```Bash  
    cp /mnt/c/users/<User>/documents/Kube/iris_service.yaml  /mnt/c/users/<User>/source/repos/HybridMLIris/HybridMLIris/iris_service.yaml
    ```

1.  Consolidar a alteração no GIT

    ```Bash  
    git add .
    git commit -m “Added Service YAML” 
    git push
    ```

![Texto alternativo](media/azure-stack-solution-machine-learning/image129.png)

#### <a name="update-the-build-definition-for-the-yaml-code"></a>Atualizar a definição de compilação para o código YAML do

1.  Selecione a secção baseia-se sob o hub de criação e versão e selecione a definição que criou anteriormente.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image130.png)

2.  Selecione o botão Editar para editar a definição.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image131.png)

3.  **Adicionar uma tarefa** para a fase. Selecione **publicar artefactos de compilação** partir a **utilitário** lista e selecione **Add**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image108.png)

4.  Atribua o nome **Kubectl aplicar** (nome predefinido) e selecione o comando apply.



#### <a name="update-the-release-definition-for-the-yaml-code"></a>Atualizar a definição de versão para o código YAML

1.  Selecione a secção de theReleases sob o hub de criação e versão e selecione a definição de versão que criou anteriormente. Em seguida, selecione a ligação editar.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image132.png)

1.  Selecione o ambiente **do Azure Stack** , em seguida, adicionar uma nova tarefa ao Azure Stack.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image133.png)

1.  Adicionar uma **nova tarefa** para esta fase, selecione o **implementar no Kubernetes** tarefas sob **implementar** e selecione **adicionar**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image134.png)

1.  Atribua o nome **Kubectl aplicar** (nome predefinido) e selecione o comando apply.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image109.png)

1.  Definir a ligação de serviço Kubernates para a ligação de pilha do Azure criado anteriormente e, em seguida, selecione o **utilizar ficheiros de configuração** caixa de verificação para adicionar um ficheiro de configuração. Procure o ficheiro de iris_service.yaml nos artefactos ligado.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image135.png)


    ![Texto alternativo](media/azure-stack-solution-machine-learning/image136.png)

1.  Guarde a definição de versão.

#### <a name="check-the-status-of-the-release-definition"></a>Verificar o estado da definição de versão

Depois de guardado, a definição de versão do VSTS automaticamente deve iniciar uma versão.

Verifique o estado da implementação, a execução de um comando rápido no prompt de comando WSL, em seguida, verificar a IU do Kubernetes.

```Bash  
kubectl get deployments
```

O resultado deverá ter um aspeto semelhante a esta, enquanto estiver no processo de implantação.

![Texto alternativo](media/azure-stack-solution-machine-learning/image127.png)


```Bash  
kubectl proxy
```

Assim que o kubernetes interface do Usuário está em execução, navegue para a implantação na [ **https://localhost:8001/** ](https://localhost:8001/) , em seguida, navegue para **cargas de trabalho -> conjuntos de réplicas**.

![Texto alternativo](media/azure-stack-solution-machine-learning/image137.png)


### <a name="kubernetes-scoring-and-validation"></a>Kubernetes de classificação e a validação

Iniciar a IU do Kubernetes

```Bash  
kubectl proxy
```

Navegue para a IU do Kubernetes em seguida, aceda a **implementações** -> **Iris-implementação** -> **novo conjunto de réplicas**  ->  **Iris-implantação-xxxxxxxxx** (onde os xs são o ID de implementação).

![Texto alternativo](media/azure-stack-solution-machine-learning/image138.png)

Em seguida, navegue até **serviços** e selecione o **ponto final externo** do serviço para validar que está a funcionar.

![Texto alternativo](media/azure-stack-solution-machine-learning/image139.png)

Deverá ser apresentada uma mensagem de validação semelhante ao seguinte:

![Texto alternativo](media/azure-stack-solution-machine-learning/image140.png)

#### <a name="create-azure-stack-scoring-function-app-in-the-azure-stack-portal"></a>Criar a pilha do Azure, aplicação de funções de classificação no Portal do Azure Stack

Uma aplicação de funções é necessário para alojar a execução de cada função. Uma aplicação function app permite agrupar como uma unidade lógica para fácil gestão, implementação e partilha de recursos da função.

1.  A partir do portal de utilizador do Azure Stack, selecione o **+ novo** botão encontrado no canto superior esquerdo, em seguida, selecione**Web + móvel** >**aplicação de funções**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image141.png)

1.  Dê um nome de função **funções de dados** e coloque-o no mesmo grupo de recursos com o Machine Learning restantes conteúdo. Permitir que a ferramenta de auto-criar um novo plano do serviço de aplicações para consumo e utilizar a conta de armazenamento criada anteriormente para o armazenamento de aplicação.

    ![Definir novas definições de aplicação de função](media/azure-stack-solution-machine-learning/image142.png)

1.  Selecione**criar**para aprovisionar e implementar a aplicação de funções.

2.  Selecione o ícone de notificação no canto superior direito do portal e procure o**implementação concluída com êxito** mensagem.

    ![Definir novas definições da aplicação de funções](media/azure-stack-solution-machine-learning/image143.png)

1.  Selecione**Ir para recurso** para ver a nova aplicação de função.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image144.png)

1.  Criar uma nova função, selecionando **funções**, em seguida, o **+ nova função** botão.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image145.png)

1.  Selecione o acionador HTTP

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image146.png)

1.  Selecione **C\#**  como o idioma e o nome da função: **apagar classificação de dados**e definir o nível de autorização **anónimo**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image147.png)

1.  Copiar-colar o conteúdo do exemplo de código para apagar classificação de dados para a função.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image148.png)

#### <a name="use-postman-to-validate-functions"></a>Utilizar o Postman para validar as funções

Para se certificar de que configurou seu Kbernetes e funções corretamente pode utilizar a aplicação gratuita Postman para testar e validar esquemas e as funções. Para iniciar este processo, primeiro tem de recolher algumas informações de sua instância do Kubernetes.

1.  Navegue para a IU do Kubernetes em seguida, aceda a **implementações** -> **Iris-implementação** -> **novo conjunto de réplicas**  ->  **Iris-implantação-xxxxxxxxx** (onde os xs são o ID de implementação)

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image138.png)

1.  Em seguida, navegue até **serviços** e copie a **ponto final externo**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image149.png)

1.  Transferir e instalar a aplicação Postman [aqui](https://www.getpostman.com/apps) se for necessário.

2.  Inicie sessão na aplicação Postman e fechar a caixa de diálogo novo ficheiro.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image150.png)

1.  Na aplicação postman, selecione POST....

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image151.png)

1.  Colar o **ponto final externo** URL na aplicação postman sob a **URL do pedido** adicionando  **\\pontuação** ao final do URL, conforme mostrado abaixo.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image152.png)

1.  Selecione o **corpo** separador e, em seguida, os dados de tipo como **brutos**, em seguida, **JSON**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image153.png)

1.  A partir de um navegador da web, navegue para **ponto final externo**. Adicionar o seguinte para o URL **/swagger.json** isso nos leva para o ficheiro Swagger de serviços, utilizado para testar o programa de configuração.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image154.png)

1.  Copie o exemplo listado na **swagger** ficheiro.

2.  Na aplicação Postman, cole o exemplo no corpo da postagem e selecione **enviar**. Ele deve retornar um valor semelhante ao mostrado abaixo.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image155.png)

## <a name="step-7-create-an-azure-stack-storage-account-and-storage-queue"></a>Passo 7: Criar uma conta de armazenamento do Azure Stack e a fila de armazenamento

Crie uma conta de armazenamento do Azure Stack e a fila de armazenamento para dados.

1.  Inicie sessão no Portal de utilizador do Azure Stack. (Cada Azure Stack tem um portal exclusivo URL)

2.  No portal de utilizador de pilha do Azure, expanda o menu no lado esquerdo para abrir o menu de serviços e escolha **todos os serviços**. Desloque para baixo até **armazenamento** e escolha **contas de armazenamento**. Na **contas de armazenamento** janela escolher **Add**.

3.  Introduza um nome para a conta de armazenamento.

4.  Selecione a opção de replicação para a conta de armazenamento: **LRS**.

5.  Especifique um novo grupo de recursos ou selecione um grupo de recursos existente.

6.  Selecione **Local** com a localização para a conta de armazenamento.

7.  Selecione**criar**para criar a conta de armazenamento.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image156.png)

1.  Selecione a conta de armazenamento recentemente criada.

2.  Selecione no**filas**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image157.png)

1.  Selecione no **+ fila** e dê o nome de fila e selecione **OK.**

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image158.png)

1.  Obter o **cadeia de ligação** para a fila de armazenamento e copie-o.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image159.png)

1.  Navegue para a aplicação de funções do Azure e, em seguida, selecione **as definições da aplicação**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image160.png)

1.  Configurações de aplicativo da aplicação de funções, desloque para baixo para as definições da aplicação e selecione **+ Adicionar nova definição**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image161.png)

1.  Introduza o nome da conta de armazenamento na **nome** campo, adicionando ao fim; _STORAGE

Isso permite que a aplicação para compreender o que se trata de um ponto de extremidade de conta de armazenamento.

1.  Em seguida, cole a cadeia de ligação para o **valor** campo.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image162.png)

1.  Desloque-se para a parte superior das configurações de aplicativo e selecione **guardar**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image163.png)

### <a name="update-the-scoring-function-to-use-storage-queue"></a>Atualizar a função de classificação de mensagens em fila para utilizar a fila de armazenamento

1.  Selecione no **integrar** na função e anule a seleção do **obter** opção.

2.  Selecione **Guardar**.

3.  Em seguida, selecione **+ nova saída** das saídas.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image164.png)

1.  Em seguida, selecione **armazenamento de filas do Azure** e escolha **selecione**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image165.png)

1.  Atualização do **nome da fila** para a fila de armazenamento criado anteriormente e, em seguida, defina o **ligação da conta de armazenamento** para a ligação de conta de armazenamento criada anteriormente e selecione **guardar.**

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image166.png)

## <a name="step-8-create-a-function-to-handle-clean-data"></a>Passo 8: Criar uma função para processar limpar dados

Crie uma nova função do Azure Stack para mover os apagar dados do Azure Stack para o Azure.

1.  Criar uma nova função, selecionando **funções**, em seguida, o **+ nova função** botão.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image167.png)

1.  Selecione **acionador de temporizador**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image168.png)

1.  Selecione **C\#**  como o idioma e o nome da função: **carregamento para o azure** e defina a agenda **0 0 \*/1 \* \* \***  que CRON notação é uma vez por hora.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image169.png)

### <a name="get-the-connection-string-to-the-azure-hosted-storage-account"></a>Obter a cadeia de ligação para a conta de armazenamento alojado do Azure

1.  Navegue até <https://portal.azure.com> e, em seguida, selecione o **conta de armazenamento do Azure** criado anteriormente.

2.  Selecione **chaves de acesso**, em seguida, copie a **cadeia de ligação** para a conta de armazenamento.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image170.png)

### <a name="update-the-upload-to-azure-function-to-use-the-azure-hosted-storage"></a>Atualizar o carregamento para o azure função a utilizar o armazenamento alojado do Azure

1.  Navegue para a aplicação de funções do Azure e, em seguida, selecione **as definições da aplicação**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image171.png)

1.  Configurações de aplicativo da aplicação de funções, desloque para baixo para as definições da aplicação e selecione **+ Adicionar nova definição**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image172.png)

1.  Introduza o nome da conta de armazenamento na **nome** campo, adicionando ao fim; _STORAGE

Isso permite que a aplicação para compreender o que se trata de um ponto de extremidade de conta de armazenamento.

1.  Em seguida, cole a Azure alojado armazenamento conta cadeia de ligação para o **valor** campo.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image173.png)

1.  Desloque-se para a parte superior das configurações de aplicativo e selecione **guardar**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image174.png)

1.  Navegue de volta para o **carregamento para o azure** função.

2.  Selecione no **integrar** na função.

3.  Em seguida, selecione **+ nova saída** das saídas.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image175.png)

1.  Em seguida, selecione **armazenamento de Blobs do Azure** e escolha **selecione**.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image176.png)

1.  Atualização do **caminho** para o contentor de armazenamento criado anteriormente no seguinte formato: **uploadeddata /. txt de {rand-guid}** e, em seguida, defina o **ligação da conta de armazenamento** para o Ligação da conta de armazenamento para o Azure criado anteriormente e selecione **guardar.**

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image177.png)

1.  Copiar-colar o conteúdo do exemplo de código para **carregamento para o azure** na função.

2.  Modificar conforme necessário para apontar para a cadeia de ligação de contas de armazenamento.

3.  Salve e execute o código.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image178.png)

1.  Verificar a conta de armazenamento alojado do Azure para ver os dados tenha sido analisada para a cloud do Azure: êxito terá um aspeto semelhante para o abaixo.

    ![Texto alternativo](media/azure-stack-solution-machine-learning/image179.png)

Foi corrigidos de dados confidenciais, o Azure Stack alojado Kubernetes Machine Learning e carregados para a Cloud pública do Azure da pilha do Azure no local, através do Azure Stack alojado aplicações Function App, os dados e pode preparar os dados para carregamentos num borda/desligado cenário.

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre os padrões de Cloud do Azure, veja [padrões de conceção de Cloud](https://docs.microsoft.com/azure/architecture/patterns).
