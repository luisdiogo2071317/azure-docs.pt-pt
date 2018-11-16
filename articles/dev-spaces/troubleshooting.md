---
title: Resolução de problemas | Documentos da Microsoft
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 09/11/2018
ms.topic: article
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contentores
ms.openlocfilehash: 36516030741678ec66b4211f49ede35cfdb98605
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706454"
---
# <a name="troubleshooting-guide"></a>Guia de resolução de problemas

Este guia contém informações sobre problemas comuns que poderá ter quando utilizar espaços de desenvolvimento do Azure.

## <a name="enabling-detailed-logging"></a>Ativar o registo detalhado

Para resolver problemas com mais eficiência, ele pode ajudar a criar registos mais detalhados para revisão.

Para a extensão do Visual Studio, defina o `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` variável de ambiente para 1. Certifique-se de que reinicie o Visual Studio para a variável de ambiente para entrar em vigor. Uma vez ativada, os registos detalhados serão gravados no seu `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` diretório.

Na CLI, o utilizador pode apresentar mais informações durante a execução do comando utilizando o `--verbose` mudar. Também pode procurar registos mais detalhados no `%TEMP%\Azure Dev Spaces`. Num Mac, o diretório TEMP do diretório pode ser encontrado ao executar `echo $TMPDIR` de uma janela de terminal. Num computador Linux, o diretório TEMP do diretório é normalmente `/tmp`.

## <a name="debugging-services-with-multiple-instances"></a>Depuração de serviços com várias instâncias

Neste momento, espaços de desenvolvimento do Azure funciona melhor quando a depuração de uma única instância (pod). O ficheiro de azds.yaml contém uma definição, replicaCount, que indica o número de pods que será executada para o seu serviço. Se alterar o replicaCount para configurar a aplicação seja executada vários pods para um determinado serviço, o depurador se ligará o pod primeiro (quando listados por ordem alfabética). Se desse pod recicla por qualquer motivo, irá anexar o depurador a um pod diferente, resultando possivelmente num comportamento inesperado.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>Erro "Falha ao criar o controlador de espaços de desenvolvimento do Azure"

Poderá ver este erro quando algo dá errado com a criação do controlador. Se for um erro transitório, eliminar e recriar o controlador irão corrigi-lo.

### <a name="try"></a>Experimente:

Para eliminar o controlador, utilize a CLI do Azure Dev espaços. Não é possível fazê-lo no Visual Studio ou no Cloud Shell. Para instalar a CLI AZDS, primeiro de instalar a CLI do Azure e, em seguida, execute este comando:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

E, em seguida, execute este comando para eliminar o controlador:

```cmd
azds remove -g <resource group name> -n <cluster name>
```

Recriar o controlador pode ser feito na CLI ou do Visual Studio. Siga as instruções nos tutoriais como se a iniciar pela primeira vez.


## <a name="error-service-cannot-be-started"></a>Erro "serviço não pode ser iniciado."

Pode ver este erro quando o seu código de serviço não iniciar. A causa é, muitas vezes, no código do usuário. Para obter mais informações de diagnóstico, efetue as seguintes alterações aos seus comandos e as definições:

### <a name="try"></a>Experimente:

Na linha de comando:

Ao usar _azds.exe_, utilize a opção-- verbose da linha de comandos e utilizar a opção-- saída da linha de comandos para especificar o formato de saída.
 
```cmd
azds up --verbose --output json
```

No Visual Studio:

1. Open **ferramentas > opções** e, em **projetos e soluções**, escolha **compilar e executar**.
2. Alterar as definições de **verbosidade de saída da compilação de projeto do MSBuild** ao **Detailed** ou **diagnóstico**.

    ![Caixa de diálogo Opções de captura de ecrã de ferramentas](media/common/VerbositySetting.PNG)
    
Poderá ver este erro quando tentar utilizar um Dockerfile vários estágio. A saída detalhada terá o seguinte aspeto:

```cmd
$ azds up
Using dev space 'default' with target 'AksClusterName'
Synchronizing files...6s
Installing Helm chart...2s
Waiting for container image build...10s
Building container image...
Step 1/12 : FROM [imagename:tag] AS base
Error parsing reference: "[imagename:tag] AS base" is not a valid repository/tag: invalid reference format
Failed to build container image.
Service cannot be started.
```

Isso é porque nós do AKS executados uma versão mais antiga do Docker não suporta vários estágios baseia-se. Terá de reescrever seu Dockerfile para evitar compilações de vários estágios.

## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>Resolução de nomes DNS falha por um URL público associado a um serviço de espaços de desenvolvimento

Quando a resolução de nome DNS falha, poderá ver um erro "não é possível aceder este site" em seu navegador da web quando tentar ligar-se para o URL público associado a um serviço de espaços de desenvolvimento ou de "Não é possível apresentar a página".

### <a name="try"></a>Experimente:

Pode utilizar o seguinte comando para listar todas as URLs associadas com os serviços de espaços de desenvolvimento:

```cmd
azds list-uris
```

Se um URL no *pendente* Estado, o que significa que os espaços de desenvolvimento ainda está a aguardar concluir o registo de DNS. Às vezes, demora alguns minutos para o registo concluir. Espaços de desenvolvimento também é aberto um túnel de localhost para cada serviço, que pode utilizar enquanto estiver aguardando o registo de DNS.

Se um URL continuará a ser o *pendente* de estado durante mais de 5 minutos, tal poderá indicar um problema com o pod DNS externo que cria o ponto final público e/ou o pod de controlador de entrada nginx adquire o ponto final público. Pode utilizar os comandos seguintes para eliminar estas pods. Eles serão recriados automaticamente.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

## <a name="error-required-tools-and-configurations-are-missing"></a>Erro "Necessário ferramentas e configurações estão em falta"

Este erro poderá ocorrer ao iniciar o VS Code: "[espaços de desenvolvimento do Azure] necessário a ferramentas e configurações para compilar e depurar"[nome do projeto]"estão em falta."
O erro significa que azds.exe não está na variável de ambiente PATH, como visto no VS Code.

### <a name="try"></a>Experimente:

Inicie o VS Code a partir de um prompt de comando em que a variável de ambiente PATH está definida corretamente.

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>Erro 'azds' não é reconhecido como um comando interno ou externo, programa operável ou arquivo em lotes
 
Poderá ver este erro se azds.exe não está instalado ou configurado corretamente.

### <a name="try"></a>Experimente:

1. Verifique a localização %ProgramFiles%/Microsoft SDKs\Azure\Azure Dev espaços CLI (pré-visualização) para azds.exe. Se existir, adicione essa localização à variável de ambiente PATH.
2. Se azds.exe não estiver instalado, execute o seguinte comando:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Aviso 'Dockerfile não foi possível gerar devido a linguagem não suportada"
Espaços de desenvolvimento do Azure fornece suporte nativo para c# e node. js. Quando executa *azds prep* num diretório que contém o código escrito em uma dessas linguagens, espaços de desenvolvimento do Azure irá criar automaticamente um Dockerfile apropriado para.

Pode continuar a utilizar espaços de desenvolvimento do Azure com código escrito em outras linguagens, mas terá de criar o Dockerfile manualmente antes de executar *azds se* pela primeira vez.

### <a name="try"></a>Experimente:
Se o aplicativo foi escrito numa linguagem que os espaços de desenvolvimento do Azure não suporta nativamente, terá de fornecer um Dockerfile adequada para criar uma imagem de contentor em execução do seu código. Docker disponibiliza um [lista de práticas recomendadas para escrever Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) , bem como um [referência do Dockerfile](https://docs.docker.com/engine/reference/builder/) que pode ajudá-lo a se escreve um Dockerfile que atenda às suas necessidades.

Depois de ter um Dockerfile adequada em vigor, pode continuar com a execução *azds se* para executar a aplicação nos espaços de desenvolvimento do Azure.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Erro 'a montante erro de ligar ou desligar/reset antes de cabeçalhos'
Poderá ver este erro ao tentar aceder ao seu serviço. Por exemplo, quando passa para o URL do serviço num navegador. 

### <a name="reason"></a>Razão 
A porta do contentor não está disponível. Esse problema pode ocorrer porque: 
* O contentor está ainda estiver no processo que está a ser compilado e implantado. Este problema pode surgir se executar `azds up` ou iniciar o depurador e, em seguida, tente aceder ao contentor antes de ser implementada com êxito.
* Configuração da porta não é consistente em toda sua _Dockerfile_, gráfico Helm e qualquer código de servidor que se abre uma porta.

### <a name="try"></a>Experimente:
1. Se o contentor está no processo a ser criado/implementado, pode aguardar 2 a 3 segundos e tente acessar o serviço novamente. 
1. Verifique a configuração de porta. Os números de porta especificado devem estar **idênticos** em todos os recursos abaixo:
    * **O Dockerfile:** especificado pelo `EXPOSE` instrução.
    * **[Gráfico do Helm](https://docs.helm.sh):** especificado pela `externalPort` e `internalPort` valores para um serviço (muitas vezes, localizados num `values.yml` ficheiro),
    * Nenhuma porta de que está a ser aberta no código da aplicação, por exemplo, no node. js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Ficheiro de configuração não encontrado
Executar `azds up` e receber o erro seguinte: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Razão
Tem de executar `azds up` do diretório de raiz do código que pretende executar e, é preciso inicializar a pasta de código para executar com espaços de desenvolvimento do Azure.

### <a name="try"></a>Experimente:
1. Altere o diretório atual para a pasta de raiz que contém o código de serviço. 
1. Se não tiver uma _azds.yaml_ ficheiro na pasta do código, executar `azds prep` para gerar os ativos de Docker, Kubernetes e espaços de desenvolvimento do Azure.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Erro: "o pipe programa 'azds' terminado inesperadamente com o código 126."
A iniciar o depurador do VS Code, às vezes, pode resultar em erro.

### <a name="try"></a>Experimente:
1. Feche e reabra o VS Code.
2. Aperte F5 novamente.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>Depuração de erro 'Falha ao localizar a extensão de depurador para o tipo: coreclr'
Executar o depurador do VS Code reporta o erro: `Failed to find debugger extension for type:coreclr.`

### <a name="reason"></a>Razão
Não tem a extensão do VS Code para c# instalado no computador de desenvolvimento. A extensão c# inclui a depuração de suporte para .net Core (CoreCLR).

### <a name="try"></a>Experimente:
Instalar o [extensão do VS Code para c#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Depuração de erro 'o tipo de depuração de configurado 'coreclr' não é suportado'
Executar o depurador do VS Code reporta o erro: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Razão
Não tem a extensão do VS Code para espaços de desenvolvimento do Azure instalado no computador de desenvolvimento.

### <a name="try"></a>Experimente:
Instalar o [extensão do VS Code para espaços de desenvolvimento do Azure](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>Não foi possível encontrar o nome do tipo ou espaço de nomes "MyLibrary."

### <a name="reason"></a>Razão 
O contexto de compilação é no nível de projeto/serviço por predefinição, portanto um projeto de biblioteca que está a utilizar não será encontrado.

### <a name="try"></a>Experimente:
O que precisa ser feito:
1. Modificar a _azds.yaml_ ficheiro para definir o contexto de compilação para o nível de solução.
2. Modificar a _Dockerfile_ e _Dockerfile.develop_ arquivos para fazer referência ao projeto (_. csproj_) ficheiros corretamente, em relação ao novo criar contexto.
3. Lugar uma _.dockerignore_ arquivo ao lado do ficheiro. sln e modifique conforme necessário.

Pode encontrar um exemplo em https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>Erro de autorização 'Microsoft.DevSpaces/register/action'
Poderá ver o erro seguinte quando estiver a gerir um espaço de desenvolvimento do Azure e estiver a trabalhar numa subscrição do Azure para o qual não tem proprietário ou contribuinte de acesso.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Razão
A subscrição do Azure selecionada não se registou o `Microsoft.DevSpaces` espaço de nomes.

### <a name="try"></a>Experimente:
Qualquer pessoa com acesso de proprietário ou contribuinte à subscrição do Azure pode executar o seguinte comando da CLI do Azure para registar manualmente o `Microsoft.DevSpaces` espaço de nomes:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="error-could-not-find-a-ready-tiller-pod-when-launching-dev-spaces"></a>"Erro: não foi possível encontrar um pod tiller pronto" ao iniciar o desenvolvimento espaços

### <a name="reason"></a>Razão
Este erro ocorre se o cliente do Helm já não pode comunicar com o pod Tiller em execução no cluster.

### <a name="try"></a>Experimente:
Reiniciar os nós de agente no seu cluster normalmente resolve este problema.

## <a name="azure-dev-spaces-proxy-can-interfere-with-other-pods-running-in-a-dev-space"></a>Proxy de espaços de desenvolvimento do Azure pode interferir com outras pods em execução num espaço de desenvolvimento

### <a name="reason"></a>Razão
Quando ativar espaços de desenvolvimento num espaço de nomes no seu cluster do AKS, um contentor adicional chamado _mindaro proxy_ está instalado em cada um dos pods em execução dentro desse espaço de nomes. Este contentor intercepta as chamadas para os serviços no pod, que é integral para recursos de desenvolvimento de equipe dos espaços de desenvolvimento.

Infelizmente, ele pode interferir com determinados serviços em execução nos pods. Especificamente, interfere com pods em execução a cache de Redis, fazendo com que os erros de ligação e falhas de comunicação de mestre/subordinado.

### <a name="try"></a>Experimente:
Pode mover o pod(s) afetados para um espaço de nomes dentro do cluster que faz _não_ ter espaços de desenvolvimento ativada, continuando a execução do resto do seu aplicativo dentro de um espaço de nomes de ativado o espaços de desenvolvimento. Espaços de desenvolvimento não irá instalar o _mindaro proxy_ espaços de nomes do contentor dentro de espaços não Dev ativados.

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Espaços de desenvolvimento do Azure parece que não usar meu Dockerfile existente para criar um contentor 

### <a name="reason"></a>Razão
Espaços de desenvolvimento do Azure pode ser configurados para apontar para um específico _Dockerfile_ no seu projeto. Se for apresentada a espaços de desenvolvimento do Azure não está a utilizar o _Dockerfile_ que espera criar os seus contentores, poderá ter de informar os espaços de desenvolvimento do Azure onde é explicitamente. 

### <a name="try"></a>Experimente:
Abra o _azds.yaml_ ficheiro que foi gerado por espaços de desenvolvimento do Azure no seu projeto. Utilizar o `configurations->develop->build->dockerfile` diretiva para apontar para o Dockerfile que pretende utilizar:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```
