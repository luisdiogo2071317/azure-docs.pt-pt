---
title: "Roaming e de colaboração no Azure Machine Learning Workbench | Microsoft Docs"
description: "Saiba como configurar roaming e de colaboração no Machine Learning Workbench."
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 137608007716452ec6468f1e13f494b095a11cb0
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming e de colaboração no Azure Machine Learning Workbench
Este artigo descreve como pode utilizar o Azure Machine Learning Workbench configurar projetos para a Itinerância entre computadores e colaborar com os membros da equipa. 

Quando cria um projeto do Azure Machine Learning que tenha uma ligação de (repositório) do repositório de Git remota, os metadados de projeto e instantâneos são armazenados na nuvem. Pode utilizar a ligação da nuvem para o projeto de acesso a partir de outro computador (roaming). Também podem colaborar com os membros da equipa, conferindo-lhes acesso ao projeto. 

## <a name="prerequisites"></a>Pré-requisitos
1. Instale o Machine Learning aplicação Workbench. Certifique-se de que tem acesso a uma conta do Azure Machine Learning experimentação. Para obter mais informações, consulte o [guia de instalação](quickstart-installation.md).

2. Acesso [Visual Studio Team Services](https://www.visualstudio.com) (Team Services) e, em seguida, crie um repositório para ligar o seu projeto para. Para obter mais informações, consulte [através de um repositório de Git com um projeto de Machine Learning Workbench](using-git-ml-project.md).

## <a name="create-a-new-machine-learning-project"></a>Criar um novo projeto de Machine Learning
Abra o Workbench do Machine Learning e, em seguida, crie um novo projeto (por exemplo, um projeto com o nome iris). No **URL do repositório do GIT Visualstudio.com** caixa, introduza um URL válido para um repositório de Git de serviços da equipa. 

> [!IMPORTANT]
> Se escolher o modelo de projeto em branco, o repositório de Git que optar por utilizar poderão já ter um ramo principal. Aprendizagem simplesmente clones localmente o ramo principal. Adiciona a pasta de aml_config e outros ficheiros de metadados de projeto para a pasta do projeto local. 
>
> Se escolher qualquer outro modelo de projeto, o repositório de Git *não é possível* já tiver um ramo principal. Se existir, verá um erro. A alternativa consiste em utilizar o `az ml project create` comando para criar o projeto, com um `--force` mudar. Isto elimina os ficheiros no ramo principal original e substitui-los com os novos ficheiros no modelo que escolher.

Depois do projeto é criado, submeta alguns é executado em quaisquer scripts que estão no projeto. Esta ação consolida o estado de projeto para a qual ramificar de histórico de execução do repositório de Git remoto. 

> [!NOTE] 
> Apenas o script é executado acionador consolidações para o ramo do histórico de execução. Preparação de dados de execução e o bloco de notas é executado não acionar instantâneos do projeto no ramo histórico de execução.

Se configurou a autenticação de Git, também pode operar no ramo principal. Em alternativa, pode criar um novo ramo. 

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

## <a name="roaming"></a>Está em roaming
<a name="roaming"></a>

### <a name="open-machine-learning-workbench-on-a-second-computer"></a>Abra o Machine Learning Workbench num segundo computador
Depois do repositório de Git de serviços da equipa está ligado com o seu projeto, pode aceder no projeto iris partir de qualquer computador que tenha o Machine Learning Workbench instalado. 

Para aceder ao projeto iris noutro computador, tem de iniciar sessão aplicação, utilizando as mesmas credenciais que utilizou para criar o projeto. Terá também de ser a mesma conta de experimentação do Machine Learning e a área de trabalho. O projeto de iris é listado por ordem alfabética com outros projetos na área de trabalho. 

### <a name="download-the-project-on-a-second-computer"></a>Transferir o projeto num segundo computador
Quando a área de trabalho está aberta no segundo computador, o ícone adjacente ao projeto iris é diferente no ícone de pasta típica. O ícone de transferência indica que o conteúdo do projeto é na nuvem e de que o projeto está pronto para ser transferidos para o computador atual. 

![Criar o projeto](./media/roaming-and-collaboration/downloadable-project.png)

Selecione o projeto de iris para iniciar uma transferência. Quando a transferência estiver concluída, o projeto está pronto para ser acedidas num segundo computador. 

No Windows, o projeto está localizado em C:\Users\\< nome de utilizador\>\Documents\AzureML.

No macOS, o projeto está localizado em /home/\<username\>documentos/AzureML.

Numa versão futura, planeamos de melhorar a funcionalidade para que possa selecionar uma pasta de destino. 

> [!NOTE]
> Se tiver uma pasta no diretório do Machine Learning que tenha o mesmo nome exato como o projeto, a transferência falhará. Para contornar este problema, temporariamente mudar o nome de pasta existente.


### <a name="work-on-the-downloaded-project"></a>Funcionam em projeto transferido 
O projeto recentemente transferido reflete o estado de projeto na última execução no projeto. Um instantâneo do Estado de projeto é automaticamente consolidado para o ramo do histórico de execução no repositório de Git de serviços da equipa sempre que submete uma execução. O instantâneo que está associado a mais recente run é utilizado para instanciar o projeto no segundo computador. 
 

## <a name="collaboration"></a>Colaboração
Podem colaborar com os membros da equipa nos projetos que estejam ligados a um repositório de Git de serviços da equipa. Pode atribuir permissões a utilizadores para a conta de experimentação do Machine Learning, a área de trabalho e o projeto. Atualmente, pode executar comandos do Azure Resource Manager utilizando a CLI do Azure. Também pode utilizar o [portal do Azure](https://portal.azure.com). Para obter mais informações, consulte [utilizar o portal do Azure para adicionar utilizadores](#portal).    

### <a name="use-the-command-line-to-add-users"></a>Utilize a linha de comandos para adicionar utilizadores
Por exemplo, a Alice é o proprietário do projeto iris. Alice pretende partilhar acesso ao projeto com Bernardo. 

Alice seleciona o **ficheiro** menu e, em seguida, seleciona o **linha de comandos** item de menu. Abre a janela de linha de comandos com o projeto de iris. Alice, em seguida, pode decidir que nível de acesso que pretende dar ao João. A Joana concede permissões executando os seguintes comandos:  

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

Após a atribuição de função, diretamente ou através de herança, João pode ver o projeto na lista de projeto de Machine Learning Workbench. Bernardo poderá ter de reiniciar a aplicação para ver o projeto. Bernardo, em seguida, pode transferir o projeto conforme descrito em [Itinerância](#roaming)e começar a colaborar com a Alice. 

O histórico de execução de todos os utilizadores colaborem num projeto é submetido para o mesmo repositório de Git remoto. Quando Alice executa uma execução, João pode ver a executar na secção de histórico de execução do projeto na aplicação do Workbench do Machine Learning. João também pode restaurar o projeto para o estado de qualquer executar, incluindo executa Alice foi iniciada. 

Ao partilhar um repositório de Git remoto para o projeto, os Alice e Bernardo também pode colaborar no ramo principal. Se for necessário, também pode criar ramos pessoais e utilizar os pedidos de solicitação do Git e intercala colaborar. 

### <a name="use-the-azure-portal-to-add-users"></a>Utilizar o portal do Azure para adicionar utilizadores
<a name="portal"></a>

Contas do Machine Learning experimentação, áreas de trabalho e projetos são recursos do Azure Resource Manager. Para atribuir funções, pode utilizar o **controlo de acesso** ligação no [portal do Azure](https://portal.azure.com). 

Localizar o recurso que pretende adicionar os utilizadores utilizando o **todos os recursos** vista. Selecione o **(IAM) do controlo de acesso** associar e, em seguida, selecione **adicionar utilizadores**. 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Fluxo de trabalho de colaboração de exemplo
Para ilustrar o fluxo de trabalho de colaboração, vamos guiá-lo através de um exemplo. Os funcionários do contoso Alice e Bernardo pretendem colaborar num projeto de ciência de dados através do Workbench do Machine Learning. As respetivas identidades pertencerem ao mesmo inquilino Contoso Azure Active Directory (Azure AD). Eis os passos de Alice e Bernardo demorar:

1. Alice cria um repositório de Git vazio de um projeto de equipa de serviços. O projeto de equipa serviços deve ter uma subscrição do Azure que é criada no inquilino Contoso do Azure AD. 

2. Alice cria uma conta de experimentação do Machine Learning, uma área de trabalho e um projeto do Workbench do Machine Learning no respetivo computador. Quando cria o projeto, ela introduza o URL do repositório de Git de serviços da equipa.

3. Alice começa a trabalhar no projeto. A Joana cria alguns scripts e executa alguns é executado. Para cada execução, um instantâneo da pasta do projeto todo é automaticamente instalado como uma confirmação um ramo de histórico de execução do repositório de Git de serviços da equipa que cria o Workbench do Machine Learning.

4. Alice é satisfeita com o trabalho em curso. Que pretende consolidar alterações ao respetiva o ramo principal local e, em seguida, emiti-las para o ramo principal do repositório de Git de serviços da equipa. Com a abertura de projeto, no Machine Learning Workbench, ela é aberta a janela de linha de comandos e, em seguida, introduz estes comandos:
    
    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Stage all changes.
    $ git add -A

    # Commit changes with a comment.
    $ git commit -m "this is a good milestone"

    # Push the commit to the master branch of the remote Git repo in Team Services.
    $ git push
    ```

5. Alice adiciona Bernardo à área de trabalho como um contribuinte. Ela pode fazer no portal do Azure, ou utilizando o `az role assignment` comando, como é demonstrado anteriormente. Alice também concede permissões para o repositório de Git de serviços da equipa de leitura/escrita Bernardo.

6. João inicia Machine Learning Workbench num computador dele. Ele pode ver a área de trabalho Alice partilhada com ele. Ele pode ver o projeto de iris listado na área de trabalho. 

7. João seleciona o nome do projeto. O projeto é transferido para o computador.
    * Os ficheiros do projeto transferido são uma cópia do instantâneo a execução mais recente que é registada no histórico de execução. Não estão a consolidação último no ramo principal.
    * A pasta local do projeto está definida para o ramo principal, com as alterações unstaged.

8. João pode procurar executa que foram executadas por Alice. Ele pode restaurar os instantâneos de qualquer executa anterior.

9. João pretende obter as alterações mais recentes que feito o Push de Alice e, em seguida, começar a trabalhar um ramo diferentes. No Machine Learning Workbench, João abre uma janela de linha de comandos e executa os comandos seguintes:

    ```sh
    # Verify that the Git remote is pointing to the Team Services Git repo.
    $ git remote -v

    # Verify that the current branch is master.
    $ git branch

    # Get the latest commit in the Team Services Git master branch and overwrite current files.
    $ git pull --force

    # Create a new local branch named "bob" so that Bob's work is done in the "bob" branch
    $ git checkout -b bob
    ```

10. Bernardo modifica o projeto e submete execuções de novo. As alterações são efetuadas no ramo Bernardo. Também executa do de Bob ficam visíveis para Alice.

11. João está pronto para emitir as alterações para o repositório de Git remoto. Para evitar conflitos com o ramo principal, onde está a funcionar Alice, Bernardo pushes o trabalho para um novo ramo remoto, que também é denominado Bernardo.

    ```sh
    # Verify that the current branch is "bob," and that it has unstaged changes.
    $ git status
    
    # Stage all changes.
    $ git add -A

    # Commit the changes with a comment.
    $ git commit -m "I found a cool new trick."

    # Create a new branch on the remote Team Services Git repo, and then push the changes.
    $ git push origin bob
    ```

12. Para dizer Alice sobre o truque novo útil no seu código, o João cria um pedido pull no repositório de Git remoto do ramo Bernardo para o ramo principal. Alice, em seguida, pode intercalar o pedido de solicitação para o ramo principal.

## <a name="next-steps"></a>Passos seguintes
- Saiba mais sobre [através de um repositório de Git com um projeto de Machine Learning Workbench](using-git-ml-project.md).
