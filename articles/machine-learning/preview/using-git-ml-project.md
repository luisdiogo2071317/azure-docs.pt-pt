---
title: "Utilizar um repositório de Git com um projeto do Workbench do Azure Machine Learning | Microsoft Docs"
description: "Este artigo explica como utilizar um repositório de Git em conjunto com um projeto do Azure Machine Learning Workbench."
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: f4f1112fe68bdb2a26f68b3da08fe97f9d22d3b7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="use-a-git-repo-with-a-machine-learning-workbench-project"></a>Utilizar um repositório de Git com um projeto do Workbench do Machine Learning
Saiba como o Azure Machine Learning Workbench utiliza o Git para fornecer controlo de versão e certifique-se reproducibility na sua experimentação de ciência de dados. Saiba como associar o seu projeto com uma nuvem repositório de Git (repositório).

Machine Learning Workbench foi concebida para a integração de Git. Quando cria um novo projeto, a pasta do projeto é automaticamente "inicializado pelo Git" como um repositório de Git local. Também é criado um repositório de Git local segundo, oculto, com um ramo denominado AzureMLHistory /\<projeto GUID\>. O ramo mantém um registo das alterações de pasta do projeto para cada execução. 

Associar o projeto do Azure Machine Learning um repositório de Git permite o controlo de versão automática, localmente e remotamente. O repositório de Git está alojado no Visual Studio Team Services (Serviços de equipa). Porque o projeto de Machine Learning é associado um repositório de Git, qualquer pessoa com acesso ao repositório remoto pode transferir o código de origem mais recente para outro computador (roaming).  

> [!NOTE]
> Serviços de equipa tem a suas próprias lista de controlo de acesso (ACL), que é independente do serviço Azure Machine Learning experimentação. Acesso de utilizador pode variar entre um repositório de Git e uma área de trabalho do Machine Learning ou o projeto. Poderá ter de gerir o acesso. 
> 
> Se pretende conceder um membro do agrupamento código ao nível do acesso ao seu projeto de Machine Learning ou simplesmente partilhe a área de trabalho, terá de conceder ao utilizador as permissões corretas para o repositório de Git de serviços da equipa de acesso. 

Para gerir o controlo de versão com o Git, pode utilizar o ramo principal ou criar outros ramos no repositório. Também pode utilizar o repositório de Git local e push para o repositório de Git remoto, se de que está aprovisionado.

Este diagrama ilustra a relação entre um repositório de Git de serviços da equipa e um projeto de Machine Learning:

![Git do Azure Machine Learning](media/using-git-ml-project/aml_git.png)

Para começar a utilizar um repositório de Git remoto, execute os passos descritos nas secções seguintes.

> [!NOTE]
> Atualmente, o Azure Machine Learning suporta repositórios de Git apenas em contas de serviços da equipa. Suporte para gerais repos de Git, como o GitHub, está a ser planeado para o futuro.

## <a name="step-1-create-a-machine-learning-experimentation-account"></a>Passo 1. Criar uma conta de experimentação do Machine Learning
Criar uma conta de experimentação do Machine Learning e instalar a aplicação do Azure Machine Learning Workbench. Para obter mais informações, consulte [instalar e criar o guia de introdução](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Passo 2. Criar um projeto de equipa ou utilizar um projeto de equipa existente
No [portal do Azure](https://portal.azure.com/), crie um novo projeto de equipa:
1. Selecione  **+** .
2. Procurar **projeto de equipa**.
3. Introduza as informações necessárias:
    - **Nome**: um nome de agrupamento.
    - **Controlo de versão**: selecione **Git**.
    - **Subscrição**: selecione uma subscrição que tenha uma conta de experimentação do Machine Learning.
    - **Localização**: Idealmente, escolha uma região próximo os recursos de experimentação do Machine Learning.
4. Selecione **Criar**. 

![Criar um projeto de equipa no portal do Azure](media/using-git-ml-project/create_vsts_team.png)

Certifique-se de que iniciar sessão utilizando a mesma conta do Azure Active Directory (Azure AD) que utiliza para aceder ao Machine Learning Workbench. Caso contrário, o sistema não consegue aceder à Machine Learning Workbench com as suas credenciais do Azure AD. Uma exceção é se utilizar a linha de comandos para criar o projeto de Machine Learning e fornecer um token de acesso pessoal para aceder o repositório de Git. Vamos discutir em mais detalhe posteriormente no artigo.

Ir diretamente para o projeto de equipa que criou, utilize o URL https://\<nome do projeto de equipa\>. visualstudio.com.

## <a name="step-3-set-up-a-machine-learning-project-and-git-repo"></a>Passo 3. Configurar um projeto de Machine Learning e o repositório de Git

Para configurar um projeto de Machine Learning, tem duas opções:
- Criar um projeto de Machine Learning que tenha um repositório de Git remoto
- Associar um projeto existente do Machine Learning um repositório de Git de serviços da equipa

### <a name="create-a-machine-learning-project-that-has-a-remote-git-repo"></a>Criar um projeto de Machine Learning que tenha um repositório de Git remoto
Abra o Workbench do Machine Learning e criar um novo projeto. No **repositório de Git** box, introduza o URL do repositório de Git de serviços da equipa do passo 2. Normalmente, parece este: https://\<nome da conta de serviços da equipa\>.visualstudio.com/_git/\<nome do projeto\>

![Criar um projeto de Machine Learning que tenha um repositório de Git](media/using-git-ml-project/create_project_with_git_rep.png)

Também pode criar o projeto utilizando a ferramenta de linha de comandos do Azure (CLI do Azure). Pode optar por introduzir um token de acesso pessoal. Aprendizagem pode utilizar este token para aceder ao repositório de Git em vez de utilizar as credenciais do Azure AD:

```
# Create a new project that has a Git repo by using a personal access token.
$ az ml project create -a <Experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <Team Services personal access token>
```

> [!IMPORTANT]
> Se escolher o modelo de projeto em branco, o repositório de Git que optar por utilizar poderão já ter um ramo principal. Aprendizagem simplesmente clones localmente o ramo principal. Adiciona a pasta de aml_config e outros ficheiros de metadados de projeto para a pasta do projeto local. 
>
> Se escolher qualquer outro modelo de projeto, o repositório de Git *não é possível* já tiver um ramo principal. Se existir, verá um erro. A alternativa consiste em utilizar o `az ml project create` comando para criar o projeto, com um `--force` mudar. Isto elimina os ficheiros no ramo principal original e substitui-los com os novos ficheiros no modelo que escolher.

É criado um novo projeto de Machine Learning, com a integração do repositório de Git remota ativada. A pasta do projeto é sempre inicializado pelo Git como um repositório de Git local. O Git remoto está definido para o repositório de Git de serviços da equipa remoto, pelo que pode emitir consolidações para o repositório de Git remoto.

### <a name="associate-an-existing-machine-learning-project-with-a-team-services-git-repo"></a>Associar um projeto existente do Machine Learning um repositório de Git de serviços da equipa
Pode criar um projeto de Machine Learning sem um repositório de Git de serviços da equipa e baseiam-se no repositório de Git local para instantâneos do histórico de execução. Mais tarde, pode associar um repositório de Git de serviços da equipa este projeto de Machine Learning existente usando o seguinte comando:

```azurecli
# Ensure that you are in the project path so Azure CLI has the context of your current project.
$ az ml project update --repo https://<Team Services account name>.visualstudio.com/_git/<project name>
```

> [!NOTE] 
> Pode efetuar a operação de repositório de atualização apenas num projeto Machine Learning que não tem um repositório de Git associado à mesma. Além disso, depois de associar um repositório de Git um Machine Learning, não é possível removê-lo.

## <a name="step-4-capture-a-project-snapshot-in-the-git-repo"></a>Passo 4. Capturar um instantâneo do projeto no repositório de Git
Executar alguns script é executado no projeto e efetuar algumas alterações entre o é executado. Para fazer isto na aplicação do ambiente de trabalho ou da CLI do Azure utilizando o `az ml experiment submit` comando. Para obter mais informações, consulte o [tutorial classificar Iris](tutorial-classifying-iris-part-1.md). Para cada execução, se é efetuada qualquer alteração em qualquer ficheiro na pasta do projeto, um instantâneo da pasta do projeto todo é consolidado e enviado para o repositório de Git remoto sob um ramo denominado AzureMLHistory /\<projeto GUID\>. Para ver os ramos e confirma, incluindo o AzureMLHistory /\<projeto GUID\> ramo, vá para o URL do repositório de Git de serviços da equipa. 

> [!NOTE] 
> O instantâneo é consolidado apenas antes de uma execução de script. Atualmente, uma preparação de dados ou uma execução de célula do bloco de notas da execução não aciona o instantâneo.

![ramo de histórico de execução](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Recomenda-se não funcionar no ramo histórico utilizando comandos de Git. -Pode interferir com o histórico de execução. Em vez disso, utilize o ramo principal ou criar outros ramos para os seus próprios operações de Git.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Passo 5. Restaurar um instantâneo anterior do projeto 
Para restaurar a pasta do projeto todo para o estado de um histórico de execução anterior instantâneo no Machine Learning Workbench:
1. Na atividade de barra (ícone ampulheta), selecione **executa**.
2. No **executar lista** ver, selecione a execução que pretende restaurar.
3. No **executar detalhe** visualizar, selecione **restaurar**.

![ramo de histórico de execução](media/using-git-ml-project/restore_project.png)

Opcionalmente, pode utilizar os seguintes comandos na janela da CLI do Azure no Machine Learning Workbench:

```azurecli
# Discover the run I want to restore a snapshot from.
$ az ml history list -o table

# Restore the snapshot from a specific run.
$ az ml project restore --run-id <run ID>
```

Tenha cuidado quando executar este comando. Executar este comando substitui a pasta do projeto todo com o instantâneo foi tirado quando foi arrancou essa execução específica. O projeto permanece no ramo atual. Isto significa que lhe **perder todas as alterações** na pasta do projeto atual.  

Poderá utilizar o Git para consolidar as alterações para o ramo atual antes de efetuar esta operação.

## <a name="step-6-use-the-master-branch"></a>Passo 6. Utilize o ramo principal
Uma forma para evitar a perda acidental o seu estado atual do projeto é para consolidar o projeto para o ramo principal do repositório de Git (ou para qualquer ramo que criou manualmente). Pode utilizar o Git na linha de comandos ou a partir da ferramenta de cliente de Git favorita a funcionar o ramo principal. Por exemplo:

```sh
# Check status to make sure you are on the master branch (or branch of your choice).
$ git status

# Stage all changes.
$ git add -A

# Commit all changes locally on the master branch.
$ git commit -m 'these are my updates so far'

# Push changes to the remote Team Services Git repo master branch.
$ git push origin master
```

Agora, pode em segurança restaurar o projeto para um instantâneo anterior, concluindo o passo 5. Pode sempre voltar para a consolidação efetuadas apenas no ramo principal.

## <a name="authentication"></a>Autenticação
Se lhe dependem apenas as funções de histórico de execução do Machine Learning para criar instantâneos de projeto e restaurá-las, não precisa de preocupar com a autenticação do repositório de Git. A autenticação é processada pela camada de serviço de experimentação do Machine Learning.

No entanto, se utilizar as suas próprias ferramentas de Git para gerir o controlo de versão, terá de processar a autenticação contra o repositório de Git remoto nos serviços de equipa. No Machine Learning, o repositório de Git remoto é adicionado ao repositório local como um Git remoto utilizando o protocolo HTTPS. Isto significa que quando emitir comandos de Git (por exemplo, o push ou pull) para remoto, tem de fornecer o nome de utilizador e palavra-passe ou um token de acesso pessoal. Para criar um token de acesso pessoal um repositório de Git de serviços de equipa, siga as instruções em [utilizam um token de acesso pessoal para autenticar](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate).

## <a name="next-steps"></a>Passos seguintes
- Saiba como [utilizar o processo de ciência de dados de agrupamento para organizar a estrutura do projeto](how-to-use-tdsp-in-azure-ml.md).
