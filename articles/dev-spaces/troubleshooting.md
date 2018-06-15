---
title: Resolução de problemas | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Desenvolvimento rápido Kubernetes com contentores e micro-serviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes serviço, contentores
manager: douge
ms.openlocfilehash: 371bb9195266f3511d115de2532e6b64f49ef26f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34199301"
---
# <a name="troubleshooting-guide"></a>Guia de resolução de problemas

Este guia contém informações sobre problemas comuns que poderá ter quando utilizar espaços de programador do Azure.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Erro 'upstream erro de ligar ou desligar/reset antes de cabeçalhos'
Poderá ver este erro quando tentar aceder ao seu serviço. Por exemplo, quando aceder ao URL do serviço num browser. 

### <a name="reason"></a>Razão 
A porta do contentor não está disponível. Isto pode dever-se ao facto: 
* O contentor está ainda a ser criada e implementada. Isto pode ser o caso, se executar `azds up` ou iniciar o depurador e, em seguida, tentar aceder ao contentor antes de ser implementada com êxito.
* Configuração da porta não é consistente em seu Dockerfile, Helm gráfico e qualquer código de servidor que se abre uma porta.

### <a name="try"></a>Tente:
1. Se o contentor está no processo de que está a ser criada/implementada, pode aguardar 2 a 3 segundos e tente novamente aceder ao serviço. 
1. Verifique a configuração de porta. Os números de porta especificado devem ser **idênticos** em todos os recursos abaixo:
    * **Dockerfile:** especificado pelo `EXPOSE` instrução.
    * **[Gráfico de Helm](https://docs.helm.sh):** especificado pelo `externalPort` e `internalPort` valores para um serviço (muitas vezes, localizado num `values.yml` ficheiro),
    * Nenhuma porta de cópia de segurança, que está a ser aberta no código da aplicação, por exemplo, no Node.js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Ficheiro de configuração não encontrado
Executar `azds up` e obterá o erro seguinte: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Razão
Tem de executar `azds up` do diretório de raiz do código que pretende executar e tem de inicializar a pasta de código para executar com espaços de programador do Azure.

### <a name="try"></a>Tente:
1. Altere o diretório atual para a pasta de raiz que contém o código do serviço. 
1. Se não tiver um ficheiro de azds.yaml na pasta de código, execute `azds prep` para gerar ativos Docker, Kubernetes e espaços de programador do Azure.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Erro: 'o pipe programa 'azds' terminado inesperadamente com o código 126.'
A iniciar o depurador VS Code, por vezes, poderá resultar neste erro. Este é um problema conhecido.

### <a name="try"></a>Tente:
1. Feche e reabra o VS Code.
2. Prima novamente F5.


## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Erro de depuração 'não é suportado o tipo de depuração de configurados 'coreclr' '
Executar o depurador VS Code relatórios de erro: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Razão
Tem a extensão de VS Code para espaços de programador do Azure instalado no computador de desenvolvimento.

### <a name="try"></a>Tente:
Instalar o [extensão VS Code para Azure Dev espaços](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>Não foi possível encontrar o nome do espaço de nomes ou tipo 'MyLibrary'

### <a name="reason"></a>Razão 
O contexto de compilação é o nível de serviço/projeto por predefinição, assim um projeto de biblioteca que está a utilizar não será possível encontrar.

### <a name="try"></a>Tente:
O que precisa de ser efetuada:
1. Modificar o ficheiro de azds.yaml para definir o contexto de compilação para o nível de solução.
2. Modificar os ficheiros Dockerfile e Dockerfile.develop para fazer referência aos ficheiros csproj corretamente, relativamente ao contexto de compilação de novo.
3. Colocar um ficheiro de .dockerignore junto o ficheiro. sln e modifique conforme necessário.

Pode encontrar um exemplo em https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftconnectedenvironmentregisteraction-authorization-error"></a>Erro de autorização 'Microsoft.ConnectedEnvironment/register/action'
Poderá ver o erro seguinte quando estiver a gerir um espaço do programador do Azure e estiver a trabalhar numa subscrição do Azure para o qual não tem proprietário ou contribuinte aceder.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.ConnectedEnvironment/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Razão
A subscrição do Azure selecionada não registou o espaço de nomes Microsoft.ConnectedEnvironment.

### <a name="try"></a>Tente:
Alguém com proprietário ou contribuinte acesso à subscrição do Azure pode executar o seguinte comando da CLI do Azure para registar manualmente o espaço de nomes Microsoft.ConnectedEnvironment:

```cmd
az provider register --namespace Microsoft.ConnectedEnvironment
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Os espaços de programador do Azure não parece estar a utilizar o meu Dockerfile existente para criar um contentor 

### <a name="reason"></a>Razão
Os espaços de programador do Azure pode ser configurados para apontar para um Dockerfile específica no seu projeto. Se parecer que os espaços de programador do Azure não está a utilizar Dockerfile esperar para criar os contentores, poderá ter explicitamente dizer espaços de programador do Azure onde se encontram. 

### <a name="try"></a>Tente:
Abra o `azds.yaml` ficheiro que foi gerado por espaços de programador do Azure no seu projeto. Utilize o `configurations->develop->build->dockerfile` diretiva para apontar para o Dockerfile que pretende utilizar:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```