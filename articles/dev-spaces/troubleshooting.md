---
title: Resolução de problemas | Documentos da Microsoft
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contentores
manager: douge
ms.openlocfilehash: fdf195d96bb455334cb4e898e560813ee8709a50
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035657"
---
# <a name="troubleshooting-guide"></a>Guia de resolução de problemas

Este guia contém informações sobre problemas comuns que poderá ter quando utilizar espaços de desenvolvimento do Azure.

## <a name="error-service-cannot-be-started"></a>Erro "serviço não pode ser iniciado."

Pode ver este erro quando o seu código de serviço não iniciar. A causa é, muitas vezes, no código do usuário. Para obter mais informações de diagnóstico, efetue as seguintes alterações aos seus comandos e as definições:

Na linha de comando:

1. Ao usar _azds.exe_, utilize a opção-- verbose da linha de comandos e utilizar a opção-- saída da linha de comandos para especificar o formato de saída.
 
    ```cmd
    azds up --verbose --output json
    ```

No Visual Studio:

1. Open **ferramentas > opções** e, em **projetos e soluções**, escolha e **compilar e executar**.
2. Alterar as definições de **verbosidade de saída da compilação de projeto do MSBuild** ao **Detailed** ou **diagnóstico**.

    ![Caixa de diálogo Opções de captura de ecrã de ferramentas](media/common/VerbositySetting.PNG)

## <a name="error-required-tools-and-configurations-are-missing"></a>Erro "Necessário ferramentas e configurações estão em falta"

Este erro poderá ocorrer ao iniciar o VS Code: "[espaços de desenvolvimento do Azure] necessário a ferramentas e configurações para compilar e depurar"[nome do projeto]"estão em falta."
O erro significa que azds.exe não está na variável de ambiente PATH, como visto no VS Code.

### <a name="try"></a>Experimente:

Inicie o VS Code a partir de um prompt de comando em que a variável de ambiente PATH está definida corretamente.

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
A iniciar o depurador do VS Code, às vezes, pode resultar em erro. Este é um problema conhecido.

### <a name="try"></a>Experimente:
1. Feche e reabra o VS Code.
2. Aperte F5 novamente.


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