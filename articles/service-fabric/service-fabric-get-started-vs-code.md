---
title: Recursos de infraestrutura de serviço do Azure com o código de VS introdução | Microsoft Docs
description: Este artigo é uma descrição geral da criação de aplicações de Service Fabric com o Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: v-jamebr
ms.openlocfilehash: 367829c269bd1d96e6aa5fab1be008483a4ab5ab
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116202"
---
# <a name="service-fabric-for-visual-studio-code"></a>Service Fabric para Visual Studio Code

O [extensão de recursos de infraestrutura de serviço Reliable Services para o VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) fornece as ferramentas necessárias para criar, criar e depurar aplicações de Service Fabric em sistemas operativos Windows, Linux e macOS.

Este artigo fornece uma descrição geral dos requisitos e a configuração da extensão, bem como a utilização dos vários comandos que são fornecidos pela extensão. 

> [!IMPORTANT]
> Aplicações de Java de recursos de infraestrutura de serviço podem ser desenvolvidas nas máquinas do Windows, mas podem ser implementadas em clusters do Linux do Azure apenas. A depuração de aplicações Java não é suportada no Windows.

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos devem ser instalados em todos os ambientes.

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [SDK do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Yeoman Generators – instalar os generators adequados para a sua aplicação

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

Os seguintes pré-requisitos tem de estar instalados para o desenvolvimento do Java:

* [Java SDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (versão 1.8)
* [Gradle](https://gradle.org/install/)
* [Depurador para a extensão de Java VS Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) necessários para os serviços de Java de depuração. Serviços de Java de depuração só é suportado no Linux. Pode instalar um clicando no ícone de extensões no **atividade barra** no VS Code e procure-os para a extensão ou no Marketplace de código de VS.

Os seguintes pré-requisitos tem de estar instalados para .NET Core / c# desenvolvimento:

* [.NET core](https://www.microsoft.com/net/learn/get-started) (versão 2.0.0 ou posterior)
* [C# para a extensão do Visual Studio Code (com tecnologia OmniSharp) VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) necessários para depurar c# serviços. Pode instalar um clicando no ícone de extensões no **atividade barra** no VS Code e procure-os para a extensão ou no Marketplace de código de VS.

## <a name="setup"></a>Configurar

1. Abra o VS Code.
2. Clique no ícone de extensões no **atividade barra** no lado esquerdo do VS Code. Procure "Service Fabric". Clique em **instalar** para a extensão de recursos de infraestrutura de serviço Reliable Services.

## <a name="commands"></a>Comandos
A extensão de recursos de infraestrutura de serviço Reliable Services para o VS Code fornece vários comandos para ajudar os programadores criar e implementar projetos de Service Fabric. Podem chamar comandos a partir do **paleta de comando** premindo `(Ctrl + Shift + p)`, escrevendo o nome do comando na barra de entrada e selecionar o comando pretendido na lista de linha de comandos. 

* Recursos de infraestrutura do serviço: Criar a aplicação 
* Recursos de infraestrutura do serviço: Publicar a aplicação 
* Recursos de infraestrutura do serviço: Implementar a aplicação 
* Recursos de infraestrutura do serviço: Remover a aplicação  
* Recursos de infraestrutura do serviço: Aplicação de compilação 
* Service Fabric: Aplicação de raiz 

### <a name="service-fabric-create-application"></a>Recursos de infraestrutura do serviço: Criar a aplicação

O **Service Fabric: Criar aplicação** comando cria uma nova aplicação de Service Fabric na sua área de trabalho atual. Dependendo do que generators yeoman estão instalados no computador de desenvolvimento, pode criar vários tipos de aplicação de Service Fabric, incluindo os projetos de Java, c#, contentor e convidado. 

1.  Selecione o **Service Fabric: Adicionar serviço** comando
2.  Selecione o tipo para a sua nova aplicação de Service Fabric. 
3.  Introduza o nome da aplicação que pretende criar
3.  Selecione o tipo de serviço que pretende adicionar à sua aplicação de Service Fabric. 
4.  Siga as instruções para o serviço de nomes. 
5.  A nova aplicação de Service Fabric aparece na área de trabalho.
6.  Abra a pasta de aplicação nova para que torna-se a pasta raiz na área de trabalho. Pode continuar a executar comandos a partir daqui.

### <a name="service-fabric-add-service"></a>Recursos de infraestrutura do serviço: Adicionar serviço
O **Service Fabric: Adicionar serviço** comando adiciona um novo serviço para uma aplicação de Service Fabric existente. A aplicação que o serviço será adicionado ao tem de ser o diretório de raiz da área de trabalho. 

1.  Selecione o **Service Fabric: Adicionar serviço** comando.
2.  Selecione o tipo da sua aplicação de Service Fabric atual. 
3.  Selecione o tipo de serviço que pretende adicionar à sua aplicação de Service Fabric. 
4.  Siga as instruções para o serviço de nomes. 
5.  O novo serviço aparece no diretório do seu projeto. 

### <a name="service-fabric-publish-application"></a>Recursos de infraestrutura do serviço: Publicar a aplicação
O **Service Fabric: publicar aplicações** comando implementa a aplicação de Service Fabric num cluster remoto. O cluster de destino pode ser segura ou um cluster não seguro. Se os parâmetros não estejam definidos no Cloud.json, a aplicação for implementada para o cluster local.

1.  Na primeira vez que a aplicação é criada, é gerado um ficheiro de Cloud.json no diretório do projeto.
2.  Introduza os valores para o cluster que pretende ligar no ficheiro Cloud.json.
3.  Selecione o **Service Fabric: publicar aplicações** comando.
4.  Visualize o cluster de destino com o Service Fabric Explorer para confirmar que a aplicação foi instalada. 

### <a name="service-fabric-deploy-application-localhost"></a>Recursos de infraestrutura do serviço: Implementar a aplicação (Localhost)
O **Service Fabric: implementar a aplicação** comando implementa a aplicação de Service Fabric ao local cluster. Certifique-se de que o seu cluster local está em execução antes de utilizar o comando. 

1.  Selecione o **Service Fabric: implementar a aplicação** comando
2.  Visualizar o cluster local com o Service Fabric Explorer (http://localhost:19080/Explorer) para confirmar que a aplicação foi instalada. Esta operação pode demorar algum tempo, por isso ser patient.
3.  Também pode utilizar **Service Fabric: publicar aplicações** comando sem parâmetros definido no ficheiro Cloud.json para implementar um cluster local.

> [!NOTE]
> Implementação de aplicações Java ao local cluster não é suportada em máquinas do Windows.

### <a name="service-fabric-remove-application"></a>Recursos de infraestrutura do serviço: Remover a aplicação
O **Service Fabric: remover aplicação** comando remove uma aplicação de Service Fabric do cluster que foi anteriormente implementado para utilizar a extensão de VS Code. 

1.  Selecione o **Service Fabric: remover aplicação** comando.
2.  Visualize o cluster com o Service Fabric Explorer para confirmar que a aplicação foi removida. Esta operação pode demorar algum tempo, por isso ser patient.

### <a name="service-fabric-build-application"></a>Recursos de infraestrutura do serviço: Aplicação de compilação
O **Service Fabric: remover aplicação** comando pode criar aplicações de Java ou c# Service Fabric. 

1.  Certifique-se de que está na pasta raiz de aplicação antes de executar este comando. O comando identifica o tipo de aplicação (c# ou Java) e baseia-se a aplicação em conformidade.
2.  Selecione o **Service Fabric: Criar aplicação** comando.
3.  A saída do processo de compilação é escrita o terminal integrado.

### <a name="service-fabric-clean-application"></a>Service Fabric: Aplicação de raiz
O **Service Fabric: aplicação limpa** comando elimina todos os ficheiros jar e bibliotecas nativas que foram geradas por compilação. Válido para apenas aplicações de Java. 

1.  Certifique-se de que está na pasta raiz de aplicação antes de executar este comando. 
2.  Selecione o **Service Fabric: aplicação limpa** comando.
3.  A saída do processo de limpa é escrita o terminal integrado.

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [desenvolver e depurar aplicações de Service Fabric de c# com o código de VS](./service-fabric-develop-csharp-applications-with-vs-code.md).
* Saiba como [desenvolver e depurar aplicações de Java do Service Fabric com o código de VS](./service-fabric-develop-java-applications-with-vs-code.md).
