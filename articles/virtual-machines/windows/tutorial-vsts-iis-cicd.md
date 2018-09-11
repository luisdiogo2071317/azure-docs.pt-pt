---
title: Tutorial - criar um pipeline CI/CD no Azure com os serviços do Azure DevOps | Documentos da Microsoft
description: Neste tutorial, saiba como criar um pipeline de serviços de DevOps do Azure para integração contínua e entrega que implementa uma aplicação web IIS numa VM do Windows no Azure.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d4edf0d22ce04eb2cb865d80c2b70f1bcc2169df
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301903"
---
# <a name="tutorial-create-a-continuous-integration-pipeline-with-azure-devops-services-and-iis"></a>Tutorial: Criar um pipeline de integração contínua com serviços de DevOps do Azure e o IIS
Para automatizar a compilação, teste e fases de implementação de desenvolvimento de aplicativos, pode usar uma integração contínua e um pipeline de implementação (CI/CD). Neste tutorial, vai criar um pipeline de CI/CD com serviços de DevOps do Azure e uma máquina virtual de Windows (VM) no Azure que executa o IIS. Saiba como:

> [!div class="checklist"]
> * Publicar uma aplicação web do ASP.NET para um projeto de serviços do Azure DevOps
> * Criar um pipeline de compilação que é acionado por consolidações de código
> * Instalar e configurar o IIS numa máquina virtual no Azure
> * Adicionar a instância do IIS a um grupo de implementação nos serviços de DevOps do Azure
> * Criar um pipeline de lançamento para publicar o novo web implementar pacotes do IIS
> * Testar o pipeline de CI/CD

Este tutorial requer a versão do módulo 5.7.0 ou posterior do Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

## <a name="create-a-project-in-azure-devops-services"></a>Criar um projeto nos serviços de DevOps do Azure
Serviços de DevOps do Azure permite a colaboração fácil e de desenvolvimento sem manter uma solução de gestão de código no local. Serviços de DevOps do Azure fornece testes de código na cloud, compilação e o application insights. Pode escolher um repositório de controle de versão e o IDE que melhor se adequa a seu desenvolvimento de código. Para este tutorial, pode utilizar uma organização gratuita para criar uma aplicação web básica do ASP.NET e um pipeline CI/CD. Se ainda não tiver uma organização de serviços do Azure DevOps [criá-lo](http://go.microsoft.com/fwlink/?LinkId=307137).

Para gerir o processo de consolidação de código, Criar pipelines e lançar pipelines, crie um projeto nos serviços de DevOps do Azure da seguinte forma:

1. Abra o dashboard de serviços de DevOps do Azure num navegador da web e escolha **novo projeto**.
2. Introduza *myWebApp* para o **nome do projeto**. Deixe todos os outros valores predefinidos para utilizar *Git* controle de versão e *Agile* processo do item de trabalho.
3. Escolha a opção para **partilhar com** *membros da Equipe*, em seguida, selecione **criar**.
5. Assim que tiver sido criado seu projeto, escolha a opção para **inicializar com um ficheiro Leia-me ou gitignore**, em seguida, **inicializar**.
6. Dentro de seu novo projeto, escolha **Dashboards** na parte superior, em seguida, selecione **abrir no Visual Studio**.


## <a name="create-aspnet-web-application"></a>Criar aplicação web ASP.NET
No passo anterior, criou um projeto nos serviços de DevOps do Azure. A etapa final é aberto o novo projeto no Visual Studio. Gerir o seu consolidações de código no **Team Explorer** janela. Criar uma cópia local do seu novo projeto e, em seguida, crie uma aplicação web ASP.NET a partir de um modelo da seguinte forma:

1. Selecione **Clone** para criar um repositório de local git do seu projeto de serviços de DevOps do Azure.
    
    ![Clone o repositório do projeto de serviços do Azure DevOps](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. Sob **soluções**, selecione **New**.

    ![Criar a solução de aplicação web](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Selecione **Web** modelos e, em seguida, escolha a **aplicação Web ASP.NET** modelo.
    1. Introduza um nome para a sua aplicação, tal como *myWebApp*e desmarque a caixa **criar diretório para a solução**.
    2. Se a opção está disponível, desmarque a caixa para **adicionar o Application Insights ao projeto**. O Application Insights tem de autorizar a sua aplicação web com o Azure Application Insights. Para simplificar este tutorial, ignore este processo.
    3. Selecione **OK**.
4. Escolher **MVC** na lista de modelos.
    1. Selecione **alterar autenticação**, escolha **sem autenticação**, em seguida, selecione **OK**.
    2. Selecione **OK** para criar a sua solução.
5. Na **Team Explorer** janela, escolha **alterações**.

    ![Confirmar alterações locais para o repositório de Git de repositórios do Azure](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Na caixa de texto de consolidação, introduza uma mensagem como *consolidação inicial*. Escolher **consolidar todas as e sincronização** no menu pendente.


## <a name="create-build-pipeline"></a>Criar o pipeline de compilação
Serviços de DevOps do Azure, vai utilizar um pipeline de compilação para descrever como seu aplicativo deve ser criado. Neste tutorial, vamos criar um pipeline básico que leva nosso código-fonte, compila a solução, em seguida, cria web implementar o pacote que podemos usar para executar a aplicação web num servidor IIS.

1. No seu projeto de serviços de DevOps do Azure, escolha **criar e lançar** na parte superior, em seguida, selecione **baseia-se**.
3. Selecione **+ nova definição**.
4. Escolha o **ASP.NET (pré-visualização)** modelo e selecione **aplicar**.
5. Deixe os valores das tarefas de todos os a predefinição. Sob **obter origens**, certifique-se de que o *myWebApp* repositório e *mestre* ramo estão selecionadas.

    ![Criar o pipeline de compilação no projeto de serviços do Azure DevOps](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Sobre o **Acionadores** separador, mova o controlo de deslize para **ativar este acionador** para *ativado*.
7. Salve o pipeline de compilação e a fila uma nova compilação selecionando **guardar e colocar em fila** , em seguida, **guardar e colocar em fila** novamente. Deixe as predefinições e selecione **fila**.

Veja como a compilação está agendada num agente alojado, em seguida, começa a criar. O resultado é semelhante ao seguinte exemplo:

![Compilação bem-sucedida do projeto de serviços do Azure DevOps](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Criar a máquina virtual
Para fornecer uma plataforma para executar a aplicação web ASP.NET, precisa de uma máquina de virtual do Windows que executa o IIS. Serviços de DevOps do Azure utiliza um agente para interagir com a instância do IIS, à medida que consolida código e compilações são acionadas.

Criar uma VM do Windows Server 2016 com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma VM com o nome *myVM* no *E.U.A. Leste* localização. O grupo de recursos *myResourceGroupAzureDevOpsServices* e recursos de rede de apoio também são criados. Para permitir o tráfego da web, a porta TCP *80* for aberta para a VM. Quando solicitado, forneça um nome de utilizador e palavra-passe para ser utilizados como as credenciais de início de sessão para a VM:

```powershell
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine
New-AzureRmVM `
  -ResourceGroupName "myResourceGroupAzureDevOpsServices" `
  -Name "myVM" `
  -Location "East US" `
  -ImageName "Win2016Datacenter" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNetworkSecurityGroup" `
  -PublicIpAddressName "myPublicIp" `
  -Credential $cred `
  -OpenPorts 80
```

Para ligar à sua VM, obtenha o endereço IP público com [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) da seguinte forma:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Crie uma sessão de ambiente de trabalho remoto à sua VM:

```cmd
mstsc /v:<publicIpAddress>
```

Na VM, abra um **do PowerShell de administrador** prompt de comando. Instale o IIS e as funcionalidades necessárias do .NET da seguinte forma:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Criar grupo de implementação

Para instalá-pacote de implementação da web para o servidor IIS, define um grupo de implementação nos serviços de DevOps do Azure. Este grupo permite que especifique quais servidores são o destino de novas compilações, à medida que consolida código aos serviços do Azure DevOps e compilações são concluídas.

1. Nos serviços de DevOps do Azure, escolha **criar e lançar** e, em seguida, selecione **grupos de implementação**.
2. Escolher **grupo de implementação de adicionar**.
3. Introduza um nome para o grupo, tal como *myIIS*, em seguida, selecione **criar**.
4. Na **registar máquinas** secção, certifique-se *Windows* está selecionado, em seguida, selecione a caixa para **utilizam um token de acesso pessoal no script para a autenticação**.
5. Selecione **copie o script para área de transferência**.


### <a name="add-iis-vm-to-the-deployment-group"></a>Adicionar a VM de IIS para o grupo de implementação
Com o grupo de implementação criado, adicione cada instância do IIS para o grupo. Serviços de DevOps do Azure gera um script que transfere e configura um agente na VM que recebe o novo web implementar pacotes, em seguida, aplica-se-lo para o IIS.

1. De volta a **do PowerShell de administrador** sessão na sua VM, colar e executar o script copiado a partir dos serviços de DevOps do Azure.
2. Quando lhe for pedido para configurar etiquetas para o agente, escolher *Y* e introduza *web*.
3. Quando lhe for pedido para a conta de utilizador, prima *retornar* para aceitar as predefinições.
4. Aguarde que o script para ser concluído com uma mensagem *vstsagent.account.computername de serviço foi iniciado com êxito*.
5. Na **grupos de implementação** página do **compilação e versão** menu, abra a *myIIS* grupo de implementação. Sobre o **máquinas** separador, certifique-se de que a sua VM está listada.

    ![VM adicionado com êxito ao grupo de implementação de serviços do Azure DevOps](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-pipeline"></a>Criar o pipeline de lançamento
Para publicar suas compilações, vai criar um pipeline de lançamento nos serviços de DevOps do Azure. Este pipeline é acionado automaticamente por uma compilação bem-sucedida da sua aplicação. Escolher o grupo de implementação para enviar sua web implementar o pacote para e definir as configurações apropriadas do IIS.

1. Escolher **criar e lançar**, em seguida, selecione **baseia-se**. Escolha o pipeline de compilação que criou no passo anterior.
2. Sob **concluídas recentemente**, escolha a compilação mais recente, em seguida, selecione **versão**.
3. Escolher **Sim** para criar um pipeline de lançamento.
4. Escolha o **vazio** modelo, em seguida, selecione **próxima**.
5. Certifique-se de que o pipeline de compilação do projeto e de origem são preenchidos com seu projeto.
6. Selecione o **implementação contínua** caixa de verificação, em seguida, selecione **criar**.
7. Selecione a caixa de lista pendente junto a **+ adicionar tarefas** e escolha *adicionar uma fase de grupo de implementação*.
    
    ![Adicionar a tarefa de pipeline nos serviços de DevOps do Azure de versões](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Escolher **Add** junto a **Deploy(Preview) de aplicação Web de IIS**, em seguida, selecione **fechar**.
9. Selecione o **executado no grupo de implementação** tarefa principal.
    1. Para **grupo de implementação**, selecione o grupo de implementação que criou anteriormente, tal como *myIIS*.
    2. No **Machine etiquetas** caixa, selecione **Add** e escolha o *web* marca.
    
    ![Tarefa do grupo de implementação de pipeline do lançamento do IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Selecione o **Deploy: implementação de aplicação Web do IIS** tarefas para configurar as definições de instância IIS da seguinte forma:
    1. Para **nome do Web site**, introduza *Web Site predefinido*.
    2. Deixe todas as outras predefinições.
12. Escolher **salvar**, em seguida, selecione **OK** duas vezes.


## <a name="create-a-release-and-publish"></a>Criar uma versão e publicar
Agora pode emitir sua web implementar o pacote como uma nova versão. Este passo se comunica com o agente em cada instância que é parte do grupo de implementação, envia a web implementar o pacote, em seguida, configura o IIS para executar a aplicação web atualizada.

1. No seu pipeline de lançamento, selecione **+ versão**, em seguida, escolha **criar versão**.
2. Certifique-se de que a compilação mais recente está selecionada na lista pendente, juntamente com **implementação automatizada: após a criação da versão**. Selecione **Criar**.
3. Pequeno é apresentada uma faixa na parte superior do seu pipeline de lançamento, tal como *lançamento "Release-1" criou*. Selecione a ligação de versão.
4. Abra o **registos** separador para ver o progresso de versão.
    
    ![Versão de serviços do Azure DevOps o êxito e a web implementar push de pacote](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Após o lançamento for concluído, abra um browser e introduza o endereço IIP público da sua VM. Aplicação web ASP.NET está em execução.

    ![Aplicação de web do ASP.NET em execução na VM de IIS](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a>Testar todo o pipeline de CI/CD
Com a sua aplicação web em execução no IIS, experimente agora a todo o pipeline de CI/CD. Depois de efetuar uma alteração no Visual Studio e consolidação seu código, uma compilação é disparado, que dispara uma versão da web atualizado implantar o pacote para o IIS:

1. No Visual Studio, abra a **Explorador de soluções** janela.
2. Navegue para e abra *myWebApp | Modos de exibição | Home page | Index. cshtml*
3. Edite linha 6 para leitura:

    `<h1>ASP.NET with Azure DevOps Services and CI/CD!</h1>`

4. Guarde o ficheiro.
5. Abra o **Team Explorer** janela, selecione a *myWebApp* do projeto, em seguida, escolha **alterações**.
6. Introduza uma mensagem de consolidação, tal como *pipeline CI/CD testes*, em seguida, escolha **consolidar tudo e sincronização** no menu pendente.
7. Na área de trabalho de serviços de DevOps do Azure, uma nova compilação é acionada da consolidação de código. 
    - Escolher **criar e lançar**, em seguida, selecione **baseia-se**. 
    - Escolha o seu pipeline de compilação, em seguida, selecione o **em fila e em execução** compilação para ver como o andamento da compilação.
9. Assim que a compilação for concluída com êxito, é acionada uma nova versão.
    - Escolher **criar e lançar**, em seguida, **versões** para ver a web implementar pacote enviada por push para a VM de IIS. 
    - Selecione o **atualizar** ícone para atualizar o estado. Quando o *ambientes* coluna mostra uma marca de verificação verde, o lançamento for implementado com êxito para o IIS.
11. Para ver as alterações aplicadas, atualize o seu Web site do IIS num browser.

    ![Aplicação de web do ASP.NET em execução numa VM de IIS a partir do pipeline CI/CD](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma aplicação web ASP.NET nos serviços de DevOps do Azure e configurado a compilação e lançar pipelines para implementar o novo web implementar pacotes do IIS em cada consolidação de código. Aprendeu a:

> [!div class="checklist"]
> * Publicar uma aplicação web do ASP.NET para um projeto de serviços do Azure DevOps
> * Criar um pipeline de compilação que é acionado por consolidações de código
> * Instalar e configurar o IIS numa máquina virtual no Azure
> * Adicionar a instância do IIS a um grupo de implementação nos serviços de DevOps do Azure
> * Criar um pipeline de lançamento para publicar o novo web implementar pacotes do IIS
> * Testar o pipeline de CI/CD

Avance para o próximo tutorial para saber como instalar o SQL&#92;IIS&#92;pilha de .NET num par de VMs do Windows.

> [!div class="nextstepaction"]
> [SQL&#92;IIS&#92;pilha de .NET](tutorial-iis-sql.md)
