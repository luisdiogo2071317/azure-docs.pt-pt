---
title: Artigo de tutorial para as funcionalidades de pré-visualização do Azure Machine Learning - Interface de Linha de Comandos | Microsoft Docs
description: Este tutorial orienta-o ao longo de todos os passos necessários para concluir uma classificação de Íris completa a partir da interface de linha de comandos.
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: tutorial
ms.date: 10/15/2017
ms.openlocfilehash: 05238c27a5654ae24c619b52d769abbf90b940e7
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Tutorial: Classificar Íris com a interface de linha de comandos
Os serviços do Azure Machine Learning (pré-visualização) são uma solução de análise avançada e ciência de dados ponto a ponto integrada para os cientistas de dados profissionais prepararem dados, desenvolverem experimentações e implementarem modelos à escala da cloud.

Neste tutorial, vai aprender a utilizar as ferramentas da interface de linha de comandos (CLI) nas funcionalidades de pré-visualização do Azure Machine Learning para: 
> [!div class="checklist"]
> * Configurar uma conta de Experimentação e criar uma área de trabalho
> * Criar um projeto
> * Submeter uma experimentação para vários destinos de computação
> * Promover e registar um modelo preparado
> * Implementar um serviço Web para classificar dados novos

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, precisa de:
- Acesso a uma subscrição do Azure e permissões para criar recursos nessa subscrição. 
  
  Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

- Aplicação do Azure Machine Learning Workbench instalada, conforme descrito em [Início Rápido: Instalar e iniciar os serviços do Azure Machine Learning](../service/quickstart-installation.md). 

  >[!IMPORTANT]
  >Não crie as contas do serviço Azure Machine Learning, uma vez que vai fazê-lo com a CLI neste artigo.
 
## <a name="getting-started"></a>Introdução
A interface de linha de comandos (CLI) do Azure Machine Learning permite-lhe realizar todas as tarefas necessárias para um fluxo de ciência de dados completo. Pode aceder às ferramentas da CLI das seguintes formas:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>Opção 1. Iniciar a CLI do Azure ML a partir da caixa de diálogo de início de sessão do Azure ML Workbench
Quando abre o Azure ML Workbench e inicia sessão pela primeira vez, e se ainda não tiver acesso a uma Conta de Experimentação, é apresentado o ecrã seguinte:

![não foi encontrada nenhuma conta](media/tutorial-iris-azure-cli/no_account_found.png)

Clique na ligação **Command Line Window** (Janela da Linha de Comandos) na caixa de diálogo para iniciar a janela da linha de comandos.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>Opção 2. Iniciar a CLI do Azure ML a partir da aplicação Azure ML Workbench
Se já tiver acesso a uma Conta de Experimentação, pode iniciar sessão com êxito. Depois, pode abrir a janela da linha de comandos ao clicar no menu **File** (Ficheiro) --> **Open Command Prompt** (Abrir Linha de Comandos).

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>Opção 3. Ativar a CLI do Azure ML numa janela da linha de comandos arbitrária
Também pode ativar a CLI do Azure ML em qualquer janela da linha de comandos. Para tal, inicie uma janela de comando e introduza os comandos seguintes:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
Para tornar a alteração permanente, pode utilizar `SETX` no Windows. Em macOS, pode utilizar `setenv`.

>[!TIP]
>Pode ativar a CLI do Azure na sua janela de terminal favorita ao definir as variáveis de ambiente anteriores.

## <a name="step-1-log-in-to-azure"></a>Passo 1. Iniciar sessão no Azure
O primeiro passo consiste em abrir a CLI a partir da aplicação AMLWorkbench (File [Ficheiro] > Open Command Prompt [Abrir Linha de comandos]). Desta forma, é garantido que o seu ambiente de Python é o correto e que os comandos da CLI do ML estão disponíveis. 

Agora, pode definir o contexto certo na CLI para aceder e gerir recursos do Azure.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>Passo 2. Criar uma Conta de Experimentação e uma Área de Trabalho novas do Azure Machine Learning

Neste passo, vai criar uma Conta de Experimentação e uma área de trabalho novas. Veja [Azure Machine Learning concepts](overview-general-concepts.md) (Conceitos do Azure Machine Learning) para obter mais detalhes sobre as contas de experimentação e as áreas de trabalho.

> [!NOTE]
> As contas de experimentação precisam de uma conta de armazenamento, que é utilizada para armazenar as saídas das suas execuções de experimentações. O nome da conta de armazenamento tem de ser globalmente exclusivo no Azure, porque existe um url associado ao mesmo. Se não especificar uma conta de armazenamento já existente, o nome da sua conta de experimentação é utilizado para criar uma nova. Confirme que utiliza um nome exclusivo ou receberá o erro _"The storage account named \<storage_account_name> is already taken."_ (“A conta de armazenamento com o nome nome_da_conta_de_armazenamento> já está a ser utilizada”). Em alternativa, pode utilizar o argumento `--storage` para indicar uma conta de armazenamento já existente.

```azure-cli
# create a resource group 
$ az group create --name <resource group name> --location <supported Azure region>

# create a new experimentation account with a new storage account
$ az ml account experimentation create --name <experimentation account name> --resource-group <resource group name>

# create a new experimentation account with an existing storage account
$ az ml account experimentation create --name <experimentation account name>  --resource-group <resource group name> --storage <storage account Azure Resource ID>

# create a workspace in the experimentation account
az ml workspace create --name <workspace name> --account <experimentation account name> --resource-group <resource group name>
```

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>Passo 2.a (opcional) Partilhar uma área de trabalho com um colega de trabalho
Aqui, pode explorar como partilhar o acesso a uma área de trabalho com um colega de trabalho. Os passos para partilhar o acesso a uma conta de experimentação ou a um projeto são os mesmos. Só é preciso atualizar a forma de obter o ID do Recurso do Azure.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com` no comando acima tem de ser uma identidade válida do Azure AD no diretório ao qual a subscrição atual pertence.

## <a name="step-3-create-a-new-project"></a>Passo 3. Criar um novo projeto
O passo seguinte é criar um projeto novo. Existem várias formas de começar um projeto novo.

### <a name="create-a-new-blank-project"></a>Criar um projeto novo em branco

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Criar um projeto novo com um modelo de projeto predefinido
Pode criar um projeto novo com um modelo predefinido.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Criar um projeto novo associado a um repositório Git na cloud
Pode criar um projeto novo associado a um repositório Git do VSTS (Visual Studio Team Service). Sempre que for submetida uma experimentação, é consolidado um instantâneo de toda a pasta do projeto para o repositório Git remoto. Veja [Using Git repository with an Azure Machine Learning Workbench project](using-git-ml-project.md) (Utilizar o repositório Git com um projeto do Azure Machine Learning Workbench) para obter mais detalhes.

> [!NOTE]
> O Azure Machine Learning só suporta repositórios Git vazios criados no VSTS.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> Se estiver a receber o erro "Repository url might be invalid or user might not have access" (“O url do repositório pode ser inválido ou o utilizador pode não ter acesso”), pode criar um token de segurança no VSTS (no menu _Security_ [Segurança], _Add personal access tokens_ [Adicionar tokens de acesso pessoal]) e utilizar o argumento `--vststoken` quando criar o projeto. 

### <a name="sample_create"></a>Criar um projeto novo a partir de um exemplo
Neste exemplo, vai criar um projeto novo mediante a utilização de um projeto de exemplo como modelo.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Quando o projeto estiver criado, utilize o comando `cd` para introduzir o diretório do mesmo.

## <a name="step-4-run-the-training-experiment"></a>Passo 4 Executar a experimentação de preparação 
Os passos seguintes pressupõem que tem um projeto com o exemplo Íris (veja [ Create a new project from an online sample](#sample_create) [Criar um projeto novo a partir de um exemplo online]).

### <a name="prepare-your-environment"></a>Preparar o ambiente 
Para o exemplo Íris, tem de instalar matplotlib.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Submeter a experimentação

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Iterar a experiência com taxas de regularização descendentes
Com alguma criatividade, é simples criar um script Python que submete experimentações com taxas de regularização diferentes. (Poderá ter de editar o ficheiro para apontar para o caminho de projeto correto.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>Passo 5. Ver o histórico de execuções
O comando a seguir apresenta uma lista de todas as execuções realizadas anteriormente. 

```azure-cli
$ az ml history list -o table
```
Executar o comando acima mostra uma lista de todas as execuções que pertencem a este projeto. Pode ver que também são mostradas as métricas das taxas de exatidão e regularização. Assim, é mais fácil identificar a melhor execução na lista.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Passo 5.a Ver o anexo criado por uma determinada execução 
Para ver o anexo associado a uma determinada execução, pode utilizar o comando “info” do histórico de execuções. Localize o ID de uma execução específica na lista anterior.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Para transferir os artefactos de uma execução, pode utilizar o comando abaixo:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>Passo 6. Promover artefactos de uma execução 
Uma das execuções tem um AUC melhor, pelo que é essa que vai ser utilizada quando criar um serviço Web de classificação para implementação em produção. Para tal, tem primeiro de promover os artefactos para um elemento.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Esta ação cria uma pasta `assets` no diretório do seu projeto com um ficheiro `model.pkl.link`. Este ficheiro link é utilizado para fazer referência a um elemento promovido.

## <a name="step-7-download-the-files-to-be-operationalized"></a>Passo 7. Transferir os ficheiros para serem operacionalizados
Transfira o modelo promovido para que possa utilizá-lo para criar um serviço Web de predição. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-set-up-your-model-management-environment"></a>Passo 8. Configurar o ambiente de gestão de modelos 
Crie um ambiente para implementar serviços Web. Pode executar o serviço Web no computador local com o Docker. Em alternativa, implemente-o num cluster do ACS para operações de grande escala. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>Passo 9. Criar uma conta de gestão de modelos 
É necessária uma conta de gestão de modelos para implementar e acompanhar os seus modelos na produção. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>Passo 10. Criar um serviço Web
Crie um serviço Web que devolva uma predição ao utilizar o modelo que implementou. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score_iris.py -r python –n <web service name>
```

## <a name="step-11-run-the-web-service"></a>Passo 11. Executar o serviço Web
Utilizando o ID do serviço Web da saída do passo anterior, chame-o e teste-o. 

```azure-cli
# Get web service usage infomration
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="step-12-deleting-all-the-resources"></a>Passo 12. Eliminar todos os recursos 
Vamos concluir este tutorial ao eliminar todos os recursos que criou, a menos que queira continuar a trabalhar com os mesmos. 

Para tal, elimine o grupo de recursos que contém os recursos. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a utilizar o Azure Machine Learning para: 
> [!div class="checklist"]
> * Configurar uma conta de experimentação, mediante a criação de uma área de trabalho
> * Criar projetos
> * Submeter experimentações para vários destinos de computação
> * Promover e registar um modelo preparado
> * Criar uma conta de gestão de modelos para gerir modelos
> * Criar um ambiente para implementar serviços Web
> * Implementar um serviço Web e classificar com dados novos
