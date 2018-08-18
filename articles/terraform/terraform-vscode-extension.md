---
title: Extensão do Terraform do Azure VS Code | Documentos da Microsoft
description: Neste artigo, saiba como instalar e utilizar a extensão do Terraform no Visual Studio Code.
keywords: terraform, devops, máquina virtual, azure
author: v-mavick
ms.author: v-mavick
ms.date: 08/14/2018
ms.topic: article
ms.prod: vs-code
ms.openlocfilehash: 0c88879faae100372055479ad4edb8c36d8f557d
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190359"
---
# <a name="azure-terraform-vs-code-extension"></a>Extensão do VS Code do Terraform do Azure

A extensão do Microsoft Azure Terraform Visual Studio Code (código de VS) foi concebida para aumentar a produtividade do desenvolvedor, criação, teste e com o Terraform e o Azure. A extensão fornece suporte de comando do Terraform, visualização de gráfico de recursos e integração de CloudShell do VS Code.

## <a name="what-you-do"></a>O que fazer

- Instale o executável do Terraform da HashiCorp de código-fonte aberto no seu computador.
- Instale a extensão de código de VS do Terraform do Azure na sua instalação local do VS Code.

## <a name="what-you-learn"></a>O que irá aprender

Neste tutorial, vai aprender:

- Como o Terraform pode automatizar e simplificar o aprovisionamento de serviços do Azure.
- Como instalar e utilizar a extensão do Microsoft Terraform VS Code para serviços do Azure.
- Como utilizar o VS Code para escrever, planejar e executar o Terraform planos.

## <a name="what-you-need"></a>Do que precisa

- Um computador com Windows 10, Linux ou macOS 10.10 +.
- [Visual Studio Code](https://www.bing.com/search?q=visual+studio+code+download&form=EDGSPH&mkt=en-us&httpsmsn=1&refig=dffc817cbc4f4cb4b132a8e702cc19a3&sp=3&ghc=1&qs=LS&pq=visual+studio+code&sk=LS1&sc=8-18&cvid=dffc817cbc4f4cb4b132a8e702cc19a3&cc=US&setlang=en-US).
- Uma subscrição ativa do Azure. [Ativar uma conta gratuita de 30 dias avaliação Microsoft Azure](https://azure.microsoft.com/free/).
- Uma instalação do [Terraform](https://www.terraform.io/) ferramenta de código-fonte aberto no seu computador local.
  
## <a name="prepare-your-dev-environment"></a>Preparar o ambiente de desenvolvimento

### <a name="install-git"></a>Instalar o Git

Para concluir os exercícios no artigo, precisa [instalar o Git](https://git-scm.com/).

### <a name="install-hashicorp-terraform"></a>Instalar o Terraform da HashiCorp

Siga as instruções da HashiCorp [Terraform instalar](https://www.terraform.io/intro/getting-started/install.html) página da Web, que abrange:

- Baixar o Terraform
- Instalar o Terraform
- Verificar o Terraform está corretamente instalado

>[!Tip]
>Certifique-se de que siga as instruções relativas ao definir a variável de sistema do caminho.

### <a name="install-nodejs"></a>Instalar o Node.js

Para utilizar o Terraform no Cloud Shell, precisa [instalar node. js](https://nodejs.org/) 6.0 +.

>[!NOTE]
>Para verificar se o node. js está instalado, abra uma janela de terminal e introduza: `node -v`

### <a name="install-graphviz"></a>Instalar GraphViz

Para utilizar o Terraform visualizar a função, precisa [instalar GraphViz](http://graphviz.org/).

>[!NOTE]
>Para verificar se GraphViz é instalado, abra uma janela de terminal e introduza: `dot -V`

### <a name="install-the-azure-terraform-vs-code-extension"></a>Instale a extensão do VS Code Terraform do Azure:

1. Inicie o VS Code.
2. Clique nas **extensões** ícone.

    ![Botão de extensões](media/terraform-vscode-extension/tf-vscode-extensions-button.png)

3. Utilize o **extensões de pesquisa no Marketplace** caixa de texto para procurar a extensão do Terraform do Azure:

    ![Extensões do VS Code de pesquisa no Marketplace](media/terraform-vscode-extension/tf-search-extensions.png)

4. Clique em **Instalar**.

    >[!NOTE]
    >Quando clica em **instalar** relativamente à extensão do Terraform do Azure, VS Code instalará automaticamente a extensão da conta do Azure. Conta do Azure é um ficheiro de dependência para a extensão do Azure do Terraform, o que ele usa para executar autenticações de subscrição do Azure e extensões de código relacionados com o Azure.

#### <a name="verify-the-terraform-extension-is-installed-in-visual-studio-code"></a>Verifique se que a extensão do Terraform está instalada no Visual Studio Code

1. Clique no ícone de extensões.
2. Tipo de `@installed` na caixa de texto de pesquisa.

    ![Extensões instaladas](media/terraform-vscode-extension/tf-installed-extensions.png)

A extensão do Terraform do Azure irá aparecer na lista de extensões instaladas.

![Extensões instaladas do Terraform](media/terraform-vscode-extension/tf-installed-terraform-extension-button.png)

Agora, pode executar todos os comandos suportados do Terraform no seu ambiente de Cloud Shell a partir do VS Code.

## <a name="exercise-1-basic-terraform-commands-walk-through"></a>Exercício 1: Noções básicas de comandos do Terraform básica

Neste exercício, pode criar e executar um ficheiro de configuração básico do Terraform que aprovisiona um novo grupo de recursos do Azure.

### <a name="prepare-a-test-plan-file"></a>Preparar um arquivo de plano de teste

1. No VS Code, selecione **ficheiro > novo ficheiro** na barra de menus.
2. Navegue para o [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html#) e copie o código na **exemplo de utilização** bloco de código:
3. Cole o código copiado para o novo ficheiro criado no VS Code.

    ![Cole o código de exemplo de utilização](media/terraform-vscode-extension/tf-paste-example-usage-code.png)

    >[!NOTE]
    >Pode alterar o **nome** valor do grupo de recursos, mas tem de ser exclusivo para a sua subscrição do Azure.

4. Na barra de menus, selecione **ficheiro > Guardar como**.
5. Na **guardar como** caixa de diálogo, navegue para um local de sua preferência e, em seguida, clique em **nova pasta**. (Alterar o nome da pasta nova para algo mais descritiva do que *nova pasta*.)

    >[!NOTE]
    >Neste exemplo, a pasta com o nome o plano de teste TERRAFORM.

6. Certifique-se de que a sua nova pasta é realçada (selecionado) e, em seguida, clique em **aberto**.
7. Na **guardar como** caixa de diálogo, altere o nome predefinido do ficheiro a *main.tf*.

    ![Guardar como main.tf](media/terraform-vscode-extension/tf-save-as-main.png)

8. Clique em **Guardar**.
- Na barra de menus, selecione **ficheiro > Abrir pasta**. Navegue até e selecione a nova pasta que criou.

### <a name="run-terraform-init-command"></a>Execute o Terraform *init* comando

1. Inicie o Visual Studio Code.
2. Na barra de Menu do código de VS, selecione **ficheiro > Abrir pasta...**  e localize e selecione sua *main.tf* ficheiro.

    ![ficheiro de main.TF](media/terraform-vscode-extension/tf-main-tf.png)

3. Na barra de menus, selecione **vista > paleta de comandos... > Azure Terraform: Init**.
4. Após alguns instantes, quando lhe for perguntado *que pretende abrir o Cloud Shell?* Clique em **OK**.

    ![Pretende abrir o Cloud Shell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloud-shell.png)

5. Na primeira vez que iniciar o Cloud Shell a partir de uma nova pasta, será solicitado para configurar a aplicação web. Clique em **aberto**.

    ![Primeira inicialização do Cloud Shell](media/terraform-vscode-extension/tf-first-launch-of-cloud-shell.png)

6. O bem-vindo à página do Azure Cloud Shell é aberto. Selecione o Bash ou o PowerShell.

    ![Bem-vindo ao Azure Cloud Shell](media/terraform-vscode-extension/tf-welcome-to-azure-cloud-shell.png)

    >[!NOTE]
    >Neste exemplo, Bash (Linux) foi selecionado.

7. Se já não configurado uma conta de armazenamento do Azure, é apresentado o ecrã seguinte. Clique em **criar armazenamento**.

    ![Não tem armazenamento montado](media/terraform-vscode-extension/tf-you-have-no-storage-mounted.png)

1. O Azure Cloud Shell é iniciado no shell selecionada e apresenta informações para a unidade de cloud que acabou de criar para anteriormente.

    ![Tiver sido criada a sua unidade de cloud](media/terraform-vscode-extension/tf-your-cloud-drive-has-been-created-in.png)

9. Agora pode sair do Cloud Shell
10. Na barra de menus, selecione **View** > **paleta de comandos** > **Azure Terraform: init**.

    ![Terraform foi inicializado com êxito](media/terraform-vscode-extension/tf-terraform-has-been-successfully-initialized.png)

### <a name="visualize-the-plan"></a>Visualizar o plano

Anteriormente neste tutorial, instalou GraphViz. Terraform pode utilizar GraphViz para gerar uma representação visual de plano de uma configuração ou a execução. A extensão do VS Code Terraform do Azure implementa esta funcionalidade através da *visualizar* comando.

- Do **barra de menus**, selecione**vista > paleta de comandos > Azure Terraform: Visualize**.

    ![Visualizar o plano](media/terraform-vscode-extension/tf-graph.png)

### <a name="run-terraform-plan-command"></a>Execute o Terraform *plano* comando

O Terraform *plano* comando é usado para verificar se o plano de execução para um conjunto de alterações, fazer o que pretendia.

>[!NOTE]
>Terraform *plano* não faça quaisquer alterações aos recursos do real Azure. Na verdade, fazer as alterações contidas no seu plano, usamos o Terraform *aplicar* comando.

- Na barra de menus, selecione **View** > **paleta de comandos** > **Azure Terraform: plano**.

    ![Plano do Terraform](media/terraform-vscode-extension/tf-terraform-plan.png)

### <a name="run-terraform-apply-command"></a>Execute o Terraform *aplicar* comando

Depois de a ser satisfeita com os resultados do Terraform *plano*, pode executar o *aplicar* comando.

1. Na barra de menus, selecione **View** > **paleta de comandos** > **Azure Terraform: aplicar**.

    ![Aplicam-se do Terraform](media/terraform-vscode-extension/tf-terraform-apply.png)

2. Tipo *Sim*.

    ![Terraform aplicar Sim](media/terraform-vscode-extension/tf-terraform-apply-yes.png)

### <a name="verify-your-terraform-plan-was-executed"></a>Certifique-se de que o plano de Terraform foi executado

Para ver se o seu novo grupo de recursos do Azure foi criado com êxito:

1. Abra o portal do Azure.
2. Selecione **grupos de recursos** no painel de navegação esquerdo.

    ![Verifique se o seu novo recurso](media/terraform-vscode-extension/tf-verify-resource-group-created.png)

Seu novo grupo de recursos deve ser listado no **nome** coluna.

>[!NOTE]
>Pode deixar a janela do Portal do Azure abertos por enquanto; usaremos-la no próximo passo.

### <a name="run-terraform-destroy-command"></a>Execute o Terraform *destruir* comando

1. Na barra de menus, selecione **View** > **paleta de comandos** > **Azure Terraform: destruir**.

    ![Destruir Terraform](media/terraform-vscode-extension/tf-terraform-destroy.png)

2. Tipo *Sim*.

    ![Terraform destruir Sim](media/terraform-vscode-extension/tf-terraform-destroy-yes.png)

### <a name="verify-your-resource-group-was-destroyed"></a>Certifique-se de que o grupo de recursos foi destruído

Para confirmar que o Terraform destruída com êxito o novo grupo de recursos:

1. Clique em **Atualize** no portal do Azure *grupos de recursos* página.
2. O grupo de recursos já não será listado.

    ![Verifique se o grupo de recursos foi destruído](media/terraform-vscode-extension/tf-refresh-resource-groups-button.png)

## <a name="exercise-2-terraform-compute-module"></a>Exercício 2: Terraform *computação* módulo

Neste exercício, vai aprender a carregar o Terraform *computação* módulo ao ambiente do VS Code.

### <a name="clone-the-terraform-azurerm-compute-module"></a>Clonar o módulo do terraform-azurerm-compute

1. Uso [esta ligação](https://github.com/Azure/terraform-azurerm-compute) para acessar o módulo de computação do Terraform do Azure Rm no GitHub.
2. Clique em **clonar ou transferir**.

    ![Clonar ou transferir](media/terraform-vscode-extension/tf-clone-with-https.png)

    >[!NOTE]
    >Neste exemplo, a nossa pasta foi chamada *terraform-azurerm-compute*.

### <a name="open-the-folder-in-vs-code"></a>Abra a pasta no VS Code

1. Inicie o Visual Studio Code.
2. Partir do **barra de menus**, selecione **ficheiro > Abrir pasta** e navegue até e selecione a pasta que criou no passo anterior.

    ![pasta do terraform-azurerm-compute](media/terraform-vscode-extension/tf-terraform-azurerm-compute-folder.png)

### <a name="initialize-terraform"></a>Inicializar o Terraform

Antes de começar a usar os comandos de Terraform a partir do VS Code, transfira os plug-ins para dois fornecedores do Azure: aleatório e azurerm.

1. No painel de Terminal do IDE de código do VS, escreva: `terraform init`

    ![comando de init terraform](media/terraform-vscode-extension/tf-terraform-init-command.png)

2. Tipo de `az login` e siga na tela instruções.

### <a name="module-test-lint"></a>Teste de módulo: *lint*

1. Do **barra de menus**, selecione **vista > paleta de comandos > Azure Terraform: Executar teste**.
2. Na lista de opções do tipo de teste, selecione **lint**.

    ![Selecione o tipo de teste](media/terraform-vscode-extension/tf-select-type-of-test-lint.png)

3. Quando lhe for perguntado *que pretende abrir CloudShell?* clique em **OK** e siga na tela instruções.

    ![Pretende abrir CloudShell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Quando quer executar o **lint** ou **ponto a ponto** teste, o Azure utiliza um serviço de contentor para Aprovisionar uma máquina de teste para executar o teste real. Por esse motivo, os resultados do teste, normalmente, podem demorar vários minutos a serem retornados.

Após alguns instantes, verá uma lista no painel de Terminal semelhante a este exemplo:

![Resultados do teste lint](media/terraform-vscode-extension/tf-lint-test-results.png)

### <a name="module-test-end-to-end"></a>Teste de módulo: *ponto-a-ponto*

1. Do **barra de menus**, selecione **vista > paleta de comandos > Azure Terraform: Executar teste**.
2. Na lista de opções do tipo de teste, selecione **ponto a ponto**.

    ![Selecione o tipo de teste](media/terraform-vscode-extension/tf-select-type-of-test-end-to-end.png)

3. Caso lhe seja perguntado *que pretende abrir CloudShell?* clique em **OK** e siga na tela instruções.

    ![Pretende abrir CloudShell?](media/terraform-vscode-extension/tf-do-you-want-to-open-cloudshell-small.png)

>[!NOTE]
>Quando quer executar o **lint** ou **ponto a ponto** teste, o Azure utiliza um serviço de contentor para Aprovisionar uma máquina de teste para executar o teste real. Por esse motivo, os resultados do teste, normalmente, podem demorar vários minutos a serem retornados.

Após alguns instantes, verá uma lista no painel de Terminal semelhante a este exemplo:

![Resultados do teste de ponto a ponto](media/terraform-vscode-extension/tf-end-to-end-test-results.png)

## <a name="next-steps"></a>Passos Seguintes

Viu alguns dos aprimoramentos que terraform pode simplificar o aprovisionamento de serviços do Azure a partir de dentro do Visual Studio Code. Agora, pode querer rever alguns destes recursos:
- O [registo de módulo do Terraform](https://registry.terraform.io/) apresenta uma lista de todos os módulos do Terraform disponíveis para o Azure e outros fornecedores suportados.

Para cada um destes módulos, são fornecidas as seguintes informações:

- Uma descrição das capacidades gerais do módulo e suas características
- Um exemplo de utilização
- Testar as configurações, que mostram-lhe como criar, executar, e a testar cada módulo no seu computador de desenvolvimento local
- Um Dockerfile para que possa criar e executar localmente um ambiente de desenvolvimento do módulo.
