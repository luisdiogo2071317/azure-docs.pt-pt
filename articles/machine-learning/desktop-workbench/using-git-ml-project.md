---
title: Utilizar um repositório de Git com um projeto do Azure Machine Learning Workbench | Documentos da Microsoft
description: Este artigo explica como utilizar um repositório de Git em conjunto com um projeto do Azure Machine Learning Workbench.
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: 58ab1d77218595344c899dff654ba5b7a5bfb0d8
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296641"
---
# <a name="use-a-git-repo-with-a-machine-learning-workbench-project"></a>Utilizar um repositório de Git com um projeto de Machine Learning Workbench
Saiba como o Azure Machine Learning Workbench usa o Git para fornecer controle de versão e certifique-se a capacidade de reprodução na sua experimentação de ciência de dados. Saiba como associar o seu projeto com uma cloud repositório de Git (repositório).

Machine Learning Workbench destina-se a integração de Git. Quando cria um novo projeto, a pasta do projeto é automaticamente "inicializado pelo Git" como um repositório de Git local. Também é criado um repositório de Git local oculto, segundo, com um ramo com o nome AzureMLHistory /\<GUID do projeto\>. O ramo mantém um registo de alterações de pasta de projeto para cada execução. 

Associar o projeto do Azure Machine Learning com um repositório de Git permite o controle de versão automática, localmente e remotamente. O repositório de Git está alojado no Azure DevOps. Uma vez que o projeto de Machine Learning é associado um repositório do Git, qualquer pessoa que tenha acesso ao repositório remoto pode transferir o código-fonte mais recente para outro computador (roaming).  

> [!NOTE]
> DevOps do Azure tem sua própria lista de controlo de acesso (ACL), que é independente do serviço experimentação do Azure Machine Learning. Acesso de utilizador pode variar entre um repositório do Git e uma área de trabalho do Machine Learning ou projeto. Poderá ter de gerir o acesso. 
> 
> Se quiser fornecer um membro da equipe de nível de código de acesso ao seu projeto de Machine Learning ou partilhar apenas a área de trabalho, terá de conceder ao utilizador as permissões corretas para acessar o repositório de Git de DevOps do Azure. 

Para gerir o controlo de versão com o Git, pode utilizar o ramo principal ou criar outros ramos no repositório. Também pode utilizar o repositório de Git local e enviar por push para o repositório de Git remoto, se for aprovisionada.

Este diagrama ilustra a relação entre um repositório de Git de DevOps do Azure e um projeto de Machine Learning:

![Git do Azure Machine Learning](media/using-git-ml-project/aml_git.png)

Para começar a utilizar um repositório de Git remoto, execute as etapas descritas nas seções a seguir.

> [!NOTE]
> Atualmente, o Azure Machine Learning suporta repositórios Git apenas em organizações de DevOps do Azure.

## <a name="step-1-create-a-machine-learning-experimentation-account"></a>Passo 1. Criar uma conta de experimentação do Machine Learning
Criar uma conta de experimentação do Machine Learning e instalar a aplicação Azure Machine Learning Workbench. Para obter mais informações, consulte [instalar e criar o guia de introdução](../service/quickstart-installation.md).

## <a name="step-2-create-an-azure-devops-project-or-use-an-existing-project"></a>Passo 2. Criar um projeto de DevOps do Azure ou utilizar um projeto existente
Na [portal do Azure](https://portal.azure.com/), crie um novo projeto:
1. Selecione **+**.
2. Procure **projeto da Equipe**.
3. Introduza as informações necessárias:
    - **Nome**: um nome de equipa.
    - **Controle de versão**: selecione **Git**.
    - **Subscrição**: selecione uma subscrição que tem uma conta de experimentação do Machine Learning.
    - **Localização**: escolher o ideal é que uma região que está próximo de seus recursos de experimentação do Machine Learning.
4. Selecione **Criar**. 

![Criar um projeto no portal do Azure](media/using-git-ml-project/create_vsts_team.png)

Certifique-se de que inicie sessão com a mesma conta do Azure Active Directory (Azure AD) que utiliza para aceder a Machine Learning Workbench. Caso contrário, o sistema não é possível aceder a Bancada de trabalho do Machine Learning, utilizando as credenciais do Azure AD. Uma exceção é se usar a linha de comandos para criar o projeto de Machine Learning e fornece um token de acesso pessoal para acessar o repositório de Git. Abordaremos isso em mais detalhes posteriormente neste artigo.

Para ir diretamente para o projeto que criou, utilize o URL https://\<nome do projeto\>. visualstudio.com.

## <a name="step-3-set-up-a-machine-learning-project-and-git-repo"></a>Passo 3. Configurar um projeto de Machine Learning e o repositório de Git

Para configurar um projeto de Machine Learning, tem duas opções:
- Criar um projeto de Machine Learning que tem um repositório de Git remoto
- Associar um projeto existente do Machine Learning com um repositório de Git de DevOps do Azure

### <a name="create-a-machine-learning-project-that-has-a-remote-git-repo"></a>Criar um projeto de Machine Learning que tem um repositório de Git remoto
Abra o Machine Learning Workbench e criar um novo projeto. Na **repositório de Git** , introduza o URL do repositório de Git de DevOps do Azure a partir do passo 2. Normalmente, é semelhante a esta: https://\<nome da organização do Azure DevOps\>.visualstudio.com/_git/\<nome do projeto\>

![Criar um projeto de Machine Learning que tem um repositório de Git](media/using-git-ml-project/create_project_with_git_rep.png)

Também pode criar o projeto com a ferramenta de linha de comandos do Azure (CLI do Azure). Tem a opção de inserir um token de acesso pessoal. Machine Learning pode utilizar este token para acessar o repositório de Git em vez de utilizar as credenciais do Azure AD:

```
# Create a new project that has a Git repo by using a personal access token.
$ az ml project create -a <Experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <Azure DevOps personal access token>
```

> [!IMPORTANT]
> Se escolher o modelo de projeto em branco, o repositório de Git que optar por utilizar já pode ter um ramo principal. Machine Learning simplesmente clona localmente o ramo principal. Ele adiciona a pasta de aml_config e outros ficheiros de metadados de projeto para a pasta do projeto local. 
>
> Se escolher qualquer outro modelo de projeto, seu repositório de Git *não é possível* já tiver um ramo principal. Se isso acontecer, verá um erro. A alternativa é usar o `az ml project create` comando para criar o projeto, com um `--force` mudar. Isso elimina os ficheiros no ramo principal original e substitui-los com os novos ficheiros no modelo que escolher.

É criado um novo projeto de Machine Learning, com a integração de repositório de Git remota ativada. A pasta de projeto é sempre inicializado pelo Git como um repositório de Git local. O Git remoto está definido para o repositório de Git de DevOps do Azure remoto, portanto, o que pode emitir consolidações para o repositório de Git remoto.

### <a name="associate-an-existing-machine-learning-project-with-an-azure-devops-git-repo"></a>Associar um projeto existente do Machine Learning com um repositório de Git de DevOps do Azure
Pode criar um projeto de Machine Learning sem um repositório de Git de DevOps do Azure e contar com o repositório de Git local para instantâneos do histórico de execuções. Mais tarde, pode associar um repositório de Git de DevOps do Azure com este projeto existente do Machine Learning, utilizando o seguinte comando:

```azurecli
# Ensure that you are in the project path so Azure CLI has the context of your current project.
$ az ml project update --repo https://<Azure DevOps organization name>.visualstudio.com/_git/<project name>
```

> [!NOTE] 
> Pode efetuar a operação do repositório de atualização apenas num projeto de Machine Learning que não tem um repositório de Git associado a ele. Além disso, depois de associar um repositório de Git um Machine Learning, não é possível removê-lo.

## <a name="step-4-capture-a-project-snapshot-in-the-git-repo"></a>Passo 4. Capturar um instantâneo do projeto no repositório de Git
Executar algumas execuções do script no projeto e fazer algumas alterações entre as execuções. Pode fazê-a aplicação de ambiente de trabalho ou da CLI do Azure utilizando o `az ml experiment submit` comando. Para obter mais informações, consulte a [tutorial Classifying Iris](tutorial-classifying-iris-part-1.md). Para cada execução, se é efetuada qualquer alteração em qualquer arquivo na pasta de projeto, um instantâneo da pasta de projeto inteiro é consolidado e enviado para o repositório de Git remoto num ramo com o nome AzureMLHistory /\<GUID do projeto\>. Para ver as ramificações e consolidações, incluindo o AzureMLHistory /\<GUID do projeto\> ramo, vá para o URL do repositório de Git de DevOps do Azure. 

> [!NOTE] 
> O instantâneo é dedicado apenas antes de uma execução de script. Atualmente, uma preparação de dados execução ou de uma execução de célula do bloco de notas não aciona o instantâneo.

![Ramo de histórico de execuções](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> É melhor se já não funcionam no ramo histórico com comandos do Git. Ele pode interferir com o histórico de execuções. Em vez disso, utilize o ramo principal ou crie outros ramos das suas próprias operações de Git.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Passo 5. Restaurar um instantâneo anterior do projeto 
Para restaurar a pasta de projeto inteiro para o estado de um histórico de execuções anterior, tire um instantâneo, no Machine Learning Workbench:
1. Na atividade de barra (ícone de ampulheta), selecione **execuções**.
2. Na **lista de execuções** ver, selecione a execução que pretende restaurar.
3. Na **executar detalhes** visualizar, selecione **restaurar**.

![Ramo de histórico de execuções](media/using-git-ml-project/restore_project.png)

Opcionalmente, pode utilizar os seguintes comandos na janela da CLI do Azure Machine Learning Workbench:

```azurecli
# Discover the run I want to restore a snapshot from.
$ az ml history list -o table

# Restore the snapshot from a specific run.
$ az ml project restore --run-id <run ID>
```

Tenha cuidado ao executar este comando. Executar este comando substitui a pasta de projeto inteiro com o instantâneo que foi tomado quando essa execução específica foi iniciada. O projeto se mantém no ramo atual. Isso significa que **perder todas as alterações** na pasta do projeto atual.  

Pode querer utilizar o Git para consolidar as alterações para o current branch, antes de efetuar esta operação.

## <a name="step-6-use-the-master-branch"></a>Passo 6. Utilize o ramo principal
Uma forma de evitar a perda acidental de seu estado atual do projeto é consolidar o projeto para o ramo principal do repositório Git (ou qualquer ramo que criou). Pode utilizar o Git da linha de comando ou a partir de sua ferramenta de cliente favorita do Git para operar no ramo principal. Por exemplo:

```sh
# Check status to make sure you are on the master branch (or branch of your choice).
$ git status

# Stage all changes.
$ git add -A

# Commit all changes locally on the master branch.
$ git commit -m 'these are my updates so far'

# Push changes to the remote Azure DevOps Git repo master branch.
$ git push origin master
```

Agora, pode restaurado com segurança o projeto para um instantâneo anterior ao concluir o passo 5. Pode sempre voltar para a consolidação que acabou de criar no ramo principal.

## <a name="authentication"></a>Autenticação
Se depende apenas as funções de histórico de execuções no Machine Learning para tirar instantâneos do projeto e restaurá-los, não terá de se preocupar com a autenticação de repositório de Git. Autenticação é manipulada pela camada de serviço de experimentação do Machine Learning.

No entanto, se usar suas próprias ferramentas de Git para gerir o controlo de versão, terá de processar a autenticação contra o repositório de Git remoto no Azure DevOps. No Machine Learning, o repositório de Git remoto é adicionado ao repositório local como um Git remoto utilizando o protocolo HTTPS. Isso significa que quando emitir comandos do Git (como push ou pull) para a ligação remota, terá de fornecer seu nome de utilizador e palavra-passe ou um token de acesso pessoal. Para criar um token de acesso pessoal num repositório de Git de DevOps do Azure, siga as instruções em [utilizam um token de acesso pessoal para se autenticar](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

## <a name="next-steps"></a>Passos Seguintes
- Saiba como [utilizar o processo de ciência de dados de equipa para organizar sua estrutura do projeto](how-to-use-tdsp-in-azure-ml.md).
