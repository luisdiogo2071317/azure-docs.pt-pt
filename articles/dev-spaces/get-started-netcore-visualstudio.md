---
title: Criar um espaço de programador do Kubernetes na cloud com o .NET Core e o Visual Studio | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Serviço Azure Kubernetes, contentores
manager: douge
ms.openlocfilehash: 93c1c9cb27e5eb2d56583dccaffe92e9d50ecc2d
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36959279"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core-and-visual-studio"></a>Introdução ao Azure Dev Spaces com o .NET Core e o Visual Studio

Neste guia, vai aprender a:

- Configurar os Espaços de Programador do Azure com um cluster Kubernetes gerido no Azure.
- Utilizar o Visual Studio para desenvolver iterativamente código em contentores.
- Desenvolver de modo independente dois serviços separados e utilizar a deteção do serviço DNS de Kubernetes para fazer uma chamada para outro serviço.
- Desenvolver e testar de forma produtiva o seu código num ambiente de equipa.

[!INCLUDE[](includes/see-troubleshooting.md)]

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="get-the-visual-studio-tools"></a>Obter as ferramentas do Visual Studio
1. Instale a versão mais recente do [Visual Studio 2017](https://www.visualstudio.com/vs/)
1. No instalador do Visual Studio, certifique-se de que a seguinte Carga de Trabalho está selecionada:
    * Desenvolvimento ASP.NET e Web
1. Instale a [extensão do Visual Studio para o Azure Dev Spaces](https://aka.ms/get-azds-visualstudio)

## <a name="create-a-web-app-running-in-a-container"></a>Criar uma aplicação Web em execução num contentor

Nesta secção, vai criar uma aplicação Web em ASP.NET Core e executá-la num contentor no Kubernetes.

### <a name="create-an-aspnet-web-app"></a>Criar uma aplicação Web ASP.NET

No Visual Studio 2017, crie um projeto novo. Atualmente, o projeto tem de ser uma **aplicação Web ASP.NET Core**. Atribua o nome "**webfrontend**" ao projeto.

![](media/get-started-netcore-visualstudio/NewProjectDialog1.png)

Selecione o modelo **Web Application (Model-View-Controller)** (Aplicação Web (Controlador de Vista de Modelo)) e certifique-se de que está a apontar para **.NET Core** e **ASP.NET Core 2.0** nos dois menus pendentes na parte superior da caixa de diálogo. Clique em **OK** para criar o projeto.

![](media/get-started-netcore-visualstudio/NewProjectDialog2.png)


### <a name="enable-dev-spaces-for-an-aks-cluster"></a>Ativar os Espaços de Programador para um cluster do AKS

Com o projeto que acabou de criar, selecione **Azure Dev Spaces** no menu pendente de definições de início, conforme mostrado abaixo.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Na caixa de diálogo apresentada a seguir, certifique-se de que tem sessão iniciada com a conta adequada e, em seguida, selecione um cluster Kubernetes existente.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.PNG)

Por agora, mantenha a predefinição `default` no menu pendente **Space** (Espaço). Vai aprender mais sobre esta opção posteriormente. Selecione a caixa de verificação **Publicly Accessible** (Acessível Publicamente) para que a aplicação Web seja acessível a partir de um ponto final público. Esta definição não é obrigatória, mas será útil para demonstrar alguns conceitos mais adiante nestas instruções. Mas não se preocupe. Em qualquer caso, poderá depurar o seu site com o Visual Studio.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Clique em **OK** para selecionar ou criar o cluster.

Se optar por um cluster que não tenha sido preparado para funcionar com o Azure Dev Spaces, ser-lhe-á apresentada uma mensagem a perguntar se pretende configurá-lo.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Escolha **OK**.

 Será iniciada uma tarefa em segundo plano para executar essa configuração. Esta operação demorará alguns minutos a ser concluída. Para ver se ainda está a ser criado, faça pairar o ponteiro sobre o ícone **Background tasks** (Tarefas em segundo plano) no canto inferior esquerdo da barra de tarefas, conforme mostrado na imagem abaixo.

![](media/get-started-netcore-visualstudio/BackgroundTasks.PNG)

> [!Note]
> Não pode depurar a sua aplicação enquanto o espaço não estiver criado.

### <a name="look-at-the-files-added-to-project"></a>Ver os ficheiros adicionados ao projeto
Enquanto aguarda pela criação do espaço de programador, veja os ficheiros que foram adicionados ao projeto quando optou por utilizar o espaço de programador.

Em primeiro lugar, pode ver que foi adicionada uma pasta com o nome `charts` e que, dentro desta, foi estruturado um [gráfico de Helm](https://docs.helm.sh) para a sua aplicação. Estes ficheiros são utilizados para implementar a aplicação no espaço de programador.

Verá que foi adicionado um ficheiro com o nome `Dockerfile`. Este ficheiro tem as informações necessárias para empacotar a aplicação no formato padrão do Docker.

Por último, verá um ficheiro denominado `azds.yaml`, que contém a configuração em tempo de desenvolvimento que é exigida pelo espaço de programador.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Depurar um contentor no Kubernetes
Assim que o ambiente de programador estiver criado, pode depurar a aplicação. Defina um ponto de interrupção no código, por exemplo, na linha 20 do ficheiro `HomeController.cs` no qual está definida a variável `Message`. Clique em **F5** para iniciar a depuração. 

O Visual Studio vai comunicar com o espaço de programador para criar e implementar a aplicação e, em seguida, vai abrir um browser com a aplicação Web em execução. Poderá parecer que o contentor está a ser executado localmente. Contudo, na verdade, está a ser executado no espaço de programador no Azure. O motivo para o endereço localhost deve-se ao facto de o Azure Dev Spaces criar um túnel SSH temporário para o contentor que está a ser executado no AKS.

Clique na ligação **About** (Acerca de) na parte superior da página para acionar o ponto de interrupção. Tem acesso total às informações de depuração, tal como aconteceria se o código estivesse a ser executado localmente, como, por exemplo, a pilha de chamadas, as variáveis locais, informações de exceção, etc.

## <a name="iteratively-develop-code"></a>Desenvolver código de forma iterativa

O Azure Dev Spaces não se limita apenas a pôr o código em execução no Kubernetes. Tem que ver com permitir-lhe ver, de forma rápida e iterativa, as alterações ao código serem aplicadas num ambiente do Kubernetes na cloud.

### <a name="update-a-content-file"></a>Atualizar um ficheiro de conteúdo
1. Localize o ficheiro `./Views/Home/Index.cshtml` e faça uma edição ao HTML. Por exemplo, altere a linha 70 que lê `<h2>Application uses</h2>` para algo semelhante a `<h2>Hello k8s in Azure!</h2>`
1. Guarde o ficheiro.
1. Aceda ao seu browser e atualize a página. Deverá ver a página Web mostrar o código HTML atualizado.

O que aconteceu? As edições aos ficheiros de conteúdos, como HTML e CSS, não requerem a recompilação numa aplicação Web .NET Core, pelo que uma sessão F5 ativa sincroniza automaticamente qualquer ficheiro de conteúdos modificado com o contentor em execução no AKS, de modo a que possa ver as edições aos conteúdos de imediato.

### <a name="update-a-code-file"></a>Atualizar um ficheiro de código
A atualização de ficheiros de código exige mais algum trabalho, porque a aplicação .NET Core tem de ser recompilada e produzir binários de aplicação atualizados.

1. Pare o depurador no Visual Studio.
1. Abra o ficheiro de código com o nome `Controllers/HomeController.cs` e edite a mensagem que a página About (Sobre) vai apresentar: `ViewData["Message"] = "Your application description page.";`
1. Guarde o ficheiro.
1. Prima **F5** para iniciar novamente a depuração. 

Em vez de reconstruir e reimplementar uma imagem de contentor nova sempre que forem feitas edições ao código, o que, muitas vezes, irá demorar um tempo considerável, o Azure Dev Spaces recompilará incrementalmente o código dentro do contentor existente para proporcionar um ciclo de edição/depuração mais rápido.

Atualize a aplicação Web no browser e aceda à página About (Sobre). Deverá ver a mensagem personalizada apresentada na IU.


## <a name="call-another-container"></a>Chamar outro contentor
Nesta secção, vai criar um segundo serviço, `mywebapi` a ser chamado por `webfrontend`. Cada serviço vai ser executado em contentores separados. Em seguida, vai realizar a depuração em ambos os contentores.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Transfira o código de exemplo para *mywebapi*
Para ser mais rápido, vamos transferir código de exemplo de um repositório do GitHub. Aceda a https://github.com/Azure/dev-spaces e selecione **Clone or Download** (Clonar ou Transferir) para transferir o repositório do GitHub. O código para esta secção está em `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Execute *mywebapi*
1. Abra o projeto `mywebapi` numa *janela separada do Visual Studio*.
1. Selecione **Azure Dev Spaces** no menu pendente de definições de início, tal como fez anteriormente para o projeto `webfrontend`. Agora, em vez de criar um novo cluster do AKS, selecione o mesmo que já criou. Tal como antes, mantenha a predefinição `default` em Space (Espaço) e clique em **OK**. Na janela Output (Saída), vai reparar que o Visual Studio começa a "preparar o arranque" deste novo serviço no seu espaço de programador, de modo a acelerar o processo quando iniciar a depuração.
1. Prima F5 e aguarde que o serviço seja criado e implementado. Saberá que está pronto quando a barra de estado do Visual Studio ficar cor de laranja
1. Tome nota do URL do ponto final apresentado no painel **Azure Dev Spaces for AKS** (Azure Dev Spaces para AKS) na janela **Output** (Saída). Terá um aspeto semelhante a http://localhost:\<portnumber\>. Poderá parecer que o contentor está a ser executado localmente. Contudo, na verdade, está a ser executado no espaço de programador no Azure.
2. Quando o projeto `mywebapi` estiver pronto, abra o browser no endereço localhost e acrescente `/api/values` ao URL para invocar a API GET predefinida para `ValuesController`. 
3. Se todos os passos tiverem sido concluídos com êxito, deverá conseguir ver uma resposta do serviço `mywebapi` com um aspeto semelhante ao seguinte.

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Efetue um pedido de *webfrontend* para *mywebapi*
Vamos agora escrever código em `webfrontend` que efetua um pedido a `mywebapi`. Mude para a janela do Visual Studio que tem o projeto `webfrontend`. No ficheiro `HomeController.cs` *substitua* o código do método About pelo seguinte código:

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

      return View();
   }
   ```

O exemplo de código anterior reencaminha o cabeçalho `azds-route-as` do pedido a receber para o pedido a enviar. Irá perceber, mais tarde, como isto facilita uma experiência de desenvolvimento mais produtiva em cenários de equipa.

### <a name="debug-across-multiple-services"></a>Depurar em vários serviços
1. Neste momento, `mywebapi` ainda deve estar em execução com o depurador anexado. Se não for esse o caso, prima F5 no projeto `mywebapi`.
1. Defina um ponto de interrupção no método `Get(int id)` no ficheiro `Controllers/ValuesController.cs` que processa os pedidos GET `api/values/{id}`.
1. No projeto `webfrontend` onde colou o código acima indicado, defina um ponto de interrupção antes de ser enviado um pedido GET para `mywebapi/api/values`.
1. Prima F5 no projeto `webfrontend`. O Visual Studio irá abrir novamente um browser para a porta localhost adequada e a aplicação Web será apresentada.
1. Clique na ligação "**About**" (Acerca de) na parte superior da página para acionar o ponto de interrupção no projeto `webfrontend`. 
1. Prima F10 para continuar. O ponto de interrupção no projeto `mywebapi` é acionado.
1. Prima F5 para continuar. Isso fará com que regresse ao código no projeto `webfrontend`.
1. Se premir F5 uma vez mais, o pedido será concluído e será devolvida uma página no browser. Na aplicação Web, a página About (Sobre) apresentará uma mensagem concatenada pelos dois serviços: "Hello from webfrontend and Hello from mywebapi".

Já está! Tem agora uma aplicação com vários contentores, na qual cada contentor pode ser desenvolvido e implementado separadamente.

## <a name="learn-about-team-development"></a>Saiba mais sobre desenvolvimento em equipa

Até aqui, executou o código da aplicação como se fosse o único programador a trabalhar na aplicação. Nesta secção, irá aprender até que ponto o Azure Dev Spaces simplifica o desenvolvimento em equipa ao:
* Permita que uma equipa de programadores trabalhe no mesmo ambiente, ao trabalhar num espaço de desenvolvimento partilhado ou em espaços de desenvolvimento distintos, conforme necessário...
* Suportar que cada programador faça iterações no respetivo código de forma isolada e sem receio de danificar o código de terceiros.
* Testar o código ponto a ponto, antes de proceder à consolidação do código, sem ter de criar cenários fictícios ou simular dependências.

### <a name="challenges-with-developing-microservices"></a>Desafios relativamente ao desenvolvimento de microsserviços
Este exemplo de aplicação não apresenta índices de complexidade elevados até ao momento. Todavia, num cenário de desenvolvimento real, os desafios começam a surgir à medida que são adicionados mais serviços e a equipa de desenvolvimento começa a aumentar.

Imagine que está a trabalhar num serviço que interage com dezenas de outros serviços.

- A execução de todos os elementos para fins de desenvolvimento a nível local pode tornar-se uma perspetiva irrealista. A máquina de desenvolvimento pode não ter recursos suficientes para executar a aplicação completa. Quem sabe, a aplicação tem pontos finais que têm de estar acessíveis publicamente (por exemplo, a aplicação responde a um webhook a partir de uma aplicação SaaS).
- Pode tentar executar apenas os serviços de que depende, mas isto significa que teria de conhecer o âmbito completo das dependências (por exemplo, as dependências das dependências). Por outro lado, pode ser uma questão de não saber exatamente como criar e executar as dependências visto não ter estado envolvido no desenvolvimento das mesmas.
- Alguns programadores recorrem à simulação ou criação de cenários fictícios de muitas das suas dependências de serviço. Esta pode ser uma abordagem útil. No entanto, mais cedo ou mais tarde, a gestão desses cenários fictícios acaba por exigir o seu próprio esforço de desenvolvimento. Além disso, o aspeto do seu ambiente de desenvolvimento acaba por divergir do da produção, dando azo ao aparecimento de erros subtis.
- Daqui decorre que se torna difícil levar a cabo qualquer tipo de teste ponto a ponto. Realisticamente, o teste de integração só pode ocorrer após uma consolidação, o que significa que irá ver os problemas numa fase posterior no ciclo de desenvolvimento.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Trabalhar num espaço de desenvolvimento partilhado
O Azure Dev Spaces permite-lhe configurar um espaço de desenvolvimento *partilhado* no Azure. Cada programador pode concentrar-se unicamente na respetiva parte da aplicação e, assim, desenvolver o *código de pré-consolidação* de forma iterativa num espaço de desenvolvimento que já contém todos os outros serviços e recursos na cloud de que os respetivos cenários dependem. As dependências estão sempre atualizadas e os programadores trabalham de uma forma que reflete a produção.

### <a name="work-in-your-own-space"></a>Trabalhar no seu próprio espaço
À medida que desenvolve o código para o seu serviço e até estar preparado para o implementar, é frequente o código não estar em bom estado. A formulação, os testes e as experiências com soluções ao nível do código são um processo iterativo contínuo. O Azure Dev Spaces fornece o conceito de **espaço**, onde pode trabalhar de forma isolada e sem receio de prejudicar os membros da sua equipa.

Proceda do seguinte modo para se certificar de que os serviços `webfrontend` e `mywebapi` estão em execução **no `default` espaço de programador** .
1. Feche as sessões de depuração/F5 de ambos os serviços, mas mantenha os projetos abertos nas respetivas janelas do Visual Studio.
2. Mude para a janela do Visual Studio que tem o projeto `mywebapi` e prima Ctrl+F5 para executar o serviço sem o depurador anexado.
3. Mude para a janela do Visual Studio que tem o projeto `webfrontend` e prima Ctrl+F5 para o executar também.

> [!Note]
> Por vezes, é necessário atualizar o browser depois de a página Web ser apresentada inicialmente após ter premido Ctrl+F5.

Qualquer pessoa que abra o URL público e navegue para a aplicação Web irá invocar o caminho do código que escreveu e que é comum a ambos os serviços que utilizam espaço `default` predefinido. Agora, suponha que quer continuar a desenvolver o serviço `mywebapi`. De que forma o poderá fazê-lo sem interromper os outros programadores que estão a utilizar o espaço de programador? Para tal, terá de configurar o seu próprio espaço.

### <a name="create-a-new-dev-space"></a>Criar um novo espaço de programador
A partir do Visual Studio, pode criar espaços adicionais que serão utilizados quando prime F5 ou Ctrl+F5 no seu serviço. Pode dar o nome que quiser a um espaço e pode ser flexível quanto ao seu significado (por exemplo, `sprint4` ou `demo`).

Faça o seguinte para criar um novo espaço:
1. Mude para a janela do Visual Studio que tem o projeto `mywebapi`.
2. Clique com o botão direito do rato no projeto em **Solution Explorer** (Explorador de Soluções) e selecione **Properties** (Propriedades).
3. Selecione o separador **Debug** (Depurar) à esquerda para mostrar as definições do Azure Dev Spaces.
4. Aqui, pode alterar ou criar o cluster e/ou o espaço que será utilizado quando premir F5 ou Ctrl+F5. *Certifique-se de que o Azure Dev Space que criou anteriormente está selecionado*.
5. Na lista pendente Space (Espaço), selecione **<Create New Space…>** (Criar Novo Espaço).

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. Na caixa de diálogo **Add Space** (Adicionar Espaço), escreva um nome para o espaço e clique em **OK**. Pode utilizar um nome próprio (por exemplo, "scott") para o novo espaço para que os seus colegas o possam identificar como o espaço em que está a trabalhar.

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Já deverá conseguir ver o seu cluster do AKS e o novo espaço selecionado na página de propriedades do projeto.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>Atualizar o código de *mywebapi*

1. No projeto `mywebapi`, introduza uma alteração no código do método `string Get(int id)` no ficheiro `Controllers/ValuesController.cs` da seguinte forma:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Defina um ponto de interrupção neste bloco de código atualizado (pode já ter um definido anteriormente).
3. Prima F5 para iniciar o serviço `mywebapi`. Isto irá iniciar o serviço no seu cluster através do espaço selecionado, que neste caso é `scott`.

Segue-se um diagrama que o ajudará a compreender como funcionam os diferentes espaços. O caminho roxo mostra um pedido feito através do espaço `default`, ou seja, o caminho predefinido utilizado caso o URL não seja precedido de qualquer espaço. O caminho rosa mostra um pedido feito através do espaço `default/scott`.

![](media/common/Space-Routing.png)

Esta capacidade incorporada do Azure Dev Spaces permite-lhe testar o código ponto a ponto num ambiente partilhado sem que cada programador tenha de recriar a pilha completa de serviços no respetivo espaço. Este tipo de encaminhamento requer que os cabeçalhos de propagação sejam reencaminhados no código da aplicação, conforme ilustrado no passo anterior deste guia.

### <a name="test-code-running-in-the-defaultscott-space"></a>Testar o código em execução no espaço `default/scott`
Para testar a nova versão de `mywebapi` em conjunto com `webfrontend`, abra o browser no URL do ponto de acesso público de `webfrontend` (por exemplo, http://webfrontend.123456abcdef.eastus.aksapp.io)) e aceda à página About (Acerca de). Deverá ver a mensagem original "Hello from webfrontend and Hello from mywebapi" (Olá de webfrontend e Olá de mywebapi).

Em seguida, adicione o elemento "scott.s." ao URL de modo a mostrar algo semelhante a http://scott.s.webfrontend.123456abcdef.eastus.aksapp.io e atualize o browser. O ponto de interrupção definido no projeto `mywebapi` deverá ser acedido. Clique em F5 para continuar. No browser, deverá ver a nova mensagem "Hello from webfrontend and mywebapi now says something new." (Olá de webfrontend e mywebapi agora indica algo de novo). Isto acontece porque o caminho para o seu código atualizado em `mywebapi` está em execução no espaço `default/scott`.

[!INCLUDE[](includes/well-done.md)]

[!INCLUDE[](includes/clean-up.md)]
