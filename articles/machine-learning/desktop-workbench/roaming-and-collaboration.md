---
title: Roaming e colaboração no Azure Machine Learning Workbench | Documentos da Microsoft
description: Saiba como configurar a mobilidade e a colaboração no Machine Learning Workbench.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 07a9d46dff17b43d01a6b411292cf240c32476f3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983723"
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming e colaboração no Azure Machine Learning Workbench

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Este artigo descreve como pode utilizar o Azure Machine Learning Workbench para configurar a projetos para roaming entre computadores e colaborar com os membros da equipe. 

Quando cria um projeto do Azure Machine Learning com uma ligação de repositório (repositório) do Git remota, os metadados de projeto e os instantâneos são armazenados na cloud. Pode utilizar a ligação de cloud para acessar o projeto num computador diferente (roaming). Também pode colaborar com os membros da Equipe, conferindo-lhes acesso ao projeto. 

## <a name="prerequisites"></a>Pré-requisitos
1. Instale o Machine Learning aplicação Workbench. Certifique-se de que tem acesso a uma conta de experimentação do Azure Machine Learning. Para obter mais informações, consulte a [guia de instalação](quickstart-installation.md).

2. Acesso [do Azure DevOps](https://www.visualstudio.com) e, em seguida, criar um repositório para ligar o seu projeto para. Para obter mais informações, consulte [através de um repositório de Git com um projeto de Machine Learning Workbench](using-git-ml-project.md).

## <a name="create-a-new-machine-learning-project"></a>Criar um novo projeto de Machine Learning
Abra o Machine Learning Workbench e, em seguida, crie um novo projeto (por exemplo, um projeto chamado íris). Na **URL do repositório GIT Visualstudio.com** , introduza um URL válido para um repositório de Git de DevOps do Azure. 

> [!IMPORTANT]
> Se escolher o modelo de projeto em branco, o repositório de Git que optar por utilizar já pode ter um ramo principal. Machine Learning simplesmente clona localmente o ramo principal. Ele adiciona a pasta de aml_config e outros ficheiros de metadados de projeto para a pasta do projeto local. 
>
> Se escolher qualquer outro modelo de projeto, seu repositório de Git *não é possível* já tiver um ramo principal. Se isso acontecer, verá um erro. A alternativa é usar o `az ml project create` comando para criar o projeto, com um `--force` mudar. Isso elimina os ficheiros no ramo principal original e substitui-los com os novos ficheiros no modelo que escolher.

Depois de criado o projeto, submeta algumas execuções em quaisquer scripts que estão no projeto. Esta ação compromete-se o estado de projeto para o ramo de histórico de execuções do repositório de Git remoto. 

> [!NOTE] 
> Apenas o script é executado consolidações de Acionador para o ramo do histórico de execuções. Execução e o bloco de notas de preparação de dados é executado não acionar instantâneos de projetos no ramo de histórico de execuções.

Se tiver definido a autenticação do Git, também pode operar no ramo principal. Em alternativa, pode criar um novo ramo. 

Exemplo: 
```
# Check current repo status.
$ git status

# Stage all changes in the current repo.
$ git add -A

# Commit changes.
$ git commit -m "my commit fixes this weird bug!"

# Push to the remote repo.
$ git push origin master
```

## <a name="roaming"></a>Roaming
<a name="roaming"></a>

### <a name="open-machine-learning-workbench-on-a-second-computer"></a>Abra o Machine Learning Workbench num segundo computador
Depois do repositório de Git de DevOps do Azure é que está associado ao seu projeto, pode acessar o projeto íris em qualquer computador que tenha o Machine Learning Workbench instalado. 

Para acessar o projeto íris noutro computador, tem de iniciar sessão na aplicação utilizando as mesmas credenciais que utilizou para criar o projeto. Também tem de estar na mesma conta de experimentação do Machine Learning e área de trabalho. O projeto íris por ordem alfabética está listado com outros projetos na área de trabalho. 

### <a name="download-the-project-on-a-second-computer"></a>Transfira o projeto num segundo computador
Quando a área de trabalho é aberta no segundo computador, o ícone adjacente ao projeto íris é diferente do ícone de pasta típico. O ícone de download indica que o conteúdo do projeto está na cloud e, se o projeto está pronto para ser transferido para o computador atual. 

![Criar o projeto](./media/roaming-and-collaboration/downloadable-project.png)

Selecione o projeto íris para iniciar um download. Quando o download for concluído, o projeto está pronto para ser acedido no segundo computador. 

No Windows, o projeto está localizado em C:\Users\\< nome de utilizador\>\Documents\AzureML.

No macOS, o projeto está localizado em /home/\<nome de utilizador \> /documentos/AzureML.

Numa versão futura, planeamos melhorar a funcionalidade para que possa selecionar uma pasta de destino. 

> [!NOTE]
> Se tiver uma pasta no diretório de Machine Learning, que tem o mesmo nome exatamente como o projeto, o download falhar. Para contornar este problema, temporariamente mudar o nome de pasta existente.


### <a name="work-on-the-downloaded-project"></a>Trabalhar no projeto transferido 
O projeto recém-baixado reflete o estado de projeto na última execução no projeto. Um instantâneo do Estado do projeto é automaticamente consolidado no ramo de histórico de execuções no repositório de Git de DevOps do Azure, sempre que submete uma execução. O instantâneo que está associado a execução mais recente é utilizado para criar uma instância do projeto no segundo computador. 
 

## <a name="collaboration"></a>Colaboração
Pode colaborar com os membros da Equipe em projetos que estão ligados a um repositório de Git de DevOps do Azure. Pode atribuir permissões aos utilizadores para a conta de experimentação do Machine Learning, a área de trabalho e o projeto. Atualmente, pode executar comandos do Azure Resource Manager com CLI do Azure. Também pode utilizar o [portal do Azure](https://portal.azure.com). Para obter mais informações, consulte [utilizar o portal do Azure para adicionar utilizadores](#portal).    

### <a name="use-the-command-line-to-add-users"></a>Utilize a linha de comando para adicionar utilizadores
Por exemplo, Alice é o proprietário do projeto íris. Alice quer partilhar o acesso ao projeto com Bob. 

Alice seleciona os **arquivo** menu e, em seguida, seleciona a **linha de comandos** item de menu. A janela de linha de comandos abre-se com o projeto íris. Alice, em seguida, pode decidir o nível de acesso que pretende dar ao Bob. Ela concede permissões ao executar os seguintes comandos:  

```azurecli
# Find the Resource Manager ID of the Experimentation account.
az ml account experimentation show --query "id"

# Add Bob to the Experimentation account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <Experimentation account Resource Manager ID>

# Find the Resource Manager ID of the workspace.
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the workspace by inheritance. Bob can invite or remove other users.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace Resource Manager ID>
```

Após a atribuição de função, diretamente ou por herança, Bob poderá ver o projeto na lista de projeto de Machine Learning Workbench. BOB poderá ter de reiniciar a aplicação para ver o projeto. BOB, em seguida, pode transferir o projeto, conforme descrito em [Roaming](#roaming)e começar a colaborar com Alice. 

O histórico de execuções para todos os utilizadores colaboram num projeto tem o compromisso do mesmo repositório de Git remoto. Quando Alice é executado uma execução, Bob poderá ver a execução na secção de histórico de execução do projeto na aplicação Machine Learning Workbench. BOB também pode restaurar o projeto para o estado de qualquer execução, incluindo as execuções que iniciou a Alice. 

Ao partilhar um repositório de Git remoto para o projeto, Alice e Bob pode colaborar também no ramo principal. Se for necessário, também pode criar ramos pessoais e utilizar o Git os pedidos pull e mesclagens para colaborar. 

### <a name="use-the-azure-portal-to-add-users"></a>Utilizar o portal do Azure para adicionar utilizadores
<a name="portal"></a>

Contas de experimentação do Aprendizado de máquina, áreas de trabalho e projetos são recursos do Azure Resource Manager. Para atribuir funções, pode utilizar o **controlo de acesso** ligação na [portal do Azure](https://portal.azure.com). 

Localizar o recurso que pretende adicionar os utilizadores com o **todos os recursos** vista. Selecione o **controlo de acesso (IAM)** associar e, em seguida, selecione **adicionar utilizadores**. 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Fluxo de trabalho de colaboração de exemplo
Para ilustrar o fluxo de trabalho de colaboração, vamos examinar um exemplo. Os funcionários do contoso Alice e Bob pretendem colaborar num projeto de ciência de dados ao utilizar o Machine Learning Workbench. As suas identidades de pertencer ao mesmo inquilino Contoso Azure Active Directory (Azure AD). Eis os passos de Alice e Bob tirar:

1. Alice cria um repositório de Git vazio num projeto de DevOps do Azure. O projeto de DevOps do Azure deve ser uma subscrição do Azure que é criado no inquilino Contoso do Azure AD. 

2. Alice cria uma conta de experimentação do Machine Learning, uma área de trabalho e um projeto de Machine Learning Workbench no seu computador. Quando cria o projeto, ela introduza o URL do repositório de Git de DevOps do Azure.

3. Alice começa a trabalhar no projeto. Ele cria alguns scripts e executa algumas execuções. Para cada execução, um instantâneo da pasta de projeto inteiro é automaticamente enviada por push como uma consolidação para um ramo de histórico de execuções do repositório de Git de DevOps do Azure Machine Learning Workbench cria.

4. Alice está satisfeita com o trabalho em andamento. Ela quer confirmar suas alterações no ramo principal local e, em seguida, enviá-las para o ramo principal do repositório Git de DevOps do Azure. Com a abertura de projeto, no Machine Learning Workbench, ela abre a janela da linha de comando e, em seguida, insere estes comandos:
    
    ```sh
    # Verify that the Git remote is pointing to the Azure DevOps Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Stage all changes.
    $ git add -A

    # Commit changes with a comment.
    $ git commit -m "this is a good milestone"

    # Push the commit to the master branch of the remote Git repo in Azure DevOps.
    $ git push
    ```

5. Alice adiciona Bob à área de trabalho como um contribuinte. Ela pode fazê-lo no portal do Azure ou utilizando o `az role assignment` de comando, como demonstrado anteriormente. Alice também concede permissões para o repositório de Git de DevOps do Azure de leitura/escrita Bob.

6. João inicia sessão no Machine Learning Workbench no seu computador. Ele pode ver a área de trabalho que Alice partilhou com ele. Ele pode ver o projeto de íris listado na área de trabalho. 

7. João seleciona o nome do projeto. O projeto é transferido para o seu computador.
    * Os arquivos do projeto transferido são uma cópia do instantâneo da execução mais recente que é registada no histórico de execuções. Não são a última confirmação no ramo principal.
    * A pasta do projeto local está definida para o ramo principal, com as alterações não preparadas.

8. BOB pode procurar execuções que foram executadas por Alice. Ele pode restaurar instantâneos de todas as execuções anteriores.

9. BOB quer obter as alterações mais recentes que Alice enviada e, em seguida, começar a trabalhar num ramo diferente. No Machine Learning Workbench, Bob abre uma janela de linha de comandos e executa os seguintes comandos:

    ```sh
    # Verify that the Git remote is pointing to the Azure DevOps Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Get the latest commit in the Azure DevOps Git master branch and overwrite current files.
    $ git pull --force

    # Create a new local branch named "bob" so that Bob's work is done in the "bob" branch
    $ git checkout -b bob
    ```

10. BOB modifica o projeto e envia novas execuções. As alterações são feitas no ramo de bob. Execuções de Bob também ficam visíveis para Alice.

11. BOB está pronto para enviar por push as alterações ao repositório de Git remoto. Para evitar conflitos com o ramo principal, onde Alice está a funcionar, Bob envia o seu trabalho para um novo ramo remoto, também denominada bob.

    ```sh
    # Verify that the current branch is "bob," and that it has unstaged changes.
    $ git status
    
    # Stage all changes.
    $ git add -A

    # Commit the changes with a comment.
    $ git commit -m "I found a cool new trick."

    # Create a new branch on the remote Azure DevOps Git repo, and then push the changes.
    $ git push origin bob
    ```

12. Dizer Alice sobre o truque esporádico de novos no seu código, o João cria um pedido pull no repositório de Git remoto do ramo de bob para o ramo principal. Alice pode, em seguida, intercalar o pedido pull no ramo principal.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [através de um repositório de Git com um projeto de Machine Learning Workbench](using-git-ml-project.md).
